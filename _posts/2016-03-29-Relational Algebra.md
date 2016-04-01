---
published: true
author: Charles
layout: post
title:  "关系代数"
date:   2016-03-29 9:30
categories: 数据挖掘 数据库
---

在关系代数的形式化语言中：

- 用表、或者数据集合表示关系或者实体
- 用行表示**元组**
- 用列表示**属性**

![此处输入图片的描述][1]

![此处输入图片的描述][2]

关系代数包含以下8个关系运算符，我们利用下表数据加以说明：

![此处输入图片的描述][3]


----------

#### 关系运算符

- **选择运算**（$\sigma$）:返回满足指定条件的元组（**行**）。

$$\sigma_{press=\text{‘高等教育出版社’}}$$

![此处输入图片的描述][4]


----------


- **投影**（$\pi$）:返回指定的指定的属性（**列**）。

$$\pi_{\text{title,price}}(\text{Book})$$

![此处输入图片的描述][5]


----------


- **广义笛卡尔积**（$R\times S$）

$$R\times S=\{ <t_r,t_s> | t_r\in R \wedge t_s \in S \}$$

<div class="inline_list">
$R$： n 目关系， $k_1$ 个元组；<br>     
$S$： m 目关系， $k_2$ 个元组；    <br>     
$R\times S$: $n+m$ 目关系， $k_1\times k_2$ 个元组；  
</div>

![此处输入图片的描述][6]

----------

- **连接**：从两个关系的笛卡尔积中选取属性间满足一定条件的元组。

<div class="inline_list">
连接运算是由一个**笛卡尔积运算**和一个**选取运算**构成的，首先用笛卡尔积完成对两个数据集合的乘运算，然后对生成的结果集合进行选取运算，确保只把分别来自两个数据集合并且具有重叠部分的行合并在一起。<br>
</div>


> 连接的全部意义在于在水平方向上合并两个数据集合（通常是表），并产生一个新的结果集合，其方法是将一个数据源中的行于另一个数据源中和它匹配的行组合成一个新元组。



----------


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/database_2.png?imageView2/2/w/400
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/database_math.png?imageView2/2/w/400
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/database_1.png?imageView2/2/w/400
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/database_3.png?imageView2/2/w/400
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/databse_4.png?imageView2/2/w/250
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/database_5.png?imageView2/2/w/300