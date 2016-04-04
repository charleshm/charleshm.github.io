---
published: true
author: Charles
layout: post
title:  "Why Can Machines Learn?"
date:   2016-04-02 7:30
categories: 机器学习 
---

### 机器学习可行性
![此处输入图片的描述][1]


----------


#### Learning is Impossible
上节我们提到，

> 机器学习就是从数据出发，通过**算法** ($\mathcal{A}$) 找到最符合我们数据集 ($\mathcal{D}$) 的 hypothesis $g$（$g \approx f$）.

这个时候就会有一个问题，虽然我们可以保证我们的 $g$ 在数据集 $\mathcal{D}$ 上的判断和真实的 $f$ 一致，但我们**如何保证**在 $\mathcal{D}$ 之外也同样如此？

我们来看下面这个例子，

![此处输入图片的描述][2]
![此处输入图片的描述][3]

可能产生这样的 $\mathcal{D}$ 的 $f$ 有8种，而且似乎都可行。实际上，如果**任何未知**的 $f$ （即建立在数据 $\mathcal{D}$ 上的规则）都是有可能的，那么从这里做出有意义的推理是**不可能**的！

也就是说这个时候我们**无法保证** $g \approx f$，在 $\mathcal{D}$ 之外的数据上？

![此处输入图片的描述][4]


----------


#### Inferring Something Unknown
我们现在希望的是 hypothesis $g$ 在 **in-sample** 上表现好，同时在 **out-sample** 表现**也要好**。

类似于我们在统计学中常见的问题，通过少量的已知样本推断整个样本集的情况，也就是通过在 **in-sample** 上的表现，推断 **out-sample** 上的表现（Inferring Something Unknown）。

有一个罐子，盛放着橙色和绿色两种颜色的小球，我们如何在不查遍所有球的情况下，得知橙色球所占的比例？

![此处输入图片的描述][5]

> 常用的方法就是采样，利用样本中橙色球的比例去估计整体中橙色球的比例。

这个时候我们可能会想，有没有可能我们抽出的球**全是绿色**，或者说我们抽出的样本中两种球的比例跟真实情况**差很远**呢，这样的话我们的估计就会出错呀。

但这种事情发生的可能性大吗？不大，当我们的**样本足够大**时，这种事情发生的可能性会**非常小**。在概率论中，可以用 **霍夫丁不等式**(Hoeffding’s Inequality)  来描述上面这件事情的概率：

![此处输入图片的描述][6]

也就是说样本中橙球的比例和整体橙球的比例相等是 **PAC** (probably approximately correct)，这取决于我们的**样本大小** ($N$) 和对误差的**容忍度** ($\epsilon$)。

![此处输入图片的描述][7]


----------

#### Connection to Learning
我们可以利用 sample 中橙球的比例来推断总体中橙球出现的概率，则同样的，我们也可以利用 **sample** 中 $h(x)\not=f(x)$ 的比例来推断**总体**中 $h(x)\not=f(x)$ 的概率。因为两者的错误率是 PAC 的，只要我们保证前者小，后者也就小了。

![此处输入图片的描述][8]
![此处输入图片的描述][9]

我们记 $h(x)$ 在 in-sample 中错误率为 $E_{in}$ (in-sample-error)，在 out-sample 中错误率为 $E_{out}$ (out-of-sample-error)。

- $E_{out}(h) = \underset{x\sim P}{\epsilon} [h(x)\neq f(x)]$，$\epsilon$ 表示数学期望
- $E_{in}(h) = \frac{1}{N}\sum_{n=1} ^ {N}[h(x_n)\neq y_n]$

![此处输入图片的描述][10]

根据**霍夫丁不等式**(Hoeffding’s Inequality)，

$$\mathbb{P}[|E_{in}(h)-E_{out}(h)|\gt \epsilon]\leq 2 exp(-2\epsilon ^2N)$$

![此处输入图片的描述][11]

#### Real Learning
那么我们是不是直接挑选出 hypothesis set 中 $E_{in}$ **最小**的 $g$ 作为我们的 final hypothesis 就行了呢？

![此处输入图片的描述][12]

当我们的 hypothesis set 中元素非常多的时候，**全对的概率**实际上也是是非常高的。就像丢硬币一样，150个同学同时丢硬币5次，有一人同时抛出**五次正面**的概率大于99%。

![此处输入图片的描述][13]

> **霍夫丁不等式**说的是 $E_{in}$ 和 $E_{out}$ 相等是 PAC 的，但当我们的 hypothesis set **很大**，如同上面掷硬币的**人很多**的情况下，就很有可能出现 $|E_{in}(h)-E_{out}(h)|\gt \epsilon$，即 $E_{in}$ 很小，而 $E_{out}$ 很大的情况。

![此处输入图片的描述][14]


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_103334.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_110139.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_110301.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_111259.png
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_122602.png
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_123127.png
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_124751.png
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_131533.png
  [9]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_131809.png
  [10]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_133439.png
  [11]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_133950.png
  [12]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_142623.png
  [13]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_144129.png
  [14]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_145215.png?imageView2/2/w/500