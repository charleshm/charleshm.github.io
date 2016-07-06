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

> - $one\ hot$编码，优点是简单，缺点是维度高了，太稀疏。    
- 对分类变量做特征工程，汇总分组啊之类的，降维。   
- 把特征转换为数值变量，例如把不同城市的编码变成对应的经纬度，或是城市人口值，或是Y值，类似于银行信用评级中的$WOE(weight\ of\ Evidence)$等。   
- 词向量的思路，做 $embedding$

推荐下肖凯老师的博客：[**数据科学中的R和Python**](http://xccds1977.blogspot.com/)

---

#### WOE and IV

在机器学习的二分类问题中，WOE(Weight of Evidence)和 Information Value 通常用来对输入变量进行编码及预测能力评估。

$$\underbrace{\log \frac{P(Y=1 | X_j)}{P(Y=0 | X_j)}}_{X_j\ log-odds} = \underbrace{\log \frac{P(Y=1)}{P(Y=0)}}_{\text{sample log-odds}} + \underbrace{\log \frac{f(X_j | Y=1)}{f(X_j | Y=0)}}_{\text{WOE}}, \nonumber$$

我们来细致分析下，讨论离散取值的情况：

$$
\begin{align*}
\text{WOE} & = \log \frac{P(X_j | Y=1)}{P(X_j | Y=0)}\\
& = \log \cfrac{\cfrac{P(X_j,Y=1)}{P(Y=1)}}{\cfrac{P(X_j,Y=0)}{P(Y=0)}}\\
& = \log \cfrac{P(X_j,Y=1)}{P(X_j,Y=0)} \cfrac{P(Y=1)}{P(Y=0)}
\end{align*}
$$

---

[^1]:[机器学习模型中的分类变量最多可以有多少个值？](https://www.zhihu.com/question/38438477/answer/76744552)
