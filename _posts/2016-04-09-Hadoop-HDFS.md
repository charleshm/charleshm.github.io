---
published: true
author: Charles
layout: post
title:  "Hadoop学习之HDFS"
date:   2016-04-09 14:28
categories: 大数据
---

![此处输入图片的描述][1]

#### 分布式文件系统

当数据集的大小超过一台独立物理计算机的存储能力时，就有必要对它进行分区(partition)并存储到若干台单独的计算机上。

管理网络中跨多台计算机存储的文件系统称为分布式文件系统(distributed file system)。该系统架构于网络之上，势必会引入网络编程的复杂性，因此分布式文件系统比普通磁盘文件系统更为复杂。例如，使文件系统能够容忍节点故障且不丢失任何数据，就是一个极大的挑战[^1]。

![此处输入图片的描述][2]

----------


#### HDFS设计目标

- 兼容廉价的硬件设备
- 流数据读写
- 大规模数据集
- 简单的一致性模型：一次写入、多次读写（设计理念）
- 强大的跨平台兼容性


----------


#### HDFS缺点
HDFS特殊的设计，在实现上述优良特性的同时，也使得自身具有一些应用局限性，

- 不适合低延迟数据访问
- 无法高效存储大量小文件
- 不支持多用户写入及任意修改文件


----------


<p class="first">不适合低延迟数据访问</p>

如果要处理一些用户要求时间比较短的低延迟应用请求，则HDFS不适合。HDFS是为了处理大型数据集分析任务的，主要是为达到高的数据吞吐量而设计的，这就可能要求以高延迟作为代价。

**改进策略**：

- 对于那些有低延时要求的应用程序，HBase是一个更好的选择，它的口号就是 goes real time。
- 使用缓存或多master设计可以降低client的数据请求压力，减小延时。
- 对HDFS系统内部改进，权衡大吞吐量与低延时。


----------


<p class="first">无法高效存储大量小文件</p>

因为Namenode把文件系统的元数据放置在内存中，所以文件系统所能容纳的文件数目是由Namenode的内存大小来决定。一般来说，每一个文件、 文件夹和Block需要占据150字节左右的空间，所以，如果你有100万个文件，每一个占据一个Block，你就至少需要300MB内存。当前来说，数 百万的文件还是可行的，当扩展到数十亿时，对于当前的硬件水平来说就没法实现了。

还有一个问题就 是，因为Map task的数量是由splits来决定的，所以用MapReduce处理大量的小文件时，就会产生过多的Map task，线程管理开销将会增加作业时间。举个例子，处理10000M的文件，若每个split为1M，那就会有10000个Map tasks，会有很大的线程开销；若每个split为100M，则只有100个Map tasks，每个Map task将会有更多的事情做，而线程的管理开销也将减小很多。

**改进策略**：

- 利用SequenceFile、MapFile、Har等方式归档小文件，这个方法的原理就是把小文件归档起来管理，HBase就是基于此的。
- 横向扩展，一个Hadoop集群能管理的小文件有限，那就把几个Hadoop集群拖在一个虚拟服务器后面，形成一个大的Hadoop集群。google也是这么干过的。
- 多Master设计，有一些Master的Block大小改为1M，调优处理小文件。

----------


<p class="first">不支持多用户写入及任意修改文件</p>

> 在 HDFS 的一个文件中只有一个写入者，而且写操作只能在文件末尾完成，即只能执行追加操作。目前 HDFS 还不支持多个用户对同一文件的写操作，以及在文件任意位置进行修改。

----------

#### HDFS结构

分布式文件系统在物理结构上是由计算机集群中的多个节点构成的，这些节点分为两类，一类叫“主节点”(Master Node)或者也被称为“名称结点”(NameNode)，另一类叫“从节点”（Slave Node）或者也被称为“数据节点”(DataNode).

![此处输入图片的描述][3]


----------

#### 基本概念

<p class="first">块(block)</p>

HDFS默认一个块64MB，一个文件被分成多个块，以块作为存储单位，块的大小远远大于普通文件系统，可以最小化寻址开销。

HDFS采用抽象的块概念可以带来以下几个明显的好处：

- **支持大规模文件存储**：文件以块为单位进行存储，一个大规模文件可以被分拆成若干个文件块，不同的文件块可以被分发到不同的节点上，因此，一个文件的大小不会受到单个节点的存储容量的限制，可以远远大于网络中任意节点的存储容量
- **简化系统设计**：首先，大大简化了存储管理，因为文件块大小是固定的，这样就可以很容易计算出一个节点可以存储多少文件块；其次，方便了元数据的管理，元数据不需要和文件块一起存储，可以由其他系统负责管理元数据
- **适合数据备份**：每个文件块都可以冗余存储到多个节点上，大大提高了系统的容错性和可用性


----------

<p class="first">namenode</p>

在HDFS中，名称节点（NameNode）负责管理分布式文件系统的命名空间（Namespace），保存了两个核心的数据结构，即 FsImage 和 EditLog：

- FsImage用于维护文件系统树以及文件树中所有的文件和文件夹的元数据
- 操作日志文件EditLog中记录了所有针对文件的创建、删除、重命名等操作

NameNode被格式化之后，将产生所示的目录结构[^2]：

{% highlight Bash %}
${dfs.name.dir}/current/VERSION
                /edits
                /fsimage
                /fstime
{% endhighlight %}

名称节点记录了每个文件中各个块所在的数据节点的位置信息

![此处输入图片的描述][4]

----------


<p class="first">FsImage</p>

FsImage文件包含文件系统中所有目录和文件inode的序列化形式。每个inode是一个文件或目录的元数据的内部表示，并包含此类信息：文件的复制等级、修改和访问时间、访问权限、块大小以及组成文件的块。对于目录，则存储修改时间、权限和配额元数据

> FsImage文件**没有记录块存储在哪个数据节点**。NameNode会在每次启动系统时动态重建这些信息，当数据节点加入HDFS集群时，数据节点会把自己所包含的块列表告知给名称节点，此后会定期执行这种告知操作，以确保名称节点的块映射是最新的。


----------


<p class="first">EditLog</p>

在名称节点启动的时候，它会将FsImage文件中的内容**加载到内存中**，之后再执行EditLog文件中的**各项操作**，使得内存中的元数据和实际的同步。一旦在内存中成功建立文件系统元数据的映射，则创建一个新的FsImage文件和一个空的EditLog文件

名称节点运行起来之后，HDFS中的更新操作会重新写到EditLog文件中，因为FsImage文件一般都很大（GB级别的很常见），如果所有的更新操作都往FsImage文件中添加，这样会导致系统运行的十分缓慢，但是，如果往EditLog文件里面写就不会这样，因为EditLog 要小很多。

由于HDFS的所有更新操作都是直接写到EditLog中，久而久之， EditLog文件将会变得**很大**。

> 虽然这对名称节点运行时候是没有什么明显影响的，但是，当名称节点重启的时候，名称节点需要先将 FsImage 里面的所有内容映像到内存中，然后再一条一条地执行 EditLog 中的记录，当 EditLog 文件非常大的时候，会导致名称节点启动操作非常慢，而在这段时间内 HDFS 系统处于安全模式，一直无法对外提供写操作，影响了用户的使用。


----------

<p class="first">Secondary NameNode</p>

第二名称节点（冷备分）HDFS架构中的一个组成部分，它是用来保存名称节点中对HDFS 元数据信息的备份，并减少名称节点重启的时间。SecondaryNameNode一般是单独运行在一台机器上。

{% highlight Bash %}
${fs.checkpoint.dir}/current/VERSION
         /edits
         /fsimage
         /fstime
         /previouts.checkpoint/VERSION
                /edits
                /fsimage
                /fstime
{% endhighlight %}

**Secondary NameNode的工作情况**：

 1. Secondary NameNode会定期和NameNode通信，请求其停止使用EditLog文件，暂时将新的写操作写到一个新的文件edit.new上来，这个操作是瞬间完成，上层写日志的函数完全感觉不到差别；
 2. Secondary NameNode通过HTTP GET方式从NameNode上获取到FsImage和EditLog文件，并下载到本地的相应目录下；
 3. Secondary NameNode将下载下来的FsImage载入到内存，然后一条一条地执行EditLog文件中的各项更新操作，使得内存中的FsImage保持最新；这个过程就是EditLog和FsImage文件合并；
 4. Secondary NameNode执行完（3）操作之后，会通过post方式将新的FsImage文件发送到NameNode节点上
 5. NameNode将从Secondary NameNode接收到的新的FsImage替换旧的FsImage文件，同时将edit.new替换EditLog文件，通过这个过程EditLog就变小了

![此处输入图片的描述][5]

----------

<p class="first">DataNode</p>

数据节点是分布式文件系统HDFS的工作节点，负责数据的**存储**和**读取**，会根据客户端或者是名称节点的调度来进行数据的存储和检索，同时，它会通过心跳(heartbeat)定时向NameNode发送所存储的文件块信息。

![此处输入图片的描述][6]

和NameNode不同，DataNode的存储目录是启动时自动创建的，不需要额外格式化，DataNode的关键文件和目录如下：

{% highlight Bash %}
${dfs.data.dir}/current/VERSION
           /blk_<id_1>
           /blk_<id_1>.meta
           /blk_<id_2>
           /blk_<id_2>.meta
           /......
           /blk_<id_64>
           /blk_<id_64>.meta
           /subdir0/
           /subdir1/
           /......
           /subdir63/
{% endhighlight %}

----------

<p class="first">通俗理解</p>

- **NameNode**：是Master节点，是大领导。管理数据块映射;处理客户端的读写请求，配置副本策略，管理HDFS的名称空间;
- **Secondary NameNode**：是一个小弟，分担大哥Namenode的工作量;是NameNode的冷备份，合并Fsimage和edits然后再发给namenode。
- **DataNode**：Slave节点，奴隶，干活的。负责存储client发来的数据块block，执行数据块的读写操作。
- **热备份**：b是a的热备份，如果a坏掉。那么b马上运行代替a的工作。
- **冷备份**：b是a的冷备份，如果a坏掉。那么b不能马上代替a工作。但是b上存储a的一些信息，减少a坏掉之后的损失。
- **Fsimage**:元数据镜像文件(文件系统的目录树。)
- **edits**：元数据的操作日志(针对文件系统做的修改操作记录)
- namenode内存中存储的是 fsimage + edits。
- Secondary NameNode负责定时默认1小时，从namenode上获取fsimage和edits来进行合并，然后再发送给namenode，减少namenode的工作量。

----------

#### HDFS存储原理

<p class="first">冗余数据保存</p>

为了保证系统的容错性和可用性，HDFS采用了多副本方式对数据进行冗余存储，通常一个数据块的多个副本会被分布到不同的数据节点上，这种多副本方式具有以下几个优点：

- 加快数据传输速度
- 容易检查数据错误
- 保证数据可靠性

![此处输入图片的描述][7]

----------

<p class="first">数据错误与恢复</p>

HDFS具有较高的容错性，可以兼容廉价的硬件，它把硬件出错看作一种常态，而不是异常，并设计了相应的机制检测数据错误和进行自动恢复，主要包括以下几种情形：名称节点出错、数据节点出错和数据出错。


----------


- **名称节点出错**

名称节点保存了所有的元数据信息，其中，最核心的两大数据结构是FsImage和Editlog，如果这两个文件发生损坏，那么整个HDFS实例将失效。因此，HDFS设置了备份机制，把这些核心文件同步复制到备份服务器SecondaryNameNode上。当名称节点出错时，就可以根据备份服务器SecondaryNameNode中的FsImage和Editlog数据进行恢复。


----------


- **数据节点出错**

每个数据节点会定期向名称节点发送“心跳”信息，向名称节点报告自己的状态。当数据节点发生故障，或者网络发生断网时，名称节点就无法收到来自一些数据节点的心跳信息，这时，这些数据节点就会被标记为“**宕机**”，节点上面的所有数据都会被标记为“**不可读**”，名称节点不会再给它们发送任何I/O请求。

这时，有可能出现一种情形，即由于一些数据节点的不可用，会导致一些数据块的副本数量小于冗余因子
名称节点会定期检查这种情况，一旦发现某个数据块的副本数量小于冗余因子，就会启**动数据冗余复制**，为它生成新的**副本**。

> HDFS和其它分布式文件系统的最大区别就是可以调整冗余数据的位置。


----------


- **数据出错**

网络传输和磁盘错误等因素，都会造成数据错误，客户端在读取到数据后，会采用md5和sha1对数据块进行校验，以确定读取到正确的数据。

在文件被创建时，客户端就会对每一个文件块进行**信息摘录**，并把这些信息写入到同一个路径的隐藏文件里面
当客户端读取文件的时候，会先读取该信息文件，然后，利用该信息文件对每个读取的数据块进行校验，如果校验出错，客户端就会请求到另外一个数据节点读取该文件块，并且向名称节点报告这个文件块有错误，名称节点会定期检查并且重新复制这个块。


----------

#### HDFS数据读写过程[^3]

<p class="first">读取数据</p>

![此处输入图片的描述][8]


----------


1. 客户端调用 FileSystem 的 get( ) 方法得到一个实例 fs (即分布式文件系统 Distributed FileSystem),然后 fs 调用 open( ) 打开希望读取的文件。
2. Distributed FileSystem( fs )通过使用 **RPC** 调用 NameNode 以确定文件起始 block 的位置。（Block位置信息存储在namenode的内存中）。对于每一个 bolck ，NameNode 返回 block 所有复本的 DataNode 地址（并根据与 client 的距离排序）。
3. DistributedFileSystem( fs ).open( )返回一个 FSDataInputStream 对象给 client 用来读数据。FSDataInputStream 封装了分布式文件输入流( DFSInputStream )用于管理 NameNode 和 DataNode 的 I/O.client 调用对这个输入流调用 read( ) 方法。
4. 此输入流 DFSInputStream 存储了 block 所在的 datanode 的位置，然后连接第一个 block 所在的**最近**的 datanode .通过对数据流反复的调用 read( ) 可以将数据从 datanode 传输到 client.
5. 对一个 block 读完时 DFSInputStream 会关闭与 datanode 的连接，然后寻找下一个 block 的最佳 datanode .当一批 blocks 读完时，DFSInputStream 会询问 namenode 下一批所需 blocks 的 datanode 地址。读取 blocks 的切换对于 client 是透明的。
6. 当 client 完成读取，调用 FSDataInputStream 的 close( ) 方法。


----------


在读的过程中，如果 DFSInputStream 和 datanode 通信时**出错**，它会尝试连接下一个最近的 datanode 。DFSInputStream 也会通过校验和确认从 datanode 读取的数据是否完整，如果发现某个block有损坏,就回报告给 namenode ，然后从其它复本读取此 block。


----------

<p class="first">写数据</p>

![此处输入图片的描述][9]


----------

1. 客户端调用 FileSystem 的 get( ) 方法得到一个实例 fs (即分布式文件系统 Distributed FileSystem ),然后 fs 调用 create( ) 创建文件。
2. DistributedFileSystem( fs ) 通过 RPC 调用 NameNode 在命名空间中创建一个新文件，此时该文件还没有相应的数据块。namenode 会检查此文件是否已存在及client是否有权限新建文件，如果检查不通过，则创建失败并向 client 抛出 IOException 异常，否则 namenode 就会创建一条记录。
3. 然后 DistributedFileSystem( fs ) 向 client 返回一个 FSDataOutputStream (封装了 DFSOutputStream )对象来写数据。在 client 写数据时，DFSOutputStream 将它分成一个个的数据包并写入内部队列，称作数据队列(data queue).
4. DFSOutputStream 会请求 namenode 在合适的 datanodes (默认3个)上分配 blocks 来存储数据队列。3个 datanodes 形成一个管线 DataStreamer 将数据包流式的传输到管线中第一个 datanode，第一个datanode存储数据包并发送的第二个datanode, 第二个datanode存储数据包并发送的第三个datanode.
5. DFSOutputStream 也维护了一个确认队列(ack queue),当收到管道中所有 datanodes 的确认信息后对应数据包才会从确认队列中删除。
6. Client 完成数据的写入后便对 FSDataOutputStream 调用 close( ) 方法。
7. 该操作将所有剩余的数据包写入管线，并等待确认，最后向 namenode 报告写完成。

----------

  [^1]: [hadoop分布式文件系统HDFS详解](http://www.36dsj.com/archives/42648)
  [^2]: [Hadoop：The Definitive Guide 总结](http://www.cnblogs.com/biyeymyhjob/archive/2012/08/13/2636452.html)
  [^3]: [HDFS读写数据流](http://blog.csdn.net/luyee2010/article/details/8643799)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/HDFS.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/dfs.jpg?imageView2/2/w/300
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/name_data_node.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/namenode.png?imageView2/2/w/500
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/secondaryNameNode.png
  [6]: http://7xjbdi.com1.z0.glb.clouddn.com/HDFS_structure.png
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/%E5%86%97%E4%BD%99%E6%95%B0%E6%8D%AE.png
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/HDFS_read.png
  [9]: http://7xjbdi.com1.z0.glb.clouddn.com/HDFS_write.png