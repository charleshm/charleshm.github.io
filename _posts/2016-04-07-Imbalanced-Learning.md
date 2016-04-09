---
published: true
author: Charles
layout: post
title:  "Imbalanced Learning"
date:   2016-04-07 15:30
categories: 机器学习
---

#### Informed Undersampling
Informed undersampling 采样技术主要包含两种方法：**EasyEnsemble 和 BalanceCascade**。

**EasyEnsemble** 类似于 Bagging 方法，它把数据划分为两部分，分别是多数类样本和少数类样本，对于多数类样本 $S_{maj}$，通过 n 次有放回抽样生成 n 个子集，少数类样本分别和这 n 个子集合并，得到 n 个训练集，训练出 n 个模型，最终的输出是这 n 个模型预测结果的平均值。

![此处输入图片的描述][1]

**BalanceCascade** 类似于 Boosting 方法，先从多数类样本中采样得到 $E$，使得 $\|E\| = \|S_{min}\|$，利用训练集 $N = E \cup S_{min}$，训练出第一个模型，找出 $E$ 中分类正确的样本 $$N^{*}_{maj}$$， 然后从 $S_{maj}$ 中**去除**，再进行采样，重复上面的步骤，依次迭代直到满足某一停止条件，最终的模型是多次迭代模型的组合。

![此处输入图片的描述][2]


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/optimized-mahg.png?imageView2/2/w/400
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-04-09_191514.png?imageView2/2/w/400