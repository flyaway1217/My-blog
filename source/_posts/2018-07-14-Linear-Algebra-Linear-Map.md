---
title: 向量代数-线性映射
tags:
  - Linear
  - 线性映射
  - 矩阵
  - 秩
categories:
  - words
keywords:
  - 线性映射
  - 矩阵
  - 秩
date: 2018-07-14 09:22:12
description:
---

在{% post_link Linear-Algebra-Vector-Space  %}中谈完了向量空间和向量的表示，我们现在来看一下向量空间中的运动，毕竟，整个线性代数描述的其实是向量空间中的运动。

<escape><!-- more --></escape>

# 线性映射

向量空间中描述的运动不是我们常规认识上的运动，向量空间中的运动都是瞬时的，它和时间无关，只描述方向和大小。
向量空间上线性映射是这么定义的:

> 一个线性映射$T$是从向量空间$V$到向量空间$W$的一个函数，这个函数具有如下的性质:
> - 相加性: $T(u+v)=Tu+Tv$,其中$u,v\in V$
> - 齐次性: $T(\lambda v)=\lambda(Tv)$, 其中$\lambda\in\mathbb{R}, v\in V$

我们可以看到，所谓的线性映射其实也不过是一种函数，将向量从一个空间映射到另外一个空间，且这种映射满足两个特殊的条件。
在数学上，我们用$\mathcal{L}(V,W)$表示所有从$V$到$W$的线性映射.
简单想一想，我们就能知道，$\mathcal{L}(V,W)$其实也是一个向量空间，其中的元素是一个一个的函数，但同时又满足向量空间定义中的条件。
相关的证明，能够在任意的线性代数的教课书上找到。

在给出了线性映射的定义之后，我们又回到了那个老问题，如何表示一个线性映射？总不至于将$V$和$W$中的每一对向量都列出来吧？

这个时候(又是见证人类智慧伟大结晶的时候了)，我们就需要引入矩阵的概念了。
根据我们在上一节中说明的，向量空间中的任意一个向量都可以用一组「基」来唯一的表示。
假设$v_1,\ldots,v_n$是$V$的一组基，$w_1,\ldots,w_m$是$W$的一组基，且$T\in\mathcal{L}(V,W)$, 那么如下的等式一定成立:

{% math %}
\begin{aligned}
Tv_1 &= A_{1,1}w_1+\cdots+A_{m,1}w_m\\
Tv_2 &= A_{1,2}w_1+\cdots+A_{m,2}w_m\\
&\vdots\\
Tv_n &= A_{1,n}w_1+\cdots+A_{m,n}w_m
\end{aligned}
{% endmath %}

$Tv_i$表示的是任何一个基向量经过线性映射之后在$W$中的向量，$w_1,\ldots w_m$又是$W$中的基，因此一定存在一组唯一的$A_{1i},\cdots,A_{mi}$坐标来表示$Tv_i$.
而，$v_1,\cdots,v_n$又能完全表示$V$中的任意一个向量，也就是说，任意个向量$v\in V$的映射后的向量$Tv$都能表示成:

{% math %}
\begin{aligned}
    Tv&=T(a_1v_1+\cdots+a_nv_n)\\
      &= a_1Tv_1+\cdots+a_nTv_n\\
      &=a_1(A_{1,1}w_1+\cdots+A_{m,1}w_m) + \cdots + a_n(A_{1,n}w_1+\cdots+A_{m,n}w_m)\\
      &= (a_1A_{11}+\cdots+a_nA_{1n})w_1 + \cdots + (a_mA_{m1}+\cdots+a_nA_{mn})w_n
\end{aligned}
{% endmath %}

上面的推导想要说明的是，如在原空间中的任意一个向量的坐标是$a_1,\cdots,a_n$的话，那么其经过$T$映射之后的向量的坐标就是$a_1A_{11}+\cdots+a_nA_{1n},\cdots,a_mA_{m1}+\cdots+a_nA_{mn}$.也就说，一旦确定了两个空间的基之后，通过$m\times n$个数，就能确定一个线性映射了。

那么，这个映射后的坐标$a_1A_{11}+\cdots+a_nA_{1n},\cdots,a_mA_{m1}+\cdots+a_nA_{mn}$
有没有觉得很眼熟？
将这个坐标表示成:


{% math %}
\left[
\begin{matrix}
A_{11} & \cdots & A_{1,n}\\
\vdots &  &\vdots \\
A_{m1} & \cdots & A_{mn}
\end{matrix}
\right]
\left[
\begin{matrix}
a_1 \\ \vdots\\a_n
\end{matrix}
\right] =
\left[
\begin{matrix}
a_1A_{11}+\cdots+a_nA_{1n}\\
\vdots\\
a_1A_{m1}+\cdots+a_nA_{mn}\\
\end{matrix}
\right]
{% endmath %}

这不就是当年我们学习矩阵和向量的乘法吗？
因此，我们就可以得出结论，所谓的矩阵其实就是线性映射的一种表示方式。

## 矩阵的秩

现在，我们知道，矩阵的每一列$A_{1i},\cdots,A_{mi}$其实是原空间上的一个基向量$v_i$经过矩阵$A$代表的线性映射$T$在新空间上的一个坐标。

也就是说$A_{1i},\cdots,A_{mi}$其实也是一个($W$空间里的)向量(其基是$w_1,\cdots,w_m$)。
我们现在用$A_i$来表示整个$A_{1i},\cdots,A_{mi}$向量序列。
我们知道，$A_i,i\in \{1,2,\cdots,m\}$是$W$上的一组向量，那么我们不经要问$span(A_1,\cdots,A_m)$是否等于整个$W$呢？

这个问题是很重要的，如果$span(A_1,\cdots,A_m)$只是$W$的一个子集，那也就是说，这个矩阵包含了比较少的信息，只能覆盖$W$的一个子集;
相反，如果$span(A_1,\cdots,A_m)$等于整个$W$，那么就说明$A$包含了''足够''的信息，能够将整个$V$映射到整个$W$上。
那么我们该如何判断$A$包含了多少信息呢？

一种度量方式就是看$span(A_1,\cdots,A_m)$的维度，如果$span(A_1,\cdots,A_m)$的维度等于$W$的维度，那么$span(A_1,\cdots,A_m)$就等于整个$W$.
而$span(A_1,\cdots,A_m)$其实就是我们常说的矩阵的秩。
我们平时所说的满秩矩阵其实就是说这个矩阵代表的线性映射能够覆盖整个目标空间。

当然，在实际应用中，我们通常比较喜欢的是低秩矩阵，因为它不是满秩的，说明线性映射之后，只是$W$的一个子空间，说明$A$的信息量少，这样的矩阵可以进行降维，可以进行大幅度的压缩。

# 参考资料

- [Linear Algebra Done Right](https://www.amazon.com/Linear-Algebra-Right-Undergraduate-Mathematics/dp/3319110799/ref=sr_1_1?ie=UTF8&qid=1530890765&sr=8-1&keywords=Linear+algebra+done+right)

# 更新日志

- 2018年7月11日写作
- 2018年7月14日发表
