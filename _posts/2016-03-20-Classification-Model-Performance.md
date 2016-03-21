---
published: true
author: Charles
layout: post
title:  "分类器性能评估指标"
date:   2016-03-20 9:30
categories: 机器学习
---

#### 混淆矩阵
- True Positive(真正, TP)：将正类预测为正类数 $\rightarrow$ **正确率**.
- True Negative(真负 , TN)：将负类预测为负类数$\rightarrow$ **正确率**.
- False Positive(假正, FP)：将负类预测为正类数 $\rightarrow$ **误报率** (Type I error).
- False Negative(假负 , FN)：将正类预测为负类数 $\rightarrow$ **漏报率** (Type II error).


![此处输入图片的描述][1]

精确率定义为：

$$P = \frac{TP}{TP+FP}$$

召回率定义为：

$$R = \frac{TP}{TP+FN}$$

刚开始接触这两个概念的时候总搞混，时间一长就记不清了。

实际上非常简单，精确率是针对我们**预测结果**而言的，它表示的是预测为正的样本中有多少是对的。那么预测为正就有两种可能了，一种就是把正类预测为正类(TP)，另一种就是把负类预测为正类(FP)。

而召回率是针对我们原来的**样本**而言的，它表示的是样本中的正例有多少被预测正确了。那也有两种可能，一种是把正类预测成正类(TN)，另一种就是把正类预测为负类(FN)。

![此处输入图片的描述][2]


[1]: http://7xjbdi.com1.z0.glb.clouddn.com/confusion_matrix%20(1).png
[2]: http://7xjbdi.com1.z0.glb.clouddn.com/Precisionrecall.svg.png