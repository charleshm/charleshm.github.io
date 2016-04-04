---
published: true
author: Charles
layout: post
title:  "C++基础札记"
date:   2016-02-04 15:30
categories: 数据结构与算法
---

#### 初始化

一般情况下，初始化可以用花括号，圆括号，等号这三种方式，

<pre class="prettyprint linenums">
vector<int> v1(10);           //v1有10个元素，每个的值都是0
vector<int> v1{10};           //v2有1个元素，该元素的值是0

vector<int> v1(10,1);         //v3有10个元素，每个的值都是1
vector<int> v1{10,1};         //v2有2个元素，值分别是10和1
</pre>