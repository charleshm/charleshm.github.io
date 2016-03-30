---
published: true
author: Charles
layout: post
title:  "回归分析"
date:   2016-03-28 14:30
categories: 机器学习 统计学
---

#### 基本假定

回归分析中对随机扰动项的假定，

$$Y_i = E(Y|X_i)+u_i$$

- **零均值**：$E(u_i\|X_i)=0$
- **同方差**：对于每一个 $X_i$，$u_i$的条件方差等于某个常数

$$Var(u_i|X_i)=\sigma^2 \Rightarrow Var(Y_i|X_i)=\sigma^2$$

![此处输入图片的描述][1]

----------

[1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-30_223648.png?imageView2/2/w/400