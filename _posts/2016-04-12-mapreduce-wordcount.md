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

在Hadoop中，用于执行MapReduce任务的机器角色有两个：一个是JobTracker；另一个是TaskTracker。JobTracker是用于调度工作的，TaskTracker是用于执行工作的。一个Hadoop集群中只有一台JobTracker。

![此处输入图片的描述][1]


----------

#### wordcount 分析
处理流程，

![此处输入图片的描述][2]


----------

#### 主方法Main分析
<p class="first">1 Job的初始化过程</p>
main函数调用Jobconf类来对MapReduce Job进行初始化，然后调用setJobName()方法命名这个Job。    
对Job进行合理的命名有助于快速地找到Job，方便在JobTracker和Tasktracker页面中对其进行监视。

{% highlight java %}
JobConf conf = new JobConf(WordCount.class);
conf.setJobName("wordcount");
{% endhighlight %}


----------

<p class="first">2 设置Job输出结果中key和value数据类型</p>
因为结果是<单词,个数>，所以key设置为"Text"类型，相当于Java中String类型。Value设置为"IntWritable"，相当于Java中的int类型。

{% highlight java %}
conf.setOutputKeyClass(Text.class );
conf.setOutputValueClass(IntWritable.class );
{% endhighlight %}


----------

<p class="first">3 设置Job处理的Map（拆分）、Combiner（中间结果合并）以及Reduce（合并）的相关处理类。</p>

{% highlight java %}
conf.setMapperClass(Map.class );
conf.setCombinerClass(Reduce.class );
conf.setReducerClass(Reduce.class );
{% endhighlight %}


----------

<p class="first">4 调用setInputPath()和setOutputPath()设置输入输出路径</p>

{% highlight java %}
FileInputFormat.setInputPaths(conf, new Path(args[0]));
FileOutputFormat.setOutputPath(conf, new Path(args[1]));
{% endhighlight %}


----------

{% highlight java %}
conf.setInputFormat(TextInputFormat.class );
conf.setOutputFormat(TextOutputFormat.class );
{% endhighlight %}

<p class="first">InputFormat 和 InputSplit</p>

InputSplit是Hadoop定义的用来传送给每个单独的map的数据，InputSplit存储的并非数据本身，而是一个分片长度和一个记录数据位置的数组。生成InputSplit的方法可以通过InputFormat()来设置。

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
import java.util.Iterator;
import java.util.StringTokenizer;

import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapred.FileInputFormat;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.MapReduceBase;
import org.apache.hadoop.mapred.Mapper;
import org.apache.hadoop.mapred.OutputCollector;
import org.apache.hadoop.mapred.Reducer;
import org.apache.hadoop.mapred.Reporter;
import org.apache.hadoop.mapred.TextInputFormat;
import org.apache.hadoop.mapred.TextOutputFormat;

public class WordCount {
    //Map
    public static class Map extends MapReduceBase implements
        Mapper<LongWritable, Text, Text, IntWritable> {
        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(LongWritable key, Text value,
                        OutputCollector<Text, IntWritable> output, Reporter reporter)
        throws IOException {
            String line = value.toString();
            StringTokenizer tokenizer = new StringTokenizer(line);
            while (tokenizer.hasMoreTokens()) {
                word.set(tokenizer.nextToken());
                output.collect(word, one);
            }
        }
    }

    //Reduce
    public static class Reduce extends MapReduceBase implements
        Reducer<Text, IntWritable, Text, IntWritable> {
        public void reduce(Text key, Iterator<IntWritable> values,
                           OutputCollector<Text, IntWritable> output, Reporter reporter)
        throws IOException {
            int sum = 0;
            while (values.hasNext()) {
                sum += values.next().get();
            }
            output.collect(key, new IntWritable(sum));
        }
    }

    public static void main(String[] args) throws Exception {
        JobConf conf = new JobConf(WordCount.class);
        conf.setJobName("wordcount");

        conf.setOutputKeyClass(Text.class);
        conf.setOutputValueClass(IntWritable.class);

        conf.setMapperClass(Map.class);
        conf.setCombinerClass(Reduce.class);
        conf.setReducerClass(Reduce.class);

        conf.setInputFormat(TextInputFormat.class);
        conf.setOutputFormat(TextOutputFormat.class);

        FileInputFormat.setInputPaths(conf, new Path(args[0]));
        FileOutputFormat.setOutputPath(conf, new Path(args[1]));

        JobClient.runJob(conf);
    }
}
{% endhighlight %}

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/optimized-ec2d.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/word-count-as-mapreduce.png