---
published: true
author: Charles
layout: post
title:  "LDA Topic Model"
date:   2016-03-19 15:30
categories: 机器学习 自然语言处理
---

前面我们已经学过了 [pLSA][1]，再来看 LDA 的时候会觉得很简单（不要被各种分布吓到了），总的来说， **LDA 可以看做是 pLSA 的贝叶斯版本**。

#### 宏观把控
pLSA 假设每篇文档的生成过程是这样的：

- 以 $P(d)$ 的先验概率选择一篇文档 $d$      
- 选定 d 后，以 $P(z\|d)$ 的概率选中主题 z       
- 选中主题 z 后，以 $P(w\|z)$ 的概率选中单词 w  

> 并且每个主题在所有词项上服从 Multinomial 分布，每个文档在所有主题上服从 Multinomial 分布。

![此处输入图片的描述][2]

在**频率学派**的观念里， 参数 $\theta = (P(z\|d), P(w\|z))$ 是确定的。放在这里，也就是说，对于一篇给定的文档，其对应的主题分布是确定的；对于一个给定的主题，其词分布也是确定的。

而**贝叶斯学派**认为，参数 $\theta = (P(z\|d), P(w\|z))$ 是随机的，服从一定的分布。也就是说，主题分布和词分布不应该是确定不变的，而是服从某种分布。


----------


现在我们来看一看 LDA 是如何贝叶斯化 pLSA.

LDA模型中，一篇文档生成的方式如下：

 - 从**狄利克雷分布** $\alpha$ 中取样生成文档 $m$ 的主题分布 $\vartheta_m$
 - 从主题的**多项式分布** $\vartheta_m$ 中取样生成文档 $m$ 第 $n$ 个词的主题 $z_{m,n}$
 - 从**狄利克雷分布** $\beta$ 中取样生成主题 $z_{m,n}$ 对应的词语分布 $\varphi_{z_{m,n}}$
 - 从词语的**多项式分布** $\varphi_{z_{m,n}}$ 中采样生成最终的词语 $w_{m,n}$

![此处输入图片的描述][3]


----------


在 "**[LDA数学八卦][4]**" 里面有两个非常形象的图，可以很直观看出 pLSA 和 LDA 之间的区别，

![此处输入图片的描述][5]

----------

#### 数学表述
待完善~

----------

  [1]: http://charlesx.top/2016/03/Plsa/
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-16_152408.png?imageView2/2/w/300
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-20_165025.png?imageView2/2/w/300
  [4]: http://files.cnblogs.com/files/zhengyuhong/LDA%E6%95%B0%E5%AD%A6%E5%85%AB%E5%8D%A6.pdf
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/plsa_lda.png?imageView2/2/w/400
