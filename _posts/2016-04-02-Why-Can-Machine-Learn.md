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

![此处输入图片的描述][2]
![此处输入图片的描述][3]

可能产生这样的 $\mathcal{D}$ 的 $f$ 有8种，而且似乎都可行。实际上，如果**任何未知**的 $f$ （即建立在数据 $\mathcal{D}$ 上的规则）都是有可能的，那么从这里做出有意义的推理是**不可能**的！

也就是说这个时候我们**无法保证** $g \approx f$，在 $\mathcal{D}$ 之外的数据上？

我们来看下面这个例子，

![此处输入图片的描述][4]


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_103334.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_110139.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_110301.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-04_111259.png?imageView2/2/w/400