---
published: true
author: Charles
layout: post
title:  "Latent Semantic Analysis"
date:   2016-03-14 9:45
categories: 自然语言处理 推荐系统
---

LSA(latent semantic analysis)潜在语义分析，也被称为 LSI(latent semantic index)，是 Scott Deerwester, Susan T. Dumais 等人在 1990 年提出来的一种新的**索引和检索**方法。该方法和传统向量空间模型(vector space model)一样使用向量来表示词(terms)和文档(documents)，并通过向量间的关系(如夹角)来判断词及文档间的关系；不同的是，LSA 将词和文档映射到**潜在语义空间**，从而去除了原始向量空间中的一些“噪音”，提高了信息检索的精确度[^1]。

![此处输入图片的描述][1]

引用吴军老师在 "矩阵计算与文本处理中的分类问题" 中的总结：

> 三个矩阵有非常清楚的物理含义。第一个矩阵 U 中的每一行表示意思相关的**一类词**，其中的每个非零元素表示这类词中每个词的重要性（或者说相关性），数值越大越相关。最后一个矩阵 V 中的每一列表示同一主题**一类文章**，其中每个元素表示这类文章中每篇文章的相关性。中间的矩阵 D 则表示类词和文章类之间的相关性。因此，我们只要对关联矩阵 X 进行一次奇异值分解，我们就可以同时完成了近义词分类和文章的分类。（同时得到每类文章和每类词的相关性）[^2]。

其实，有了之前对SVD理解，理解起来应该相当容易。

----------

#### 举个栗子
传统向量空间模型使用精确的词匹配，即精确匹配用户输入的词与向量空间中存在的词，无法解决一词多义(polysemy)和一义多词(synonymy)的问题。实际上在搜索中，我们实际想要去比较的不是词，而是隐藏在词之后的意义和概念。

![此处输入图片的描述][2]

> LSA 的核心思想是将词和文档映射到**潜在语义空间**，再比较其相似性。

我们通过一个简单的例子来看一下整个流程[^3].

| Index Words | Titles |    |    |    |    |    |    |    |   |
|-------------|--------|----|----|----|----|----|----|----|---|
| T1          | T2     | T3 | T4 | T5 | T6 | T7 | T8 | T9 |   |
| book        |        |    | 1  | 1  |    |    |    |    |   |
| dads        |        |    |    |    |    | 1  |    |    | 1 |
| dummies     |        | 1  |    |    |    |    |    | 1  |   |
| estate      |        |    |    |    |    |    | 1  |    | 1 |
| guide       | 1      |    |    |    |    | 1  |    |    |   |
| investing   | 1      | 1  | 1  | 1  | 1  | 1  | 1  | 1  | 1 |
| market      | 1      |    | 1  |    |    |    |    |    |   |
| real        |        |    |    |    |    |    | 1  |    | 1 |
| rich        |        |    |    |    |    | 2  |    |    | 1 |
| stock       | 1      |    | 1  |    |    |    |    | 1  |   |
| value       |        |    |    | 1  | 1  |    |    |    |   |

Term-Document 矩阵：这里的一行表示一个词在哪些title中出现了，一列表示一个title中出现了哪些词（停词已去除）。比如说T1这个title中就有guide、investing、market、stock四个词，各出现了一次。

对 Term-Document 矩阵做SVD分解：

<table>
    <tbody>
        <tr>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <td bgcolor="#ffccff">book</td>
                            <td>0.15</td>
                            <td>-0.27</td>
                            <td>0.04</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">dads</td>
                            <td>0.24</td>
                            <td>0.38</td>
                            <td>-0.09</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">dummies</td>
                            <td>0.13</td>
                            <td>-0.17</td>
                            <td>0.07</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">estate</td>
                            <td>0.18</td>
                            <td>0.19</td>
                            <td>0.45</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">guide</td>
                            <td>0.22</td>
                            <td>0.09</td>
                            <td>-0.46</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">investing</td>
                            <td>0.74</td>
                            <td>-0.21</td>
                            <td>0.21</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">market</td>
                            <td>0.18</td>
                            <td>-0.30</td>
                            <td>-0.28</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">real</td>
                            <td>0.18</td>
                            <td>0.19</td>
                            <td>0.45</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">rich</td>
                            <td>0.36</td>
                            <td>0.59</td>
                            <td>-0.34</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">stock</td>
                            <td>0.25</td>
                            <td>-0.42</td>
                            <td>-0.28</td>
                        </tr>
                        <tr>
                            <td bgcolor="#ffccff">value</td>
                            <td>0.12</td>
                            <td>-0.14</td>
                            <td>0.23</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <td>3.91</td>
                            <td>0</td>
                            <td>0</td>
                        </tr>
                        <tr>
                            <td>0</td>
                            <td>2.61</td>
                            <td>0</td>
                        </tr>
                        <tr>
                            <td>0</td>
                            <td>0</td>
                            <td>2.00</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr bgcolor="#00ccff">
                            <td>T1</td>
                            <td>T2</td>
                            <td>T3</td>
                            <td>T4</td>
                            <td>T5</td>
                            <td>T6</td>
                            <td>T7</td>
                            <td>T8</td>
                            <td>T9</td>
                        </tr>
                        <tr>
                            <td>0.35</td>
                            <td>0.22</td>
                            <td>0.34</td>
                            <td>0.26</td>
                            <td>0.22</td>
                            <td>0.49</td>
                            <td>0.28</td>
                            <td>0.29</td>
                            <td>0.44</td>
                        </tr>
                        <tr>
                            <td>-0.32</td>
                            <td>-0.15</td>
                            <td>-0.46</td>
                            <td>-0.24</td>
                            <td>-0.14</td>
                            <td>0.55</td>
                            <td>0.07</td>
                            <td>-0.31</td>
                            <td>0.44</td>
                        </tr>
                        <tr>
                            <td>-0.41</td>
                            <td>0.14</td>
                            <td>-0.16</td>
                            <td>0.25</td>
                            <td>0.22</td>
                            <td>-0.51</td>
                            <td>0.55</td>
                            <td>0.00</td>
                            <td>0.34</td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
    </tbody>
</table>

我们可以将左奇异向量和右奇异向量都取后2维（之前是3维的矩阵），投影到一个平面上（潜在语义空间），可以得到：

![此处输入图片的描述][3]

在图上，每一个红色的点，都表示一个词，每一个蓝色的点，都表示一篇文档，这样我们可以对这些词和文档进行聚类，比如说 stock 和 market 可以放在一类，因为他们老是出现在一起，real 和 estate 可以放在一类，dads，guide 这种词就看起来有点孤立了，我们就不对他们进行合并了。按这样聚类出现的效果，可以提取文档集合中的近义词，这样当用户检索文档的时候，是用语义级别（**近义词集合**）去检索了，而不是之前的词的级别。这样一减少我们的检索、存储量，因为这样压缩的文档集合和PCA是异曲同工的，二可以提高我们的用户体验，用户输入一个词，我们可以在这个词的近义词的集合中去找，这是传统的索引无法做到的[^4]。

----------

#### 总结
> LSA可以处理向量空间模型无法解决的一义多词(synonymy)问题，但不能解决**一词多义**(polysemy)问题。因为LSA将每一个词映射为潜在语义空间中的一个点，也就是说一个词的多个意思在空间中对于的是同一个点，并没有被区分。

另外[^5]，

 - SVD的优化目标基于L-2 norm 或者 **Frobenius Norm** 的，这相当于隐含了对数据的高斯分布假设。而 term 出现的次数是非负的，这明显不符合 Gaussian 假设，而更接近 Multi-nomial 分布；
 - 特征向量的方向没有对应的物理解释；
 - SVD的计算复杂度很高，而且当有新的文档来到时，若要更新模型需重新训练；

----------


  [^1]: Latent semantic analysis note By  Zhou Li 
  [^2]: 数学之美
  [^3]: [Latent Semantic Analysis (LSA) Tutorial - A Small Example](http://www.puffinwarellc.com/index.php/news-and-articles/articles/33-latent-semantic-analysis-tutorial.html?start=1)
  [^4]: [强大的矩阵奇异值分解(SVD)及其应用](http://www.cnblogs.com/LeftNotEasy/archive/2011/01/19/svd-and-applications.html)
  [^5]: [LSA vs PLSA及EM求解 ](http://blog.sina.com.cn/s/blog_6a1b8c6b0101hgxg.html)

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/SDfTM.jpg
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/diagram2.png
  [3]: http://7xjbdi.com1.z0.glb.clouddn.com/xygraph2.png?imageView2/2/w/500