---
published: true
author: Charles
layout: post
title:  "漫谈奇异值分解"
date:   2016-03-10 14:28
categories: 机器学习 推荐系统 数学基础
---

在进一步学习推荐系统之前，先把奇异值分解这块的理论理清。

#### 矩阵变换

我们知道，方阵和向量相乘，从几何意义上来讲，就是对向量作 **旋转、伸缩** 变换。比如下图中单位圆上不同颜色的点，在与矩阵$ 
\left[\begin{array}{cc}   
1 & 3 \\\\   
-3 & 2 
\end{array}\right]
$相乘后，进行了相应旋转(rotating)和拉伸(stretching)变换[^6]。

![此处输入图片的描述][7]

再来瞅瞅三维矩阵 $$ 
\left[
\begin{array}{ccc}
2 & -0.48 & -0.56 \\
-0.072 & 0.74 & -2 \\
1.1  &  0.45  &  -1.5 
\end{array}
\right],
$$

![此处输入图片的描述][8]

----------

#### 特征值分解

如果方阵对某个向量只产生伸缩，而不产生旋转效果，那么这个向量就称为矩阵的**特征向量**，伸缩的比例就是对应的**特征值**。

$$A\mathbf{x} = \lambda \mathbf{x} \tag{1.1}$$

![此处输入图片的描述][1]

学线性代数的时候，我们应该都学过这样一个定理：

> 若 $A$ 为 $n$ 阶实对称阵（方阵），则存在由特征值组成的对角阵 $\Lambda$ 和特征向量组成的正交阵 $Q$，使得：    
$$A = Q\Lambda Q^T \tag{1.2}$$

这就是我们所说的 **特征值分解**（Eigenvalue decomposition: EVD）($R^n \rightarrow R^n$)，而奇异值分解其实可以看做是特征值分解在任意矩阵($m \times n$)上的推广形式 ($R^n \rightarrow R^m$)。（只有对方阵才有特征值的概念，所以对于任意的矩阵，我们引入了奇异值）

----------

#### 奇异值分解
上面的特征值分解的A矩阵是对称阵，可以将一组正交基映射到另一组正交基！那么现在来分析：对任意$m \times n$的矩阵，**能否找到一组正交基使得经过它变换后还是正交基**？答案是肯定的，它就是SVD分解的精髓所在。

下面我们从特征值分解出发，导出奇异值分解。

首先我们注意到 $A^TA$ 为 $n$ 阶对阵矩阵，我们可以对它做特征值分解。

$$A^TA = VDV^T$$

这个时候我们得到一组正交基，$\\{v_1,v_2,\cdots,v_n\\}$：

$$\begin{align*}
(A^TA)v_i & = \lambda_iv_i\\
(Av_i,Av_j) & = (Av_i)^T(Av_j)\\
& = v_i^TA^TAv_j\\
& = v_i^T(\lambda_jv_j)\\
& = \lambda_jv_i^Tv_j\\
& = 0
\end{align*}$$

由 $r(A^TA)=r(A)=r$，这个时候我们得到了另一组正交基，$\\{Av_1,Av_2,\cdots,Av_r\\}$。先将其标准化，令：

$$\begin{align*}
u_i & = \frac{Av_i}{\lvert Av_i \rvert} = \frac{1}{\sqrt{\lambda_i}}Av_i\\
\Rightarrow Av_i & =  \sqrt{\lambda_i}u_i = \delta_i u_i \tag{2.1}
\end{align*}$$

![此处输入图片的描述][2]

注：

$$\begin{align*}
\lvert Av_i \rvert^2 & = (Av_i,Av_i) = \lambda_iv_i^Tv_i = \lambda_i\\
\Rightarrow \lvert Av_i \rvert & = \sqrt{\lambda_i} = \delta_i \ \ \text{(奇异值)}
\end{align*}$$


将向量组 $\\{u_1,u_2,\cdots,u_r\\}$ 扩充为 $R^m$中的标准正交基 $\\{u_1,u_2,\cdots,u_r,\cdots,u_m\\}$。

则：

$$\begin{align*}
AV & = A(v_1 v_2 \cdots v_n) = (Av_1\ Av_2\ \cdots\ Av_r\ 0 \cdots\ 0)\\
& = (\delta_1 u_1\ \delta_2 u_2\ \cdots \delta_r u_r\ 0 \cdots\ 0)\\
& =  U\Sigma\\
\Rightarrow A &= U\Sigma V^T \tag{2.2}
\end{align*}$$

这就表明任意的矩阵 A 是可以分解成三个矩阵。V 表示了原始域的标准正交基，U 表示经过 A 变换后的co-domain的标准正交基，Σ 表示了 V 中的向量与 U 中相对应向量之间的关系[^2]。

> This shows how to decompose the matrix A into the product of three matrices: V describes an orthonormal basis in the domain, and U describes an orthonormal basis in the co-domain, and Σ describes how much the vectors in V are stretched to give the vectors in U. 

上面说了一大段，描述的都是**几何意义**，那么在实际中SVD分解意味呢什么呢？我们将通过一个例子进行说明 .

----------

#### 举个栗子[^4]
我们现在有一批高尔夫球手对九个不同hole的所需挥杆次数数据，我们希望基于这些数据建立模型，来预测选手对于某个给定hole的挥杆次数。（这个例子来自于： **[Singular Value Decomposition (SVD) Tutorial][3]**，强烈建议大家都去看看）

| Hole | **Par** | Phil | Tiger | Vijay |
|:----:|:---:|:----:|:-----:|:-----:|
|   1  |  4  |   4  |   4   |   4   |
|   2  |  5  |   5  |   5   |   5   |
|   3  |  3  |   3  |   3   |   3   |
|   4  |  4  |   4  |   4   |   4   |
|   5  |  4  |   4  |   4   |   4   |
|   6  |  4  |   4  |   4   |   4   |
|   7  |  4  |   4  |   4   |   4   |
|   8  |  3  |   3  |   3   |   3   |
|   9  |  5  |   5  |   5   |   5   |

> 注: **[par(标准杆)][4]**，是一个高尔夫球运动术语，用以定義某一球洞從開球到完成進洞所需要的揮桿次數，以作為比賽成績的參考依據。

最简单的一个思路，我们对每个hole设立一个难度评价指标 **HoleDifficulty** ，对每位选手的能力也设立一个评价指标 **PlayerAbility**，实际的得分取决于这两者的乘积：

$$PredictedScore = HoleDifficulty \cdot PlayerAbility$$

我们可以简单的把每位选手的Ability都设为1，那么：

<table>
    <tbody>
        <tr>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th>Phil</th>
                            <th>Tiger</th>
                            <th>Vijay</th>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>5</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>3</td>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>3</td>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>5</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">=</td>
            <td class="noborder" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th>HoleDifficulty</th>
                        </tr>
                        <tr>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>5</td>
                        </tr>
                        <tr>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>5</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th colspan="3">PlayerAbility</th>
                        </tr>
                        <tr>
                            <td>Phil</td>
                            <td>Tiger</td>
                            <td>Vijay</td>
                        </tr>
                        <tr>
                            <td>1</td>
                            <td>1</td>
                            <td>1</td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
    </tbody>
</table>

接着我们将  HoleDifficulty 和  PlayerAbility 这两个向量 **标准化**，可以得到如下的关系：

<table>
    <tbody>
        <tr>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th>Phil</th>
                            <th>Tiger</th>
                            <th>Vijay</th>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>5</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>3</td>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>3</td>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>5</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder" >$=$</td>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th>HoleDifficulty</th>
                        </tr>
                        <tr>
                            <td>0.33</td>
                        </tr>
                        <tr>
                            <td>0.41</td>
                        </tr>
                        <tr>
                            <td>0.25</td>
                        </tr>
                        <tr>
                            <td>0.33</td>
                        </tr>
                        <tr>
                            <td>0.33</td>
                        </tr>
                        <tr>
                            <td>0.33</td>
                        </tr>
                        <tr>
                            <td>0.33</td>
                        </tr>
                        <tr>
                            <td>0.25</td>
                        </tr>
                        <tr>
                            <td>0.41</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th>ScaleFactor</th>
                        </tr>
                        <tr>
                            <td>21.07</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th colspan="3">PlayerAbility</th>
                        </tr>
                        <tr>
                            <td>Phil</td>
                            <td>Tiger</td>
                            <td>Vijay</td>
                        </tr>
                        <tr>
                            <td>0.58</td>
                            <td>0.58</td>
                            <td>0.58</td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
    </tbody>
</table>


好熟悉，这不就是传说中的 SVD 吧，这样就出来了。

这里面蕴含了一个非常有趣的思想，也是 SVD 这么有用的核心：

> 最开始高尔夫球员和 holes 之间是没有直接联系的，我们通过 feature 把他们联系在一起：不同的 hole 进洞难度是不一样的，每个球手对进度难度的把控也是不一样的，那么我们就可以通过 **进洞难度** 这个 feature 将它们联系在一起。将它们乘起来就得到了我们想要的挥杆次数。

这个思想很重要，对于我们理解 LSI 和 SVD在推荐系统中的应用相当重要。

----------

#### 继续挖这个栗子
**SVD分解就是利用隐藏的 feature 建立起矩阵行和列之间的联系。**

大家可能注意到，上面那个矩阵秩为1，所以我们很容易就能将其分解，但是在实际问题中我们就得依靠SVD分解了，这个时候的隐藏特征往往也不止一个了。

我们将上面的数据稍作修改，

<table>
    <tbody>
        <tr>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th>Phil</th>
                            <th>Tiger</th>
                            <th>Vijay</th>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>5</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>3</td>
                            <td>2</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>5</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>5</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>2</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>5</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$=$</td>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th colspan="3">HoleDifficulty 1-3</th>
                        </tr>
                        <tr>
                            <td>4.34</td>
                            <td>-0.18</td>
                            <td>-0.90</td>
                        </tr>
                        <tr>
                            <td>4.69</td>
                            <td>-0.38</td>
                            <td>-0.15</td>
                        </tr>
                        <tr>
                            <td>2.66</td>
                            <td>0.80</td>
                            <td>0.40</td>
                        </tr>
                        <tr>
                            <td>4.36</td>
                            <td>0.15</td>
                            <td>0.47</td>
                        </tr>
                        <tr>
                            <td>4.00</td>
                            <td>0.35</td>
                            <td>-0.29</td>
                        </tr>
                        <tr>
                            <td>4.05</td>
                            <td>-0.67</td>
                            <td>0.68</td>
                        </tr>
                        <tr>
                            <td>3.66</td>
                            <td>0.89</td>
                            <td>0.33</td>
                        </tr>
                        <tr>
                            <td>3.39</td>
                            <td>-1.29</td>
                            <td>0.14</td>
                        </tr>
                        <tr>
                            <td>5.00</td>
                            <td>0.44</td>
                            <td>-0.36</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th colspan="3">PlayerAbility 1-3</th>
                        </tr>
                        <tr>
                            <td>Phil</td>
                            <td>Tiger</td>
                            <td>Vijay</td>
                        </tr>
                        <tr>
                            <td>0.91</td>
                            <td>1.07</td>
                            <td>1.00</td>
                        </tr>
                        <tr>
                            <td>0.82</td>
                            <td>-0.20</td>
                            <td>-0.53</td>
                        </tr>
                        <tr>
                            <td>-0.21</td>
                            <td>0.76</td>
                            <td>-0.62</td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
    </tbody>
</table>

进行奇异值分解，可以得到：

<table>
    <tbody>
        <tr>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th>Phil</th>
                            <th>Tiger</th>
                            <th>Vijay</th>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>5</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>3</td>
                            <td>2</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>5</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>3</td>
                            <td>5</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>4</td>
                            <td>4</td>
                            <td>3</td>
                        </tr>
                        <tr>
                            <td>2</td>
                            <td>4</td>
                            <td>4</td>
                        </tr>
                        <tr>
                            <td>5</td>
                            <td>5</td>
                            <td>5</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$=$</td>
            <td class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <th colspan="3">HoleDifficulty 1-3</th>
                        </tr>
                        <tr>
                            <td>0.35</td>
                            <td>0.09</td>
                            <td>-0.64</td>
                        </tr>
                        <tr>
                            <td>0.38</td>
                            <td>0.19</td>
                            <td>-0.10</td>
                        </tr>
                        <tr>
                            <td>0.22</td>
                            <td>-0.40</td>
                            <td>0.28</td>
                        </tr>
                        <tr>
                            <td>0.36</td>
                            <td>-0.08</td>
                            <td>0.33</td>
                        </tr>
                        <tr>
                            <td>0.33</td>
                            <td>-0.18</td>
                            <td>-0.20</td>
                        </tr>
                        <tr>
                            <td>0.33</td>
                            <td>0.33</td>
                            <td>0.48</td>
                        </tr>
                        <tr>
                            <td>0.30</td>
                            <td>-0.44</td>
                            <td>0.23</td>
                        </tr>
                        <tr>
                            <td>0.28</td>
                            <td>0.64</td>
                            <td>0.10</td>
                        </tr>
                        <tr>
                            <td>0.41</td>
                            <td>-0.22</td>
                            <td>-0.25</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <td colspan="3">ScaleFactor 1-3</td>
                        </tr>
                        <tr>
                            <td>21.07</td>
                            <td>0</td>
                            <td>0</td>
                        </tr>
                        <tr>
                            <td>0</td>
                            <td>2.01</td>
                            <td>0</td>
                        </tr>
                        <tr>
                            <td>0</td>
                            <td>0</td>
                            <td>1.42</td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td valign="middle" class="noborder">$\times$</td>
            <td valign="middle" class="noborder">
                <table>
                    <tbody>
                        <tr>
                            <td colspan="3">PlayerAbility 1-3</td>
                        </tr>
                        <tr>
                            <td>Phil</td>
                            <td>Tiger</td>
                            <td>Vijay</td>
                        </tr>
                        <tr>
                            <td>0.53</td>
                            <td>0.62</td>
                            <td>0.58</td>
                        </tr>
                        <tr>
                            <td>-0.82</td>
                            <td>0.20</td>
                            <td>0.53</td>
                        </tr>
                        <tr>
                            <td>-0.21</td>
                            <td>0.76</td>
                            <td>-0.62</td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
    </tbody>
</table>

**隐藏特征的重要性是与其对应的奇异值大小成正比的，也就是说奇异值越大，其所对应的隐藏特征也越重要。**

我们将这个思想推广一下，

> 在推荐系统中，用户和物品之间没有直接关系。**但是我们可以通过 feature 把它们联系在一起。**对于电影来说，这样的特征可以是：喜剧还是悲剧，是动作片还是爱情片。用户和这样的 feature 之间是有关系的，比如某个用户喜欢看爱情片，另外一个用户喜欢看动作片；物品和 feature 之间也是有关系的，比如某个电影是喜剧，某个电影是悲剧。那么通过和 feature 之间的联系，我们就找到了用户和物品之间的关联。

![此处输入图片的描述][5]

有了这样了解，再去看看奇异值分解的应用会事半功倍[^5]。

----------

#### 后记
Markdown排版表格是件麻烦事，google了一下，发现个在线网站，可以很方便生成 $\LaTeX$ 和 Markdown 表格，安利下这个 **[神器][6]~** 

----------

  [^1]: [奇异值分解(SVD)原理详解及推导](http://blog.csdn.net/zhongkejingwang/article/details/43053513)
  [^2]: [奇异值分解(SVD) - 几何意义](http://blog.sciencenet.cn/blog-696950-699432.html)
  [^3]: [A Singularly Valuable Decomposition: The SVD of a Matrix](http://www.math.umn.edu/~lerman/math5467/svd.pdf)
  [^4]: [Singular Value Decomposition (SVD) Tutorial](http://www.puffinwarellc.com/index.php/news-and-articles/articles/30-singular-value-decomposition-tutorial.html?showall=1)
  [^5]: [强大的矩阵奇异值分解(SVD)及其应用](http://www.cnblogs.com/LeftNotEasy/archive/2011/01/19/svd-and-applications.html)
  [^6]: [Introduction to the Singular Value Decomposition](http://websites.uwlax.edu/twill/svd/index.html)

  [1]: http://7xjbdi.com1.z0.glb.clouddn.com/500px-Eigenvalue_equation.svg.png?imageView2/2/w/350
  [2]: http://7xjbdi.com1.z0.glb.clouddn.com/svd_vc.png?imageView2/2/w/500
  [3]: http://www.puffinwarellc.com/index.php/news-and-articles/articles/30-singular-value-decomposition-tutorial.html
  [4]: https://zh.wikipedia.org/zh/%E6%A0%87%E5%87%86%E6%9D%86
  [5]: http://7xjbdi.com1.z0.glb.clouddn.com/2016-03-13_123323.png
  [6]: http://www.tablesgenerator.com/markdown_tables
  [7]: http://7xjbdi.com1.z0.glb.clouddn.com/MatrixAction.gif
  [8]: http://7xjbdi.com1.z0.glb.clouddn.com/surface.gif