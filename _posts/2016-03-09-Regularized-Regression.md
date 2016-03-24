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

----------

### 贝叶斯角度谈正则化
解决 overfitting 最常用的办法就是 regularization，我们常用的有：$L_1$正则，$L_2$正则等。

通常我们知道$L_1$正则会使得参数稀疏化，$L_2$正则可以起到平滑的作用，关于这方面的讲解已经很多[^3]。今天我们主要从贝叶斯理论的角度来审视下正则化。

![此处输入图片的描述][3]

> 我们先抛给大家一个结论：从贝叶斯的角度来看，正则化等价于对模型参数引入 **先验分布** 。

----------

#### Linear Regression
我们先看下最原始的Linear Regression[^5]:

![此处输入图片的描述][4]

$$\begin{align*}
 & p(\epsilon^{(i)})  = \frac{1}{\sqrt{2\pi}\delta}exp\left(  -\frac{(\epsilon^{(i)})^2}{2\delta^2} \right)\\
 \Rightarrow & p(y^{(i)}|x^{(i)};\theta) = \frac{1}{\sqrt{2\pi}\delta}exp\left( -\frac{(y^{(i)} - w^Tx^{(i)})^2}{2\delta^2}  \right)
\end{align*}$$

由最大似然估计(MLE):

$$\begin{align*}
L(w) & = p(\vec{y}|X;w)\\
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

$$w_{MLE} = \arg \underset{w}{\min} \sum_{i=1}^{m} (y^{(i)} - w^Tx^{(i)})^2 \tag{1}$$

这就导出了我们原始的least-squares损失函数，但这是在我们对参数 $w$ 没有加入任何先验分布的情况下。在数据维度很高的情况下，我们的模型参数很多，模型复杂度高，容易发生过拟合。

比如我们常说的 “small n, large p problem”。（我们一般用 $n$ 表示数据点的个数，用 $p$ 表示变量的个数 ，即数据维度。当 $p\gg n$ 的时候，不做任何其他假设或者限制的话，学习问题基本上是没法进行的。因为如果用上所有变量的话， $p$ 越大，通常会导致模型越复杂，但是反过来 $n$ 又很小，于是就会出现很严重的 overfitting 问题[^7]。

![此处输入图片的描述][5]

这个时候，我们可以对参数 $w$ 引入先验分布，降低模型复杂度。

----------

####  Ridge Regression
我们对参数 $w$ 引入协方差为 $\alpha$ 的零均值**高斯先验**。

![此处输入图片的描述][6]

由于引入了先验分布，我们用最大后验估计(MAP)[^6]：

$$\begin{align*}
L(w) & = p(\vec{y}|X;w)p(w)\\
& = \prod_{i=1}^{m} p(y^{(i)}|x^{(i)};\theta)p(w)\\
& = \prod_{i=1}^{m} \frac{1}{\sqrt{2\pi}\delta}exp\left( -\frac{(y^{(i)} - w^Tx^{(i)})^2}{2\delta^2}  \right)\prod_{j=1}^{n}\frac{1}{\sqrt{2\pi\alpha}}exp\left( -\frac{(w^{(j)})^2}{2\alpha}  \right)\\
& = \prod_{i=1}^{m} \frac{1}{\sqrt{2\pi}\delta}exp\left( -\frac{(y^{(i)} - w^Tx^{(i)})^2}{2\delta^2}  \right)\frac{1}{\sqrt{2\pi\alpha}}exp\left( -\frac{w^Tw}{2\alpha}  \right)
\end{align*}$$

取对数：

$$\begin{align*}
l(w) & = \log L(w)\\
& = m \log \frac{1}{\sqrt{2\pi}\delta}+ n \log \frac{1}{\sqrt{2\pi\alpha}} - \frac{1}{\delta^2}\cdot \frac{1}{2} \sum_{i=1}^{m} (y^{(i)} - w^Tx^{(i)})^2 - \frac{1}{\alpha}\cdot \frac{1}{2} w^Tw\\
 \Rightarrow & w_{MAP_{Guassian}} = \arg \underset{w}{\min} \left( \frac{1}{\delta^2}\cdot \frac{1}{2} \sum_{i=1}^{m} (y^{(i)} - w^Tx^{(i)})^2 + \frac{1}{\alpha}\cdot \frac{1}{2} w^Tw \right) \tag{2}
\end{align*}$$

等价于：

$$J_R(w) = \frac{1}{n}\lVert y- w^TX \rVert_2 + \lambda \lVert w \rVert_2$$

这不就是 **Ridge Regression** 吗？

![此处输入图片的描述][7]

看我们得到的参数，在零附近是不是很密集，老实说 ridge regression 并不具有产生稀疏解的能力，也就是说参数并不会真出现很多零。假设我们的预测结果与两个特征相关，$L_2$正则倾向于综合两者的影响，给影响大的特征赋予高的权重；而$L_1$正则倾向于选择影响较大的参数，而舍弃掉影响较小的那个。实际应用中 $L_2$ 正则表现往往会优于 $L_1$正则，但 $L_1$ 正则会大大降低我们的计算量。

> Typically ridge or ℓ2 penalties are **much better** for minimizing prediction error rather than ℓ1 penalties. The reason for this is that when two predictors are highly correlated, ℓ1 regularizer will simply pick one of the two predictors. In contrast, the ℓ2 regularizer will keep both of them and jointly shrink the corresponding coefficients a little bit. Thus, while the ℓ1 penalty can certainly reduce overfitting, you may also experience a loss in predictive power. [^3]

**那现在我们知道了，对参数引入 高斯先验 等价于 $L_2$ 正则化。**

----------

#### LASSO
上面我们对 $w$ 引入了高斯分布，那么拉普拉斯分布(Laplace distribution)呢？

注：LASSO - least absolute shrinkage and selection operator.

![此处输入图片的描述][8]

我们看下拉普拉斯分布长啥样：

$$f(x\mid\mu,b) = \frac{1}{2b} \exp \left( -\frac{|x-\mu|}{b} \right) \,\!$$

![此处输入图片的描述][9]

关于拉普拉斯和正态分布的渊源，大家可以参见 "[**正态分布的前世今生**][10]".

重复之前的推导过程我们很容易得到：

$$w_{MAP_{Laplace}} = \arg \underset{w}{\min} \left( \frac{1}{\delta^2}\cdot \frac{1}{2} \sum_{i=1}^{m} (y^{(i)} - w^Tx^{(i)})^2 + \frac{1}{b^2}\cdot \frac{1}{2} \lVert w \rVert_1 \right) \tag{3}$$

该问题通常被称为 LASSO (least absolute shrinkage and selection operator) 。LASSO 仍然是一个 convex optimization 问题，不具有解析解。它的优良性质是能产生稀疏性，导致 $w$ 中许多项变成零。

> 再次总结下，对参数引入 **拉普拉斯先验** 等价于 $L_1$ 正则化。

----------

#### Elastic Net
可能有同学会想，既然 $L_1$ 和 $L_2$正则各自都有自己的优势，那我们能不能将他们 combine 起来？

可以，事实上，大牛早就这么玩过了，参见大牛 [**PPT**][11]。

![此处输入图片的描述][12]

因为lasso在解决之前提到的“small n, large p problem”存在一定缺陷。

![此处输入图片的描述][13]

这个我们就直接给结果了，不推导了哈。（好麻烦的样子。。。逃）

$$\hat{\beta} = \arg \underset{\beta}{\min} \lVert y - X\beta \rVert_2 + \lambda_2 \lVert \beta \rVert_2 + \lambda_1 \lVert \beta \rVert_1 \tag{4}$$

![此处输入图片的描述][14]

![此处输入图片的描述][15]

----------

#### 总结
> 正则化参数等价于对参数引入 **先验分布**，使得 **模型复杂度** 变小（缩小解空间），对于噪声以及outliers的鲁棒性增强（泛化能力）。整个最优化问题从贝叶斯观点来看是一种贝叶斯最大后验估计，其中 **正则化项** 对应后验估计中的 **先验信息** ，损失函数对应后验估计中的似然函数，两者的乘积即对应贝叶斯最大后验估计的形式。

ps: 本文写作过程中参考了知乎和网上的很多文章,同时也加入了自己的一些理解，热烈欢迎广大机器学习爱好者一起讨论问题，互通有无！

----------


资料传送门：

 - [Regularized Regression](http://www.unicog.org/pmwiki/uploads/Main/PresentationMM_02_10.pdf)
 - [Bayesian Linear Regression](http://web.cse.ohio-state.edu/~kulis/teaching/788_sp12/scribe_notes/lecture5.pdf)
 - [《A Few useful things to Know About machine Learning》读后感](http://blog.csdn.net/danameng/article/details/21563093)

  [^1]: [Bias 和 Variance](http://charlesx.top/2016/03/Bias-Variance/)
  [^2]: [《A Few useful things to Know About machine Learning》读后感](http://blog.csdn.net/danameng/article/details/21563093)
  [^3]: [What is the difference between L1 and L2 regularization?](https://www.quora.com/What-is-the-difference-between-L1-and-L2-regularization)
  [^4]: [ Bayesian Linear Regression](http://web.cse.ohio-state.edu/~kulis/teaching/788_sp12/scribe_notes/lecture5.pdf)
  [^5]: [Bayesian statistics and regularization](http://cs229.stanford.edu/notes/cs229-notes5.pdf)
  [^6]: [最大似然估计和最小二乘法怎么理解？](https://www.zhihu.com/question/20447622)
  [^7]: [Sparsity and Some Basics of L1 Regularization](http://freemind.pluskid.org/machine-learning/sparsity-and-some-basics-of-l1-regularization/)

----------


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_170512.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_171146.png?imageView2/2/w/350
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/117ec65eb609d8ea9f05c227130724a6_b.png?imageView2/2/w/350
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_180932.png
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_204329.png
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/ridge_re.png
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_195835.png?imageView2/2/w/300
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_201452.png
  [9]: http://7xjbdi.com1.z0.glb.clouddn.com/Laplace_pdf_mod.png?imageView2/2/w/350
  [10]: http://www.med.mcgill.ca/epidemiology/hanley/bios601/Mean-Quantile/intro-normal-distribution-2.pdf
  [11]: http://web.stanford.edu/~hastie/TALKS/enet_talk.pdf
  [12]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_203846.png
  [13]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_205117.png
  [14]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_205924.png
  [15]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_205943.png?imageView2/2/w/350