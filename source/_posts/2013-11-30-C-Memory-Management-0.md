---
layout: post
date: 2013/11/30
title: C语言中的内存管理那些事0-基本概念
categories: 
    - programming-language
keywords: C语言,Memory Management,存储类,作用域,存储时期,链接
tags: 
    - C语言
    - 存储类
    - 作用域
    - 存储时期
    - 链接
description: 本文仔细分析了C语言中的各种变量的生存期、作用域及其内存管理机制
---

# 前言

最近一直在看Python的源码剖析，Python源码是用C写的，在阅读源码过程中，发现Guido van Rossum大量使用了C语言中的**宏定义**、**static**、**extern**、**register**等关键字，虽然可以说C语言是我的第一门编程语言，但是那也是一件挺久远的事情了，很多基本概念已经模糊了，这个几个关键字大体知道是干什么的，但是具体细节已经不记得了。为了能够更好地分析Python源代码，于是翻出了那本已经蒙上一层灰的《C Primer Plus》，找到内存管理的那一章，又好好啃了一遍，再加上网上的一些分析文章，现在将学习到的内容记录下来，防止又被遗忘了。

# 需要明确的几个概念

关于编程语言中的变量，首先重要的当然是它的类型，不管是像Python、Ruby这种的动态弱类型脚本语言还是像C\C++、Java这种强类型语言，每一个变量都是有类型的，差别只是在于：

是在**编译时**进行类型检查还是在**运行时**进行类型检查。

所以，对于任何一个变量来说，类型信息可以理解成是它的属性信息，但是，需要强调的一点是：**类型信息并不是变量的唯一属性信息**，尤其是在C\C++语言中，一个变量还有很多其他信息，包括:

- **存储时期(storage duration)**
- **作用域(scope)**
- **链接(linkpage)**

虽然这几个概念在不同的语言中可能实现的方式不同，但是基本思想应该还是一致的。由于C语言是可以直接操控内存的，所以这几个概念尤为重要。

# 存储时期(sotrage duration)

《C Primer Plus》中的原文是如此描述的:

> in terms of its sotrage duration, which is how long it stays in memory.

看定义是非常好理解的，**存储时期(storage duration)**就是指变量在内存中的保留时间，需要注意的是，**变量在内存中存在，并不代表你就可以访问它**。

**存储时期(storage duration)**一共分为两种情况，分别是

- **静态存储时期(static storage duration)**：在程序执行过程中一直存在，通常来说，任何不在代码块内定义的变量都具有静态存储时期，但是并不是任何的具有静态存储时期的变量都不在代码块内，比如`malloc`申请的内存块和`static`关键字，`malloc`可以出现在任何位置，`malloc`函数后面会深入分析。
- **自动存储时期(automatic storage duration)**：除了静态存储时期以外的变量都是自动存储时期的，这有点废话……其实，可以理解成只要是在代码块内定义的变量[^1]都是自动存储时期的，当程序执行到该代码块时，内存会为其变量分配内存，当程序执行完成当前代码块以后，分配的内存就将被释放。这些自动存储时期的变量的生命周期只存在与这段代码块之中。

# 作用域(scope)

《C Primer Plus》中的原文:

> Scope describes the region or regions of a program that can access an identifier.\\
> Its scope and its linkpage,which together indicate which parts of a program can use it by name.

**作用域(scope)**，简单来说，就是指**变量在程序的哪些部分可以被引用。**

这里需要和存储时期的概念区分开来，存储时期用来表示**变量在内存中的存在时间**，而作用域用来表示**变量可以被引用的范围**。

在C语言中，一共有3种作用域：

- **代码块作用域(block scope)**: 所有在一对花括号之间出现的代码都是一个代码块，在代码块中的定义的变量都具有代码块作用域。从这个变量定义地方开始，到这个代码块结束，该变量均是可见的。需要注意的，函数的**形式参数**也是具有代码块作用域的。
- **函数原型作用域(function prototype scope)**: 出现在函数原型中的变量，都具有函数原型作用域，函数原型作用域从变量定义处一直到原型申明的末尾。
- **文件作用域(file scope)**: 一个在所有函数之外定义的变量具有文件作用域，具有文件作用域的变量从它的定义处到包含该定义的文件结尾处都是可见的。

# 链接(linkpage)

链接也是用来表示**变量可以被引用的范围**的，链接和作用域的共同作用下，决定了一个变量可以被引用的范围。链接的主要作用是在程序具有多个文件时，可以实现跨文件共享变量。

C语言中一共有三种不同的链接:

- **外部链接(external linkpage)**: 具有外部链接的变量可以在多文件的程序中任何位置使用。
- **内部链接(internal linkpage)**: 具有内部链接的变量可以在定义它的文件中的任何位置使用。
- **空链接(no linkpage)**: 只是被当前代码块所私有，不能被程序的其他部分所引用。

具有代码作用域和函数原型作用域的变量都具有空链接，因为它们是由其所在的代码块或函数原型所私有的。

具有文件作用域的变量可以是外部链接也可以是内部链接，通过关键字`static`[^2]来指明。

{% codeblock lang:c %}
int giants = 5; //文件作用域，外部链接
static int dodgers = 3; //文件作用于 内部链接
int main()
{
    ...
}
    ...
{% endcodeblock %}

上述的代码定义了两个具有**静态存储时期**、**文件作用域**的变量的，所不同的是`giants`没有用`static`来修饰，因此具有**外部链接**，而`dodgers`通过`static`的修饰，具有了**内部链接**。可以看到，具有文件作用域的变量默认是**外部链接**。

对上述代码所在文件来说，`giants`和`dodgers`在该文件内部的任何位置都是可见的，但是对于其他文件来说， 具**内部链接**的`dodgers`是不可见的，而具有**外部链接**的`giants`却是可见的，只要在需要引用这个变量的外部文件中存在以下的声明:

{% codeblock lang:c %}
extern int giants
{% endcodeblock %}

上述代码中的`extern`关键字告诉编译器，这不是一个定义，而只是一个**声明**，它告诉编译器这个变量是在其他文件中的。注意:**不要用`extern`关键字进行外部定义，它只是用来引用一个已经存在的外部定义。**[^3]


关键字`extern`只是一个引用申明，而不是定义申明。

# 总结

从上述说明可以看到，在C语言中，一个变量的属性信息不仅仅是其**类型**，还包括了**存储时期**、**作用域**和**链接**。这些属性的不同组合方式呈现了变量的不同表现，这些属性的不同组合是如何决定变量的表现的将会在一篇文章中说明。

# 参考资料

- 《C Primer Plus》


[^1]: 其实这里这么说，有些不太准确，因为在一个代码块内，可以通过`static`关键字来申明一个具有静态存储时期但是却是在代码块内定义的变量。`static`关键字后面会进行说明。

[^2]: 这里是`static`的第一个用法，它改变了变量的链接，后面还会看到`static`的第二种用法，用来改变变量的存储时期。

[^3]: 以前常犯这个错误，像`extern int giants=1`的代码是错误的。

