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

- 皮尔森相关系数：

$$\rho_{X,Y}= \frac{\operatorname{cov}(X,Y)}{\sigma_X \sigma_Y} =\frac{\sum ^n _{i=1}(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum ^n _{i=1}(x_i - \bar{x})^2} \sqrt{\sum ^n _{i=1}(y_i - \bar{y})^2}}$$