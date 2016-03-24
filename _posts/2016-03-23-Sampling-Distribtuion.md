---
published: true
author: Charles
layout: post
title:  "抽样分布"
date:   2016-03-23 9:30
categories: 机器学习 统计学
---

> 由于统计量依赖于样本,而样本又是随机变量,故统计量也是随机变量,因而就有一定的分布，我们称之为“抽样分布”。也就是说，**抽样分布实际上就是统计量的分布**.
 
 $$\text{抽样分布} \Rightarrow \text{统计推断}(\text{假设检验})$$
 
#### 统计量

 设 $(X_1,X_2,\cdots,X_n)$  是来自总体 $X$ 的一个样本， $g(X_1,X_2,\cdots,X_n)$ 是样本的函数，若 $g$ 中不含任何未知参数，则称 $g(X_1,X_2,\cdots,X_n)$ 是一个统计量。
 
常用的统计量包括: **样本均值，样本方差，样本矩**。

统计量是我们对总体的分布函数或数字特征进行**统计推断**的最重要的基本概念，所以寻求统计量的分布成为数理统计的基本问题之一。我们把统计量的分布称为抽样分布。然而要求出一个统计量的精确分布是十分困难的。而在实际问题中，大多总体都服从正态分布：而对于正态分布，我们可以求出一些重要统计量的精确分布。

![此处输入图片的描述][1]
 
> 下面我们介绍下，三大抽样分布： $\chi^2$ 分布，t 分布，F 分布

#### 卡方($\chi^2$)分布
 
![此处输入图片的描述][2]

$\chi^2$ 分布对应的概率密度函数，

$$f_k(x)=
\frac{(1/2)^{k/2}}{\Gamma(k/2)} x^{k/2 - 1} e^{-x/2}$$

![此处输入图片的描述][3]

> 卡方检验一般用于检验样本是否符合预期的一个分布。具体点就是统计样本的实际观测值与理论推断值之间的偏离程度，卡方值越大，偏差越大；若两个值完全相等时，卡方值就为0，表明与理论值完全符合[^1]。


----------


#### t 分布
先介绍下 t 分布的来由。

我们知道正态分布有两个参数：$\mu$ 和 $\sigma$，决定了正态分布的位置和形态。为了应用方便，常将一般的正态变量 X 通过 u 变换转化成标准正态变量u，使得原来各种形态的正态分布都转换为 $\mu=0，\sigma=1$ 的标准正态分布（standard normal distribution）,亦称 $u$ 分布。

根据中心极限定理，若从正态总体 $N(\mu,\sigma^2)$ 中，反复多次随机抽取样本含量固定为n  的样本，样本均数 $\overline{X}$ 仍服从正态分布，即 $N（\mu,\frac{\sigma^2}{n}）$ 。所以，对**样本均数的分布**进行 $u$ 变换，也可变换为标准正态分布 $N (0,1)$.

> 在实际工作中，往往 $\sigma$ 是未知的，常将样本的方差 $S_n$ 作为 $\sigma$的估计值，为了与 $u$ 变换区别，称为 $t$ 变换，统计量 $t$ 值的分布称为 $t$ 分布。

![此处输入图片的描述][4]

假设 $X$ 是呈正态分布的独立的随机变量（随机变量的期望值是 $\mu$，方差是 $\sigma^{2}$）。

$$\begin{align*}
\overline{X}_n & =(X_1+\cdots+X_n)/n & \text{样本均值}\\
{S_n}^2 & =\frac{1}{n-1}\sum_{i=1}^n\left(X_i-\overline{X}_n\right)^2 & \text{样本方差}\\
Z & =\frac{\overline{X}_n-\mu}{\sigma/\sqrt{n}} & \text{u 变换}\\
T & =\frac{\overline{X}_n-\mu}{S_n/\sqrt{n}} & \text{t 变换}\tag{2}
\end{align*}$$

> 上面 T 的分布称为 **t-分布**.

T 的概率密度函数,

$$f(t) = \frac{\Gamma((\nu+1)/2)}{\sqrt{\nu\pi\,}\,\Gamma(\nu/2)} (1+t^2/\nu)^{-(\nu+1)/2}$$

 $\nu$  等于 $n − 1$，称为自由度。

![此处输入图片的描述][5]

----------

#### F 分布
> 设 $X \sim  \chi^2(d_1),Y \sim \chi^2(d_2)$，且 $X$ 与 $Y$ 相互独立，则称随机变量 $F=\frac{X/d_1}{Y/d_2}$ 的分布为服从第一自由度 $d_1$（分子 $X$ 的自由度）和第二自由度 $d_1$（分母 $Y$ 的自由度）的 F 分布，记做: $F \sim F(d_1,d_2)$.

F 分布的概率密度函数，

$$\begin{align*}
f(x; d_1,d_2) &= \frac{\sqrt{\frac{(d_1\,x)^{d_1}\,\,d_2^{d_2}} {(d_1\,x+d_2)^{d_1+d_2}}}} {x\,\mathrm{B}\!\left(\frac{d_1}{2},\frac{d_2}{2}\right)} \\
&=\frac{1}{\mathrm{B}\!\left(\frac{d_1}{2},\frac{d_2}{2}\right)} \left(\frac{d_1}{d_2}\right)^{\frac{d_1}{2}} x^{\frac{d_1}{2} - 1} \left(1+\frac{d_1}{d_2}\,x\right)^{-\frac{d_1+d_2}{2}}
\end{align*}$$

![此处输入图片的描述][6]

----------


#### 举个栗子
> 以往通过大规模调查已知某地新生儿出生体重为3.30kg.从该地难产儿中随机抽取35名新生儿作为研究样本,平均出生体重为3.42kg,标准差为0.40kg,问该地难产儿出生体重是否与一般新生儿体重不同?

本例已知总体均数0=3.30kg，但总体标准差未知,n=35为小样本,，S=0.40kg，故选用单样本t检验。


----------


  [^1]: [卡方检验](http://download.bioon.com.cn/upload/201311/02171856_1166.pdf)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-24_110445.png?imageView2/2/w/400
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-24_110212.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/325px-Chi-square_distributionPDF.png?imageView2/2/w/400
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/t_distribution.png
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/TStudent.png?imageView2/2/w/400
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/F_pdf.svg.png