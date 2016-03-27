---
published: true
author: Charles
layout: post
title:  "随机森林中的数学理论"
date:   2016-03-25 15:30
categories: 机器学习
---

随机森林是一种有效的分类预测方法，它有很高的**分类精度**，对于噪声和异常值有较好的稳健性，且具有较强的**泛化能力**。Breiman[^1]在论文中提出了随机森林的数学理论，证明随机森林不会出现决策树的过拟合问题，推导随机森林的泛化误差界，并且利用 Bagging 产生的袋外数据给出泛化误差界的估计。

![此处输入图片的描述][1]


----------

#### 随机森林
![此处输入图片的描述][2]


----------


#### 为什么不会过拟合
Breiman证明了，随着随机森林中决策树增加，其泛化误差会趋于一个**有限的上限**。

下面我们来分析下具体证明过程。

定义**余量函数**(margin function)：

$$\begin{align*}
mg(\mathbf{x},y) & = av_kI(h_k(\mathbf{x})=y)-\underset{j\not =Y}{\max} av_kI(h_k(\mathbf{x})=j) \tag{1}\\
& = \hat{P}_k(h_k(\mathbf{x})=y) - \underset{j\not =y}{\max} \hat{P}_k(h_k(\mathbf{x})=j)
\end{align*}$$

其中，$I(\cdot)$为指示函数(indicator function)，$av_k(\cdot)$表示取平均。余量函数衡量了组合分类器将样本分类正确的平均票数与错分为其他类的平均票数之差。也就是说**余量越大，分类正确可能性越大**。

 - $mg(\mathbf{x},y)>0$ : 分类正确.
 - $mg(\mathbf{x},y)>0$ : 分类错误.

定义**泛化误差**( generalization error)：

$$\begin{align*}
PE^* & = P_{X,Y}(mg(\mathbf{x},y)<0) \tag{2}
\end{align*}$$

> **定理**：随着 $K \rightarrow \infty$（决策树数目）， 
$$PE^* \underset{K \rightarrow \infty}{\rightarrow} P_{\mathbf{x},y}[P_{\Theta}(h(\mathbf{x},\Theta)=y)-\underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)<0] \tag{3}$$

其中， $h_k(\mathbf{x})\equiv h(\mathbf{x},\Theta_k)$.

又有，

$$\hat{P}_k(h_k(\mathbf{x})=j)=E_k[I(h_k(\mathbf{x})=j)]=\frac{1}{K}\sum_{k=1}^K I(h_k(\mathbf{x})=j)$$

实际上我们需要证明的是，

$$\frac{1}{K}\sum_{k=1}^K I(h(\mathbf{x},\Theta)=j) \rightarrow P_{\Theta}(h(\mathbf{x},\Theta)=j) \tag{4}$$


原因也很明了，令：

$$\begin{align*}
g_K(\mathbf{x},y) & = \hat{P}_k(h_k(\mathbf{x})=y) - \underset{j\not =y}{\max} \hat{P}_k(h_k(\mathbf{x})=j)\\
g(\mathbf{x},y) & = P_{\Theta}(h(\mathbf{x},\Theta)=y)-\underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j) 
\end{align*}$$

而实际上，
$$\begin{align*}
P_{\mathbf{x},y}(g_K(\mathbf{x},y)<0) & = E_{\mathbf{x},y}[I(g_K(\mathbf{x},y)<0)] \underset{K \rightarrow \infty}{\rightarrow} P_{\mathbf{x},y}(g(\mathbf{x},y)<0) \\
\text{原问题} & \overset{等价于证明}{\Longrightarrow} g_K(\mathbf{x},y) \underset{K \rightarrow \infty}{\rightarrow} g(\mathbf{x},y) \\
g_K(\mathbf{x},y) \rightarrow g(\mathbf{x},y) &\overset{等价于证明}{\Longrightarrow} \hat{P}_k(h_k(\mathbf{x})=j)=\frac{1}{K}\sum_{k=1}^K I(h_k(\mathbf{x})=j) \underset{K \rightarrow \infty}{\rightarrow} P_{\Theta}(h(\mathbf{x},\Theta)=j)
\end{align*}$$

----------


  [^1]: [Random Forest](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-27_164830.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-27_165533.png