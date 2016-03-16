---
published: true
author: Charles
layout: post
title:  "Bias 和 Variance"
date:   2016-03-9 12:20
categories: 机器学习
---

#### 通俗版[^1]
我们机器学习的模型，必不可少地对数据非常依赖。然而，如果你不知道数据服从一个什么样的分布，或者你没有办法拿到所有可能的数据（肯定拿不到所有的），那么我们训练出来的模型和真实模型之间，就会存在不一致。这种不一致表现在两个方面。

用打靶的例子来说明。**偏差好比你的瞄准能力；方差好比你使用的枪的性能**。

瞄准的时候，本来你准备瞄准的是十环，而实际上你瞄准的是九环，那么你的偏差就是一环。

而枪的性能也很重要。好的枪精度高，只要你瞄的准，他都能打到瞄准点附近非常小的范围之内；而差的枪，比如你用弹弓，就算每次都瞄的准，但是它打到瞄准点附近的范围变化就比较大。 

![此处输入图片的描述][1]

----------

#### 学术版
我们给出一个学术点的概念：

> 偏差：描述的是预测值（估计值）的期望与真实值之间的差距。偏差越大，越偏离真实数据。        
方差：描述的是预测值的变化范围，离散程度，也就是离期望值距离的分散程度。方差越大，数据的分布就越分散。

**注意**，这里我们提到的是预测值的 **期望**、预测值的 **变化范围**，这就表明我们并不是只有一个模型，而是在同一现象所产生的不同训练数据上训练出的 **一系列模型**，而偏差和方差评价的是这一系列模型的好坏程度。（如果理解成一个模型，你可能会觉得上述的表达非常奇怪）

引用“**[机器学习那些事][2]**”里面的一段表述：偏置度量了学习器倾向于一直学习相同错误的程度。方差则度量了学习器倾向于忽略真实信号、学习随机事物的程度。

----------

#### 数学表示[^2]

假设$Y = f(X) + \epsilon$，其中误差$\epsilon$服从高斯分布：$\epsilon \sim \mathcal{N}(0,\sigma_\epsilon)$。我们的模型对$f(x)$的估计值为$\hat{f}(x)$，则：

$$\begin{align*}
Err(x) &= E\left[(Y-\hat{f}(x))^2\right]  \\
& = E\left[(Y- f(x) + f(x)-\hat{f}(x))^2\right]\\
& = \left(E[\hat{f}(x)]-f(x)\right)^2 + E\left[\left(\hat{f}(x)-E[\hat{f}(x)]\right)^2\right] +\sigma_e^2\\
& = \mathrm{Bias}^2 + \mathrm{Variance} + \mathrm{Irreducible\ Error}
\end{align*}$$

----------

#### Bias/Variance Tradeoff[^3]
在一个实际系统中，Bias与Variance往往是不能兼得的。如果要降低模型的Bias，就一定程度上会提高模型的Variance，反之亦然。

造成这种现象的根本原因是，我们总是希望用有限训练样本去**估计**无限的真实数据。当我们更加相信这些数据的真实性，而忽视对模型的先验知识，就会尽量保证模型在训练样本上的准确度，这样可以减少模型的Bias。但是，这样学习到的模型，很可能会失去一定的泛化能力，从而造成过拟合（当我们的样本不具备对整体数据的代表性）。

相反，如果更加相信我们对于模型的先验知识，在学习模型的过程中对模型增加更多的限制，就可以降低模型的variance，提高模型的稳定性，但也会使模型的Bias增大。Bias与Variance两者之间的trade-off是机器学习的基本主题之一。

![此处输入图片的描述][3]


----------


  
  [^1]: [从集成学习到模型的偏差和方差的理解，集成模型偏差理解](http://www.bkjia.com/yjs/1044343.html)
  [^2]: [Understanding the Bias-Variance Tradeoff](http://scott.fortmann-roe.com/docs/BiasVariance.html)
  [^3]: [机器学习中的Bias(偏差)，Error(误差)，和Variance(方差)有什么区别和联系？](https://www.zhihu.com/question/27068705)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_130216.png?imageView2/2/w/450
  [2]: http://www.valleytalk.org/wp-content/uploads/2012/11/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E9%82%A3%E4%BA%9B%E4%BA%8B.pdf
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/biasvariance.png