---
published: true
author: Charles
layout: post
title:  "随机森林中的数学理论(1)"
date:   2016-03-25 15:30
categories: 机器学习
---

随机森林是一种有效的分类预测方法，它有很高的**分类精度**，对于噪声和异常值有较好的稳健性，且具有较强的**泛化能力**。Breiman[^1]在论文中提出了随机森林的数学理论，证明随机森林不会出现决策树的过拟合问题，推导随机森林的泛化误差界，并且利用 Bagging 产生的袋外数据给出泛化误差界的估计。

![此处输入图片的描述][1]


----------

#### 随机森林
![此处输入图片的描述][2]

前几天面试的时候，面试官问了下随机森林与 Bagging 的区别，

![此处输入图片的描述][3]

> - 每棵树的训练样本    
- 划分时属性的随机选择

----------

#### Why not overfit
Breiman证明了，随着随机森林中决策树增加，其泛化误差会趋于一个**有限的上限**。

> 但是我们需要指出的是，任何机器学习算法都可能会过拟合，作者证明的是随机森林不会随着决策树的增加而产生过拟合问题（但会产生一定限度内的泛化误差），**随机森林也是会过拟合的**[^4]。    
This result explains why random forests do not overfit as more trees are added, but produce a limiting value of the generalization error.（作者原话）

下面我们来分析下具体**证明过程**[^2]。

**Weak Classifiers**:  $h = \\{h_1(\mathbf{x}),h_2(\mathbf{x}),\cdots,h_K(\mathbf{x})\\}$.

定义**余量函数**(margin function)：

$$\begin{align*}
\color{orange}{mg(\mathbf{x},y)} & = av_kI(h_k(\mathbf{x})=y)-\underset{j\not =Y}{\max} av_kI(h_k(\mathbf{x})=j) \tag{1}\\
& = \hat{P}_k(h_k(\mathbf{x})=y) - \underset{j\not =y}{\max} \hat{P}_k(h_k(\mathbf{x})=j)
\end{align*}$$

其中，$I(\cdot)$为示性函数(indicator function)，$av_k(\cdot)$表示取平均。余量函数衡量了组合分类器将样本分类正确的平均票数与错分为其他类的平均票数之差。也就是说**余量越大，分类正确可能性越大**。

 - $mg(\mathbf{x},y)>0$ : 分类正确.
 - $mg(\mathbf{x},y)>0$ : 分类错误.

定义**泛化误差**( generalization error)：

$$\begin{align*}
\color{orange}{PE^*} & = P_{X,Y}(mg(\mathbf{x},y)<0) \tag{2}
\end{align*}$$

> **定理**：随着 $K \rightarrow \infty$（决策树数目）， 
$$PE^* \underset{K \rightarrow \infty}{\rightarrow} P_{\mathbf{x},y}[P_{\Theta}(h(\mathbf{x},\Theta)=y)-\underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)<0] \tag{3}$$

其中， $h_k(\mathbf{x})\equiv h(\mathbf{x},\Theta_k)$.


----------

又有，

$$\color{orange}{\hat{P}_k(h_k(\mathbf{x})=j)}=E_k[I(h_k(\mathbf{x})=j)]=\frac{1}{K}\sum_{k=1}^K I(h_k(\mathbf{x})=j)$$

**实际上我们需要证明的是**，

$$\color{red}{\frac{1}{K}\sum_{k=1}^K I(h(\mathbf{x},\Theta)=j) \rightarrow P_{\Theta}(h(\mathbf{x},\Theta)=j)} \tag{4}$$

原因也很明了，令：

$$\begin{align*}
g_K(\mathbf{x},y) & = \hat{P}_k(h_k(\mathbf{x})=y) - \underset{j\not =y}{\max} \hat{P}_k(h_k(\mathbf{x})=j)\\
g(\mathbf{x},y) & = P_{\Theta}(h(\mathbf{x},\Theta)=y)-\underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j) 
\end{align*}$$

而实际上，
$$\begin{align*}
\text{原问题} & \overset{等价于}{\Longrightarrow} g_K(\mathbf{x},y) \underset{K \rightarrow \infty}{\rightarrow} g(\mathbf{x},y) \\
g_K(\mathbf{x},y) \rightarrow g(\mathbf{x},y) &\overset{等价于}{\Longrightarrow} \hat{P}_k(h_k(\mathbf{x})=j)=\frac{1}{K}\sum_{k=1}^K I(h_k(\mathbf{x})=j) \underset{K \rightarrow \infty}{\rightarrow} P_{\Theta}(h(\mathbf{x},\Theta)=j)
\end{align*}$$


----------

我们知道，对于一个给定的训练集 $\mathbf{x}$ 和 $\Theta$，使得 $h(x,\Theta)=j$ 成立的所有 $x$构成一个**超立方体**(hyper-rectangle)的并集[^3]。我们假设对于所有的 $h(x,\Theta_k)$ 最后组成 $K$ 个这样的超立方体: $\\{S_k\\}_{k=1}^K$.

![此处输入图片的描述][4]

定义，

$$\begin{align*}
\phi(\Theta) & = k\quad \text{if}\quad \{\mathbf{x}: h(\mathbf{x},\Theta)=j\}=S_k\\
N_k & = \text{# times} \quad \phi(\Theta_m) = S_k
\end{align*}$$

可知，

$$\frac{1}{M}\sum_{m=1}^{M}I(h(\mathbf{x},\Theta_m)=j)= \frac{1}{M}\sum_{k}N_kI(\mathbf{x}\in S_k)$$

根据 $N_k$ 的定义可知，

$$\begin{align*}
\frac{N_k}{M} &  = \frac{1}{M} \sum_{m=1}^{M} I(\phi(\Theta_m)=k)\\
& = E_{\Theta}[I(\phi(\Theta_m)=k)] \\
& \rightarrow P_{\Theta}(\phi(\Theta_m)=k)
\end{align*}$$

更进一步，

$$\begin{align*}
\color{blue}{\frac{1}{M}\sum_{m=1}^{M}I(h(\mathbf{x},\Theta_m)=j)} & \rightarrow \sum_k P_{\Theta}(\phi(\Theta_m)=k)I(\mathbf{x}\in S_k)\\
& = \color{red}{P_{\Theta}(h(\mathbf{x},\Theta)=j)}
\end{align*}$$

得证。


----------


#### 总结
好吧，我还没法完全领悟这个证明的内涵，知其形而不知其意。

----------

资料传送门：

- [Awesome Random Forest](https://github.com/kjw0612/awesome-random-forest)

----------


  [^1]: [Random Forest](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)
  [^2]: [Mathematics of Random Forests](http://math.bu.edu/people/mkon/MA751/L19RandomForestMath.pdf)
  [^3]: [随机森林理论浅析](http://www.cnki.com.cn/Article/CJFDTotal-JCJI201301001.htm)
  [^4]: [Do Random Forest overfit?](http://datascience.stackexchange.com/questions/1028/do-random-forest-overfit)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-27_164830.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-27_165533.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-28_094540.png?imageView2/2/w/350
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-27_184949.png?imageView2/2/w/400
