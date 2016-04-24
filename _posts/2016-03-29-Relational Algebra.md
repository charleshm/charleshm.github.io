---
published: true
author: Charles
layout: post
title:  "关系代数"
date:   2016-03-29 9:30
categories: 数据挖掘 数据库
---

在关系代数的形式化语言中：

- 用表、或者数据集合表示**关系**或者**实体**
- 用行表示**元组**
- 用列表示**属性**

![此处输入图片的描述][1]

----------

**关系代数运算符**，

![此处输入图片的描述][2]

我们利用下表数据加以说明[^3]：

![此处输入图片的描述][3]

----------

#### 关系运算符[^1][^2]

- **选择运算**（$\sigma$）:返回满足指定条件的元组（**行**）。

$$\sigma_{press=\text{‘高等教育出版社’}}$$

![此处输入图片的描述][4]


----------


- **投影**（$\pi$）:返回指定的指定的属性（**列**）。

$$\pi_{\text{title,price}}(\text{Book})$$

![此处输入图片的描述][5]


----------


- **广义笛卡尔积**（$R\times S$）

$$R \times S = \{r \cup s| r \in R, s \in S\}$$

<div class="inline_list">
$R$： n 目关系， $k_1$ 个元组；    <br> 
$S$： m 目关系， $k_2$ 个元组；        <br> 
$R\times S$: $n+m$ 目关系， $k_1\times k_2$ 个元组；  
</div>

![此处输入图片的描述][6]

----------

- **连接**：连接运算是由一个**笛卡尔积运算**和一个**选取运算**构成的，它是从两个关系的笛卡尔积中选取属性间满足一定条件的元组。

<div class="inline_list">
首先用笛卡尔积完成对两个数据集合的乘运算，然后对生成的结果集合进行选取运算，确保只把分别来自两个数据集合并且具有重叠部分的行合并在一起。
</div>

> 连接的全部意义在于在水平方向上合并两个数据集合（通常是表），并产生一个新的结果集合，其方法是将一个数据源中的行于另一个数据源中和它匹配的行组合成一个新元组。

![此处输入图片的描述][7]

----------

- **除**：返回两个数据集之间的**精确匹配**

<div class="inline_list">
设有关系 $R(X，Y)$ 与关系 $S(Y，Z)$，其中 X，Y，Z 为属性集合，R 中的 Y 与 S 中的 Y 可以有不同的属性名，但对应属性必须出自<strong>相同的域</strong>。<br>
关系 $R\div S$ 所得的商是一个新关系 $P(X)$，P 是 R 中满足下列条件的元组在 X 上的<strong>投影</strong> ：元组在 X 上分量值 x 的象集 $Y_x$ 包含关系 S 在 Y 上投影的集合。
</div>

$$R\div S=\{t_r[X] | t_r\in R \wedge \pi_y(S) \subseteq Y_x\}$$

![此处输入图片的描述][13]

----------

#### 连接操作实战

![此处输入图片的描述][8]


----------


- **内连接**(INNER JOIN)：

{% highlight c++ %}
select Person.*,Works.* from Person inner join Works on Person.person-name =  Works.person-name
{% endhighlight %}


![此处输入图片的描述][9]


----------


- **左外连接**(LEFT  JOIN 或 LEFT OUTER JOIN)：

{% highlight c++ %}
select Person.*,Works.* from Person left join Works on Person.person-name =  Works.person-name
{% endhighlight %}

![此处输入图片的描述][10]


----------


- **右外连接**(RIGHT  JOIN 或 RIGHT  OUTER  JOIN)：

{% highlight c++ %}
select Person.*,Works.* from Person right join Works on Person.person-name =  Works.person-name
{% endhighlight %}


![此处输入图片的描述][11]


----------


- **全外连接**(FULL  JOIN 或 FULL OUTER JOIN)：

{% highlight c++ %}
select Person.*,Works.* from Person full join Works on Person.person-name =  Works.person-name
{% endhighlight %}

![此处输入图片的描述][12]

----------

[^1]: [数据库的外联和内联知识](http://www.360doc.com/content/11/0923/12/3589172_150598768.shtml)
[^2]: [深入理解SQL的四种连接-左外连接、右外连接、内连接、全连接](http://f.dataguru.cn/thread-477900-1-1.html)
[^3]: 数据库原理与应用(常玉慧，钱进，张俐)

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/database_2.png?imageView2/2/w/400
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/database_math.png?imageView2/2/w/300
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/database_1.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/database_3.png?imageView2/2/w/500
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/databse_4.png?imageView2/2/w/300
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/database_5.png?imageView2/2/w/300
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/database_7.png
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/connection_1.png?imageView2/2/w/450
  [9]: http://7xjbdi.com1.z0.glb.clouddn.com/connection_2.png?imageView2/2/w/400
  [10]: http://7xjbdi.com1.z0.glb.clouddn.com/connection_3.png?imageView2/2/w/400
  [11]: http://7xjbdi.com1.z0.glb.clouddn.com/connection_4.png?imageView2/2/w/400
  [12]: http://7xjbdi.com1.z0.glb.clouddn.com/connection_5.png?imageView2/2/w/400
  [13]: http://7xjbdi.com1.z0.glb.clouddn.com/connection_8.png?imageView2/2/w/300