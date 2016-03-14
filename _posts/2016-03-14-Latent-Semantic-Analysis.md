---
published: true
author: Charles
layout: post
title:  "Latent Semantic Analysis"
date:   2016-03-14 9:45
categories: 机器学习 自然语言处理 推荐系统
---

LSA(latent semantic analysis)潜在语义分析，也被称为 LSI(latent semantic index)，是 Scott Deerwester, Susan T. Dumais 等人在 1990 年提出来的一种新的索引和检索方法。该方法和传统向量空间模型(vector space model)一样使用向量来表示词(terms)和文档(documents)，并通过向量间的关系(如夹角)来判断词及文档间的关系；而不同的是，LSA 将词和文档映射到潜在语义空间，从而去除了原始向量空间中的一些“噪音”，提高了信息检索的精确度。

![此处输入图片的描述][1]

引用吴军老师在 "矩阵计算与文本处理中的分类问题" 中的总结：

> 三个矩阵有非常清楚的物理含义。第一个矩阵 U 中的每一行表示意思相关的**一类词**，其中的每个非零元素表示这类词中每个词的重要性（或者说相关性），数值越大越相关。最后一个矩阵 V 中的每一列表示同一主题**一类文章**，其中每个元素表示这类文章中每篇文章的相关性。中间的矩阵 D 则表示类词和文章类之间的相关性。因此，我们只要对关联矩阵 X 进行一次奇异值分解，我们就可以同时完成了近义词分类和文章的分类。（同时得到每类文章和每类词的相关性）。



  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/SDfTM.jpg