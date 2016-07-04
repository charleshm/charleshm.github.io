---
published: true
author: Charles
layout: post
title:  "机器学习性能评估指标"
date:   2016-03-20 9:30
categories: 机器学习
---

![此处输入图片的描述][5]

### 分类

#### 混淆矩阵[^1]
- True Positive(真正, TP)：将正类预测为正类数.
- True Negative(真负 , TN)：将负类预测为负类数.
- False Positive(假正, FP)：将负类预测为正类数 $\rightarrow$ **误报** (Type I error).
- False Negative(假负 , FN)：将正类预测为负类数 $\rightarrow$ **漏报** (Type II error).


![此处输入图片的描述][1]

**精确率**(precision)定义为：

$$P = \frac{TP}{TP+FP} \tag{1}$$

需要注意的是**精确率**(precision)和**准确率**(accuracy)是不一样的，

$$ACC = \frac{TP + TN}{TP+TN+FP+FN}$$

> 在正负样本不平衡的情况下，准确率这个评价指标有很大的缺陷。比如在互联网广告里面，点击的数量是很少的，一般只有千分之几，如果用acc，即使全部预测成负类（不点击）acc 也有 99% 以上，没有意义。

**召回率**(recall,sensitivity,true positive rate)定义为：

$$R = \frac{TP}{TP+FN} \tag{2}$$

此外，还有 $F_1$ 值，是精确率和召回率的**调和均值**，

$$\begin{align*}
\frac{2}{F_1} & = \frac{1}{P} + \frac{1}{R}\\
F_1 & = \frac{2TP}{2TP + FP + FN} \tag{3}
\end{align*}$$

> 精确率和准确率都高的情况下，$F_1$ 值也会高。

----------

#### 通俗版本
刚开始接触这两个概念的时候总搞混，时间一长就记不清了。

实际上非常简单，**精确率**是针对我们**预测结果**而言的，它表示的是预测为正的样本中有多少是对的。那么预测为正就有两种可能了，一种就是把正类预测为正类(TP)，另一种就是把负类预测为正类(FP)。

而**召回率**是针对我们原来的**样本**而言的，它表示的是样本中的正例有多少被预测正确了。那也有两种可能，一种是把原来的正类预测成正类(TP)，另一种就是把原来的正类预测为负类(FN)。

![此处输入图片的描述][2]

----------

在信息检索领域，精确率和召回率又被称为**查准率**和**查全率**，

$$\begin{align*}
\text{查准率}&＝\frac{\text{检索出的相关信息量}}{\text{检索出的信息总量}}\\
\text{查全率}&＝\frac{\text{检索出的相关信息量}}{\text{系统中的相关信息总量}}
\end{align*}$$

----------

#### ROC 曲线

我们先来看下维基百科的定义，

> In signal detection theory, a receiver operating characteristic (ROC), or simply ROC curve, is a graphical plot which illustrates the performance of a binary classifier system **as its discrimination threshold is varied**.

比如在逻辑回归里面，我们会设一个阈值，大于这个值的为正类，小于这个值为负类。如果我们减小这个阀值，那么更多的样本会被识别为正类。这会提高正类的识别率，但同时也会使得更多的负类被错误识别为正类。为了形象化这一变化，在此引入 ROC ，ROC 曲线可以用于评价一个分类器好坏。

ROC 关注两个指标，

$$\begin{align*}
\text{true positive rate}: \qquad &TPR = \cfrac{TP}{TP+FN}\\      
\text{false positive rate}: \qquad &FPR = \cfrac{FP}{FP + TN}
\end{align*}$$

直观上，TPR 代表能将正例分对的概率，FPR 代表将负例错分为正例的概率。在 ROC 空间中，每个点的横坐标是 FPR，纵坐标是 TPR，这也就描绘了分类器在 TP（**真正率**）和 FP（**假正率**）间的 trade-off[^3]。

![此处输入图片的描述][3]

----------

#### AUC
AUC（Area Under Curve）被定义为ROC曲线下的面积，显然这个面积的数值不会大于1。

> The AUC value is equivalent to the probability that a randomly chosen positive example is ranked higher than a randomly chosen negative example.      

翻译过来就是，随机挑选一个正样本以及一个负样本，分类器判定正样本的值高于负样本的概率就是 AUC 值。

简单说：AUC值越大的分类器，正确率越高[^2]。

- $AUC = 1$，**完美分类器**，采用这个预测模型时，不管设定什么阈值都能得出完美预测。绝大多数预测的场合，不存在完美分类器。
- $0.5 < AUC < 1$，优于**随机猜测**。这个分类器（模型）妥善设定阈值的话，能有预测价值。
- $AUC = 0.5$，跟**随机猜测一样**（例：丢铜板），模型没有预测价值。
- $AUC < 0.5$，比**随机猜测**还差；但只要总是反预测而行，就优于随机猜测，因此不存在 $AUC < 0.5$ 的情况。

![此处输入图片的描述][4]

> 既然已经这么多评价标准，为什么还要使用ROC和AUC呢？因为ROC曲线有个很好的特性：当测试集中的正负样本的分布变化的时候，ROC曲线能够保持不变。在实际的数据集中经常会出现类不平衡（class imbalance）现象，即负样本比正样本多很多（或者相反）

----------

### 回归[^4]

#### 平均绝对误差
平均绝对误差MAE（Mean Absolute Error）又被称为 $l_1$ 范数损失（l1-norm loss）：

$${\rm MAE}(y, \hat{y})=\frac{1}{n_{\rm samples}}\sum\limits_{i=1}^{n_{\rm samples}}|y_i-\hat{y}_i|$$

----------

#### 平均平方误差
平均平方误差 MSE（Mean Squared Error）又被称为 $l_2$ 范数损失（l2-norm loss）：

$${\rm MSE}(y, \hat{y})=\frac{1}{n_{\rm samples}}\sum\limits_{i=1}^{n_{\rm samples}}(y_i-\hat{y}_i)^2$$

[1]: http://7xjbdi.com1.z0.glb.clouddn.com/confusion_matrix%20(1).png
[2]: http://7xjbdi.com1.z0.glb.clouddn.com/Precision_Recall.png?imageView2/2/w/400
[3]: http://7xjbdi.com1.z0.glb.clouddn.com/ROC.png
[4]: http://7xjbdi.com1.z0.glb.clouddn.com/f03add592a75ef5b5e7346a5209b0cb8.png
[5]: http://7xjbdi.com1.z0.glb.clouddn.com/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E6%80%A7%E8%83%BD%E8%AF%84%E4%BC%B0.png

----------

[^1]: 统计学习方法
[^2]: [AUC与ROC - 衡量分类器的好坏](http://beader.me/2013/12/15/auc-roc/)
[^3]: [ROC和AUC介绍以及如何计算AUC](http://alexkong.net/2013/06/introduction-to-auc-and-roc/)
[^4]: [机器学习评价指标大汇总](http://www.zhaokv.com/2016/03/ml-metric.html)
