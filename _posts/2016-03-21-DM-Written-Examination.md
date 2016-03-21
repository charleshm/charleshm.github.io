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
- 空间距离
- 欧氏距离
- 曼哈顿距离
- 切比雪夫距离
- 闵可夫斯基距离（统一）

$$\begin{align*}
d & = \sqrt{\sum_{k=1}^{n}(a_k-b_k)} = \sqrt{(a-b)(a-b)^T} \rightarrow \text{欧氏距离(Euclidean Distance)}\\
d & = \sum_{k=1}^{n} |a_k-b_k| \rightarrow \text{曼哈顿距离(Manhattan Distance)}\\
d & = \underset{i}{\max}(|a_i-b_i|)  \rightarrow  \text{切比雪夫距离(Chebyshev Distance)}\\
d & = \sqrt[q]{\sum_{k=1}^{n} |a_k-b_k|^p}  \rightarrow  \text{闵可夫斯基距离(Minkowski Distance)}\\
d = 
\end{align*}$$