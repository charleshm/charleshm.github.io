---
published: true
author: Charles
layout: post
title:  "基于内容的推荐系统"
date:   2016-03-08 15:20
categories: 推荐系统 机器学习
---

协同过滤是目前最流行的一种推荐方法，但它通常只考虑了用户评分数据, 而忽略了 item 和用户自身的很多特征, 如电影的导演、演员和发布时间等, 用户的地理位置、性别、年龄等. 

如何充分、合理的利用这些特征, 获得更好的推荐效果, 是基于内容推荐所要解决的主要问题. 目前工业界真正使用的系统往往会综合这两种模型。

#### Content-based Recommendations
核心思想：根据用户过去喜欢的产品（item），为用户推荐和他过去喜欢风格相似的产品。例如,在电影推荐中,基于内容的系统首先分析用户已经看过的打分比较高的电影的共性(演员、导演、风格等),再推荐与这些用户感兴趣的电影内容相似度高的其它电影.

![此处输入图片的描述][1]

#### 框架结构

![此处输入图片的描述][2]

 1. CONTENT ANALYZER：首先我们需要对非结构化的信息（网页，文档等）进行处理，也就是提取特征，将其转化为方便处理的形式。比如对网页做关键词提取(TF-IDF)，生成  keyword vectors 来表征该网页。
 
![此处输入图片的描述][3]
2. PROFILE LEARNER：该模块收集、泛化代表用户偏好的数据，生成用户概要信息（user profile）。通常，是采用机器学习方法从用户之前喜欢和不喜欢的商品信息中推出一个表示用户喜好的模型。
3. FILTERING COMPONENT：计算各项 Item Profile 与该 User Profile 的相似度   ，给出推荐列表。

通俗版：

1. 为每个物品（Item）构建一个物品的属性资料（Item Profile）
2. 为每个用户（User）构建一个用户的喜好资料（User Profile）
3. 计算用户喜好资料与物品属性资料的相似度，相似度高意味着用户可能喜欢这个物品，相似度低往往意味着用户不喜欢这个物品。

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/Content-based%20Recommendations.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-09_140031.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-09_142149.png