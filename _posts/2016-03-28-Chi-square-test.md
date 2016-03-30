---
published: true
author: Charles
layout: post
title:  "卡方检验"
date:   2016-03-27 9:30
categories: 机器学习 统计学
---

卡方检验(Pearson’s Chi-square test)主要用来做两件事：

- **适配度检验**（Test for Goodness of fit），检查样本是否符合某种随机分布；
- **独立性检验**（Test for Independence），检查变量之间是否独立。

我们前面提到过，各种检验方法的差别在于计算的**检验统计量**不同。我们先来关注下 $\chi^2$ 统计量，