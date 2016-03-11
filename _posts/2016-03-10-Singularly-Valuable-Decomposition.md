---
published: true
author: Charles
layout: post
title:  "漫谈奇异值分解"
date:   2016-03-10 14:28
categories: 机器学习 推荐系统 数学基础
---

在进一步学习推荐系统之前，先把奇异值分解这块的理论理清。

#### 特征值分解
首先简单介绍下特征值。

我们知道，方阵和向量相乘，从几何意义上来讲，就是对向量作 **旋转、伸缩** 变换。如果方阵对某个向量只产生伸缩，而不产生旋转效果，那么这个向量就称为矩阵的**特征向量**，伸缩的比例就是对应的**特征值**。

$$A\mathbf{x} = \lambda \mathbf{x} \tag{1.1}$$

![此处输入图片的描述][1]

学线性代数的时候，我们应该都学过这样一个定理：

> 若 $A$ 为 $n$ 阶实对称阵（方阵），则存在由特征值组成的对角阵 $\Lambda$ 和特征向量组成的正交阵 $Q$，使得：    
$$A = Q\Lambda Q^T \tag{1.2}$$

这就是我们所说的 **特征值分解**（Eigenvalue decomposition: EVD）($R^n \rightarrow R^n$)，而奇异值分解其实可以看做是特征值分解在任意矩阵($m \times n$)上的推广形式 ($R^n \rightarrow R^m$)。（只有对方阵才有特征值的概念，所以对于任意的矩阵，我们引入了奇异值）

我们来从数学的角度分析下矩阵与向量相乘到底发生了啥？

$$A\mathbf{x} = Q\Lambda Q^T\mathbf{x} = Q\Lambda (Q^T\mathbf{x})$$

待整理。。。

#### 奇异值分解
上面的特征值分解的$A$矩阵是对称阵，根据$EVD$可以找到一个（超）矩形使得变换后还是（超）矩形，也即$A$可以将一组正交基映射到另一组正交基！那么现在来分析：对任意$m \times n$的矩阵，能否找到一组正交基使得经过它变换后还是正交基？答案是肯定的，它就是SVD分解的精髓所在。

我们从特征值分解出发，导出奇异值分解。

首先我们注意到 $A^TA$为 $n$ 阶对阵矩阵，我们可以对它做特征值分解。


http://blog.csdn.net/zhongkejingwang/article/details/43053513
http://blog.csdn.net/zhongkejingwang/article/details/43083603
http://www.ams.org/samplings/feature-column/fcarc-svd
http://blog.sciencenet.cn/blog-696950-699432.html

http://www.niubua.com/?p=1080
https://www.zhihu.com/question/22237507
http://chenbiaolong.com/2015/07/01/svd/


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/500px-Eigenvalue_equation.svg.png?imageView2/2/w/350