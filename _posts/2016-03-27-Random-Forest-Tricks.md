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

- **方法二**（rfImpute）这个方法计算量大，至于比方法一好坏？不好判断。先用na.roughfix补上缺失值，然后构建森林并计算proximity matrix，再回头看缺失值，如果是分类变量，则用没有缺失的观测实例的proximity中的权重进行投票。如果是连续型变量，则用proximity矩阵进行加权平均的方法补缺失值。然后**迭代**4-5次，这个补缺失值的思想和KNN有些类似[^1][^2]。

---

> Proximity 用来衡量两个样本之间的相似性。原理就是如果两个样本落在树的同一个叶子节点的次数越多，则这两个样本的相似度越高。当一棵树生成后，让数据集通过这棵树，落在同一个叶子节点的"样本对$(x_i,x_j)$" proximity 值 $P(i,j)$ 加 1 。所有的树生成之后，利用树的数量来归一化 proximity matrix。

![Proximity matrix][1]

---

#### Variable importance

衡量变量重要性的方法有两种，Decrease GINI 和 Decrease Accuracy：

- **Decrease GINI**： 对于回归问题，直接使用$\arg \max(Var-VarLeft-VarRight)$作为评判标准，即当前节点训练集的方差$Var$减去左节点的方差$VarLeft$和右节点的方差$VarRight$。

- **Decrease Accuracy**：对于一棵树$T_{b}(x)$，我们用OOB样本可以得到测试误差1；然后随机改变OOB样本的第$j$列：保持其他列不变，对第$j$列进行随机的上下置换，得到误差2。至此，我们可以用误差1-误差2来刻画变量$j$的重要性。基本思想就是，如果一个变量$j$足够重要，那么改变它会极大的增加测试误差；反之，如果改变它测试误差没有增大，则说明该变量不是那么的重要。

$$
OOB=\left[\begin{array}{ccccc}x_{11} & \cdots & x{}_{1j} & \cdots & x_{1p}\\ \vdots & \vdots & \vdots & \vdots & \vdots\\ x_{k1} & \cdots & x{}_{kj} & \cdots & x_{kp}\end{array}\right]
$$

![Proximity matrix][2]

[1]:http://7xjbdi.com1.z0.glb.clouddn.com/proximity_matrix.png?imageView2/2/w/300
[2]:http://7xjbdi.com1.z0.glb.clouddn.com/building-random-forest-at-scale-26-638.jpg?imageView2/2/w/400


[^1]:[Random Forests(Leo Breiman and Adele Cutler)](https://www.stat.berkeley.edu/~breiman/RandomForests/cc_home.htm)
[^2]:[随机森林算法](http://www.cnblogs.com/litao1105/p/5021747.html)
