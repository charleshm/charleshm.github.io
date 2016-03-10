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


http://www.unicog.org/pmwiki/uploads/Main/PresentationMM_02_10.pdf
Bayesian linear Regression
http://freemind.pluskid.org/machine-learning/sparsity-and-some-basics-of-l1-regularization/


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_170512.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_171146.png
  
  [^1]: http://charlesx.top/2016/03/Bias-Variance/
  [^2]: [《A Few useful things to Know About machine Learning》读后感](http://blog.csdn.net/danameng/article/details/21563093)