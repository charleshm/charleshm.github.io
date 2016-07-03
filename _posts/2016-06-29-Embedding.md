---
published: true
author: Charles
layout: post
title:  "特征工程之Embedding"
date:   2016-06-29 8:30
categories: 机器学习 推荐系统
---

### 词向量

#### One-hot Representation

自然语言理解的问题要转化为机器学习的问题，第一步肯定是要找一种方法把这些符号数学化。

NLP 中最直观，也是到目前为止最常用的词表示方法是 One-hot Representation，这种方法把每个词表示为一个很长的向量。这个向量的维度是词表大小，其中绝大多数元素为 0，只有一个维度的值为 1，这个维度就代表了当前的词。

举个栗子，

> “话筒”表示为 [0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 …]   
“麦克”表示为 [0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 …]

这种表示方法存在一个重要的问题就是“词汇鸿沟”现象：任意两个词之间都是孤立的。光从这两个向量中看不出两个词是否有关系，哪怕是话筒和麦克这样的同义词也不能幸免于难。

> Word embedding is the collective name for a set of language modeling and feature learning techniques in natural language processing (NLP) where words or phrases from the vocabulary are mapped to vectors of real numbers in a low-dimensional space relative to the vocabulary size ("continuous space")[^1].


---

[^1]:https://en.wikipedia.org/wiki/Word_embedding
