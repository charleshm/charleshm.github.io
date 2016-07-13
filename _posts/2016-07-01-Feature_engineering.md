---
published: true
author: Charles
layout: post
title:  "特征工程中的trick"
date:   2016-07-01 8:30
categories: 机器学习
---

#### 取值很多的类别特征

比如现在有一维城市特征，取值1000多个，常用的处理方法大概可分为四种[^1]：

> - $one\ hot$ 编码，优点是简单，缺点是维度高了，太稀疏。    
- 对分类变量做特征工程，汇总分组啊之类的，降维。   
- 把特征转换为数值变量，例如把不同城市的编码变成对应的经纬度，或是城市人口值，或是Y值，类似于银行信用评级中的$WOE(weight\ of\ Evidence)$等。   
- 词向量的思路，做 $embedding$

推荐下肖凯老师的博客：[**数据科学中的R和Python**](http://xccds1977.blogspot.com/)

> Learning with counts can be summarized as aggregating sufficient statistics for conditional probability distributions for various attributes and combinations – or, concretely, utilizing tables of per-class counts for each unique value or combination. 

---

#### WOE and IV

在机器学习的二分类问题中，WOE(Weight of Evidence)和 Information Value 通常用来对输入变量进行编码及预测能力评估[^3]。

> - WOE describes the relationship between a predictive variable and a binary target variable.  
- IV measures the strength of that relationship.

---

$$\underbrace{\log \frac{P(Y=1 | X_j)}{P(Y=0 | X_j)}}_{X_j\ log-odds} = \underbrace{\log \frac{P(Y=1)}{P(Y=0)}}_{\text{sample log-odds}} + \underbrace{\log \frac{f(X_j | Y=1)}{f(X_j | Y=0)}}_{\text{WOE}}, \nonumber$$

我们来细致分析下，讨论离散取值的情况[^2]：

$$
\begin{align*}
\text{WOE} & = \log \frac{P(X_j | Y=1)}{P(X_j | Y=0)}\\
& = \log \frac{P(Y=1 | X_j)}{P(Y=0 | X_j)} - \log \frac{P(Y=1)}{P(Y=0)}
\end{align*}
$$

> woe 反映的是在自变量每个分组下违约用户对正常用户占比和总体中违约用户对正常用户占比之间的差异，从而可以直观的认为 woe 蕴含了自变量取值对于目标变量（违约概率）的影响。

---

$$\text{IV}_j = \int \log \frac{f(X_j | Y=1)}{f(X_j | Y=0)} \, (f(X_j | Y=1) - f(X_j | Y=0)) \, dx. \nonumber$$

> IV 衡量的是某一个变量的预测能力，从公式来看的话，相当于是自变量 woe 值的一个加权求和，其值的大小决定了自变量对于目标变量的影响程度。一般来说，$\text{IV} < 0.05$，说明变量的预测能力非常低。

$$\text{IV}_j =  \sum_{i=1}^k (P(X_j \in B_i | Y=1) - P(X_j \in B_i | Y=0)) \times \text{WOE}_{ij}. \nonumber$$

![][1]

---

[1]:http://7xjbdi.com1.z0.glb.clouddn.com/woeOverviewExample.gif


[^1]:[机器学习模型中的分类变量最多可以有多少个值？](https://www.zhihu.com/question/38438477/answer/76744552)
[^2]:[Data Exploration with Weight of Evidence and Information Value in R](http://multithreaded.stitchfix.com/blog/2015/08/13/weight-of-evidence/)
[^3]:[评分卡模型剖析之一（woe、IV、ROC、信息熵）](http://blog.sina.com.cn/s/blog_8813a3ae0102uyo3.html)


- 方法一（na.roughfix）简单粗暴，对于训练集,同一个class下的数据，如果是分类变量缺失，用众数补上，如果是连续型变量缺失，用中位数补。
- 方法二（rfImpute）这个方法计算量大，先用na.roughfix补上缺失值，然后构建随机森林并计算proximity matrix，再回头看缺失值，如果是分类变量，则用没有缺失的观测实例的proximity中的权重进行投票。如果是连续型变量，则用proximity矩阵进行加权平均的方法补缺失值。然后迭代4-6次，这个补缺失值的思想和KNN有些类似。
