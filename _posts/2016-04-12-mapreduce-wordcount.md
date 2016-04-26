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

在Hadoop中，用于执行MapReduce任务的机器角色有两个：一个是JobTracker；另一个是TaskTracker，JobTracker是用于调度工作的，TaskTracker是用于执行工作的。一个Hadoop集群中只有一台JobTracker。

![此处输入图片的描述][1]


----------

#### wordcount 分析
处理流程，

![此处输入图片的描述][2]

----------

源码：

{% highlight c++ %}
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

----------

#### 主方法Main分析
首先是Job的初始化过程。main函数调用Jobconf类来对MapReduce Job进行初始化，然后调用setJobName()方法命名这个Job。

对Job进行合理的命名有助于更快地找到Job，以便在JobTracker和Tasktracker的页面中对其进行监视。

{% highlight c++ %}
JobConf conf = new JobConf(WordCount.class);
conf.setJobName("wordcount");
{% endhighlight %}

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/optimized-ec2d.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/word-count-as-mapreduce.png