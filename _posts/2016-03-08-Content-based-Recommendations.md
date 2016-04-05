---
published: true
author: Charles
layout: post
title:  "基于内容的推荐系统"
date:   2016-03-08 15:20
categories: 推荐系统 机器学习
---

协同过滤是目前最流行的一种推荐方法，但它通常只考虑了**用户对item的喜好**, 而忽略了 item 和用户自身的很多**特征**, 如电影的导演、演员和发布时间等, 用户的地理位置、性别、年龄等. 

如何充分、合理的利用这些特征, 获得更好的推荐效果, 是基于内容推荐所要解决的主要问题. 目前工业界真正使用的系统往往会**综合这两种模型。

![此处输入图片的描述][1]

----------

#### Content-based Recommendations
**核心思想**：根据用户过去喜欢的产品（item），为用户推荐和他过去喜欢风格相似的产品。例如,在电影推荐中,基于内容的系统首先分析用户已经看过的打分比较高的电影的共性(演员、导演、风格等),再推荐与这些用户感兴趣的电影内容相似度高的其它电影.

![此处输入图片的描述][2]

----------

#### 框架结构[^1]

![此处输入图片的描述][3]

 1. **CONTENT ANALYZER**：首先我们需要对非结构化的信息（网页，文档等）进行处理，也就是提取特征，将其转化为方便处理的形式。比如对网页做关键词提取(TF-IDF)，生成  keyword vectors 来表征该网页。
 
![此处输入图片的描述][4]
2. **PROFILE LEARNER**：该模块收集、泛化代表用户偏好的数据，生成用户概要信息（user profile）。通常，是采用机器学习方法从用户之前喜欢和不喜欢的商品信息中推出一个表示用户喜好的模型。
3. **FILTERING COMPONENT**：计算各项 Item Profile 与该 User Profile 的相似度，给出推荐列表。

----------


**通俗版：**[^2]

1. 为每个物品（Item）构建一个物品的属性资料（Item Profile）
2. 为每个用户（User）构建一个用户的喜好资料（User Profile）
3. 计算用户喜好资料与物品属性资料的相似度，相似度高意味着用户可能喜欢这个物品，相似度低往往意味着用户不喜欢这个物品。

----------

#### 模型优缺点[^3]
**优点：**

1. **用户之间的独立性**（User Independence）：既然每个用户的profile都是依据他本身对item的喜好获得的，自然就与他人的行为无关。而CF刚好相反，CF需要利用很多其他人的数据。CB的这种用户独立性带来的一个显著好处是别人不管对item如何作弊（比如利用多个账号把某个产品的排名刷上去）都不会影响到自己。    
2. **好的可解释性**（Transparency）：如果需要向用户解释为什么推荐了这些产品给他，你只要告诉他这些产品有某某属性，这些属性跟他的品味很匹配等等。      
3. **新的item**可以立刻得到推荐（New Item Problem）：只要一个新item加进item库，它就马上可以被推荐，被推荐的机会和老的item是一致的。而CF对于新item就很无奈，只有当此新item被某些用户喜欢过（或打过分），它才可能被推荐给其他用户。所以，如果一个纯CF的推荐系统，新加进来的item就永远不会被推荐。（**冷启动**问题）

----------

**缺点：**

1. item的**特征抽取**一般很难（Limited Content Analysis）：如果系统中的item是文档（如个性化阅读中），那么我们现在可以比较容易地使用信息检索里的方法来“比较精确地”抽取出item的特征。但很多情况下我们很难从item中抽取出准确刻画item的特征，比如电影推荐中item是电影，社会化网络推荐中item是人，这些item属性都不好抽。其实，几乎在所有实际情况中我们抽取的item特征都仅能代表item的一些方面，不可能代表item的所有方面。这样带来的一个问题就是可能从两个item抽取出来的特征完全相同，这种情况下CB就完全无法区分这两个item了。比如如果只能从电影里抽取出演员、导演，那么两部有相同演员和导演的电影对于CB来说就完全不可区分了。    
2. 无法挖掘出用户的**潜在兴趣**（Over-specialization）：既然CB的推荐只依赖于用户过去对某些item的喜好，它产生的推荐也都会和用户过去喜欢的item相似。如果一个人以前只看与推荐有关的文章，那CB只会给他推荐更多与推荐相关的文章，它不会知道用户可能还喜欢数码。       
3. 无法为**新用户**产生推荐（New User Problem）：新用户没有喜好历史，自然无法获得他的profile，所以也就无法为他产生推荐了。当然，这个问题CF也有。

----------
  
  [^1]: [Content-based Recommender Systems: State of the Art and Trends](http://download.springer.com/static/pdf/632/chp%253A10.1007%252F978-0-387-85820-3_3.pdf?originUrl=http%3A%2F%2Flink.springer.com%2Fchapter%2F10.1007%2F978-0-387-85820-3_3&token2=exp=1457503523~acl=%2Fstatic%2Fpdf%2F632%2Fchp%25253A10.1007%25252F978-0-387-85820-3_3.pdf%3ForiginUrl%3Dhttp%253A%252F%252Flink.springer.com%252Fchapter%252F10.1007%252F978-0-387-85820-3_3*~hmac=6724ad95407a4435a5ccb34b75f12e02b130ffe683266f67b547224c032d2c31)
  [^2]: [一个简单的基于内容的推荐算法](http://dataunion.org/7542.html)
  [^3]: [基于内容的推荐](http://www.cnblogs.com/breezedeus/archive/2012/04/10/2440488.html)


  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-09_204218.png
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/Content-based%20Recommendations.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-09_140031.png
  [4]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-09_142149.png