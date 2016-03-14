---
published: true
author: Charles
layout: post
title:  "Factorization Machines"
date:   2016-03-13 9:10
categories: 机器学习 推荐系统
---

#### 基本模型
**Factorization Machines**, 简单点理解可以看做是在线性回归的基础上，考虑上特征之间的相互联系 (interaction)[^1]。

$$\hat{y}(x) = w_0 + \sum_{i=1}^{n} w_ix_i + \sum_{i=1}^{n-1}\sum_{j=i+1}^{n}w_{ij}x_ix_j  \tag{1}$$

![此处输入图片的描述][1]

和我们在SVD++里面提到的trick一样，$w_{ij}$使得我们的**参数过多**，在数据稀疏的情况下很容易发生过拟合。我们利用矩阵分解来替换它：

$$\hat{w}_{ij} = v_i^Tv_j:=\sum_{l=1}^{k}v_{il}v_{jl}$$

现在，我们的表达式变为：

$$\hat{y}(x) = w_0 + \sum_{i=1}^{n} w_ix_i + \sum_{i=1}^{n-1}\sum_{j=i+1}^{n}(v_i^Tv_j)x_ix_j  \tag{2}$$

#### 复杂度
模型中需要估计的参数包括：

$$w_o\in \mathbb{R}, w\in \mathbb{R}^n, V \in \mathbb{R}^{n \times k}$$

此时整体的复杂度为 $\mathcal{O}(kn^2)$，作者证明实际上只需要线性的复杂度$\mathcal{O}(kn)$[^2]，

$$\begin{align*}
&\sum_{i=1}^n\sum_{j=i+1}^n (v_i^Tv_j)x_ix_j\\
= & \frac {1}{2}\left( \sum_{i=1}^n\sum_{j=1}^n (v_i^Tv_j)x_ix_j - \sum_{i=1}^n (v_i^Tv_i)x_ix_i\right)\\
= & \frac {1}{2}\left( \sum_{i=1}^n\sum_{j=1}^n\sum_{l=1}^{k}v_{il}v_{jl}x_ix_j - \sum_{i=1}^{n}\sum_{l=1}^{k}v_{il}^2x_i^2 \right)\\
= & \frac {1}{2} \sum_{l=1}^{k}\left( \sum_{i=1}^n(v_{il}x_i)\sum_{j=1}^n(v_{jl}x_j) - \sum_{i=1}^n v_{il}^2x_i^2  \right)\\
= & \frac {1}{2} \sum_{l=1}^{k} \left[ \left(  \sum_{i=1}^{n}(v_{il}x_i) \right)^2 - \sum_{i=1}^{n}v_{il}^2x_{i}^2 \right]
\end{align*}$$


----------



  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-14_135341.png
  
  [^1]: [Factorization Machines](http://www.csie.ntu.edu.tw/~b97053/paper/Rendle2010FM.pdf)
  [^2]: [Factorization Machines 学习笔记](http://blog.csdn.net/itplus/article/details/40534885)