---
title: 线性代数-向量空间
tags:
  - 向量空间
  - 坐标
  - 基
  - 线性组合
  - 线性无关
categories:
  - words
keywords:
  - 向量空间
  - 坐标
  - 基
  - 线性组合
  - 线性无关
description: 本文将会从向量空间的定义开始，真正理解什么才是向量和向量空间。通过逻辑梳理，理解为什么向量可以表示成一组实数的形式。
date: 2018-07-06 21:12:08
---




最近乘着暑假，空闲时间比较多，花了两周的时间，重新复习了一下线性代数的一些基本概念。
这次的复习，我只关注于各种概念的直观意义及其之间的相互关系.
这个系列的文章全都没有相应的证明，
我只希望能够在直觉上增进对线性代数的理解，具体的各种定义和定理，就留给数学专业的人去解决好了。

本文将会从向量空间的定义开始，真正理解什么才是向量和向量空间。
通过逻辑梳理，理解为什么向量可以表示成一组实数的形式。

# 向量空间的定义

所谓的向量空间本质就是满足一定条件的集合，这个集合中的元素就被称为**向量**。
假设存在一个集合$V$及其其中任意的三个元素$u,v,w$, 如果这个集合满足如下条件[^1][^2]，那么这个集合就被称为**向量空间**.


> - 向量加法$+$: 一个映射，将$V$中的任意两个元素映射到$V$中的另一个元素。$V\times V\rightarrow V$. $u+v\in V$
> - 标量乘法: 把一个标量$a$和$V$中的一个元素映射为$V$中的另一个元素.$R\times V\rightarrow V$ $av\in V$.
> - 交换律: $u+v=v+u$.
> - 结合律: $(u+v)+w=w+(v+w)$ 且 $(ab)v=a(bv)$, 其中$a,b\in\mathbb{R}$.
> - 分配率: $a(u+v)=au+bv$ 且 $(a+b)v=av+bv$，其中$a,b\in\mathbb{R}$.
> - 加法恒等: 集合$V$中存在一个特殊的元素$z$，对于$V$中的任意元素,满足$v+z=v$.
>             (我们给这个特殊的元素起了个名字，称为$0$.)
> - 加法逆元: 对于$V$中的任意元素$v\in V$, 都存在一个$z\in V$，使得$v+z=0$
>             (我们在这个集合上定义了减法)
> - 乘法恒等: 存在一个特殊元素$1\in\mathbb{R}$，满足$1v=v$.

让我们仔细来看一下向量空间定义中的这些条件:
首先，前两个条件定义了集合上的两种运算，「加法」和「标量乘法」，接下去的三个条件要求这两个运算满足三种特性: 交换律、结合律和分配率，后面两个个条件又约束了向量空间中必须存在的特殊元素: $0$和加法逆元。
实际上，正是因为加法逆元的存在，我们就在向量空间上定义了「减法」。
最后一个条件定义了一个特殊的标量$1$.

所有满足这些条件的集合$V$都可以被称为是「向量空间」.
需要注意的是，在向量空间的定义中，我们并没有说明这些元素是什么。
我们只是定义了这些元素之间的运算法则。
实际上，这些元素可以是任意的对象，可以是函数，可以是数字，可以是列表，只要这个集合满足向量空间定义中的条件。
$\mathbb{R}^n$就是一个常见的向量空间，它的元素是一个长度为$n$的列表，列表中的元素是$\mathbb{R}$中的元素.

另外需要了解的是，关于子空间的定义。
一个向量空间$V$的子空间$U$定义为: $U$是$V$的一个子集，且$U$也是一个向量空间。

好了，这些就是我们关于向量空间定义所需要了解的全部内容。
其实非常简单，所谓向量空间就**是一个满足一些特殊条件的集合，集合元素可以是任意的对象。**

# 向量的表示

在定义了向量空间之后，我们接下去想要解决的问题就是如何表示一个向量？
根据向量空间的定义，向量空间中的元素「向量」可以是任意对象，可以是一个函数，可以是一个数字，可以是另外一种集合。
为了避免当我们说一个向量$v$，每次都要说明这个向量是什么，我们需要一种表示方式来表示任意一个向量，而忽略其本体是什么。
因为，对于向量空间来说，我们关心的其实是向量之间的关系，而不是向量本身。(这也是为什么在向量空间的定义中，我们没有限制其中的元素是什么，而只是定义了元素之间的运算.)
那么，我们有没有办法用统一的方式来表示向量，而不用考虑这个向量本身代表的是什么呢？

答案当然是可以的(下面将是展现人类智慧伟大结晶的时刻!)。

但是在表示任意一种向量的时候，我们需要引入一些新的定义来帮助我们得到向量的表示方式。
对于一组向量序列$v_1,\ldots,v_m\in V$, 我们定义它的「线性组合[^3]」为

$$
a_1v_1+\cdots+a_mv_m
$$
其中$a_1,\ldots,a_m\in\mathbb{R}$.

根据向量空间的定义，我们很容易知道，一组向量序列的线性组合必定也是同一个空间中的向量。
在此基础上，我们又可以给出「线性生成空间[^4]」的定义:

> 一个向量序列的所有线性组合组成的集合称为这个向量序列的线性生成空间。
> 我们用$span(v_1,\ldots,v_m)$来表示$v_1,\ldots,v_m$的线性生成空间。
> $$
span(v_1,\ldots,v_m) = \{a_1v_1+\cdots+a_mv_m:a_1\cdots,a_m\in\mathbb{R}\}
> $$

容易验证，$span(v_1,\ldots,v_m)$也是一个线性空间。
注意，此时我们从没说过线性组合是唯一的。
也就说，可能存在两组不同的$a_1,\ldots,a_m$和$b_1,\ldots,b_m$，使得$v=a_1v_1+\cdots+a_mv_m$ 和$v=b_1v_1+\cdots+b_mv_m$同时成立。

现在我们来仔细看看线性组合，我们到底为什么要定义这个「线性组合」的概念。
根据线性组合和线性生成空间的定义，任何一个$span(v_1,\ldots,v_m)$中的元素都可以被表示为$v_1,\ldots,v_m$的一个线性组合。

那么，我们能不能做出这样的猜想，如果这个$span(v_1,\ldots,v_m)$等于整个$V$空间，而且每个$v\in V$都有且只有一组$a_1,\ldots,a_n$使得$v=a_1v_1+\ldots+a_mv_m$成立，那么这个$v$就能用$(a_1,\ldots,a_m)$这个序列来唯一表示了。
那么我们可以得出结论，$v$能够被$(a_1,\ldots,a_m)$唯一表示需要满足两个条件:

1. $span(a_1,\ldots,a_m)$等于整个向量空间$V$
2. 有且只有一组$(a_1,\ldots,a_m)$使得$v=a_1v_1+\ldots+a_mv_m$成立，其中$v\in V$.

进一步分析，这两个条件是由什么决定的？
仔细想想，我们就知道，这两个条件完全是由$v_1,\ldots,v_m$这个序列决定的，如果选择得很巧，能够满足上面这两个条件，那么$V$中的任意一个元素都能被唯一地表示为$a_1,\ldots,a_m$这样的形式，而可以忽略这个元素本体是什么。

对于第一个条件来说，我们没有什么能做的。
而对于第二个条件来说，它有另外一种等价的形式，称为「线性无关[^5]」(是不是感到很熟悉?):

> 对于一个向量序列$v_1,\ldots,v_m$来说，如果能够使得$a_1v_1+\cdots a_mv_m=0$成立的唯一选择是$a_1=\cdots=a_m=0$，那么这组向量序列$v_1,\ldots,v_m$就被称为线性无关。

线性无关和上面提到的第二个条件是等价，相关的证明能在任何一本线性代数教材中找到证明。

好了，现在我们知道，要唯一表示一个向量，我们只需要找到满足下列两个条件的一组向量:

1. 这组向量的线性生成空间等于原来的向量空间
2. 这组向量是线性无关的

一旦找到这样一组的向量，我们就能够用一组实数序列$(a_1,\cdots,a_m)$来唯一表示空间中的一个向量。
这样的一组向量是如此的重要，我们把它称之为「基[^6]」.

一旦为一个向量空间选定了一组基，那么这个向量空间中的任意一个元素都可以被唯一的表示   。
而这种唯一表示的形式就是我们熟知的坐标形式,比如$(1,2,3)$.
平时，我们提到向量的时候，通常都是默认向量空间的基是标准基。
比如，对于向量$(1,2,3)$来说，它的基就可以是$(1,0,0),(0,1,0),(0,0,1)$，因为

$$
(1,2,3) = 1(1,0,0) + 2(0,1,0) + 3(0,0,1)
$$

但是，需要知道的是，不是所有的特征空间都能找到这样的基的。
能够找到基的特征空间称为「有限维向量空间」.
通常我们使用都是有限维向量空间。
其中，这组基的个数，就是被称为这个向量空间的维度。


# 总结

1. 所谓向量空间，其实就是满足一定条件的集合，集合中的元素可以是任意对象，可以是一个函数、一个数，甚至可以是一个集合。
2. 向量空间的中的元素称为向量。
3. 向量空间中的元素都可以用一组向量序列的线性组合来表示。
4. 如果一组向量序列能够唯一地表示向量空间中的每一个元素，那么这组向量序列就被称为向量空间的「基」
5. 一个向量空间存在很多组不同的基

# 参考资料

- [Linear Algebra Done Right](https://www.amazon.com/Linear-Algebra-Right-Undergraduate-Mathematics/dp/3319110799/ref=sr_1_1?ie=UTF8&qid=1530890765&sr=8-1&keywords=Linear+algebra+done+right)

[^1]: [Linear Algebra Done Right](https://www.amazon.com/Linear-Algebra-Right-Undergraduate-Mathematics/dp/3319110799/ref=sr_1_1?ie=UTF8&qid=1530890765&sr=8-1&keywords=Linear+algebra+done+right)
[^2]: [向量空间 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E5%90%91%E9%87%8F%E7%A9%BA%E9%97%B4)
[^3]: [线性组合 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E7%BA%BF%E6%80%A7%E7%BB%84%E5%90%88)
[^4]: [线性生成空间 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E7%BA%BF%E6%80%A7%E7%94%9F%E6%88%90%E7%A9%BA%E9%97%B4)
[^5]: [線性無關 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E7%B7%9A%E6%80%A7%E7%84%A1%E9%97%9C)
[^6]: [基 (線性代數) - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E5%9F%BA_(%E7%B7%9A%E6%80%A7%E4%BB%A3%E6%95%B8))


# 更新日志

- 2018年7月6日写作并发表
