---
published: true
author: Charles
layout: post
title:  "数据处理之pandas"
date:   2016-03-15 15:30
categories: 数据处理 Python
---

要掌握 pandas，首先得了解它的两大主要数据结构： Series 和 DataFrame .

边学边整理~

#### Series
Series 是一种类似于一维数组的对象，它由一组数据以及与这组数据相对应的索引组成。

![此处输入图片的描述][1]

我们可以通过 Series 的 values 和 index 属性获取**数组值**和**索引**。

![此处输入图片的描述][2]

上面的索引是默认的，实际上我们可以进行自定义：

![此处输入图片的描述][3]

可以通过索引的方式选择 Series 中的单个值或一组值。另外，我们可以将 Series 看成是一个定长的**有序字典**，因为它是索引值到数据值的一个映射。如果数据被存放在 Python 字典中，我们可以直接通过这个字典来创建 Series。

Series 对象和它的 index 都含有一个 name 属性：

<pre class="prettyprint linenums">
>>> s = Series(data=[1,3,5,7],index = ['a','b','x','y'])
>>> s
a    1
b    3
x    5
y    7
dtype: int64
>>> s.index
Index(['a', 'b', 'x', 'y'], dtype='object')
>>> s.values
array([1, 3, 5, 7], dtype=int64)
</pre>

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/31102.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/31103.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/31104.png