---
title: 三种梯度下降法
date: 2016-03-28 20:13:22
updated: 2016-03-28 20:40:22
tags: 
    - gradient-descent
    - SGD
    - BGD
    - mini-batch
categories: 
    - machine-learning
description: 总结了三种最常用的梯度下降算法
---

# 写在前面

最近需要手写一个Feedforward的网络结构，在选择优化算法的时候，猛然发现原来Batch Gradient Descent和mini-Batch Gradient Descent是两种不同的优化策略，感觉这里遗漏了一大块知识，赶忙补充了一下，此处做一下记录。

# Batch Gradient Descent

在优化目标函数的时候，Batch Gradient Descent(BGD)是先计算整个数据集上的梯度，然后再进行更新操作。对于参数$\theta$来说，每更新一次其中的某一位权重$\theta_j$，BGD都需要遍历整个数据集。

对于目标函数$h_{\theta}(x)$用公式来表示就是:

{% math %}
\begin{equation}
\theta_j := \theta_j + \alpha\sum\limits_{i=1}^m(y^{(i)}-h_{\theta}(x^{(i)}))x_j^{(i)} \label{BGD}
\end{equation}
{% endmath %}

其中的{% math %}(y^{(i)}-h_{\theta}(x^{(i)}))x_j^{(i)}{% endmath %}其实就是对于训练样例{% math %} x^{(i)} {% endmath %}的$j$属性的梯度,$m$是训练集的大小,具体的推导过程可以在{% post_link Gradient-Descent 这里 %}查看。

从公式$\ref{BGD}$中可以看到，BGD是对整个数据集进行扫描然后计算整体梯度($\sum$求和过程)，进行更新。其实，这才是真正的梯度.

BGD的优点在于对于凸问题，它是能够保证收敛到全局最优点的。而缺点就是，计算量很大，计算每一位的权重都要遍历整个数据集，这代价未免太大了，计算量是无法接受的。随之而来的另外一个缺点就是BGD是无法进行online训练的，它必须要知道全部的训练集的情况下才能进行训练，这对于一些线上系统也是一个问题。


# Stochastic Gradient Descent

SGD是对BGD的一个改进方案，改变之处在更新时不需要遍历整个数据集，而是每一个实例都进行更新。具体公式是:

{% math %}
\begin{equation}
\theta_j := \theta_j + \alpha(y^{(i)}-h_{\theta}(x^{(i)}))x_j^{(i)} \label{SGD}
\end{equation}
{% endmath %}

比较公式$\ref{BGD}$和$\ref{SGD}$，我们可以发现区别就在省略了求和过程$\sum$，也就是说更新权重的时候，不需要计算整体的梯度，而是仅仅依靠当前实例的梯度进行更新。

如此改变之后，速度明显提高了很多，但是这也是有风险的。由于进行频繁的梯度更新，很有可能直接跳过了最优点。因此，SGD实际上是无法保证收敛到全局最优点的，而且不是那么的稳定。

# Mini-Batch Gradient Descent

而Mini-Batch是对上述两种策略的一种中和，它的基本思想就是从整个训练集上选取一个子集，对这个自己进行BGD的更新。具体公式可以表示为:

{% math %}
\begin{equation}
\theta_j := \theta_j + \alpha\sum\limits_{i=1}^n(y^{(i)}-h_{\theta}(x^{(i)}))x_j^{(i)} \label{mini-BGD}
\end{equation}
{% endmath %}

比较公式$\ref{BGD}$和$\ref{mini-BGD}$会发现唯一的区别在于求和时的项数不一样，此处的$n$不再是训练集的大小，而是一个小于或等于$m$的数，通常范围在于50-256。

简单来说，先把大小为$m$的训练集平均分为大小为$n$的$\frac{m}{n}$个子集，每次读入一个子集，进行梯度计算，更新权重。

相比SGD来说，它更加稳定；相比BGD来说，它计算量较小。

# 总结

|                |     BGD    |   SGD  |  mini-Batch  |
|:--------------:|:----------:|:------:|:------------:|
|     训练集     |    固定    |  固定  |     固定     |
| 单次迭代样本数 | 整个训练集 |  固定  | 训练集的子集 |
|   算法复杂度   |     高     |   低   |     一般     |
|     收敛性     |    稳定    | 不稳定 |    较稳定    |

