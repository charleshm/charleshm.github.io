---
published: true
author: Charles
layout: post
title:  "数据挖掘笔试准备"
date:   2016-03-21 9:30
categories: 数据挖掘  工作
---
向量 $a = (a_1,a_2,\cdots,a_n),b = (b_1,b_2,\cdots,b_n)$ .

#### 空间距离
- 欧氏距离：n 维空间内两点之间的距离.
- 曼哈顿距离：两个点在标准坐标系上的绝对轴距之和.
- 切比雪夫距离：n 维空间两点维度上的最大差值.
- 闵可夫斯基距离（统一）：一种距离的定义方式.

$$\begin{align*}
d & = \sqrt{\sum_{k=1}^{n}(a_k-b_k)} = \sqrt{(a-b)(a-b)^T} \rightarrow \text{欧氏距离(Euclidean Distance)}\\
d & = \sum_{k=1}^{n} |a_k-b_k| \rightarrow \text{曼哈顿距离(Manhattan Distance)}\\
d & = \underset{i}{\max}(|a_i-b_i|)  \rightarrow  \text{切比雪夫距离(Chebyshev Distance)}\\
d & = \sqrt[q]{\sum_{k=1}^{n} |a_k-b_k|^p}  \rightarrow  \text{闵可夫斯基距离(Minkowski Distance)}
\end{align*}$$

----------

#### 编码距离
- 汉明距离：两个等长字符串s1与s2，将其中一个变为另外一个所需要的最小替换次数。
- 编辑距离：两个字符串s1与s2，将其中一个变为另外一个所需要的最小最小编辑次数（插入、删除和替换）次数。

----------

#### 相关距离
- Jaccard相似系数：衡量两个集合的相似度.

$$J=\dfrac {|A \cap B|}{|A \cup B|}$$

- 余弦相似性

$$\cos \theta = \frac {\sum_{k=1}^{n} x_{k}y_{k}}{\sqrt{\sum_{k=1}^{n} x_k^{2}}\sqrt{\sum_{k=1}^{n} y_k^2}}$$

- 皮尔森相关系数：

$$\rho_{X,Y}= \frac{\operatorname{cov}(X,Y)}{\sigma_X \sigma_Y} =\frac{\sum_{i=1}^n(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^n(x_i - \bar{x})^2} \sqrt{\sum_{i=1}^n(y_i - \bar{y})^2}}$$

----------

#### TF-IDF
在进行关键词提取时，我们不仅应该考虑到到此出现的**次数**(词频:Term Frequency)，同时也要考虑单词的**重要性**(逆文档频率:Inverse Document Frequency)。

$$TF-IDF = \text{词频}(TF)\times \text{逆文档频率}(IDF)$$

其中，

$$\begin{align*}
\text{词频}(TF) & = \frac{\text{某个词在文章中出现的次数}}{\text{文章的总词数}}\\
\text{词频}(IDF) & = \log(\frac{\text{语料库的文档总数}} {\text{包含该词的文档数}+1})
\end{align*}$$

----------

#### 决策树
**ID3**

> ID3算法采用**信息增益**作为属性选择度量。信息增益的缺点是倾向于选择取值较多的属性，在有些情况下这类属性可能不会提供太多有价值的信息。同时ID3算法只能处理离散型的描述性属性。

**C4.5**

> C4.5算法是ID3算法的一种改进，使用**信息增益率**来作为属性选择量度。对于连续型的描述性属性，通过将这些连续型属性的值分成不同的区间，即“离散化”，再进行处理。在构造树的过程中，需要对数据集进行多次的顺序扫描和排序，因而导致算法的低效。

**CART**

> CART算法使用 Gini 指标作为属性选择度量, CART生成的是二叉树，不会像多叉树那样形成过多的数据碎片。决策树的生成就是递归构建二叉决策树的过程，对回归树用平方误差最小化的准则，对分类树采用基尼指数最小化准则，进行特征选择，生成二叉树。

对于回归树选择最优切分变量$j$与切分点$s$，求解：
$$\underset{j,s}{\min} \left[\underset{c_1}{\min} \sum_{x_i\in R_1(j,s)} (y_i-c_i)^2 + \underset{c_2}{\min} \sum_{x_i\in R_2(j,s)} (y_i-c_i)^2\right]$$

基本计算流程（要求熟练掌握）：

- 计算数据集$D$的经验熵$H(D)$

  $$H(D) = -\sum_{k=1}^{K} \frac{|C_k|}{|D|}\log_2 \frac{|C_k|}{|D|}$$

- 计算特征$A$对数据集$D$的经验条件熵$H(D|A)$

  $$H(D|A) = \sum_{i=1}^{n}\frac{|D_i|}{|D|}H(D_i) = -\sum_{i=1}^{n} \frac{|D_i|}{|D|} \sum_{k=1}^{K} \frac{|D_{ik}|}{|D_i|}\log_2 \frac{|D_{ik}|}{|D_i|}$$

- 计算信息增益(信息增益率).

$$\begin{align*}
g(D,A) & = H(D) - H(D|A)\\
g_R(D,A) & = \frac{g(D,A)}{H(D)}
\end{align*}$$
