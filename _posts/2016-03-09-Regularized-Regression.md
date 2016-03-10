---
published: true
author: Charles
layout: post
title:  "Regularized Regression: A Bayesian point of view"
date:   2016-03-09 15:21
categories: 机器学习
---

#### 过拟合
谈正则化之前，我们先来看一看过拟合问题。

![此处输入图片的描述][1]

以一维的回归分析为例，如上图，如果用高阶多项式去拟合数据的话，可以使得训练误差$E_{in}$很小，但是在测试集上的误差就可能很大。

造成这种现象的原因就是因为我们使用的模型过于复杂，根据VC维理论：**VC维**很高的时候，就容易发生$E_{in}$（Bias）很低，但$E_{out}$(Variance)[^1]很高的情形.

![此处输入图片的描述][2]

### 贝叶斯角度谈正则化
解决 overfitting 最常用的办法就是 regularization，我们常用的有：$L_1$正则，$L_2$正则等。

通常我们知道$L_1$正则会使得参数稀疏化，$L_2$正则可以起到平滑的作用，关于这方面的讲解已经很多[^3]。今天我们从贝叶斯理论的角度来审视下正则化。

![此处输入图片的描述][3]

> 我们先抛给大家一个结论：从贝叶斯的角度来看，正则化等价于对模型参数引入**先验约束**。

#### Linear Regression
我们先看下最原始的Linear Regression:

![此处输入图片的描述][4]

$$\begin{align*}
 & p(\epsilon^{(i)})  = \frac{1}{\sqrt{2\pi}\delta}exp\left(  -\frac{(\epsilon^{(i)})^2}{2\delta^2} \right)\\
 \Rightarrow & p(y^{(i)}|x^{(i)};\theta) = \frac{1}{\sqrt{2\pi}\delta}exp\left( -\frac{(y^{(i)} - w^Tx^{(i)})^2}{2\delta^2}  \right)
\end{align*}$$

由最大似然估计(MLE):

$$\begin{align*}
L(w) & = p(\vec(y)|X;w)\\
& = \prod_{i=1}^{m} p(y^{(i)}|x^{(i)};\theta)\\
& = \prod_{i=1}^{m} \frac{1}{\sqrt{2\pi}\delta}exp\left( -\frac{(y^{(i)} - w^Tx^{(i)})^2}{2\delta^2}  \right)
\end{align*}$$

取对数：

$$\begin{align*}
l(w) & = \log L(w)\\
& =\log \prod_{i=1}^{m} \frac{1}{\sqrt{2\pi}\delta}exp\left( -\frac{(y^{(i)} - w^Tx^{(i)})}{2\delta^2}  \right)\\
& = \sum_{i=1}^{m} \log \frac{1}{\sqrt{2\pi}\delta}exp\left( -\frac{(y^{(i)} - w^Tx^{(i)})^2}{2\delta^2}  \right)\\
& = m \log \frac{1}{\sqrt{2\pi}\delta} - \frac{1}{\delta^2}\cdot \frac{1}{2} \sum_{i=1}^{m} (y^{(i)} - w^Tx^{(i)})^2
\end{align*}$$

即：

$$w_{MLE} = \arg \underset{w}{\max} \sum_{i=1}^{m} (y^{(i)} - w^Tx^{(i)})^2$$
http://www.unicog.org/pmwiki/uploads/Main/PresentationMM_02_10.pdf
Bayesian linear Regression
http://freemind.pluskid.org/machine-learning/sparsity-and-some-basics-of-l1-regularization/


  [^1]: [Bias 和 Variance](http://charlesx.top/2016/03/Bias-Variance/)
  [^2]: [《A Few useful things to Know About machine Learning》读后感](http://blog.csdn.net/danameng/article/details/21563093)
  [^3]: [What is the difference between L1 and L2 regularization?](https://www.quora.com/What-is-the-difference-between-L1-and-L2-regularization)

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_170512.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_171146.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/117ec65eb609d8ea9f05c227130724a6_b.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_180932.png