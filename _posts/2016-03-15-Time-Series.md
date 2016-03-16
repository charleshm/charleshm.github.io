---
published: true
author: Charles
layout: post
title:  "时间序列分析"
date:   2016-03-15 9:45
categories: 统计学
---

在统计学中，数据的独立性是我们希望的性质，问题是时间序列数据间往往是相关的。因而时间序列分析的目的就是要通过统计模型提取一个序列的结构，将原始的序列转化为相互独立的序列。

#### ARIMA模型
ARIMA（p，d，q）称为差分自回归移动平均模型，AR是自回归， p为自回归项；MA为移动平均，q为移动平均项数，d为时间序列成为平稳时所做的差分次数。

所谓ARIMA模型，是指将非平稳时间序列转化为平稳时间序列，然后将因变量仅对它的滞后值以及随机误差项的现值和滞后值进行回归所建立的模型。ARIMA模型根据原序列是否平稳以及回归中所含部分的不同，包括移动平均过程（MA）、自回归过程（AR）、自回归移动平均过程（ARMA）以及ARIMA过程[^1]。

ARIMA模型可分为3种：(1)自回归模型(简称AR模型)；(2)滑动平均模型(简称MA模型)；(3)自回归滑动平均混合模型(简称ARIMA模型)。

我们来看一下几种最简单的形式，

$$\begin{array}{l l l}
ARIMA(0,0,0) & \rightarrow & \text{White Noise}\\
ARIMA(0,1,0) & \rightarrow & \text{Random Walk}\\
ARIMA(1,0,0) & \rightarrow & \text{Autoregressive Model(order 1)}\\
ARIMA(0,0,1) & \rightarrow & \text{Moving Average Model(order 1)}\\
ARIMA(1,0,1) & \rightarrow & \text{Simple Mixed Model}\\
\end{array}$$


[^1]: [ARIMA模型](http://baike.baidu.com/link?url=TVGuqY12wgvY8EroyQaIwFwk73Qj4jANkDAJAMFrQKmwjfW3rJWiyYBhYXhod9m9Kx3_sQF_bCxwzTQhIRTr3a)