---
published: true
author: Charles
layout: post
title:  "假设检验"
date:   2016-03-23 15:30
categories: 机器学习 统计学
---

假设检验（hypothesis  test）亦称**显著性检验**（significant test），是先对总体的参数或分布作出某种假设，然后用适当的方法，根据样本对总体提供的信息，推断此假设应当拒绝或不拒绝。

其实质是判断观察到的“差别”是**抽样误差**引起还是总体上的不同，目的是评价两个不同的参数或两种不同处理引起效应不同的证据有多强，这种证据的强度用概率 P 度量和表示。

![此处输入图片的描述][1]

----------

#### P值
P值就是当原假设为真时出现现状或更差的情况的概率。如果P值很小，说明这种情况的发生的概率很小，而如果出现了，根据小概率原理，我们就有理由拒绝原假设，P值越小，我们拒绝原假设的理由越充分。

![此处输入图片的描述][2]

"P" 值的使用也引起了统计学家的争议: [Scientific method: Statistical errors : Nature News & Comment](http://www.guokr.com/article/438043/).

----------

#### 基本步骤

假设检验的基本步骤包括，

 1. 建立假设，选择一个显著性水平 $\alpha$ .
 2. 选择检验方法和计算检验统计量 .
 3. 确定 P 值和做出统计推断结论 .

> 所有的假设检验都按照这三个步骤进行，各种检验方法的差别在于第二步计算的**检验统计量**不同。

![此处输入图片的描述][3]

比如，我们观察到一组独立地从同一个分布采样出来的样本。如果想了解这个分布的性质，可以针对这个分布的参数做出两个不同的假设。一个叫**零假设** $H_0$，另一个叫**备选假设** $H_1$ 。比如针对分布的期望值 $\mu$，零假设可以是 $\pmb{H}_0:u = u_0$，备选假设可以是 $\pmb{H}_1: u\neq u_0$。假设检验过程根据观察到的样本决定接受其中一个假设。

----------


#### 两类错误[^1]

假设检验有**两类错误**，第一类型错误和第二类型错误。第一类型错误，是零假设成立的情况下**拒绝**零假设。第二类型错误，是备选假设成立的情况下**接受**零假设。

|     Type    | Decision:Accept $H_0$ | Decision:Reject $H_0$ |
|:-----------:|:---------------------:|:---------------------:|
| Truth:$H_0$ |    Correct Decision   |      Type I Error     |
| Truth:$H_1$ |     Type II Error     |    Correct Decison    |

> 是不是想起了我们分类器评估准则里面的 [混淆矩阵][4] .


----------

#### 常识

3 $\sigma$ 原则:

- 数值分布在 $(\mu—\sigma,\mu+\sigma)$ 中的概率为0.68.
- 数值分布在 $(\mu—2\sigma,\mu+2\sigma)$ 中的概率为0.95.
- 数值分布在 $(\mu—3\sigma,\mu+3\sigma)$ 中的概率为0.997. 

![此处输入图片的描述][8]


----------

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-25_132117.png?imageView2/2/w/400
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/ajyqbCYCd2xbzjci_1ms8i9Tb_BotSwAKpsDRNHLoqd4BQAAGAMAAEpQ.jpg
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-30_111057.png
  [4]: http://charlesx.top/2016/03/Classification-Model-Performance/
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-30_110626.png?imageView2/2/w/400
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/Empirical_Rule.PNG

  [^1]: [假设检验原理一:t检验](http://www.gotoli.us/%E7%BB%9F%E8%AE%A1%E5%81%87%E8%AE%BE%E6%A3%80%E9%AA%8C%E4%B8%80t%E6%A3%80%E9%AA%8C)
