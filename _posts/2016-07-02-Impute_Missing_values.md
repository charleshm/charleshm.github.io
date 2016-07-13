---
published: true
author: Charles
layout: post
title:  "缺失值处理技巧"
date:   2016-07-02 8:30
categories: 机器学习
---

### 数据离散化

#### 为什么要离散化

1. 稀疏向量内积乘法运算速度快，计算结果方便存储，容易scalable（扩展）。
2. 离散化后的特征对异常数据有很强的鲁棒性：比如一个特征是年龄>30是1，否则0。如果特征没有离散化，一个异常数据“年龄300岁”会给模型造成很大的干扰。
3. 逻辑回归属于广义线性模型，表达能力受限；单变量离散化为N个后，每个变量有单独的权重，相当于为模型引入了非线性，能够提升模型表达能力，加大拟合。
4. 离散化后可以进行特征交叉，由M+N个变量变为M*N个变量，进一步引入非线性，提升表达能力。
5. 特征离散化后，模型会更稳定，比如如果对用户年龄离散化，20-30作为一个区间，不会因为一个用户年龄长了一岁就变成一个完全不同的人。当然处于区间相邻处的样本会刚好相反，所以怎么划分区间是门学问。

> 李沐少帅指出，模型是使用离散特征还是连续特征，其实是一个“海量离散特征+简单模型” 同 “少量连续特征+复杂模型”的权衡。既可以离散化用线性模型，也可以用连续特征加深度学习。就看是喜欢折腾特征还是折腾模型了。通常来说，前者容易，而且可以n个人一起并行做，有成功经验；后者目前看很赞，能走多远还须拭目以待[^1]。

----------

#### 离散化方法

- **分裂式的和合并式的**
分裂的离散化方法起始的分裂点列表是**空的**，通过离散化过程逐渐往列表中**加入分裂点**，而合并的离散化方法则是将所有的连续值**都看作**可能的分裂点，再逐渐**合并**相邻区域的值形成区间。

- **有监督的和无监督**
根据离散化方法是否使用数据集的类信息，离散化方法可以分为有监督的和无监督的。

- **动态和静态**
**动态**的离散化方法就是在建立分类模型的同时对连续特征进行离散化，如有名的 C4.5。**静态**的离散化方法就是在进行分类之前完成离散化处理。

- **全局的和局部的**
根据离散化过程是否是针对整个训练数据空间的，离散化方法又可分为全局的和局部的。

![][1]

----------

### 实际应用

实际用比较常用的无监督方法主要包括等宽，等频，Kmeans等。

![][2]

----------

有监督方法包括两大流派，基于Chi2和基于信息熵。

python中没有找到相关的包，R语言有专用的包：[discretization](https://cran.r-project.org/web/packages/discretization/discretization.pdf)，[smbinning](https://cran.r-project.org/web/packages/smbinning/smbinning.pdf)。例子很详细，不赘述。

----------

### 缺失值处理

#### 常规方法

- 方法一（na.roughfix）简单粗暴，对于训练集,同一个class下的数据，如果是分类变量缺失，用众数补上，如果是连续型变量缺失，用中位数补。
- 方法二（rfImpute）这个方法计算量大，先用na.roughfix补上缺失值，然后构建随机森林并计算proximity matrix，再回头看缺失值，如果是分类变量，则用没有缺失的观测实例的proximity中的权重进行投票。如果是连续型变量，则用proximity矩阵进行加权平均的方法补缺失值。然后迭代4-6次，这个补缺失值的思想和KNN有些类似。

![][5]

> 实际应用中还有一些小trick，比如会把缺失值标记为missing处理（缺失值比例比较大时），这在常规的数据挖掘比赛中经常用。

----------

#### Autoencoder

自动编码是目前深度学习在无监督问题上应用非常广泛的一种方法，可以得到数据的本征表示，可以用来做降维，feature learning，数据恢复等。

![][3]

如下图是Denoise Autoencoder基本原理图，类似于dropout，该算法会随机屏蔽掉一部分输入，增强模型的泛化能力，我们也可据此来做数据恢复。

![][4]

具体实验可以参看教程：[http://deeplearning.net/tutorial/dA.html](http://deeplearning.net/tutorial/dA.html)

[1]:http://7xjbdi.com1.z0.glb.clouddn.com/Feature%20Discretization1.png
[2]:http://7xjbdi.com1.z0.glb.clouddn.com/binning.png?imageView2/2/w/500
[3]:http://7xjbdi.com1.z0.glb.clouddn.com/sda.png
[4]:http://7xjbdi.com1.z0.glb.clouddn.com/denoisingautoencoder.png
[5]:http://7xjbdi.com1.z0.glb.clouddn.com/proximity_matrix.png?imageView2/2/w/200

----------

[^1]:[机器学习模型为什么要将特征离散化](http://weibo.com/p/1001603740289536770057)



http://nerds.airbnb.com/overcoming-missing-values-in-a-rfc/

http://www.analyticsvidhya.com/blog/2016/03/tutorial-powerful-packages-imputing-missing-values/

https://github.com/hammerlab/fancyimpute（重点）

Autoencoder

http://stackoverflow.com/questions/32407621/impute-multiple-missing-values-in-a-feature-vector
https://www.reddit.com/r/MachineLearning/comments/3yeepn/does_autoencoder_have_advantages_in_data/
http://www.rubylab.io/2015/04/28/denoising-autoencoder-tutorial/
