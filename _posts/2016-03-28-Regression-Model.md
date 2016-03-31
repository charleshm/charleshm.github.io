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

$$Y = X\hat{\beta}+e$$

- **零均值**：$E(u_i\|X_i)=0$
- **同方差**：对于每一个 $X_i$，$u_i$的条件方差等于某个常数

![此处输入图片的描述][1]

- **无自相关**：

$$Var(u_i|X_i)=\sigma^2 \Rightarrow Var(Y_i|X_i)=\sigma^2$$

- **随机扰动项与解释变量不相关**
- **无多重共线性假定**

> 假定各解释变量之间不存在线性关系，或各个解释变量观测值之间线性无关。

- **正态性假定**

----------

#### OLS 估计

![此处输入图片的描述][2]

**统计特性**， 

![此处输入图片的描述][4]

**无偏估计**：估计量的数学期望等于被估计参数。

![此处输入图片的描述][3]

http://www.sssidea.org/ecn/chapter3.pdf

----------

#### 假设不满足

- **多重共线性**

> 存在多重共线性时，OLS估计式变得不确定或不精确.    
OLS估计式方差变得很大，标准误差增大.          
当多重共线性严重时，甚至可能使估计的回归系数 符号相反，得出完全错误的结论.        
区间估计和假设检验会出现错误.      






----------

[1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-30_223648.png?imageView2/2/w/400
[2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-31_092444.png
[3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-31_092931.png
[4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-31_093425.png