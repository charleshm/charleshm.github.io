---
published: true
author: Charles
layout: post
title:  "数据处理之pandas"
date:   2016-03-15 15:30
categories: 数据处理 Python
---

要掌握 pandas，首先得了解它的两大主要数据结构： Series 和 DataFrame .

#### Series
Series 是一种类似于一维数组的对象，它由一组数据以及与这组数据相对应的索引组成。

![此处输入图片的描述][1]

我们可以通过 Series 的 values 和 index 属性获取数组值和索引。

![此处输入图片的描述][2]

上面的索引是默认的，实际上我们可以进行自定义：

![此处输入图片的描述][3]


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/31102.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/31103.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/31104.png