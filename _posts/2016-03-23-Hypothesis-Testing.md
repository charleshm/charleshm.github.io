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

### $t$ 检验[^2]

![此处输入图片的描述][5]

#### 单个样本的 $t$ 检验
单样本均数 t 检验(one sample t test)，用样本均数代表的未知总体均数和已知总体均数进行比较，来观察此组样本与总体的差异性。

> 应用条件：总体标准差 $\sigma$ 未知的小样本( 如 $n<50$ )，且服从正态分布。

在 $H_0:\mu = \mu_0$ 的假定下，可以认为样本是从已知总体中抽取的，根据t分布的原理，单个样本t检验的公式为：

$$t = \frac{\bar{X}-\mu_0}{S/\sqrt{n}} \tag{1}$$

![此处输入图片的描述][6]

----------

#### 配对双体检验
**配对双体检验**，又称非独立两样本均数 t 检验，其假设两组样本之间的**差值**服从正态分布。如果该正态分布的期望为零，则说明这两组样本不存在显著差异。

> 适用于 **配对设计** 计量资料均数的比较，其比较目的是检验两相关样本均数所代表的未知总体均数是否有差别。配对设计方法观察以下几种情形：两个**同质**受试对象分别接受两种不同的处理；同一受试对象接受两种不同的处理；同一受试对象处理前后(self-contrast)。

该检验可理解为**差值样本均数**与已知总体均数 $\mu_d(\mu_d = 0)$ 比较的单样本 t 检验，其检验统计量为：

$$t = \frac{\bar{d}-\mu_d}{S_{\bar{d}}}=\frac{\bar{d}-0}{S_{\bar{d}}}=\frac{\bar{d}}{S_d/\sqrt{n}} \tag{2}$$

----------

#### 非配对双体检验
**非配对双体检验**，又称为两独立样本 t 检验(成组 t 检验)，其假设两组样本是从不同的正态分布($\mathcal{N}(\mu_1,\sigma_1^2)$,$\mathcal{N}(\mu_2,\sigma_2^2)$)采样出来的。根据两个正态分布的标准差是否相等，非配对双体检验又可以分两类。

 - 两总体方差 $\sigma_1^2,\sigma_2^2$ 相等,即**方差齐性**(homogeneity of variance):

> 适用于 **完全随机设计** 的两样本均数的比较，其目的是检验两样本所来自总体的均数是否相等。 

$$\begin{align*} 
t & = \frac{(\bar{X}_1-\bar{X}_2)-(\mu_1-\mu_2)}{S_{\bar{X}_1-\bar{X}_2}}= \frac{|\bar{X}_1-\bar{X}_2|}{S_{\bar{X}_1-\bar{X}_2}} \tag{3}\\ 
S_{\bar{X}_1-\bar{X}_2} & = \sqrt{S_c^2(\frac{1}{n_1}+\frac{1}{n_2})}\\
S_c &= \sqrt{ \frac{(n_1-1)S_1^2+(n_2-1)S_2^2}{n_1+n_2-2}  } \qquad  \text{合并方差}\nonumber 
\end{align*}$$

![此处输入图片的描述][9]

 - 两总体方差不等,即方差不齐，可采用 $t^{\prime}$ 检验，或进行变量变换，或用 **秩和检验** 方法处理。

#### 总结

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
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/t_ca.png
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/hy_set.png?imageView2/2/w/200
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-30_110626.png?imageView2/2/w/400
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/Empirical_Rule.PNG
  [9]: http://7xjbdi.com1.z0.glb.clouddn.com/t_ht2.png?imageView2/2/w/400

  [^1]: [假设检验原理一:t检验](http://www.gotoli.us/%E7%BB%9F%E8%AE%A1%E5%81%87%E8%AE%BE%E6%A3%80%E9%AA%8C%E4%B8%80t%E6%A3%80%E9%AA%8C)
  [^2]: [t 检验](http://jingpin.qmu.edu.cn/yufang/html/kejian.htm)