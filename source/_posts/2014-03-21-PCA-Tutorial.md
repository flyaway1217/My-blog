---
layout: post
date: 2014/03/21
title: PCA原理分析
categories: 
    - machine-learning
keywords: PCA,主城份分析
tags: 
    - PCA
    - 主成分分析
description: 本文简要阐述了PCA的原理及其算法过程。
---

# 动机

在机器学习领域中，我们常常会遇到维数很高的数据，有些数据的特征维度高达上百万维，很显然这样的数据是无法直接计算的，而且维度这么高，其中包含的信息一定有冗余，这时就需要进行降维，总的来说，我们降维的主要目的有如下几条:

1. 在原始的高维空间中，包含有冗余信息以及噪音信息，在实际应用例如图像识别中造成了误差，降低了准确率；而通过降维,我们希望减少冗余信息所造成的误差,提高识别(或其他应用)的精度。
2. 希望通过降维算法来寻找数据内部的本质结构特征。
3. 通过降维来加速后续计算的速度
4. 还有其他很多目的，如解决数据的sparse问题

而比较常用的一种降维方法就是[PCA(主成分分析)][PCA].

# PCA思路

降维的过程其实可以看成是一种映射的过程，把在高维空间中的点投影到低维空间中，在这个投影的过程中，我们应当尽量使得信息最大程度的保留。那么，我们应该如何来度量包含信息的多少呢？一种比较常见的方法就是用[方差(Variance)][Variance]来衡量。这在直观上很容易理解，对于数据的一个维度来说，如果这个维度上的数据具有很大的方差，说明这个维度对于数据来说有很大的差异性，其中包含了更多的信息。

另外，如果两个维度之间是无关的，那么这两个维度所包含的信息是没有"重叠"部分的，这种情况包含的信息是最多的;反过来说，如果两个维度是高度相关的，从一个维度就能推出另外一个维度，那么很显然，这两个相关的维度其实最多只包含了一个维度的信息，这就造成了冗余。那么，我们又应该用什么来衡量两个维度之间的相似程度呢？在数学上，我们可以使用[协方差(Covariance)][Covariance]来衡量两个随机变亮之间的相似程度，因此我们可以利用**协方差**来衡量维度之间的相似程度。协方差为0时，说明两个随机变量是完全无关的。

因此，PCA的基本思想是这样的:

**将高维空间中的点投影(线性映射)到某个低维空间中间，使得投影之后的点**:

1. **每一个维度内部的方差尽量大.**
2. **维度之间的协方差为0,也即每一个维度两两正交。**

假设我们现在有$m\times n$的数据$X$,其中$m$表示数据的大小，$n$表示维度的大小。

对于维度$j$来说，它的方差为:

{% math %}
\begin{aligned}
Var_j &= \frac{1}{m}\sum\limits_{i=1}^m(X_{ij}-\mu_j)^2 \\
\mu_j &= \frac{1}{m}\sum\limits_{i=1}^mX_{ij}
\end{aligned}
{% endmath %}

上式中的$\mu\_j$其实是维度$j$的均值.


对于维度$p$,$q$来说，他们之间的协方差为:


$$
\begin{equation}
Cov_{pq} = \frac{1}{m}\sum\limits_{i=1}^m(X_{ip}-\mu_p)(X_{iq}-\mu_q)
\end{equation}
$$

上式中的$\mu\_p,\mu\_q$其实是维度$q,q$的均值.

如果光看上述的式子，也许会觉得计算$X$的 每一个维度的方差和协方差是非常麻烦的，其实不然，我们可以利用矩阵运算来简化我们的计算过程。

首先，我们注意到不管在计算方差还是协方差的时候，我们都需要计算$\mu_j$，因此，我们可以先对$X$进行如下的处理:

计算每一个维度的均值$\mu\_j$，然后将每个维度上的值减去这个均值，这样就使得每个维度上的均值都变成了0.也即令$X\_{ij}=X\_{ij} - \mu\_j$.

经过这样处理之后，原来的方差和协方差就可以表示为:

{% math %}
\begin{aligned}
Var_j &= \frac{1}{m}\sum\limits_{i=1}^m X_{ij}^{2} \\
Cov_{pq} &= \frac{1}{m}\sum\limits_{i=1}^m X_{ip}X_{iq}
\end{aligned}
{% endmath %}

这看起来还是比较繁琐，但是，事实上，根据矩阵乘法运算的规则，我们可以得到如下的等式:

{% math %}
\begin{equation}
X^{T}X= \begin{bmatrix}
\sum\limits_{i=1}^m X_{i1}^{2} &  \dots & \sum\limits_{i=1}^m X_{i1}X_{in}\ \\
\vdots & \ddots & \vdots \\
\sum\limits_{i=1}^m X_{in}X_{i1} & \dots & \sum\limits_{i=1}^m X_n^2
\end{bmatrix}
\end{equation}
{% endmath %}

可以看到,其实$\frac{1}{m}X^T X$是一个对称矩阵，其对角线上的元素就是我们的**方差**，而其他元素就是对应维度之间的**协方差**!

这看来，计算方差和协方差是非常容易的！

令$C=\frac{1}{m}X^{T}X$,则$C$就称为$X$的协方差矩阵。

我们需要进一步具体化我们的优化目标,令$Y$表示经过映射之后的数据，则$Y=XP$,假设$D$是$Y$的协方差矩阵，那么我们有如下的推导:

{% math %}
\begin{aligned}
D &= \frac{1}{m}Y^t Y \\
&=  \frac{1}{m}(XP)^T(XP)\\
&= \frac{1}{m} P^TX^TXP \\
&= P^T(\frac{1}{m}X^T X)P\\
&= P^T C P
\end{aligned}
{% endmath %}

也就是说，我们的目标可以变得非常具体了:想要找到这样一个矩阵$P$，使得$P^T C P$是一个对角矩阵，其对角线元素从大到小一次排列，并且除了对角线以外的其他元素均为0.

从大到小排列是因为方便选取方差较大的维度，对角线以外的元素为0表示新数据的各个维度之间是相互无关的。

现在，我们的问题就是如何使得协方差矩阵$C$对角化，这在数学上其实早就已经有了成熟的方法，在矩阵论中，有这样的结论:**一个n行n列的实对称矩阵一定可以找到n个单位正交特征向量**.

设$n\times n$的实对称矩阵有特征向量$e\_1,e\_2,\cdots,e\_n$,将其组成矩阵$E=(e\_1,e\_2,\cdots,e\_n)$

则对于$C$来说，有如下的结论:

{% math %}
\begin{equation}
E^TC E = \Lambda = \begin{bmatrix}
\lambda_1 &  & &  \\
 & \lambda_2 & & \\
& & \ddots & \\
& & & \lambda_n \\
\end{bmatrix}
\end{equation}
{% endmath %}

其中$\Lambda$是一个对角矩阵，其对角元素是个特征向量对应的特征值。

到了这里，我们就可以发现，我们想要寻找的$P$其实就是$E$

# 算法过程

根据上面的分析，我们就能够得出计算PCA时的几个步骤:

假设原始数据是$X_{m\times n}$，其中$m$表示数据大小，$n$表示维度大小。

1. 将$X$中的数据进行零均值化，即每一列都减去其均值。
2. 计算协方差矩阵$C=\frac{1}{m}X^T X$
3. 求出$C$的特征值和特征向量
4. 将特征向量按对应特征值大小从上到下按行排列成矩阵，取前k行组成矩阵P
5. $Y=XP$就是降维到k维后的数据。

# 参考资料

- [主成分分析（PCA）][csdn_PCA]
- [PCA的数学原理][CodingLabs]


[csdn_PCA]: http://blog.csdn.net/jirongzi_cs2011/article/details/9499011

[CodingLabs]: http://blog.codinglabs.org/articles/pca-tutorial.html

[PCA]: http://en.wikipedia.org/wiki/Principal_component_analysis

[Variance]: http://en.wikipedia.org/wiki/Variance

[Covariance]: http://en.wikipedia.org/wiki/Covariance


