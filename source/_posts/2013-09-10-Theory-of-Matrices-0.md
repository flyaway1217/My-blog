---
layout: post
date: 2013/09/10
title: 矩阵论学习笔记0-线性空间
categories: 
    - matrix-theory
keywords: 矩阵论,数环,数域,线性空间,线性组合,线性无关
tags: 
    - 矩阵论
    - 数学
description: 矩阵论课程的学习笔记
---

本人觉得自己的数学功底一向很差，总是觉得在被数学折磨，本学期学校开了一门矩阵论的课，学工科的朋友都知道矩阵这个东西在工程运算，计算机算法中起着非常重要的作用，为了克服自己的短板，我决定把每次课学到的东西都以博客的方式记录下来，加深自己的理解。由于上课的教授非常强调知识产权的问题，我在写博客的时候会尽量回避上课讲义上的内容，用自己的理解将内容描述出来。

本文是第一次上课的内容，主要是一些具体的定义。


# 数环

**定义1** 设Z为非空数集且其中任何两个数的和、差与积(注意，这里*没有商*)仍属于Z，则称Z是一个**数环**。

数学上还有关于"环"的定义，环的定义和数环有点类似，但是它是定义在**任何集合**上的，而数环是定义在**数集**上的，并且它的和、差、积运算并不一定是指一般意义上的加减乘。环具体的定义可以查看参考资料2。[^1]

数环上有如下的性质:

1. 任何数环都包含数零，即零环是数环中最小的数环。(*这个性质很重要*)
2. 设S是一个数环。若$a\in S$,则$na\in S(n\in Z)$
3. 若M,N都是数环，则$M\cap N$也是数环。

[^1]:学习这个概念的时候，经常把环和数环的定义弄混，后来才发现这其实是两个不同的定义，可以说数环是环的一个特殊情况。

# 数域

**定义2** 如果F是至少含有两个互异数的数环，并且其中任意两个数a与b之商($b\neq 0$)仍然属于F，则F是一个数域。

数域的定义是在数环之上的，在数环的基础之上增加了商的运算，可以说数域是数环的一个特殊情况。

数域的性质:

- 任何数域都包含有**理数域Q**，即Q是最小的数域。[^2]

常用的数域有:复数域$C$,实数域$R$,有理数域$Q$

[^2]:证明过程请查看参考资料3

# 线性空间

**定义3** 设V是一个非空集合，F是一个数域，在集合V的元素之间定义一种代数运算，叫做加法；这就是说，给出了一个法则，对于V中任意两个元素x和y，在V中都有唯一的一个元素z与他们对应，称为x与y的和，记为z=x+y．在数域F与集合V的元素之间还定义了一种运算，叫做数量乘法；这就是说，对于数域F中任一数k与V中任一元素x，在V中都有唯一的一个元素y与他们对应，称为k与x的数量乘积，记为$y=kx$。如果加法与乘法还满足下述规则，那么V称为数域F上的**线性空间**．[^3]

对加法满足：

1. (交换律) $x+y=y+x$；
2. (结合律) $(x+y)+z=x+(y+z)$
3. (零元素) 在V中有一元素0，对于V中任一元素x都有$x+0=x$
4. (负元素) 对于V中每一个元素x，都有V中的元素y，使得$x+y=0$

对数量乘法满足：

1. $1x=x$
2. $k(lx)=(kl)x$

数量乘法和加法满足：

1. $(k+l)x=kx+lx$
2. $k(x+y)=kx+ky$


其中x,y,z为V中任意元素，k,l为数域F中的任意元素，1是F的乘法单位元。
数域F称为线性空间V的**系数域**或**基域**，F中元素称为**纯量**或**数量(scalar)**，V中元素称为**向量(vector)**。
当系数域F为实数域时，V称为实线性空间。当F为复数域时，V称为复线性空间。

需要注意的是，此处加法和数量乘法并不一定是传统意义上的加法和数量乘法，比如:

> $\forall a,b\in R+$及$\forall k\in R$,定义加法为$a\oplus b=ab$, 数量乘法为$k\otimes a=a^k$


可以证明$R^+$对于上述定义的加法$\oplus$和数乘运算$\otimes$是一个线性空间。[^4]

[^3]:这个定义是直接来自参考资料4的，上课时老师给出的定义是这样表述的:"数域F(R或C)上的集合V中定义了下列两种数学运算..."，根据这个定义我的直观理解是V就是一个数域的子集，但其实并不是这样的，V是一个抽象的集合。不太明白是我自己理解不对，还是老师的PPT上表述有错误。

[^4]:这个例子来自于老师的PPT

事实上，可以将**线性空间**看成是一种满足一定条件的集合，而这个条件就是上述所说的对该集合上元素进行加法和数乘的约束条件。

# 线性组合

**定义4** 设V是数域F上的**线性空间**，$x\_1,x\_2,\cdots,x\_n$是V的向量，$\lambda\_i\in F(i=1,2,\cdots,n)$。称V的元素$x=\lambda\_1x\_1+\lambda\_2x\_2+\cdots+\lambda\_nx\_n$是集合$x\_1,x\_2,\cdots,x\_n$的*线性组合*。

# 线性无关

**定义5** 设V施数域F上的**线性空间**，$\alpha\_1,\alpha\_2,\cdots,\alpha\_r(r\geq 1)$是V中的一组向量，如果存在一组不全为0的数$k\_1,k\_2,\cdots,k\_f\in F$，使得$k\_1\alpha\_1+k\_2\alpha\_2+\cdots+k\_r\alpha\_r=0$,则称向量组$\alpha\_1,\alpha\_2,\cdots,\alpha\_r$是*线性相关*的,反之是*线性无关*的。根据**线性组合**和**线性无关**的定义，我们可以得出结论:

**V中一组向量$\alpha\_1,\alpha\_2,\cdots,\alpha\_r$线性相关的充要条件是其中至少有一个向量是其余向量的线性组合。**

# 维数

**定义6** 线性空间V中存在有n个**线性无关**的向量，而任何$n+1$个向量都是**线性相关**的，则称V是*n维*的，记为$dim(V)=n$,如果在V中可以找到任意多个线性无关的向量，则称V是*无限维*的


在实际应用中，常常会把有限维作为无限维的近似计算。

# 基底

**定义7** 设V是数域F上的n维线性空间，则V中一定存在一组线性无关的向量$\alpha\_1,\alpha\_2,\cdots,\alpha\_n$，使得V中任一向量都可以唯一表示成$\alpha\_1,\alpha\_2,\cdots,\alpha\_n$的线性组合，则$\alpha\_1,\alpha\_2,\cdots,\alpha\_n$称为V的一组*基底*.

基底必须满足两个条件:

1. 它是线性无关的(Linearly Independedt)
2. 它的线性组合充满整个空间V(span V)

# 坐标

**定义8** 若$\alpha\_1,\alpha\_2,\cdots,\alpha\_n$是V的一组基底，则$\forall \alpha\in V$可以唯一的表示成$\alpha = x\_1\alpha\_1+x\_2\alpha\_2+\cdots+x\_n\alpha\_n$,其中$x\_1,x\_2,\cdots,x\_n$称为$\alpha$在基$\alpha\_1,\alpha\_2,\cdots,\alpha\_n$下的*坐标*.

**坐标**这个概念非常重要，它给了我们表示具体向量的方法。**向量**最初的定义是**线性空间**中的元素，而**线性空间**是满足特定条件的一个抽象集合，对于抽象的东西，我们很难用具体的数字来表示。

比如，假设在一个直角坐标系中，有一个点$A(1,1)$，其实这样的表述可以理解成:在一个二维的线性空间中，选择$(0,1)$和$(1,0)$作为其基底，则点A可以表示成$(1,1)$。

但是，我们也可以给它换一个基底:如果我们选择$(0,2)$和$(1,0)$作为其基底的话，则点A表示成$(\frac{1}{2},1)$[^5]

[^5]:$\left(\begin{array}{ccc}1\\\1\\\ \end{array}\right)=\frac{1}{2}\left(\begin{array}{ccc}0\\\2\\\ \end{array}\right)+1\left(\begin{array}{ccc}1\\\0\\\ \end{array}\right)$

因此， **坐标**把抽象向量和具体向量相互联系了起来，让我们能够用具体的数字来描述一个抽象的元素。

# 参考资料

1. [百度百科:数环](http://baike.baidu.com/view/522801.htm '数环')
2. [维基百科:环](http://zh.wikipedia.org/wiki/%E7%8E%AF_(%E4%BB%A3%E6%95%B0) '环')
3. [百度百科:数域](http://baike.baidu.com/view/69652.htm '数域')
4. [百度百科:线性空间](http://baike.baidu.com/view/545522.htm '线性空间')
5. 《Linear Algebra Done Right》


