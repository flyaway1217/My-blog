---
title: 为什么word embedding不能是一维的？
tags:
  - embedding
  - 泛化性
categories:
  - words
keywords:
  - embedding
  - 泛化性
date: 2018-09-08 10:27:30
description:
---




自从开始接触word embedding之后，其实我一直在思考为什么word embedding必须是高维的？
要知道，即使只有一个维度，只要它是一个实数，理论上它也能够包含无穷的信息。
可是，为什么word embedding必须是高维的？

<escape><!-- more --></escape>

虽然这个问题不影响我去理解各种各样的embedding模式，但是在学习和使用embedding的过程中，总觉得缺了一点什么的东西，让我无法彻底理解word embedding.

知道最近我开始阅读关于embedding最早的几篇论文，尤其是Hinton在三十多年前的{% link Distributed Representation  http://web.stanford.edu/~jlmcc/papers/PDP/Volume%201/Chap3_PDP86.pdf Distributed Representation %}，我才慢慢开始理解所谓的embedding到底是什么东西。

首先，回到我最初提出的问题，为什么embedding不能是一维的？多维的向量能够表示的信息，我用一个实数也能表示，因为实数域本是就是无限的，我们可以把任何信息编码为一个实数。
那么高维的优势到底在什么地方呢？

根据Hinton的分析，高维度embedding的优势在于泛化性。
一个实数确实可以表示无穷的信息，但是它缺乏足够的泛化性。
比如，假设我用维度为4的embedding来表示「猫」和「狗」的概念。
「猫」和「狗」这概念具有很强的相似性: 它们都是人类的宠物，都是哺乳动物。
因此，它们的embedding应该具有很强的相似性。
比如「猫」这个概念的embedding可以是$<1,10,1,2>$，「狗」的embedding可以是$<2,20,3,4>$。
而这两个embedding的相似性体现在其前两个维度是1比10的比率，这是一种pattern，也许这种pattern表示的是「人类宠物」这一抽象概念。
而如果，我们将「猫」和「狗」这两给概念编码为两个实数，它就失去了这种pattern的表示。
这种pattern的优势在于，它不限于「猫」和「狗」这两个概念，同样的pattern可以出现在其他概念的embedding中。
比如，金鱼的embedding可以是$<3,30,1,30>$，它同样包含了「人类宠物」这一pattern。
即使我们不知道它表示的是什么，我们也能知道它表示一定是某一种「人类宠物」。
这就是泛化性。
这是只有一个维度的embedding难以做到的事情。
维度越高，那么embedding能够包含的pattern就越多，因此在一般情况下，高维的embedding通常要比低维的质量要高。

当年刚接触embedding概念的时候，最惊艳的是觉得它最大的优点在于使得很多语义上的抽象概念变得可计算了。
任何一个语义、抽象概念都可以表示为空间中的一个向量，而我们对于向量可以应用各种计算。
在理解了泛化性之后，我发现这才是embedding真正的优势。
它可以将一个抽象表示打散成多个维度上面的表示，从而形成一系列的pattern，而这些pattern是可以出现其他不同的embedding中的，这大大增加了这种表示方式的泛化能力。
而可计算的特点仅仅是这种泛化性的一个副作用而已。


# 参考资料

- [Distributed Representation][]

# 更新日志

- 2018年9月8日写作并发表

[Distributed Representation]:http://web.stanford.edu/~jlmcc/papers/PDP/Volume%201/Chap3_PDP86.pdf
