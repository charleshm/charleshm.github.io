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

#### 贝叶斯角度谈正则化
解决 overfitting 最常用的办法就是 regularization，我们常用的有：$L_1$正则，$L_2$正则等。
通常我们知道$L_1$正则会使得参数稀疏化，$L_2$正则可以起到平滑的作用，关于这方面的讲解已经很多。今天我们从贝叶斯理论的角度来审视下正则化。

我们先看下Linear Regression:

![此处输入图片的描述][3]



http://www.unicog.org/pmwiki/uploads/Main/PresentationMM_02_10.pdf
Bayesian linear Regression
http://freemind.pluskid.org/machine-learning/sparsity-and-some-basics-of-l1-regularization/


  [^1]: http://charlesx.top/2016/03/Bias-Variance/
  [^2]: [《A Few useful things to Know About machine Learning》读后感](http://blog.csdn.net/danameng/article/details/21563093)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_170512.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_171146.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_180451.png