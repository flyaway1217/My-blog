---
title: Perceptron的Mistake Bound
tags:
  - mistake-bound
  - perceptron
  - machine-learning
categories:
  - machine-learning
keywords:
  - mistake-bound
  - perceptron
  - machine-learning
description: Perceptron的mistake bound证明. 内容提要 1. Perceptron的Mistake Bound是什么 2. 如何证明
date: 2016-10-10 22:02:01
---




大概3年前第一次学习[Perceptron(感知机)][Perceptron]的时候，就已经写过相关的{% post_link Statistical-Learning-Perceptron 博文 %}了。
但是，当时仅仅只是在搬运知识而已，其实当时很多内容自己都是一知半解的。
但是现在随着学习的深入，逐渐领悟到了更多的东西，逐渐理清了整个体系。
因此，觉得有必要再好好整理输出一下，加深理解。

# Perceptron(感知机)

[Perceptron(感知机)][Perceptron]算是机器学习中最简单的模型之一了，其基本框架如下所示:

**Input**: A sequence of training examples $(x_1,y_1),(x_2,y_2),\cdots$, 其中$x_i\in R^n,y_i\in \\{-1,1\\}$

1. 初始化: $w_0=0\in R^{n+1}$
2. 对于每一个训练样本$(x_i,y_i)$:
    - 使用权重$w_t$进行预测:{% math %}y^{'}=sgn(x_t^T x_i){% endmath %}
    - 如果$y_i\neq y^{'}$: 更新$w_{t+1}=w_t+r(y_ix_i)$
3. 返回最终的权重

# Perceptron的Mistake Bound

Perceptron是针对线性可分数据的一种分类器，它属于[Online Learning][]的算法。
我在之前的一篇{% post_link Mistake-Bound-Algorithm 博文 %}中提到了[Online Learning][]模型的Mistake Bound衡量标准。
现在我们就来分析一下Perceptron的Mistake Bound是多少。

在分析其Mistake Bound之前，我们首先需要定义几个概念。

1. 我们使用$R$表示训练数据集中距离原点最远的点的距离。
    也就是说，对于数据集$(x_1,y_1),(x_2,y_2),\cdots,(x_m,y_m)$来说，其中任意一个实例$(x_i,y_i)$都满足$\Vert x_i \Vert\le R$.
2. Perceptron最终学习到的其实是一个分割正负例数据的超平面，我们定义数据集中距离超平面距离最近的点的距离为$\gamma$

在定义了上述两个概念之后，我们就能给出结论:**Perceptron的Mistake Bound是: $(\frac{R}{\gamma})^2$**.

也就是说，Perceptron在训练集上最多会犯$(\frac{R}{\gamma})^2$次错误.
这被称为**Novikoff定理**.


# Novikoff定理的证明

在开始我们的证明过程之前，我们首先需要明确一些基本设定:

1. 数据是线性可分的
2. 权重初始值为0向量，且更新步长为1.
3. 训练集中最远点的距离为$R$
4. $u$是一个单位向量，且能够完美分割数据的最优超平面(因为已经假设数据是线性可分的，因此$u$必然是存在的)
5. $\gamma$是数据集中距离超平面$u$最近点的距离

另外，我们需要知道Perceptron的更新条件和更新法则:

1. Perceptron更新条件是: $y_i(w_t x_i) < 0$
2. Perceptron的更新法则是: $w_{t+1}=w_t+r(y_i x_i)$

要证明Novikoff定理，我们只需要证明如下两个不等式.

## 1. $u^Tw_t\ge t\gamma$

假设Perceptron在学习过程中已经犯了$t$次错误,则:

{% math %}
\begin{aligned}
u^Tw_{t+1} &= u^T(w_t+y_ix_i)\\
           &= u^Tw_t + u^T y_i x_i\\
           &\ge u^Tw_t + \gamma
\end{aligned}
{% endmath %}

1. 其中第一个等号是根据Perceptron的更新法则得到的
2. 不等号是因为: 实际上$u^T y_i x_i$表示的是点$x_i$到超平面$u$的距离，因此这个距离必然大于等于$\gamma$.

又因为$w$的初始值为$0$向量，利用归纳法，我们能得到$u^Tw_t\ge t\gamma$.


## 2. $\Vert w_t\Vert^2 \le tR^2$

假设Perceptron载学习过程中已经犯了$t$次错误，则:

{% math %}
\begin{aligned}
\Vert w_{t+1} \Vert^2 &= \Vert w_t+x_i y_i \Vert^2 \\
                      &= w_t^2 + 2w_t x_i y_i + x_i^2\\
                      &\le w_t^2 + R^2 + 2w_t x_i y_i \\
                      &\le w_t^2 + R^2
\end{aligned}
{% endmath %}

1. 其中第一个等号是根据Perceptron的更新法则得到的
2. 第一个不等号是根据$R$的定义得到的
3. 第二个不等号是因为根据Perceptron的更新条件:$w_t x_i y_i <0$.

又因为$w$的初始值为$0$向量，利用归纳法，我们能得到$\Vert w_t\Vert^2 \le tR^2$.


## 综合

根据前面两个不等式，我们有:

1. $\Vert w_t \Vert \le \sqrt{t}R$
2. $u^Tw_t\ge t\gamma$
3. $u^Tw_t \le \Vert w_t\Vert$

第三个不等式是因为:
{% math %}
u^Tw_t = \Vert u^T \Vert \cdot \Vert w_t \Vert \cdot \cos(\theta) = \Vert w_t \Vert \cdot \cos(\theta) \le \Vert w_t\Vert
{% endmath %}

因此根据这三个不等式，我们能得到:

{% math %}
t\gamma \le u^Tw_t \le \Vert w_t\Vert \le \sqrt{t} R  \Rightarrow t\gamma \le \sqrt{t}R \Rightarrow t\le(\frac{R}{\gamma})^2
{% endmath %}

证明完成!

# Novikoff定理

Novikoff定理说明了Perceptron的可学习性，也就是说针对任意的线性可分的数据，Perceptron最多犯$(\frac{R}{\gamma})^2$次错误就能学到最优的超平面。
同时，也说明了Perceptron算法的收敛性: 只要数据是线性可分的，该算法就一定会收敛。

# 参考资料

- Vivek Srikumar教授的[课件][Vivek]

# 更新日志

- 2016年10月10日完成初稿并发布。
- 2016年10月11日修正了一些书写错误。

[Perceptron]: https://en.wikipedia.org/wiki/Perceptron
[Online Learning]: https://en.wikipedia.org/wiki/Online_machine_learning
[Vivek]: http://svivek.com/teaching/machine-learning/fall2016/lectures/06-online-learning-perceptron.html
