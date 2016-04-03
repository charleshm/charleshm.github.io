---
published: true
author: Charles
layout: post
title:  "随机森林中的数学理论(3)"
date:   2016-03-27 7:30
categories: 机器学习
---

我们知道，在构建每棵树时，我们对训练集使用了不同的 bootstrap sample：$T_k$（随机且有放回地抽样）。所以对于每棵树而言（假设对于第k棵树），大约有 1/3 的训练实例**没有**参与第 k 棵树的生成，它们称为第 k 棵树的 **oob 样本**。

![此处输入图片的描述][1]

这一部分未被选中的**袋外数据**可用于估计随机森林的单棵决策树分类强度 $s$ 、决策树之间的相关性 $\overline{\rho}$，进而可得到随机森林**泛化误差界**的估计。


----------


#### 分类强度 $s$

**分类强度**的定义：

$$\begin{align*}
s & = E_{\mathbf{x},y}[mr(\mathbf{x},y)]\\
& = E_{\mathbf{x},y}[P_{\Theta}(h(\mathbf{x},\Theta)=y) - \underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)]
\end{align*} \tag{1.1}$$


令，

$$Q(\mathbf{x},j)=\frac{\sum_k I(h(\mathbf{x},\Theta_k)=j;(y,\mathbf{x})\not\in T_{k})}{\sum_k I((y,\mathbf{x})\not\in T_{k})} \tag{1.2}$$

其中， $(y,\mathbf{x})\not\in T_{k}$ 表示未被抽中来参与构建第 $k$ 棵决策树的袋外数据. 

$Q(x,j)$ 是概率 $P_{\Theta}(h(x,\Theta)=j)$ 的袋外估计，我们可以用 $Q(x,y),Q(x,j)$ 来分别表示 $P_{\Theta}(h(\mathbf{x},\Theta)=y),P_{\Theta}(h(\mathbf{x},\Theta)=j)$ 的估计。

则有，

$$\begin{align*}
s & = E_{\mathbf{x},y}[P_{\Theta}(h(\mathbf{x},\Theta)=y) - \underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)]\\
& \rightarrow E_{\mathbf{x},y}[Q(x,y)-\underset{j\not= y}{\max}Q(x,j)]\\
& = \frac{1}{N}\sum_{i=1}^{N}(Q(x_i,y_i)-\underset{j\not= y_i}{\max}Q(x_i,j)) \tag{1.3}
\end{align*}$$


----------

#### 相关性 $\overline{\rho}$
**相关性** $\overline{\rho}$ 的定义：

$$\overline{\rho} = \frac{var(mr)}{(E_{\Theta}sd(\Theta))^2} \tag{2.1}$$

我们需要分别估计 $var(mr),E_{\Theta}sd(\Theta)$ .

$$\begin{align*}
var(mr) & = E_{\mathbf{x},y}[mr(\mathbf{x},y)]^2 - (E_{\mathbf{x},y}[mr(\mathbf{x},y)])^2 \\
\end{align*}$$

----------


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_112544.png