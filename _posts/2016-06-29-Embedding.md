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

自然语言理解的问题要转化为机器学习的问题，第一步肯定是要找一种方法把这些符号数学化[^2]。

NLP 中最直观，也是到目前为止最常用的词表示方法是 One-hot Representation，这种方法把每个词表示为一个很长的向量。这个向量的维度是词表大小，其中绝大多数元素为 0，只有一个维度的值为 1，这个维度就代表了当前的词。

举个栗子，

> “话筒”表示为 [ 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 … ]   
“麦克”表示为 [ 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 … ]

这种表示方法存在一个重要的问题就是“词汇鸿沟”现象：任意两个词之间都是孤立的。光从这两个向量中看不出两个词是否有关系，哪怕是话筒和麦克这样的同义词也不能幸免于难。

---

#### Distributed representation[^3]

Distributed representation 用来表示词，通常被称为“Word Representation”或“Word Embedding”，比如像word2vec生成的词向量就属于这种。

我们来看下维基百科给的定义，

> Word embedding is the collective name for a set of language modeling and feature learning techniques in natural language processing (NLP) where words or phrases from the vocabulary are mapped to vectors of real numbers in a low-dimensional space relative to the vocabulary size ("continuous space")[^1].

所谓的word embedding，就是找到一个映射或者函数，生成在一个新的空间上的表达。

---

#### 特征工程

我们在实际应用中经常会对类别特征做one hot encoding，这样生成的特征维度往往非常大，最近比较流行的一种趋势是将这些类别特征连接到embedding层，映射到低维的连续空间，可以看做一种特征提取的方法。

最近google的一篇深度学习做推荐的论文：[Wide & Deep Learning for Recommender Systems](https://arxiv.org/pdf/1606.07792v1.pdf)，使用了类似的技巧，

![][1]

效果还是非常不错的，

![][2]

---

#### 具体流程

老实说刚开始我只是简单的认为应该和word2vec生成词向量差不多吧，后来一细想不对呀，word2vec 训练词向量利用的是上下文信息，我们现在要做的实际上是多one hot之后的特征做降维，解释不通啊。

![][3]

然后搜索了下文献，其实非常简洁，只是做了一层映射，one hot 之后的特征乘上 embedding matrix ($W_E \in \mathcal{R}^{V \times D}$)，得到的就是embedding之后的特征[^4]。


[1]:http://7xjbdi.com1.z0.glb.clouddn.com/wdms.png
[2]:http://7xjbdi.com1.z0.glb.clouddn.com/wdfs_results.png
[3]:http://7xjbdi.com1.z0.glb.clouddn.com/simple_embedding.png

---

[^1]:[Word embedding](https://en.wikipedia.org/wiki/Word_embedding)
[^2]:[Deep Learning in NLP （一）词向量和语言模型](http://licstar.net/archives/328)
[^3]:[深度学习、自然语言处理和表征方法](http://blog.jobbole.com/77709/)
[^4]:[A Theoretically Grounded Application of Dropout in Recurrent Neural Networks](http://arxiv.org/pdf/1512.05287v3.pdf)
