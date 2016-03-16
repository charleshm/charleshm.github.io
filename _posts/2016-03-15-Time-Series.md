---
published: true
author: Charles
layout: post
title:  "时间序列分析"
date:   2016-03-15 9:45
categories: 统计学
---

在统计学中，数据的独立性是我们希望的性质，问题是时间序列数据间往往是相关的。时间序列分析的目的就是要通过统计模型提取一个序列的结构，将原始的序列转化为相互独立的序列。

刚开始接触，很多东西没有啥深刻的理解。

### ARIMA模型
ARIMA（p，d，q）称为差分自回归移动平均模型，AR是自回归， p为自回归项；MA为移动平均，q为移动平均项数，d为时间序列成为平稳时所做的差分次数。

> 所谓 ARIMA 模型，是指将非平稳时间序列转化为平稳时间序列，然后将因变量仅对它的**滞后值**以及随机误差项的**现值**和**滞后值**进行回归所建立的模型。ARIMA 模型根据原序列是否平稳以及回归中所含部分的不同，包括移动平均过程（MA）、自回归过程（AR）、自回归移动平均过程（ARMA）以及 ARIMA 过程[^1]。

![此处输入图片的描述][1]

我们先来看一下几种最简单的形式长啥样，

$$\begin{array}{l l l}
ARIMA(0,0,0) & \rightarrow & \text{White Noise}\\
ARIMA(0,1,0) & \rightarrow & \text{Random Walk}\\
ARIMA(1,0,0) & \rightarrow & \text{Autoregressive Model(order 1)}\\
ARIMA(0,0,1) & \rightarrow & \text{Moving Average Model(order 1)}\\
ARIMA(1,0,1) & \rightarrow & \text{Simple Mixed Model}\\
\end{array}$$


----------


#### MA模型[^2]
一般的滑动平均模型被定义为：

$$Y_t=e_t-\theta_{1}e_{t-1}-\theta_{2}e_{t-2}-\cdots-\theta_{q}e_{t-q} \tag{1}$$

> MA(q) 在 t 时刻的值就是白噪声序列在 t 时刻到 t-q 时刻的线性组合。

自相关系数：

$$\rho_k=\left\{
\begin{array}{l l}
1                          & (k=0) \\
\frac{-\theta_k+\theta_1\theta_{k+1}+\cdots+\theta_{q-k}\theta_{q}}{1+\theta_1^2+\theta_2^2+\cdots+\theta_q^2} & (k=1,2,\cdots,q) \\
0                          & (k>q)
\end{array}
\right.$$

> 这里我就可以得到一个基本的结论，MA(q) 在时滞大于q后没有相关性，也就说 MA(q) 模型每一个序列值只与其前 q 个值有相关性。

我们可以直观的看下 MA(1) 这样一个最简单的模型，

$$Y_t=e_t-\theta e_{t-1}$$

![此处输入图片的描述][3]


----------


#### AR模型
自回归模型就是用自身做回归变量，具体来说，p 阶自回归过程满足方程：

$$Y_t=e_t+\phi_{1}Y_{t-1}+\phi_{2}Y_{t-2}+\cdots+\phi_{p}Y_{t-p} \tag{2}$$

> 序列 $Y_t$ 的当前值是自身最近 p 阶滞后项和 $e_t$ 的线性组合， 其中 $e_t$ 包括了序列在 $t$ 时刻无法用过去值来解释的所有新信息。

从公式上看，AR应该比MA具有更强的自相关性，因为MA仅与滞后的白噪声因素相关，而AR是时间序列前后直接相关。

我们看下 AR(1) 时间序列，

$$Y_t=e_t+\phi Y_{t-1}$$

![此处输入图片的描述][4]

现实建模时一般很少使用高于AR(2)的模型，因为过高的阶会导致复杂的模型和提高过拟合风险。因此在实际使用中了解AR(1)和AR(2)的特性一般就足够了。


----------


#### ARMA模型[^3]
如果假定序列中部分是自回归 (AR)，部分是滑动平均 (MA)，我们就可以的到一个相当普遍的时间序列模型：**自回归滑动平均混合过程**，

$$Y_t=e_t+\underbrace{\phi_{1}Y_{t-1}+\phi_{2}Y_{t-2}+\cdots+\phi_{p}Y_{t-p}}_{AR}\underbrace{-\theta_{1}e_{t-1}-\theta_{2}e_{t-2}-\cdots-\theta_{q}e_{t-q}}_{MA} \tag{3}$$

ARMA(1,1)时间序列：

$$Y_t=e_t+\phi Y_{t-1}-\theta e_{t-1}$$

![此处输入图片的描述][5]


----------


#### ARIMA模型
前面我们谈到的模型都要求时间序列，是平稳的，那么遇到非平稳的该怎么办呢？

这个时候就要引出我们的ARIMA模型了（实际上就加上了差分的操作），

> 如果一个时间序列 $\{ Y_t \}$ 的 d 次差分 $W_t = \nabla^d Y_t$ 是一个平稳的 ARMA 过程，则称 $\{ Y_t \}$ 为差分自回归移动平均模型 .

![此处输入图片的描述][2]

----------


[^1]: [ARIMA模型](http://baike.baidu.com/link?url=TVGuqY12wgvY8EroyQaIwFwk73Qj4jANkDAJAMFrQKmwjfW3rJWiyYBhYXhod9m9Kx3_sQF_bCxwzTQhIRTr3a)
[^2]: [时间序列分析基础](http://blog.codinglabs.org/articles/time-series-analysis-foundation.html)（重点 "Copy"）
[^3]: 时间序列分析及应用：R语言

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-16_100741.png?imageView2/2/w/500
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/arima-models.png?imageView2/2/w/400
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/ma1.png?imageView2/2/w/400
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/ar1.png?imageView2/2/w/400
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/arma11.png?imageView2/2/w/400