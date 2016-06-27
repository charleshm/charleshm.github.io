---
published: true
author: Charles
layout: post
title:  "随机森林中的数学理论(3)"
date:   2016-03-27 7:30
categories: 机器学习
---

我们知道，在构建每棵树时，我们对训练集使用了不同的 bootstrap sample：$T_k$（随机且有放回地抽样）。所以对于每棵树而言（假设对于第k棵树），大约有 1/3 ($\lim\limits_{N\to\infty}(1-\frac{1}{N})^{N}=e^{-1}\approx33\%$)的训练实例**没有**参与第 k 棵树的生成，它们称为第 k 棵树的 **oob 样本**[^1]。

![此处输入图片的描述][1]

这一部分未被选中的**袋外数据**可用于估计随机森林的单棵决策树**分类强度** $s$ 、决策树之间的**相关性** $\overline{\rho}$，进而可得到随机森林**泛化误差界**的估计。


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

又，

$$\begin{align*}
var(mr) & = E_{\mathbf{x},y}[mr(\mathbf{x},y)]^2 - (E_{\mathbf{x},y}[mr(\mathbf{x},y)])^2 \\
& = E_{\mathbf{x},y}[P_{\Theta}(h(\mathbf{x},\Theta)=y) - \underset{j\not= y}{\max}P_{\Theta}(h(\mathbf{x},\Theta)=j)]^2 - s^2\\
& = \frac{1}{N}\sum_{i=1}^{N}(Q(x_i,y_i)-\underset{j\not= y_i}{\max}Q(x_i,j))^2 - (\frac{1}{N}\sum_{i=1}^{N}(Q(x_i,y_i)-\underset{j\not= y_i}{\max}Q(x_i,j)))^2 
\end{align*}$$

而， 

$$\begin{align*}
rmg(\Theta,\mathbf{x},y) & = I(h(\mathbf{x},\Theta)=y)-I(h(\mathbf{x},\Theta)=\hat{j}(\mathbf{x},y))\\
E_{\Theta}sd(\Theta) & = E_{\Theta}[var_{\mathbf{x},y}rmg(\Theta,\mathbf{x},y)] \tag{2.2}
\end{align*}$$

$rmg(\Theta,\mathbf{x},y)$ 的分布律如下，

![此处输入图片的描述][2]

那么，

$$\begin{align*}
var_{\mathbf{x},y}rmg(\Theta,\mathbf{x},y) & = E_{\mathbf{x},y}[rmg(\Theta,\mathbf{x},y)]^2 - (E_{\mathbf{x},y}[rmg(\Theta,\mathbf{x},y)])^2\\
& = [p_1+p_2+(p_1-p_2)^2]^{\frac{1}{2}} \tag{2.3}
\end{align*}$$

故，

$$\overline{\rho} = \frac{\frac{1}{N}\sum_{i=1}^{N}(Q(x_i,y_i)-\underset{j\not= y_i}{\max}Q(x_i,j))^2 - (\frac{1}{N}\sum_{i=1}^{N}(Q(x_i,y_i)-\underset{j\not= y_i}{\max}Q(x_i,j)))^2}{E_{\Theta}[p_1+p_2+(p_1-p_2)^2]^{\frac{1}{2}}} \tag{2.4}$$

----------

#### OOB ERROR
**oob 估计**计算流程[^3]：

- 对每个样本，计算把它作为 oob 样本的树对它的分类情况（约1/3的树）；
- 然后以简单多数**投票**作为该样本的分类结果；
- 最后用误分个数占样本总数的比率作为随机森林的 oob 误分率。

> Put each case left out in the construction of the kth tree down the kth tree to get a classification. In this way, a test set classification is obtained for each case in about one-third of the trees. At the end of the run, take j to be the class that got most of the votes every time case n was oob. The proportion of times that j is not equal to the true class of n averaged over all cases is the oob error estimate. This has proven to be unbiased in many tests[^2].

![此处输入图片的描述][3]

有一个问题值得注意的是，作者说 OOB 是一种**无偏估计**，是通过实验的方式得出的结论，并没有理论证明。另外有[**论文**][4]论证了在样本数少于特征数的情况下(“small n, large p problem”)，这种估计方式是**有偏**的[^4]。

![此处输入图片的描述][5]

----------

资料传送门：

- [Awesome Random Forest](https://github.com/kjw0612/awesome-random-forest)

----------


[^1]: [Random Forest](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)
[^2]: [Random Forests(Leo Breiman and Adele Cutler)](http://www.stat.berkeley.edu/~breiman/RandomForests/cc_home.htm#inter)
[^3]: [随机森林（Random Forest）](http://www.cnblogs.com/maybe2030/p/4585705.html)
[^4]: [How would one formally prove that the OOB error in random forest is unbiased?](http://stats.stackexchange.com/questions/105811/how-would-one-formally-prove-that-the-oob-error-in-random-forest-is-unbiased)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_112544.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/RF_Math_3.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_151911.png
  [4]: http://www.scirp.org/journal/PaperInformation.aspx?paperID=8072#.U7bDnf74o3w
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-10_204329.png
