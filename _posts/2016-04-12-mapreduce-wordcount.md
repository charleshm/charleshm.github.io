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

> "分而治之"：我们会首先把任务划分为多个小任务，map 对各个小任务进行处理，reduce 负责把 map 的输出的中间结果汇总起来。

不论是现实社会，还是在程序设计中，一项工作往往可以被拆分成为多个任务，任务之间的关系可以分为两种：一种是不相关的任务，可以并行执行；另一种是任务之间有相互的依赖，先后顺序不能够颠倒，这类任务是无法并行处理的。

在分布式系统中，机器集群就可以看作硬件资源池，将并行的任务拆分，然后交由每一个空闲机器资源去处理，能够极大地提高计算效率，同时 这种资源无关性，对于计算集群的扩展无疑提供了最好的设计保证。

在Hadoop中，用于执行MapReduce任务的机器角色有两个：一个是JobTracker；另一个是TaskTracker。JobTracker是用于调度工作的，TaskTracker是用于执行工作的。一个Hadoop集群中只有一台JobTracker。

![此处输入图片的描述][1]


----------

#### wordcount 分析[^1]
处理流程，

![此处输入图片的描述][2]


----------

#### 主方法Main分析
<p class="first">1 Job的初始化过程</p>
main函数调用Jobconf类来对MapReduce Job进行初始化，然后调用setJobName()方法命名这个Job。    
对Job进行合理的命名有助于快速地找到Job，方便在JobTracker和Tasktracker页面中对其进行监视。

{% highlight java %}
Configuration conf = new Configuration();
Job job = Job.getInstance(conf, "word count");
{% endhighlight %}

----------

<p class="first">2 设置Job处理的Map(拆分)、Combiner(中间结果合并)以及Reduce(合并)的相关处理类。</p>

{% highlight java %}
job.setMapperClass(TokenizerMapper.class);
job.setCombinerClass(IntSumReducer.class);
job.setReducerClass(IntSumReducer.class);
{% endhighlight %}

----------

<p class="first">3 设置Job输出结果中key和value数据类型</p>
因为结果是<单词,个数>，所以key设置为"Text"类型，相当于Java中String类型。Value设置为"IntWritable"，相当于Java中的int类型。

{% highlight java %}
job.setOutputKeyClass(Text.class);
job.setOutputValueClass(IntWritable.class); 
{% endhighlight %}

----------

<p class="first">4 调用setInputPath()和setOutputPath()设置输入输出路径</p>

{% highlight java %}
FileInputFormat.addInputPath(job, new Path(args[0]));
FileOutputFormat.setOutputPath(job, new Path(args[1]));
{% endhighlight %}


----------

#### InputFormat 和　OutputFormat

<p class="first">InputFormat 和 InputSplit</p>

> InputSplit是Hadoop定义的用来传送给每个单独的map的数据，InputSplit存储的并非数据本身，而是一个分片长度和一个记录数据位置的数组。生成InputSplit的方法可以通过InputFormat( )来设置。

当数据传送给map时，map会将输入分片传送到InputFormat，InputFormat则调用方法getRecordReader()生成RecordReader，RecordReader再通过creatKey()、creatValue()方法创建可供map处理的<key,value>对。简而言之，InputFormat()方法是用来生成可供map处理的<key,value>对的。

其中TextInputFormat是Hadoop默认的输入方法，在TextInputFormat中，每个文件（或其一部分）都会单独地作为map的输入，而这个是继承自FileInputFormat的。之后，每行数据都会生成一条记录，每条记录则表示成<key,value>形式：

- key值是每个数据的记录在数据分片中字节偏移量，数据类型是LongWritable；　　
- value值是每行的内容，数据类型是Text。


----------

<p class="first">OutputFormat</p>

每一种输入格式都有一种输出格式与其对应。默认的输出格式是TextOutputFormat，这种输出方式与输入类似，会将每条记录以一行的形式存入文本文件。不过，它的键和值可以是任意形式的，因为程序内容会调用toString()方法将键和值转换为String类型再输出。

----------

#### 源码

{% highlight java %}
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable> {

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                       ) throws IOException, InterruptedException {
            StringTokenizer itr = new StringTokenizer(value.toString());
            while (itr.hasMoreTokens()) {
                word.set(itr.nextToken());
                context.write(word, one);
            }
        }
    }

    public static class IntSumReducer
        extends Reducer<Text, IntWritable, Text, IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                          ) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable val : values) {
                sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
{% endhighlight %}

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/optimized-ec2d.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/word-count-as-mapreduce.png

----------

  [^1]: [Hadoop集群（第6期）_WordCount运行详解](http://www.cnblogs.com/xia520pi/archive/2012/05/16/2504205.html)
