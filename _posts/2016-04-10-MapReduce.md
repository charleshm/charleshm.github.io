---
published: true
author: Charles
layout: post
title:  "MapReduce详解"
date:   2016-04-10 14:28
categories: 大数据
---

### MapReduce的体系结构
Map/Reduce计算集群由一个单独的 JobTracker（master） 和每个集群节点一个 TaskTracker（slave）共同组成。JobTracker负责调度构成一个作业的所有任务，这些任务会被分派到不同的 TaskTracker 上去执行，JobTracker 会监控它们的执行情况。而 TaskTracker 仅负责执行由 JobTracker 指派的任务。

![此处输入图片的描述][1]


----------

### MapReduce各个执行阶段[^1]

![此处输入图片的描述][2]


----------


#### 分片(split)
HDFS 以固定大小的block 为基本单位存储数据，而对于MapReduce 而言，其处理单位是split。split 是一个逻辑概念，它只包含一些元数据信息，比如数据起始位置、数据长度、数据所在节点等。它的划分方法完全由用户自己决定。

![此处输入图片的描述][3]

> Hadoop会为每个 split 创建一个Map任务，split 的多少决定了 Map 任务的数目。大多数情况下，理想的分片大小是一个 HDFS 块。最优的 Reduce 任务个数取决于集群中可用的 reduce 任务槽(**slot**)的数目。


----------

#### Map

![此处输入图片的描述][4]

----------


### Shuffle
http://www.aboutyun.com/thread-8927-1-1.html

#### 为什么要Shuffle[^2]
在Hadoop这样的集群环境中，大部分map task 与 reduce task 的执行是在不同的节点上。很多情况下 Reduce 执行时需要跨节点去拉取其它节点上的 map task 的输出结果。如果集群正在运行的job有很多，那么 task 对集群内部的网络资源消耗会很严重，我们希望能最大化地减少不必要的消耗。

我们希望　Shuffle 过程可以帮我们做到： 

- 完整地从 map task 端拉取数据到 reduce 端。
- 在跨节点拉取数据时，尽可能地减少对带宽的不必要消耗。
- 减少磁盘 IO 对 task 执行的影响。

----------

#### Shuffle过程


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/hadoop_job.png?imageView2/2/w/450
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/map_shuffle_reduce.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/mapreduce_spilt.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/hadoop_MapReduceWordCountOverview1.png
  
  
  [^1]: http://blog.csdn.net/u014374284/article/details/49205885
  [^2]: http://langyu.iteye.com/blog/992916