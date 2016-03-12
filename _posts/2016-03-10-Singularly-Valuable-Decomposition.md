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

我们来从数学的角度分析下矩阵与向量相乘到底发生了啥？[^1]

$$A\mathbf{x} = Q\Lambda Q^T\mathbf{x} = Q\Lambda (Q^T\mathbf{x})$$

待整理。。。

#### 奇异值分解
上面的特征值分解的A矩阵是对称阵，根据EVD可以找到一个（超）矩形使得变换后还是（超）矩形，也即A可以将一组正交基映射到另一组正交基！那么现在来分析：对任意$m \times n$的矩阵，**能否找到一组正交基使得经过它变换后还是正交基**？答案是肯定的，它就是SVD分解的精髓所在。

我们从特征值分解出发，导出奇异值分解。

首先我们注意到 $A^TA$ 为 $n$ 阶对阵矩阵，我们可以对它做特征值分解。

$$A^TA = VDV^T$$

这个时候我们得到一组正交基，$\\{v_1,v_2,\cdots,v_n\\}$，：

$$\begin{align*}
(Av_i,Av_j) & = (Av_i)^T(Av_j)\\
& = v_i^TA^TAv_j\\
& = v_i^T(\lambda_jv_j)\\
& = \lambda_jv_i^Tv_j\\
& = 0
\end{align*}$$

由 $r(A^TA)=r(A)=r$，这个时候我们得到了另一组正交基，$\\{Av_1,Av_2,\cdots,Av_r\\}$。先将其标准化，令：

$$\begin{align*}
u_i & = \frac{Av_i}{\lvert Av_i \rvert} = \frac{1}{\sqrt{\lambda_i}}Av_i\\
\Rightarrow Av_i & =  \sqrt{\lambda_i}u_i = \delta_i u_i \tag{2.1}
\end{align*}$$

![此处输入图片的描述][2]

注：

$$\begin{align*}
\lvert Av_i \rvert^2 = (Av_i,Av_i) & = \lambda_iv_i^Tv_i = \lambda_i\\
\Rightarrow \lvert Av_i \rvert & = \sqrt{\lambda_i} = \delta_i \ \ \text{(奇异值)}
\end{align*}$$


将向量组 $\\{u_1,u_2,\cdots,u_r\\}$ 扩充为 $R^m$中的标准正交基 $\\{u_1,u_2,\cdots,u_r,\cdots,u_m\\}$。

则：

$$\begin{align*}
AV & = A(v_1 v_2 \cdots v_n) = (Av_1\ Av_2\ \cdots\ Av_r\ 0 \cdots\ 0)\\
& = (\delta_1 u_1\ \delta_2 u_2\ \cdots \delta_r u_r\ 0 \cdots\ 0)\\
& =  U\Sigma\\
\Rightarrow A &= U\Sigma V^T \tag{2.2}
\end{align*}$$

这就表明任意的矩阵 A 是可以分解成三个矩阵。V 表示了原始域的标准正交基，U 表示经过 A 变换后的co-domain的标准正交基，Σ 表示了 V 中的向量与 U 中相对应向量之间的关系[^2]。

> This shows how to decompose the matrix A into the product of three matrices: V describes an orthonormal basis in the domain, and U describes an orthonormal basis in the co-domain, and Σ describes how much the vectors in V are stretched to give the vectors in U. 

#### 举个栗子

| Hole | Par | Phil | Tiger | Vijay |
|:----:|:---:|:----:|:-----:|:-----:|
|   1  |  4  |   4  |   4   |   4   |
|   2  |  5  |   5  |   5   |   5   |
|   3  |  3  |   3  |   3   |   3   |
|   4  |  4  |   4  |   4   |   4   |
|   5  |  4  |   4  |   4   |   4   |
|   6  |  4  |   4  |   4   |   4   |
|   7  |  4  |   4  |   4   |   4   |
|   8  |  3  |   3  |   3   |   3   |
|   9  |  5  |   5  |   5   |   5   |


#### 后记
Markdown排版表格是件麻烦事，google了一下，发现个在线网站，可以很方便生成 $\LaTeX$ 和 Markdown 表格，安利下这个 **[神器][3]** ~

----------


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/500px-Eigenvalue_equation.svg.png?imageView2/2/w/350
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/svd_vc.png?imageView2/2/w/500
  [3]: http://www.tablesgenerator.com/markdown_tables
  
  [^1]: [奇异值分解(SVD)原理详解及推导](http://blog.csdn.net/zhongkejingwang/article/details/43053513)
  [^2]: [奇异值分解(SVD) - 几何意义](http://blog.sciencenet.cn/blog-696950-699432.html)
  [^3]: [A Singularly Valuable Decomposition: The SVD of a Matrix](http://www.math.umn.edu/~lerman/math5467/svd.pdf)