---
published: true
author: Charles
layout: post
title:  "Hadoop学习之MapReduce"
date:   2016-04-10 14:28
categories: 大数据
---

#### 体系结构
Map/Reduce计算集群由一个单独的 JobTracker（master，NameNode） 和每个集群节点一个 TaskTracker（slave，DataNode）共同组成。JobTracker负责调度构成一个作业的所有任务，这些任务会被分派到不同的 TaskTracker 上去执行，JobTracker 会监控它们的执行情况。而 TaskTracker 仅负责执行由 JobTracker 指派的任务。

![此处输入图片的描述][1]


----------

#### 执行过程[^1]

![此处输入图片的描述][2]


----------


#### 分片(split)
HDFS 以固定大小的block 为基本单位存储数据，而对于MapReduce 而言，其处理单位是split。split 是一个逻辑概念，它只包含一些元数据信息，比如数据起始位置、数据长度、数据所在节点等。它的划分方法完全由用户自己决定。

![此处输入图片的描述][3]

> Hadoop会为每个 split 创建一个Map任务，split 的多少决定了 Map 任务的数目。大多数情况下，理想的分片大小是一个 HDFS 块。最优的 Reduce 任务个数取决于集群中可用的 reduce 任务槽(**slot**)的数目。

注：map/reduce 任务槽(slot) 就是集群能够同时运行的map/reduce任务的最大数量。

----------

#### Map

Mapper将输入键值对(key/value pair)映射到一组中间格式的键值对集合。

![此处输入图片的描述][4]

> 注意，map的输出结果最后是写入本地的磁盘中，并不是写到HDFS，因为map的输出在job完成后即可删除，没必要写到HDFS上。

----------


### Shuffle

#### 为什么要Shuffle[^2]
在Hadoop这样的集群环境中，大部分map task 与 reduce task 的执行是在不同的节点上。很多情况下 Reduce 执行时需要跨节点去拉取其它节点上的 map task 的输出结果。如果集群正在运行的job有很多，那么 task 对集群内部的网络资源消耗会很严重，我们希望能最大化地减少不必要的消耗。

我们希望　Shuffle 过程可以帮我们做到： 

- 完整地从 map task 端拉取数据到 reduce 端。
- 在跨节点拉取数据时，尽可能地减少对带宽的不必要消耗。
- 减少磁盘 IO 对 task 执行的影响。

----------

#### Shuffle过程

<p class="first">Partition</p>

> 对于map输出的每一个键值对，系统都会给定一个partition，partition值默认是通过计算key的hash值后对Reduce task的数量取模获得。如果一个键值对的partition值为1，意味着这个键值对会交给第一个Reducer处理[^3]。

{% highlight java %}
reducer=(key.hashCode() & Integer.MAX_VALUE) % numReduceTasks
{% endhighlight %}

<p class="first">Collector</p>

然后将数据写入内存环形缓冲区中，缓冲区的作用是批量收集map结果，减少磁盘IO的影响。key/value对以及 Partition 的结果都会被写入**缓冲区**。写入之前，key 与value 值会被序列化成字节数组(Kvbuffer)。

----------

<p class="first">spill触发</p>

由于内存缓冲区的大小限制（默认100MB），当map task输出结果很多时就可能发生内存溢出，所以需要在一定条件下将缓冲区的数据临时写入磁盘，然后重新利用这块缓冲区。这个从内存往磁盘写数据的过程被称为Spill，中文可译为溢写。这个溢写是由另外单独线程来完成，不影响往缓冲区写map结果的collector线程。

整个缓冲区有个溢写的比例spill.percent。这个比例默认是0.8，也就是当缓冲区的数据已经达到阈值（buffersize * spill percent = 100MB * 0.8 = 80MB），溢写线程启动，锁定这80MB的内存，执行溢写过程。Maptask的输出结果还可以往剩下的20MB内存中写，互不影响。 

![此处输入图片的描述][5]


----------

<p class="first">Sort And Spill</p>

当Spill触发后，Sort And Spill先把Kvbuffer中的数据按照partition值和key两个关键字升序排序，移动的只是索引数据，排序结果是Kvmeta中数据按照partition为单位聚集在一起，同一partition内的按照key有序排列。


----------

<p class="first">Combine And Spill</p>

Combiner 将有相同key的 key/value 对加起来，减少溢写spill到磁盘的数据量。

Spill线程为这次Spill过程创建一个磁盘文件：从所有的本地目录中轮训查找能存储这么大空间的目录，找到之后在其中创建一个类似于“spill12.out”的文件。Spill线程根据排过序的Kvmeta挨个partition的把数据吐到这个文件中。

在这个过程中如果用户配置了combiner类，那么在写之前会先调用combineAndSpill()，对结果进行进一步合并后再写出。Combiner会优化MapReduce的中间结果，所以它在整个模型中会多次使用。

![此处输入图片的描述][6]

----------

#### MapReduce任务的优化


----------
  
  [^1]: [MapReduce shuffle过程详解](http://blog.csdn.net/u014374284/article/details/49205885)
  [^2]: [MapReduce:详解Shuffle过程](http://langyu.iteye.com/blog/992916)
  [^3]: [Hadoop Map/Reduce 执行流程详解](http://zheming.wang/hadoop-mapreduce-zhi-xing-liu-cheng-xiang-jie.html)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/hadoop_job.png?imageView2/2/w/450
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/map_shuffle_reduce.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/mapreduce_spilt.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/word-count-as-mapreduce.png
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/hadoop_map_unused.png
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/hadoop_002.png