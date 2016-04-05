---
published: true
author: Charles
layout: post
title:  "When Can Machines Learn?"
date:   2016-04-01 7:30
categories: 机器学习 
---

![此处输入图片的描述][1]

----------

### 宏观把控[^1]

![此处输入图片的描述][2]

- **什么是机器学习**？

<div class="inline_list">
Mitchell 的 "Machine Learning" 里面是这样来定义机器学习的：<strong>利用经验来改善计算机系统自身的性能</strong>。
</div>

![此处输入图片的描述][3]

----------


- **为什么需要机器学习**？

<div class="inline_list">
我们有的时候会遇到一些很难的 case，比如我们希望设计一个程序来<strong>辨识树</strong>，这个时候我们需要对树<strong>下定义</strong>，听起来似乎很简单，却又无从下手。
</div>

![此处输入图片的描述][4]

----------


- **什么时候使用机器学习**？

<div class="inline_list">
其实就<strong>三要素</strong>：<br>
<ol>
<li>有规律可以学习；</li>
<li>编程很难做到；</li>
<li>有能够学习到规律的数据；</li>
</ol>
</div>


![此处输入图片的描述][5]

----------

### 机器学习模型

我们通过一个银行是否应该给用户发信用卡的信用卡例子引出了机器学习模型。


#### 基本概念
![此处输入图片的描述][6]


----------

#### 学习流程
learning干的事情，就是从 hypothesis set 里面挑一个“长”的最像 $f$ 的方程 $g$.

![此处输入图片的描述][7]


----------


#### 总结

> 机器学习就是从数据出发，通过**算法** ($\mathcal{A}$) 找到最符合我们数据集 ($\mathcal{D}$) 的 hypothesis $g$（$g \approx f$）.

![此处输入图片的描述][8]

$$\text{learning model} \rightarrow \text{learning algorithm}(\mathcal{A}) + \text{hypothesis set}(\mathcal{H})$$

对比一下李航老师总结的，

![此处输入图片的描述][9]

----------

### 概念辨析

#### Machine Learning vs Data Mining
**数据挖掘就是试图从海量数据中找出有用的知识**。

大体上看，数据挖掘可以视为**机器学习**和**数据库**的交叉，它主要利用机器学习界提供的技术来分析海量数据，利用数据库界提供的技术来管理海量数据[^2]。

![此处输入图片的描述][10]


----------

#### Machine Learning vs Artificial Intelligence
机器学习是实现人工智能的一种方式 .

![此处输入图片的描述][11]

----------

#### Machine Learning vs Statistics
![此处输入图片的描述][12]

----------


[^1]: [机器学习基石（Machine Learning Foundations）](https://www.coursera.org/course/ntumlone)
[^2]: [机器学习与数据挖掘](http://wenku.baidu.com/link?url=uGAlJOxGjWmgJ4tD5mJnabjHU1ziMt3OaN8UnMwuPJKfHxpPc93eDnmMdV3fL4SShXstrlcfMFeh-Vgc5JaUlBBdCi21atBUcdz0axRZiO7)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_194735.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/mc_a.png?imageView2/2/w/300
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_200749.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_200613.png
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_200945.png
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_202929.png
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_205722.png
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_211027.png
  [9]: http://7xjbdi.com1.z0.glb.clouddn.com/lihang_mmodel.png?imageView2/2/w/300
  [10]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_213147.png
  [11]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_213601.png
  [12]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-03_213727.png