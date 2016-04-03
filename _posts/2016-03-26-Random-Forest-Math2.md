---
published: true
author: Charles
layout: post
title:  "随机森林中的数学理论(2)"
date:   2016-03-26 15:30
categories: 机器学习
---

Breiman[^1]证明了泛化误差的上界是可以得到的，它主要由两方面因素决定：单棵决策树的**分类强度**，决策树之间的**相关性**。

![此处输入图片的描述][1]

----------

#### 泛化误差上界
定义单棵决策树的**余量函数**：

$$mr(\mathbf{x},y) = P_{\Theta}(h(\mathbf{x},\Theta)=y) - \underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)$$

**分类强度**(余量函数在整个 data space 的期望)：

$$s = E_{\mathbf{x},y}[mr(\mathbf{x},y)]$$

假定 $s>0$（即随机森林对各个样本的分类结果是可信的），根据切比雪夫不等式：

$$\begin{align*}
P_{\mathbf{x},y}(mr(\mathbf{x},y)<0) & = P_{\mathbf{x},y}(mr(\mathbf{x},y) - E_{\mathbf{x},y}[mr(\mathbf{x},y)]<-E_{\mathbf{x},y}[mr(\mathbf{x},y)])\\
& \leq P_{\mathbf{x},y}(|mr(\mathbf{x},y)-E_{\mathbf{x},y}[mr(\mathbf{x},y)]|>E_{\mathbf{x},y}[mr(\mathbf{x},y)])\\
& \leq \frac{var(mr)}{s^2} \tag{1}
\end{align*}$$


----------

#### 准备工作
要求出这个上界，我们需要先分析下 $var(mr)$。为了解决这个问题，我们引入几个新的量，

$$\begin{align*}
\hat{j}(x,y) & = \arg \underset{j\not= y}{\max} P_{\Theta}(h(\mathbf{x},\Theta)=j)\\
mr(\mathbf{x},y) & = P_{\Theta}(h(\mathbf{x},\Theta)=y) - \underset{j\not= Y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)\\
& = P_{\Theta}(h(\mathbf{x},\Theta)=y) - P_{\Theta}(h(\mathbf{x},\Theta)=\hat{j}(x,y))\\
& = E_{\Theta}[I(h(\mathbf{x},\Theta)=y)-I(h(\mathbf{x},\Theta)=\hat{j}(x,y))]\\
rmg(\Theta,\mathbf{x},y) & = I(h(\mathbf{x},\Theta)=y)-I(h(\mathbf{x},\Theta)=\hat{j}(\mathbf{x},y)) \tag{2}
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
& = E_{\Theta,\Theta^{'}}(Cov_{\mathbf{x},y}(rmg(\Theta,\mathbf{x},y)rmg(\Theta^{'},\mathbf{x},y)))\\
& = E_{\Theta,\Theta^{'}}(\rho(\Theta,\Theta^{'})sd(\Theta)sd(\Theta^{'}))
\end{align*}$$


----------

#### 正式开推
定义决策树之间的平均相关系数 $\overline{\rho}$，

$$\overline{\rho} = \frac{E_{\Theta,\Theta^{'}}(\rho(\Theta,\Theta^{'})sd(\Theta)sd(\Theta^{'})}{E_{\Theta,\Theta^{'}}(sd(\Theta)sd(\Theta^{'}))} \tag{3}$$

则，

$$\begin{align*}
var(mr) & = E_{\Theta,\Theta^{'}}(\rho(\Theta,\Theta^{'})sd(\Theta)sd(\Theta^{'})\\
& = \overline{\rho}(E_{\Theta}sd(\Theta))^2\\
& \leq \overline{\rho}E_{\Theta} var(\Theta)\\
& = \overline{\rho}E_{\Theta} var_{\mathbf{x},y}(rmg(\Theta,\mathbf{x},y)\\
& = \overline{\rho}E_{\Theta}(E_{\mathbf{x},y}(rmg(\Theta,\mathbf{x},y))^2 - (E_{\mathbf{x},y}rmg(\Theta,\mathbf{x},y)^2)\\
& \leq \overline{\rho}(E_{\Theta}E_{\mathbf{x},y}(rmg(\Theta,\mathbf{x},y))^2 - (E_{\Theta}E_{\mathbf{x},y}rmg(\Theta,\mathbf{x},y))^2)\\
& \leq \overline{\rho}(1-s^2)
\end{align*}$$

故，

$$\Rightarrow  P_{\mathbf{x},y}(mr(\mathbf{x},y)<0) \leq \frac{\overline{\rho}(1-s^2)}{s^2} \tag{4}$$

----------

#### 总结
随机森林错误率与两个因素有关[^2]：

 - 森林中任意两棵树的**相关性**(correlation)：相关性越大，错误率越大；
 - 森林中每棵树的**分类强度**(strength)：每棵树的分类能力越强，整个森林的错误率越低。

减小特征选择个数m，树的相关性和分类强度也会相应的降低；增大m，两者会随之增大。所以关键的问题是如何选择最优的 m（或者是范围），通过计算**袋外错误率** oob error，我们可以很快锁定 m.

----------

资料传送门：

- [Awesome Random Forest](https://github.com/kjw0612/awesome-random-forest)

----------


[^1]: [Random Forest](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)
[^2]: [Random Forests(Leo Breiman and Adele Cutler)](http://www.stat.berkeley.edu/~breiman/RandomForests/cc_home.htm#inter)

[1]: http://7xjbdi.com1.z0.glb.clouddn.com/tree_RF.png?imageView2/2/w/400