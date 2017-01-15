---
title: PAC-Learning
tags:
  - machine-learning
  - 机器学习
  - PAC
categories:
  - machine-learning
keywords:
  - machine-learning
  - 机器学习
  - PAC
description: 关于PAC(Probably Approximately Correct) Learning的总结.
date: 2017-01-14 21:10:12
---




# 评价机器学习算法

在之前的{% post_link Mistake-Bound-Algorithm 博文 %}中，我们说明了对于[Online Learning][]的机器学习算法的评价方法--Mistake Bound Algorithm.
今天这篇文章将要分析其他类型的机器学习模型的评价方法。

与[Online Learning][]相对应的另外一种学习模型是**Batch Learning**，与[Online Learning][]不同的是，Batch Learning首先是在固定的训练集上进行训练，训练完成之后用测试集进行测试。
这也是最常见的机器学习模型。
Batch learning关注的是我们的模型在未来数据上的预测能力。

针对[Online Learning][]的Mistake Bound Algorithm只是一种理论方法，根据犯错误的次数来评价一个模型的好坏。
它对数据的分布没有什么要求，也不在乎未来数据。
它能够回答:"我们需要犯多少次错误才能收敛?"，但是却不能回答: "我们需要多少数据才能收敛?"

然而，对于Batch learning来说，我们需要考虑:

1. 假设空间的大小
2. 成功学习的可能性
3. 需要多少样本才能保证学习成功
4. 我们能够目标函数的准确性

这些内容在Mistake Bound Algorithm 中都没有被考虑到。
因此，Mistake Bound Algorithm的分析方法并不适用于Online Learning的模型。

由此，我们引入了[Probably Approximately Correct][](PAC)学习框架。

# 基本概念

## 基本假设

在PAC的学习框架中，我们有两个**重要**的基本假设:

1. 训练集和测试集是来源于同一个分布的。
2. 对于Concept集合中的任何一个Concept，假设空间中总存在一个函数$h$能够满足所有的训练样本。(也就是说，Concept集合是假设空间的一个子集)

第一个假设非常重要，它是Batch Learning的基本前提，这保证了我们从训练集中学习到的知识同样符合测试集。

## 定义错误

对于Batch Learning模型来说，假设空间中的任何一个函数$h$都存在两种不同的错误:

1. 经验错误(Empirical Error): 训练集$S$中被$h$预测错的数据比例。用公式来表示就是:$error_S(h)=Pr_{x\in S}[h(x)\neq f(x)]$.
2. 真实错误(True Error): 对于整个数据分布$D$中的数据，$h$预测错的概率。用公式来表示就是:$error_D(h)=Pr_{x\sim D}[h(x)\neq f(x)]$.

而过拟合的情况其实就是出现$err_S(h) << err_D(h)$时的现象。

# PAC学习

## PAC定义

为了能够衡量Batch Learning，我们需要新的评价方式，能够将假设空间的大小、学习成功率、需要的样本数等条件一起考虑。

首先，我们要知道，不管你的学习器有多么强大，我们是永远无法学习到一个完美的模型，能够对所有的数据(包括训练集和测试集)进行正确地预测。
其次，我们也无法期望学习器能够学习到一个无限接近真实函数的模型，因为训练集总是有限的，不可能涵盖所有的未知情况。
所以，唯一可实现的是，期望我们的学习器能够以**大概率**学习到一个足够**近似**真实函数的模型。
因此我们能够得出PAC的定义:

> 对算法$L$和假设空间$H$来说，如果Concept集合$C$:
> - 对于所有的$f\in C$;
> - 对所有的可能的分布$D$，存在常数$0\le \epsilon, \delta <1$;
>
> 对所有$m$个从$D$中独立采样得到样本，算法$L$能够以$1-\delta$的概率得到一个假设$h\in H$且$h$的真实错误最多是$\epsilon$.
> 并且，此时的$m$是$\frac{1}{\epsilon},\frac{1}{\delta},n$和$\vert H \vert$的多项式结果。
那么,Concept集合是**PAC可学习的**，
> 如果$L$能够在$\frac{1}{\epsilon},\frac{1}{\delta},n$和$\vert H \vert$的多项式时间内产生$h$,那么$C$是**高效可学习**的。

上述的定义非常绕人，需要多读几遍才能正确理解。
如果，理解上还是有困难，可以参考下面奥卡姆剃原理中的公式推导。

## 奥卡姆剃刀原理

[奥卡姆剃刀原理][Occam's razor]是一个解决问题的方法论，它的核心思想其实就是一句话:

**在相同结果的条件下，我们应该选择最简单的模型。**

更加简略的表述是:

**如无必要，勿增实体**

好，现在我们回到PAC问题上。
我们首先提出一个结论:

> 存在一个假设$h$满足:
> 1. 能够和所有的$m$个训练样本保持一致
> 2. 它的真实错误$err_D(h)>\epsilon$
>
> 的概率小于$\vert H\vert (1-\epsilon)^m$.

证明:

假如存在真实错误大于$\epsilon$的假设$h$，那么$h$预测对一个训练样本的概率是$Pr[f(x)=h(x)]<1-\epsilon$.
因为$m$个训练样本是独立同分布的，所以，$h$预测对全部的$m$个样本的概率$<(1-\epsilon)^m$.

根据[Union Bound][],大小为$\vert H\vert$的集合中存在这样的$h$的概率小于$\vert H\vert (1-\epsilon)^m$. 

$\Box$

我们可以进一步推导，我们希望$\vert H\vert (1-\epsilon)^m<\delta$:

{% math %}
\begin{aligned}
    & \ln(\vert H \vert) + m\ln(1-\epsilon) < \ln\delta \\
    &\Rightarrow \ln(\vert H\vert) - m\epsilon < \ln\delta\\
    &\Rightarrow \ln(\vert H\vert) + \ln\frac{1}{\delta} < m\epsilon\\
    &\Rightarrow m > \frac{1}{\epsilon}\left[\ln(\vert H \vert) + \ln\frac{1}{\delta}\right]
\end{aligned}
{% endmath %}

最终我们可以得到如下的不等式:

{% math %}
m > \frac{1}{\epsilon}\left[\ln(\vert H \vert) + \ln\frac{1}{\delta}\right]
{% endmath %}

其中:

- $\frac{1}{\epsilon}$表示随着真实错误率的提高，我们需要更多的训练样本。
- $\ln(\vert H\vert)$表示越大的假设空间，我们需要更多的训练样本。
- $\ln\frac{1}{\delta}$表示置信度越高，我们需要更多的训练样本。

其中的$\frac{1}{\epsilon},\ln(\vert H\vert),\ln\frac{1}{\delta}$正式PAC中所要求的。
因此，对于一个学习器，我们能够使用上述的不等式来判断该学习器是否是PAC可学习的。

同时，这个公式也告诉我们，在$\epsilon$和$\delta$固定的情况下，我们应该选择更小的假设空间，这就体现了奥卡姆剃刀原则的要求。

# 总结

PAC是一种分析机器学习模型的理论框架，它能够同时兼顾多项标准:

- 训练集大小
- 真实错误率
- 置信度

PAC是用来衡量Batch Learning模型的评价体系，它有两个重要的基本假设:

1. 训练集和测试集是来源于同一个分布的。
2. 对于Concept集合中的任何一个Concept，假设空间中总存在一个函数$h$能够满足所有的训练样本。

因为有了第一个假设，我们才能使真实错误和经验错误发生联系；因为有了第二个假设，我们才能使用Union Bound，才能确保$H$中一定包含我们需要学习的目标函数。

其实，这两个假设也是很强的假设，接下去的博文将会说明，当这些条件不满足时，我们该如何分析机器学习模型。

# 参考资料

- 奥卡姆剃刀: [https://en.wikipedia.org/wiki/Occam's_razor][Occam's razor]
- Union Bound: [https://en.wikipedia.org/wiki/Boole%27s_inequality][Union Bound]
- Vivek Srikumar教授的[课件](http://svivek.com/teaching/machine-learning/fall2016/lectures/08-colt.html)

# 更新日志

- 2017年1月14日 完成初稿并发布

[Online Learning]: https://en.wikipedia.org/wiki/Online_machine_learning
[Probably Approximately Correct]: https://en.wikipedia.org/wiki/Probably_approximately_correct_learning
[Occam's razor]: https://en.wikipedia.org/wiki/Occam's_razor
[Union Bound]: https://en.wikipedia.org/wiki/Boole%27s_inequality

