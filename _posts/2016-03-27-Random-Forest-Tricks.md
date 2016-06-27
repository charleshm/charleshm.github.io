---
published: true
author: Charles
layout: post
title:  "随机森林中的一些Tricks"
date:   2016-03-27 8:30
categories: 机器学习
---

####  缺失值处理

RandomForest包里有两种补全缺失值的方法：

---

- **方法一**（na.roughfix）简单粗暴，对于训练集,同一个class下的数据，如果是分类变量缺失，用众数补上，如果是连续型变量缺失，用中位数补。

- **方法二**（rfImpute）这个方法计算量大，至于比方法一好坏？不好判断。先用na.roughfix补上缺失值，然后构建森林并计算proximity matrix，再回头看缺失值，如果是分类变量，则用没有缺失的观测实例的proximity中的权重进行投票。如果是连续型变量，则用proximity矩阵进行加权平均的方法补缺失值。然后迭代4-5次，这个补缺失值的思想和KNN有些类似。

---

> Proximity 用来衡量两个样本之间的相似性，当一棵树生成后，让数据集通过这棵树，落在同一个叶子节点的"样本对" proximity 值加 1 。所有的树生成之后，利用树的棵树归一化 proximity matrix。

![Proximity matrix][1]

---



[1]:http://7xjbdi.com1.z0.glb.clouddn.com/proximity_matrix.png?imageView2/2/w/300
