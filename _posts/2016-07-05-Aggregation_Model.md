---
published: true
author: Charles
layout: post
title:  "Aggregation 模型学习笔记"
date:   2016-07-05 7:30
categories: 机器学习 
---

由多个弱分类器来构建强分类器，三个臭皮匠顶个诸葛亮。

![][1]


----------


#### Uniform Blending

> 赋予每个模型相同的权重。

- Uniform Blending for Classification

![][2]

- Uniform Blending for Regression

![][3]


----------


#### 为什么 Work

![][4]

> bias 和 variance 之间的 trade off.

![][5]


----------

#### Linear Blending

对不同的模型做线性加权。

![][6]

> 利用测试集而不是训练集来做 Selection。

![][7]


----------


#### Any Blending

> 不一定用 Linear Blending,可以是 Any Blending(stacking).

![][8]

----------


#### 台大冠军队分享

![][9]


----------

### Bagging

> 前面我们做的工作都是先训练出一些模型，然后对这些模型做 Blending。那么我们能不能在训练模型的过程中，做 Blending呢？

利用 Bootstrap生成不一样的训练集得到不同的模型，然后做 Uniform Blending.

![][10]


----------

![][11]


----------

总结下，

![][12]

[1]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_065406.png
[2]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_070221.png
[3]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_070342.png
[4]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_070818.png
[5]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_072235.png
[6]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_080712.png
[7]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_081657.png
[8]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_081852.png
[9]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_082216.png
[10]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_083059.png
[11]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_083347.png
[12]:http://7xjbdi.com1.z0.glb.clouddn.com/2016-09-17_083439.png
