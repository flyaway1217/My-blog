---
title: Mistake Bound Algorithm
tags:
  - mistake-bound-algorithm
  - Halving
categories: machine-learning
description: Mistake Bound Algorithm的理解。 1. Generic Mistake Bound Algorithm 2. Halving Algorithm
date: 2016-10-10 20:04:51
keywords:
---





# 前言

在机器学习中，如何衡量一个机器学习模型的好坏是非常重要的一个方面。
对于[Online Learning][]模型来说，它主要的衡量标准就是它的Mistake Bound。
Mistake Bound用一个模型在停止训练前所犯的错误次数来衡量一个模型的好坏。
当然，对于一个online模型来说，训练过程中它犯的错误越少越好。

# Online Learning

[Online Learning][]是一种基本的机器学习策略，它是一种错误驱动的学习模型。
学习器无法看到整体数据集合，它一次只能看到一个数据实例，处理完当前实例之后，当前实例将会被丢弃。
然后接着处理下一个实例。
整个过程就像工厂里的流水线，样本实例一个一个顺序地过来，学习器一个一个地处理，它永远无法一次看到两个以上的实例。

假设样本空间是$X$，目标函数是$f: X\rightarrow \\{0,1\\}$.
则Online Learning的基本框架是:
1. 学习器$h$遇到一个样本$x_i\in X$
2. 学习器$h$作出一个预测$h(x)$
3. 如果$h(x)\neq f(x)$，则学习器进行更新

当$h(x)\neq f(x)$时，我们就认为模型发生了一次错误，我们的目标是希望在训练过程中，这样的错误发生的越少越好。

## 优点

1. 对数据分布没有要求
2. 不需要很多的内存消耗
3. 可以容易的转化为batch learning

## 缺点

1. 太过简单
2. 无法预测错误的发生

# Mistake Bound Algorithm

首先，我们需要给出两个定义:

1. $M_A(f,S)$表示: 算法$A$在训练序列$S$上学习目标函数$f$时所犯的错误次数。
2. $M_A(C)=\max\limits_{f,S}M_A(f,S)$表示: 算法$A$在任意的训练序列$S$上学习任意的目标函数$f$时最多所犯的错误次数。

则，如果一个算法$A$满足:*$M_A(C)$和特征空间的维度$n$是多项式关系的*，那么我们就称算法$A$是一个**Mistake Bound Algorithm**.
这里的$n$用来表示问题的输入规模，而$M_A(C)$表示问题的学习代价，如果这两个值是多项式关系的，那么该算法的犯错次数是有一个上界的。


# Generic Mistake Bound Algorithm

我们首先介绍一种最基本的Mistake Bound Algorithm.
抽象的常规Mistake Bound Algorithm的学习流程如下所示:

在每一次的迭代过程中:
1. $H_i$表示在第$i$步时的假设空间。
2. 随机地从$H_i$中取出一个函数$h\in H_i$,然后用它去进行预测。
3. 如果错误发生了($h(x)\neq f(x)$)，那么将其从假设空间中去除。
4. 我们得到$H_{i+1}\subseteq H_i$.如果发生了错误，那么$\mid H_{i+1}\mid < \mid H_i \mid$.

不断循环重复上述过程，我们就能逐渐缩小$H_i$，直至$H_i$只剩下一个假设函数。
因此这个算法最多会犯$\mid H_0\mid-1$次错误。

很明显，这个算法其实就是一种穷举的方式，尝试假设空间中所有的函数，直至只剩下最后一个。
因此它的Mistake Bound就是它假设空间的大小。


# Halving Algorithm

除了上述这种穷举的方式以外，我们还有一种更快的算法，称为Halving Algorithm.
其过程如下:
1. 初始化$H_0$为整个假设空间
2. 当一个样本$x$到来时:
    1. 使用$H_i$中的每一个函数进行预测，选取多数函数预测的标记作为最终的预测结果。
        如果多数函数预测为$0$，则最终的预测结果就是$0$，反之亦然。
    2. 如果预测与真实值($f(x)$)不同，则$H_{i+1}=H_i\text{中所有预测值与f(x)相同的函数}$

不断重复上述过程，直至$H_i$中只剩下一个元素(函数).

那么对于Halving算法来说，它的Mistake Bound是多少呢？

其实只要有一点算法基础的人都能看出来，整个Halving算法其实就是一个二分搜索的过程，其Mistake Bound是$\log \mid H \mid$.

证明:
{% math %}
\begin{aligned}
1 = \mid H_n \mid &< \frac{1}{2}\mid H_{n-1} \mid\\
                  &< \frac{1}{2}\mid H_{n-2} \mid\\
                  &< \cdots\\
                  &< \frac{1}{2^n}\mid H_0 \mid = \frac{1}{2^n}\mid H \mid\\
                  &\Downarrow\\
            \mid H \mid &> 2^n \\
                  &\Downarrow\\
            n &< \log \mid H \mid
\end{aligned}
{% endmath %}


# 总结

Mistake Bound限制了一个学习算法在停止前的犯错次数，这对于Online Learning来说是比较重要，这使得我们能够明确地知道，最多需要犯多少次错误我们的算法才能学得足够好！

但是，我们也能看到，上述的这两个算法都是非常理想化的算法，在实际使用过程中我们永远无法穷举整个假设空间$H$.
在接下去的博文中我将会讨论两个具体的[Online Learning][]的算法，并分析其Mistake Bound.


# 参考资料

- Vivek Srikumar教授的[课件][Vivek]

# 更新日志

- 2016年10月9日写成初稿并发布。
- 2016年10月11日 润色部分细节问题

[Online Learning]: https://en.wikipedia.org/wiki/Online_machine_learning
[Vivek]: http://svivek.com/teaching/machine-learning/fall2016/lectures/06-online-learning-perceptron.html
