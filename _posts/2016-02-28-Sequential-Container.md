---
published: true
author: Charles
layout: post
title:  "顺序容器"
date:   2016-02-28 14:28
categories: 数据结构与算法
---

### 顺序容器

#### 容器定义和初始化
> 当一个容器初始化为另一容器的拷贝时，两个容器的容器类型和元素类型都必须相同。

{% highlight c++ %}
vector<const char*> articles = {"a", "an", "the"};
vector<string> words(articles);     //错误：容器类型不匹配
{% endhighlight %}


----------


#### 固定大小数组array，

{% highlight c++ %}
array<int,42>
{% endhighlight %}

> 虽然我们不能对内置数组类型进行拷贝和赋值操作，但 array 并无此限制：

{% highlight c++ %}
int digs[10] = {0,1,2};
int cpy[10] = digs;                //内置数组不支持拷贝和赋值

array<int,10> digits = {0,1,2};
array<int,10> copy = digits;       
{% endhighlight %}


----------


#### 常用操作
- **swap**：交换函数
- **assign**
- **size** ( forward_list 不支持 )
- **push_back, insert, push_front**
- **emplace_front, emplace, emplace_back**


----------

#### vector对象是如何增长的
我们可以向vector中添加新的元素，在定义的时候不需要指定其大小，那么vector对象是如何增长的呢？

> 当不得不获取新的内存空间时，vector 和　string 的实现通常会分配比新空间需求更大的内存空间作为备用，这样就不需要每次添加新元素都重新分配内存空间了。

相关的操作，

{% highlight c++ %}
c.shrink_to_fit()            //将capacity()减小为size()相同大小
c.capacity()                 //不重新分配内存空间的话，c 可以保存多少空间
c.reserve(n)                 //分配至少能容纳n个元素的内存空间
{% endhighlight %}

reserve 并不改变容器中元素的数量，它仅影响vector **预先分配**多大的内存空间。

> 只有当需求的内存空间超过当前容量时，reserve 调用才会改变 vector 的容量。当需求大小小于或等于当前容量时，reserve 什么也不做。在调用 reserve 之后， capacity 将会大于或等于传递给　reserve　的参数。

也就是说　reserve 永远不会减小容器占用的内存空间。类似的， resize 成员函数只改变容器中元素的数目，而不是容器的容量。