---
published: true
author: Charles
layout: post
title:  "SVD在推荐系统中的应用"
date:   2016-03-12 15:20
categories: 机器学习 推荐系统
---

之前在 “**[漫谈奇异值分解][1]**” 里面我们已经对SVD的理论做了阐述。其实，在工程领域，其应用相当广泛，很多东西都可以和奇异值扯上关系，比如主成分分析（PCA），LSI（Latent Semantic Indexing），推荐系统等等。

我们重复强调一下SVD的思想（很重要），

> 在推荐系统中，用户和物品之间没有直接关系。但是我们可以通过 feature 把它们联系在一起。对于电影来说，这样的特征可以是：喜剧还是悲剧，是动作片还是爱情片。用户和这样的 feature 之间是有关系的，比如某个用户喜欢看爱情片，另外一个用户喜欢看动作片；物品和 feature 之间也是有关系的，比如某个电影是喜剧，某个电影是悲剧。那么通过和 feature 之间的联系，我们就找到了用户和物品之间的关联。

![此处输入图片的描述][2]

我们引用 Koren 论文里面的一段简述，

> Latent factor models, such as Singular Value Decomposition (SVD), comprise an alternative approach by transforming both items and users to the same **latent factor space**, thus making them directly **comparable**. The latent space tries to explain ratings by characterizing both products and users on **factors** automatically inferred from user feedback. For example, when the products are movies, factors might measure obvious dimensions such as comedy vs. drama,amount of action, or orientation to children; less well defined dimensions such as depth of character development or “quirkiness”; or completely uninterpretable dimensions[^2].

假设我们现在有评分矩阵 $V \in \mathbb{R}^{n \times m}$,SVD实际上就是去找到两个矩阵： $U \in \mathbb{R}^{f \times n}$，$M \in \mathbb{R}^{f \times m}$，其中矩阵 $U$ 表示 User 和 feature 之间的联系，矩阵 $V$ 表示 Item 和 feature 之间的联系。

$$V = U^TM \tag{1}$$ 

![此处输入图片的描述][3]

大家这时候肯定会想，我们的评分矩阵里面一般会有很多**缺失值**，那要怎么去得到 $U$ 和 $M$ 呢？实际上，这两个矩阵是通过**学习**的方式得到的，而不是直接做矩阵分解。我们定义如下的损失函数：

$$E = \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{m}I_{ij}(V_{ij} - p(U_i,M_j))^2+\frac{k_u}{2}\sum_{i=1}^{n}\lVert U_i \rVert^2 + \frac{k_m}{2}\sum_{j=1}^{m}\lVert M_j \rVert^2 \tag{2}$$

> 注： 实际上就是最小化平方误差，同时加入正则项防止过拟合。

其中 $p(U_i,M_j)$ 表示我们对用户 i 对 物品 j 的评分预测：

$$p(U_i,M_j) = U_i^TM_j$$

这样就变成了机器学习中常见的优化问题了，剩下的问题就是优化算法的选择了。

----------

#### Batch learning[^1]
最简单的，我们可以采用随机梯度下降法，算法流程如下：

![此处输入图片的描述][4]

其中，

$$
\begin{align*}
-\frac{\partial E}{\partial U_i} & = \sum_{j=1}^{M}I_{ij}((V_{ij}-p(U_i,M_j))M_j) - k_uU_i \tag{3} \\
-\frac{\partial E}{\partial M_j} & = \sum_{i=1}^{n}I_{ij}((V_{ij}-p(U_i,M_j))U_i) - k_mM_j \tag{4}
\end{align*}$$

**这就是基础版的SVD算法，下面我们慢慢的去完善它。**

----------

#### Adding Biases
实际上，我们给一部电影评分时，除了考虑电影是否合自己口味外，还和自己的评分习惯有关。例如：一个严格评分者给的分大多数情况下都比一个宽松评分者的低。另外，你看到这部电影的评分大部分较高时，可能也倾向于给较高的分。在SVD中，口味问题已经有因子来表示了，但是剩下因素并没有考虑在内。

$$\hat{r}_{ui} = \mu + b_i + b_u + \mathbf{q}_i^T\mathbf{p}_u \tag{5}$$

 - $\mu$： 训练集中所有记录的评分的**全局平均数**。在不同网站中，因为网站定位和销售的物品不同，网站的整体评分分布也会显示出一些差异。比如有些网站中的用户就是喜欢打高分，而另一些网站的用户就是喜欢打低分。而全局平均数可以表示网站本身对用户评分的影响。
 
 - $b_u$： **用户偏置**（user bias）项。这一项表示了用户的评分习惯中和物品没有关系的那种因素。比如有些用户就是比较苛刻，对什么东西要求都很高，那么他的评分就会偏低，而有些用户比较宽容，对什么东西都觉得不错，那么他的评分就会偏高。
 
 - $b_i$： **物品偏置**（item bias）项。这一项表示了物品接受的评分中和用户没有什么关系的因素。比如有些物品本身质量就很高，因此获得的评分相对都比较高，而有些物品本身质量很差，因此获得的评分相对都会比较低。（摘录自"**推荐系统实战**"）

这个时候我们的损失函数变为：

$$E = \sum_{(u,i)\in \mathcal{k}}(r_{ui}-\mu - b_i - b_u - \mathbf{q}_i^T\mathbf{p}_u) + \lambda (\lVert p_u \rVert_2) + \lVert q_i \rVert_2 + b_u^2 + b_i^2) \tag{6}$$

----------

### Implicit feedback[^3]
数据是一切推荐系统的基础。良好的推荐效果一定是来自于丰富而准确的数据。这些数据既包括了用户（user）和待推荐物品（item）相关的基础信息，另一方面，user和item之间在网站或应用中发生的用户行为和关系数据也非常重要。因为这些用户行为和关系数据能真实的反映每个用户的偏好和习惯。采集这些基础数据，并做好清洗和预处理，是整个推荐系统的基石。

用户行为数据，又可细分为两部分：**显式反馈数据**（explicit feedbacks）和**隐式反馈**（implicit feedbacks）数据。

显式反馈是指能明确表达用户好恶的行为数据，例如用户对某商品的购买、收藏、评分等数据。与之相反，隐式反馈数据是指无法直接体现用户偏好的行为，例如用户在网站中的点击、浏览、停留、跳转、关闭等行为。通过挖掘显式反馈数据能明确把握用户的偏好，但在很多应用中，显式反馈数据通常很稀疏，导致对用户偏好的挖掘无法深入。这个问题在一些刚上线的应用、或者偏冷门的物品或用户身上反映尤其明显。在这种情况下，用户的隐式反馈数据就显得尤为重要。因为虽然用户在网站中的点击等行为很庞杂，但其中蕴藏了大量信息。在2006-2008年间进行的国际著名推荐竞赛Netflix Prize中，冠军队成员Koren发现将用户租用影片的记录，转换为特征向量注入奇异值分解算法（SVD）用于影响用户兴趣向量，能够很好的提高推荐准确率。

> **那么，接下来的重点是，如何在我们的SVD模型里面利用上这些隐式反馈？**

----------

#### 改进Item CF模型
但上面的模型并没有显式地考虑用户的历史行为对用户评分预测造成的影响，也就是没有利用到用户的 Implicit feedback。在传统的 Item CF 里面，我们的预测模型通常是：

$$\hat{r}_{ui} = b_{ui} + \sum_{j \in S^k(i;u)} \theta_{ij}^u(r_{uj} - b_{uj})$$

> 注： $S^k(i;u)$ 表示用户u评过分的商品集合中与 $i$ 最相近的 $k$ 项.

这个时候 $\theta_{ij}^u$ 是与 User 相关的，我们可以尝试用全局权重去简化：

$$\hat{r}_{ui} = b_{ui} + \sum_{j \in R(u)} w_{ij}(r_{uj} - b_{uj})$$

> 注：R(u)表示用户u评过分的商品集合，N(u)表示用户u浏览过但没有评过分的商品集合.

另外，我们知道一个用户的品味并不仅仅由他评分过的物品决定，也一定程度上取决于他所浏览过的物品（未评分）。那么，我们可以试着加入 Implicit feedback 的影响：

$$\hat{r}_{ui} = b_{ui} + \sum_{j \in R(u)} w_{ij}(r_{uj} - b_{uj}) + \sum_{j \in N(u)} c_{ij}$$

关于这个权重项，我们可以这样去理解：假设我们在电影评分数据集中发现，给"指环王3"打高分的用户，通常也会给“指环王1-2”高分，我们现在要预测A用户对“指环王3”的评分，但它并没有对“指环王1-2”评分，那么这个时候我们没有利用上这样的信息。加上 $c_{ij}$ 这项后我们就可以更加充分的利用这些信息。

再加点小 trick,

$$\hat{r}_{ui} = b_{ui} + \vert R(u) \vert^{-\frac{1}{2}}\sum_{j \in R(u)} w_{ij}(r_{uj} - b_{uj}) + \vert N(u) \vert^{-\frac{1}{2}}\sum_{j \in N(u)} c_{ij} \tag{7}$$

改进到这样，我们已经可以通过**学习**的方式获取这些参数，这时的损失函数变为：

$$E = \sum_{(u,i)\in \mathcal{K}} \left( r_{ui} - \mu - b_u - b_i  - \vert R(u) \vert^{-\frac{1}{2}}\sum_{j \in R(u)} w_{ij}(r_{uj} - b_{uj}) - \vert N(u) \vert^{-\frac{1}{2}}\sum_{j \in N(u)} c_{ij} \right) + \lambda_4\left( (b_u)^2 + (b_i)^2 + \sum_{j \in R(u)} w_{ij}^2 + \sum_{j \in N(u)} c_{ij}^2 \right)$$

----------


#### Asymmetric-SVD
将传统Item CF模型改进为可学习的模型后，这个时候我们就可以尝试着将它和SVD模型混合。

$$\hat{r}_{ui} = b_{ui} + p_u^Tq_i$$

> Koren的想法很有意思，利用用户评过分的商品和用户浏览过尚未评分的商品属性来表示用户属性。

**用 Item 来建模 User**.

这有一定的合理性，因为用户的行为记录本身就能反应用户的喜好。而且，这个模型可以带来一个很大的好处，一个商场或者网站的用户数成千上万甚至过亿，存储用户属性会占用巨大的存储空间，而商品数相比来说会小很多。

$$\hat{r}_{ui} = b_{ui} + q_i^T\left( \vert R(u) \vert^{-\frac{1}{2}}\sum_{j \in R(u)} x_{j}(r_{uj} - b_{uj}) + \vert N(u) \vert^{-\frac{1}{2}}\sum_{j \in N(u)} y_{j}\right) \tag{8}$$

这个时候，我们是用 $\vert R(u) \vert^{-\frac{1}{2}}\sum_{j \in R(u)} x_{j}(r_{uj} - b_{uj}) + \vert N(u) \vert^{-\frac{1}{2}}\sum_{j \in N(u)} y_{j}$ 来建模 $p_u$ . 

这里还有个细节问题，就是之前的 $w_{ij}$ 是一个比较稠密的矩阵，存储它需要比较大的空间。此外，如果有n个物品，那么该模型的参数个数就是 $n^2$ 个，这个参数个数比较大，容易造成结果的过拟合。因此，Koren提出应该对 w 矩阵也进行分解，将参数个数降低到 $2nF$ 个，模型如下：

$$\sum_{j \in R(u)} w_{ij} = \sum_{j \in R(u)} x_i^Ty_j$$


----------


#### SVD++
SVD++里面则使用 $p_u + \vert N(u) \vert^{-\frac{1}{2}}\sum_{j \in N(u)} y_{j}$ 来对用户进行建模，其思路来源和Asymmetric-SVD是类似的。

$$\hat{r}_{ui} = b_{ui} + q_i^T\left( p_u + \vert N(u) \vert^{-\frac{1}{2}}\sum_{j \in N(u)} y_{j}\right) \tag{9}$$

#### 后记
Koren的想法太灵活了，各种玩，乐在其中呀~

最近的学习计划：

$$LSA \rightarrow pLSA \rightarrow LDA$$

![topic models][5]

----------


  [1]: http://charlesx.top/2016/03/Singularly-Valuable-Decomposition/
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-13_123323.png?imageView2/2/w/500
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-13_125348.png?imageView2/2/w/600
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-13_132245.png
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-16_143218.png?imageView2/2/w/500
  
  [^1]: [A Guide to Singular Value Decomposition for Collaborative Filtering](http://www.csie.ntu.edu.tw/~r95007/thesis/svdnetflix/report/report.pdf)
  [^2]: [Factorization Meets the Neighborhood: a Multifaceted Collaborative Filtering Model](https://github.com/gpfvic/IRR/blob/master/Factorization%20meets%20the%20neighborhood-%20a%20multifaceted%20collaborative%20filtering%20model.pdf)
  [^3]: [推荐系统开发的十个关键点](http://www.wtoutiao.com/p/h1fp8s.html)