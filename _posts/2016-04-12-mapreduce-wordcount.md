---
published: true
author: Charles
layout: post
title:  "MapReduce之wordcount"
date:   2016-04-10 14:28
categories: 大数据
---

####  MapReduce编程模型
MapReduce是一种高度抽象的模型，它屏蔽了并行计算、容错、数据分布、负载均衡等复杂的细节，把处理过程高度抽象为两个过程：map 和 reduce。

> "分而治之"：我们会把任务划分为许多小的任务，map 对各个子任务进行处理，reduce 负责把 map 的输出的中间结果汇总起来。

![此处输入图片的描述][1]


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/optimized-ec2d.png