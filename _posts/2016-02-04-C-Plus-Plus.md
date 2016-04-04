---
published: true
author: Charles
layout: post
title:  "C++基础札记"
date:   2016-02-04 15:30
categories: 数据结构与算法
---

#### 初始化
一般情况下，初始化可以用**花括号，圆括号，等号**这三种方式。作为 C++11 新标准的一部分，用花括号来初始化变量得到了全面应用。无论是初始化对象还是某些时候为对象**赋新值**，都可以使用这样一组由花括号起来的**初始值**了。

<pre class="prettyprint linenums">
    int a = 0;
    int a = {0};
    int a(0);
    int a{0};
</pre>