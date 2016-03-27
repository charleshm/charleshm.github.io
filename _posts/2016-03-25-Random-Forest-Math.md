---
published: true
author: Charles
layout: post
title:  "随机森林中的数学理论"
date:   2016-03-25 15:30
categories: 机器学习
---

随机森林是一种有效的分类预测方法，它有很高的**分类精度**，对于噪声和异常值有较好的稳健性，且具有较强的**泛化能力**。Breiman[^1]在论文中提出了随机森林的数学理论，证明随机森林不会出现决策树的过拟合问题，推导随机森林的泛化误差界，并且利用 Bagging 产生的袋外数据给出泛化误差界的估计。

![此处输入图片的描述][1]


----------

#### 随机森林
![此处输入图片的描述][2]

#### 为什么不会过拟合
Breiman证明了，随着随机森林中决策树增加，其泛化误差会趋于一个**有限的上限**。

  [^1]: [Random Forest](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-27_164830.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-27_165533.png