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

> 在推荐系统中，我们假设用户和物品之间没有直接关系。但是我们可以通过 feature 把它们联系在一起。 feature 是用来刻画特征的，比如描述某个电影是喜剧还是悲剧，是动作片还是爱情片。用户和 feature 之间是有关系的，比如某个用户喜欢看爱情片，另外一个用户喜欢看动作片；物品和 feature 之间也是有关系的，比如某个电影是喜剧，某个电影是悲剧，那么通过和 feature 之间的联系，我们就找到了用户和物品之间的关联。

![此处输入图片的描述][2]

假设我们现在有评分矩阵 $V \in \mathbb{R}^{n \times m}$,SVD实际上就是去找到两个矩阵： $U \in \mathbb{R}^{f \times n}$，$M \in \mathbb{R}^{f \times m}$，其中矩阵 $U$ 表示 User 和 feature 之间的联系，矩阵 $V$ 表示 Item 和 feature 之间的联系。

$$V = U^TM \tag{1}$$ 

![此处输入图片的描述][3]

大家这时候肯定会想，我们的评分矩阵里面一般会有很多**缺失值**，那要怎么去得到 $U$ 和 $M$ 呢？实际上，这两个矩阵是通过**学习**的方式得到的，而不是直接做矩阵分解。我们定义如下的损失函数：

$$E = \frac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{m}I_{ij}(V_{ij} - p(U_i,M_j))^2+\frac{k_u}{2}\sum_{i=1}^{n}\lVert U_i \rVert^2 + \frac{k_m}{2}\sum_{j=1}^{m}\lVert M_j \rVert^2 \tag{2}$$

> 注： 实际上就是最小化平方误差，同时加入正则项防止过拟合。

其中 $p(U_i,M_j)$ 表示我们对用户 i 对 物品 j 的评分预测：

$$p(U_i,M_j) = U_i^TM_j$$

这样就变成了机器学习中常见的优化问题了，剩下的问题就是优化算法的选择了。

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

#### Adding Biases
实际上，我们给一部电影评分时，除了考虑电影是否合自己口味外，还和自己的评分习惯有关。例如：一个严格评分者给的分大多数情况下都比一个宽松评分者的低。另外，你看到这部电影的评分大部分较高时，可能也倾向于给较高的分。在SVD中，口味问题已经有因子来表示了，但是剩下因素并没有考虑在内。

$$\hat{r}_{ui} = \mu + b_i + b_u + \mathbf{q}_i^T\mathbf{p}_u \tag{5}$$

 - $\mu$： 训练集中所有记录的评分的**全局平均数**。在不同网站中，因为网站定位和销售的物品不同，网站的整体评分分布也会显示出一些差异。比如有些网站中的用户就是喜欢打高分，而另一些网站的用户就是喜欢打低分。而全局平均数可以表示网站本身对用户评分的影响。
 
 - $b_u$： **用户偏置**（user bias）项。这一项表示了用户的评分习惯中和物品没有关系的那种因素。比如有些用户就是比较苛刻，对什么东西要求都很高，那么他的评分就会偏低，而有些用户比较宽容，对什么东西都觉得不错，那么他的评分就会偏高。
 
 - $b_i$： **物品偏置**（item bias）项。这一项表示了物品接受的评分中和用户没有什么关系的因素。比如有些物品本身质量就很高，因此获得的评分相对都比较高，而有些物品本身质量很差，因此获得的评分相对都会比较低。（摘录自"**推荐系统实战**"）

这个时候我们的损失函数变为：

$$E = \sum_{(u,i)\in \mathcal{k}}(r_{ui}-\mu - b_i - b_u - \mathbf{q}_i^T\mathbf{p}_u) + \lambda (\lVert p_u \rVert_2) + \lVert q_i \rVert_2 + b_u^2 + b_i^2) \tag{6}$$

  [1]: http://charlesx.top/2016/03/Singularly-Valuable-Decomposition/
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-13_123323.png?imageView2/2/w/500
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-13_125348.png?imageView2/2/w/600
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-13_132245.png
  
  [^1]: [A Guide to Singular Value Decomposition for Collaborative Filtering](http://www.csie.ntu.edu.tw/~r95007/thesis/svdnetflix/report/report.pdf)