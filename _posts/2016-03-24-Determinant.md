---
published: true
author: Charles
layout: post
title:  "漫谈行列式"
date:   2016-03-24 9:30
categories: 线性代数
---
因为不少公司的机器学习（数据挖掘）的笔试题中都涉及到行列式的求解，故稍微做了下总结。

#### 几何意义
一个解释是行列式就是矩阵的行（列）向量所构成的**超平行多面体**的有向面积或有向体积[^1]；

另一个解释是矩阵A的行列式就是**线性变换** A 下的图形面积或体积的**伸缩因子**。

这两个几何解释一个是**静态**的体积概念，一个是**动态**的变换比例概念。但具有相同的几何本质，因为矩阵 A 表示的（矩阵向量所构成的）几何图形相对于单位矩阵 E 的所表示的单位面积或体积而言，伸缩因子本身就是矩阵A表示的几何图形的面积或体积。

比如，一个 $3\times 3$ 阶的行列式，其行向量或列向量所张成的平行六面体。

![此处输入图片的描述][1]

----------

#### 常规求法


$$A = \begin{bmatrix} a_{11} & a_{12} & \dots & a_{1n} \\
a_{21} & a_{22} & \dots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{n1} & a_{n2} & \dots & a_{nn} \end{bmatrix}.$$

利用代数余子式：

$$\det (A)=\sum_{k=1}^{n} a_{k1}(-1)^{k+1}\det (A(k,1))$$

----------


#### 常用性质

 - 互换行列式的两行或两列，行列式变号。
 - $A$ 可逆 $\Rightarrow \det (A)\not= 0$
 - $Ax=0$ 只有零解 $\Rightarrow \det (A)\not= 0(\text{A 可逆})$


----------


#### 分块矩阵[^2]
我们知道二阶矩阵的行列式计算式:

$$ 
\left|\begin{array}{cccc}   
a & b \\   
c & d 
\end{array}\right| = ad -bc,  
$$

那么对于分块矩阵$ 
\left|\begin{array}{cccc}   
A & B \\\\   
C & D 
\end{array}\right|
$ 是否存在类似的计算式呢？

> 一般情况下，相应的分块矩阵的行列式公式并不存在，但如果 $A,B,C,D$ 满足某些特定的条件，则会存在**简明**的计算公式。


----------


**公式一**：若 A 和 D 是方阵 (但大小可以相异)，则

$$\begin{vmatrix}  A&B\\  0&D  \end{vmatrix}=(\det A)(\det D)$$

推论： 

$$\begin{vmatrix}  0&B_s\\  A_r&0  \end{vmatrix}=(-1)^{rs}(\det A)(\det B)$$

----------


**公式二**：设 A 和 D 是方阵。若 A 可逆，则

$$\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}=(\det A)(\det (D-CA^{-1}B))$$

若 D 可逆，则

$$\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}=(\det D)(\det (A-BD^{-1}C))$$


----------

**公式三**：设 A, B, C, D 是 $n\times n$ 阶矩阵。若 A, B, C, D 其中之一是零矩阵，则

$$\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}=\det (AD-BC)$$


----------

**公式四**：设 A, B, C, D 是 $n\times n$ 阶矩阵。若 AC=CA，则

$$\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}=\det (AD-CB)$$

若 CD=DC，则

$$\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}=\det (AD-BC)$$

若 BD=DB，则

$$\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}=\det (DA-BC)$$

若 AB=BA，则

$$\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}=\det (DA-CB)$$

注意，即使前提满足，$\det(AD-CB)$ 未必等于 $\det (AD-BC)$，同样的，$\det(AD-BC)$ 也未必等于 $\det (DA-BC)$。


----------

#### 举个栗子


求行列式，

$$\begin{vmatrix}  
0&a&b&0\\
a&0&0&b\\
0&c&d&0\\
c&0&0&d
\end{vmatrix}$$

首先我们将第一列与第二列交换，这样分块矩阵中的每个矩阵都是对角矩阵 :

$$\begin{vmatrix}  
0&a&b&0\\
a&0&0&b\\
0&c&d&0\\
c&0&0&d
\end{vmatrix} \overset{列变换}{\longrightarrow} - \begin{vmatrix}  
a&0&b&0\\
0&a&0&b\\
c&0&d&0\\
0&c&0&d
\end{vmatrix}=-\begin{vmatrix}  A&B\\  C&D  \end{vmatrix}$$

再利用**公式四**：

$$\begin{align*}
-\begin{vmatrix}  A&B\\  C&D  \end{vmatrix} & = -\det (AD-BC)\\
& = -\begin{vmatrix}  ad-bc&0\\  0&ad-bc  \end{vmatrix}\\
& = -(ad-bc)^2
\end{align*}$$

----------


  [^1]: [行列式的几何意义](http://www.cnblogs.com/AndyJee/p/3491487.html)
  [^2]: [分块矩阵的行列式](https://ccjou.wordpress.com/2013/06/07/%E5%88%86%E5%A1%8A%E7%9F%A9%E9%99%A3%E7%9A%84%E8%A1%8C%E5%88%97%E5%BC%8F/)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/3_det.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/tecent_test.png?imageView2/2/w/450