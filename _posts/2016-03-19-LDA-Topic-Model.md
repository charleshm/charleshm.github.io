---
published: true
author: Charles
layout: post
title:  "LDA Topic Model"
date:   2016-03-18 15:30
categories: 机器学习 自然语言处理
---

前面我们已经学过了 [pLSA][1]，再来看 LDA 的时候会觉得很简单（不要被各种分布吓到了），总的来说， **LDA 可以看做是 pLSA 的贝叶斯版本**。

pLSA假设每篇文档的生成过程是这样的：

- 以 $P(d)$ 的先验概率选择一篇文档 $d$      
- 选定 d 后，以 $P(z\|d)$ 的概率选中主题 z       
- 选中主题 z 后，以 $P(w\|z)$ 的概率选中单词 w  


  [1]: http://charlesx.top/2016/03/Plsa/