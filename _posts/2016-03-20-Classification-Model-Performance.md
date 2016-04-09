---
published: true
author: Charles
layout: post
title:  "分类器性能评估指标"
date:   2016-03-20 9:30
categories: 机器学习
---

#### 混淆矩阵[^1]
- True Positive(真正, TP)：将正类预测为正类数.
- True Negative(真负 , TN)：将负类预测为负类数.
- False Positive(假正, FP)：将负类预测为正类数 $\rightarrow$ **误报** (Type I error).
- False Negative(假负 , FN)：将正类预测为负类数 $\rightarrow$ **漏报** (Type II error).


![此处输入图片的描述][1]

精确率(precision)定义为：

$$P = \frac{TP}{TP+FP} \tag{1}$$

召回率(recall,sensitivity,true positive rate)定义为：

$$R = \frac{TP}{TP+FN} \tag{2}$$

此外，还有 $F_1$ 值，是精确率和召回率的调和均值，

$$\begin{align*}
\frac{2}{F_1} & = \frac{1}{P} + \frac{1}{R}\\
F_1 & = \frac{2TP}{2TP + FP + FN} \tag{3}
\end{align*}$$

----------

#### 通俗版本
刚开始接触这两个概念的时候总搞混，时间一长就记不清了。

实际上非常简单，**精确率**是针对我们**预测结果**而言的，它表示的是预测为正的样本中有多少是对的。那么预测为正就有两种可能了，一种就是把正类预测为正类(TP)，另一种就是把负类预测为正类(FP)。

而**召回率**是针对我们原来的**样本**而言的，它表示的是样本中的正例有多少被预测正确了。那也有两种可能，一种是把原来的正类预测成正类(TN)，另一种就是把原来的正类预测为负类(FN)。

![此处输入图片的描述][2]

----------

#### ROC 曲线

我们先来看下维基百科的定义，

> In signal detection theory, a receiver operating characteristic (ROC), or simply ROC curve, is a graphical plot which illustrates the performance of a binary classifier system **as its discrimination threshold is varied**.

比如在逻辑回归里面，我们会设一个阈值，大于这个值的为正类，小于这个值为负类。如果我们减小这个阀值，那么更多的样本会被识别为正类。这会提高正类的识别率，但同时也会使得更多的负类被错误识别为正类。为了形象化这一变化，在此引入 ROC ，ROC 曲线可以用于评价一个分类器好坏。

ROC 关注两个指标，

true positive rate： ($TPR = \frac{TP}{TP+FN}$)      
false positive rate： ($TPR = \frac{FPR}{FP + TN}$)      

直观上，TPR代表能将正例分对的概率，FPR代表将负例错分为正例的概率。在 ROC 空间中，每个点的横坐标是 FPR，纵坐标是 TPR，这也就描绘了分类器在 TP（真正率）和 FP（假正率）间的 trade-off。

![此处输入图片的描述][3]

#### AUC
为啥要用AUC呢？传统的ACC（准确度）为啥不用呢？

在互联网广告里面，点击的数量是很少的，一般千分之几，如果用acc，全部预测成负类（不点击）acc也有99%以上，根本无法评价。

[1]: http://7xjbdi.com1.z0.glb.clouddn.com/confusion_matrix%20(1).png
[2]: http://7xjbdi.com1.z0.glb.clouddn.com/Precision_Recall.png?imageView2/2/w/400
[3]: http://7xjbdi.com1.z0.glb.clouddn.com/ROC.png

----------

[^1]: 统计学习方法