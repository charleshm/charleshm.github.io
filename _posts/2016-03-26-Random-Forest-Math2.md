---
published: true
author: Charles
layout: post
title:  "随机森林中的数学理论(2)"
date:   2016-03-26 15:30
categories: 机器学习
---

Breiman[^1]证明了泛化误差的上界是可以得到的，它主要由两方面因素决定：单棵决策树的**分类强度**，决策树之间的**相关性**。

定义单棵决策树的**余量函数**：

$$mr(\mathbf{x},y) = P_{\Theta}(h(\mathbf{x},\Theta)=y) - \underset{j\not= Y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)$$

**分类强度**(余量函数在整个 data space 的期望)：

$$s = E_{\mathbf{x},y}[mr(\mathbf{x},y)]$$

假定 $s>0$（即随机森林对各个样本的分类结果是可信的），根据切比雪夫不等式：

$$\begin{align*}
P_{\mathbf{x},y}(mr(\mathbf{x},y)<0) & = P_{\mathbf{x},y}(mr(\mathbf{x},y) - E_{\mathbf{x},y}[mr(\mathbf{x},y)]<-E_{\mathbf{x},y}[mr(\mathbf{x},y)])\\
& \leq P_{\mathbf{x},y}(|mr(\mathbf{x},y)-E_{\mathbf{x},y}[mr(\mathbf{x},y)]|>E_{\mathbf{x},y}[mr(\mathbf{x},y)])\\
& \leq \frac{var(mr)}{s^2} \tag{1}
\end{align*}$$


----------


要求出这个上界，我们需要先分析下 $var(mr)$。为了解决这个问题，我们引入几个新的量，

$$\begin{align*}
\hat{j}(x,y) & = \arg \underset{j\not= y}{\max} P_{\Theta}(h(\mathbf{x},\Theta)=j)\\
mr(\mathbf{x},y) & = P_{\Theta}(h(\mathbf{x},\Theta)=y) - \underset{j\not= Y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)\\
& = P_{\Theta}(h(\mathbf{x},\Theta)=y) - P_{\Theta}(h(\mathbf{x},\Theta)=\hat{j}(x,y))\\
& = E_{\Theta}[I(h(\mathbf{x},\Theta)=y)-I(h(\mathbf{x},\Theta)=\hat{j}(x,y))]\\
rmg(\Theta,\mathbf{x},y) & = I(h(\mathbf{x},\Theta)=y)-I(h(\mathbf{x},\Theta)=\hat{j}(x,y)) \tag{2}
\end{align*}$$

也就是说，

$$mr(\mathbf{x},y) = E_{\Theta}rmg(\Theta,\mathbf{x},y)$$

另外有恒等式，

$$[E_{\Theta}f(\Theta)]^2 = E_{\Theta,\Theta^{'}}f(\Theta)f(\Theta^{'})$$

----------

好，现在我们来推 $var(mr)$，

$$\begin{align*}
var(mr) & = E_{\mathbf{x},y}[mr(\mathbf{x},y)]^2 - (E_{\mathbf{x},y}[mr(\mathbf{x},y)])^2 \\
& = E_{\mathbf{x},y}[E_{\Theta}rmg(\Theta,\mathbf{x},y)]^2 - (E_{\mathbf{x},y}[E_{\Theta}rmg(\Theta,\mathbf{x},y)])^2\\
& = E_{\mathbf{x},y}E_{\Theta,\Theta^{'}}(rmg(\Theta,\mathbf{x},y)rmg(\Theta^{'},\mathbf{x},y)) - E_{\Theta,\Theta^{'}}(E_{\mathbf{x},y}rmg(\Theta,\mathbf{x},y)E_{\mathbf{x},y}rmg(\Theta^{'},\mathbf{x},y))\\
& = E_{\Theta,\Theta^{'}}(\underbrace{E_{\mathbf{x},y}(rmg(\Theta,\mathbf{x},y)rmg(\Theta^{'},\mathbf{x},y)) - E_{\mathbf{x},y}rmg(\Theta,\mathbf{x},y)E_{\mathbf{x},y}rmg(\Theta^{'},\mathbf{x},y)}_{Cov(X,Y) = E[XY]-E[X]E[Y]})\\
& = E_{\Theta,\Theta^{'}}(Cov_{\mathbf{x},y}(rmg(\Theta,\mathbf{x},y)E_{\mathbf{x},y}rmg(\Theta^{'},\mathbf{x},y)))
\end{align*}$$

----------


[^1]: [Random Forest](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)