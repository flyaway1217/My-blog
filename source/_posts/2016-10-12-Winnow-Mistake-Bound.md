---
title: Winnow算法及其Mistake Bound
tags:
  - mistake-bound
  - Winnow
  - machine-learning
categories:
  - machine-learning
keywords:
  - mistake-bound
  - Winnow
  - machine-learning
description: Winnow算法的介绍及其Mistake Bound的证明。
date: 2016-10-12 15:59:09
---




# Winnow算法

[Winnow][]和{% post_link Perceptron-Mistake-Bound Perceptron %}算法很相似，都是一种[Online Learning][]的算法，主要区别在于其更新方式的不同。

Winnow算法的基本设定如下:

**Input**: A sequence of training examples $(x_1,y_1),(x_2,y_2),\cdots$, 其中$x_i\in R^n,y_i\in \\{-1,1\\}$

1. 初始化: $w_0=1\in R^n$, $\theta = n$
2. 对于每一个训练样本$(x_i,y_i)$:
    1. 使用$w_t$和$\theta$进行预测: {% math %}y^{'}=sgn(w_t^Tx_i - \theta){% endmath %}
    2. 如果$y_i=+1$而$y^{'}=-1$，则:
        - 对于$w_t$中所有$x_i$不为零的权重进行更新: $w_t=2w_t$
    3. 如果$y_i=-1$而$y^{'}=+1$，则:
        - 对于$w_t$中所有$x_i$不为零的权重进行更新: $w_t=\frac{w_t}{2}$
3. 返回最终的权重

我们可以看到，[Winnow][]算法的框架和{% post_link Perceptron-Mistake-Bound Perceptron %}是非常接近的，只不过**Perceptron**是加法式更新，而**Winnow**是乘法式更新。

## 适用问题

从上述的算法流程中，我们可以看到，**Winnow**算法每次更新时，只更新部分的权重，提高那些有正影响的权重，而降低那些有负影响的权重。
因此，对于那些维度($n$)很大，但实际真正有影响力的维度($k$)却很小($k\ll n$)的问题，**Winnow**应该是一个更好的选择。
在这里，我们能给出**Winnow**算法的Mistake Bound: **Winnow算法的Mistake Bound是$k(1+\log n)$**

# Winnow的Mistake Bound

基本设定:

- 对任意的训练数据$(x_i,y_i)$:我们假设$x_i\in \\{0,1\\}^n, y_i\in \\{0,1\\}$

在上述这个假设下，我们就来证明**Winnow**的Mistake Bound.

就像算法所描述的，要证明**Winnow**的Mistake Bound，我们需要考虑两方面的内容:

1. Winnow在正例上犯错次数的界,我们用$m^{+}$表示
2. Winnow在负例上犯错次数的界,我们用$m^{-}$表示

则，整体的Mistake Bound就是$m^{-}+m^{+}$.

## 在正例上犯错

根据**Winnow**的更新法则，每一次在正例上犯错，相应的权重都会提高2倍，而预测标准则是$sgn(w_t^T x - \theta)$.
这就说明，对于任意一个有影响力的特征来说，它最多会被提高$1+\log n$次。
某个有影响力的特征被提高$1+\log n$次之后，它就不会再犯错了(此时该权重已经大于$\theta$)。

因为一共有$k$个具有影响力的特征，因此我们就能得到$m^{+}=k(1+\log n)$.

## 在负例上犯错

在负例上的犯错次数就比较麻烦了，我们需要一些技巧。

令$TW_t$表示在第$t$步时，所有权重的和，也即$TW_t=\sum\limits_{i}^n w_{ti}$.

**在正例上犯错**

如果在$t$步时，算法在正例上犯错了，我们可以很容易得到: $TW_{t+1}<TW_t + n$.
因此，$TW$的整体的增量小于$nm^{+}$.

**在负例上犯错**

如果在$t$步时，算法在负例上犯错了，我们就能得到:$WT_{t+1}<TW_t - \frac{n}{2}$.
因此，$TW$的整体减量小于$\frac{n}{2}m^{-}$.

因为$TW_0=n$,则最终我们能得到:

{% math %}
0 < TW_t < n+nm^{+}-\frac{n}{2}m^{-} \Rightarrow m^{-} < 2(1+m^{+})
{% endmath %}

## 整体的Mistake Bound

所以整体的犯错次数是:

{% math %}
m^{+} + m^{-} < m^{+} + 2(1+m^{+}) = 3m^{+} + 2 < 3k(1+\log n) + 2
{% endmath %}

也就是说，**Winnow**算法的Mistake Bound是$O(k\log n)$.


# 参考资料

- Vivek Srikumar教授的[课件][Vivek]
- Avrim Blum的[算法笔记](https://www.cs.cmu.edu/~avrim/ML10/lect0120.txt)

# 更新日志

- 2016年10月12日完成初稿并发布


[Winnow]: https://en.wikipedia.org/wiki/Winnow_(algorithm)
[Online Learning]: https://en.wikipedia.org/wiki/Online_machine_learning
[Vivek]: http://svivek.com/teaching/machine-learning/fall2016/lectures/07-multiplicative-update.html
