---
published: true
author: Charles
layout: post
title:  "数据挖掘笔试准备"
date:   2016-03-17 9:30
categories: 数据挖掘
---

关联规则反映一个事物与其它事物之间的相互依存性和关联性。如果两个或者多个事物之间存在一定的关联关系，那么，其中一个事物发生就能够预测与它相关联的其它事物的发生。比如经典的购物篮事务(market basket transaction)，通过顾客放入购物篮中商品之间的**同现关系**(co-occurrence)来分析顾客的购买习惯，实现商品的交叉销售和推荐。 

![此处输入图片的描述][1]

从上面的数据中我们可以提取如下的规则：

$$\{ \text{尿布} \} \rightarrow \{ \text{啤酒} \} $$

那我们如何提取出这样的关联规则呢？

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-21_195111.png