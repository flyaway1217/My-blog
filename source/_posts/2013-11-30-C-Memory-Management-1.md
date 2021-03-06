---
layout: post
date: 2013/11/30 23:30:20
title: C语言中的内存管理那些事1-存储类
categories: 
    - programming-language
keywords: C语言,Memory Management,存储类,静态变量
tags: 
    - C语言
    - Memory Management
    - 存储类
    - 静态变量
    - 存储类
description: 本文仔细分析了C语言中的各种变量的生存期、作用域及其内存管理机制
---

# 存储类(storage class)

{% post_link C-Memory-Management-0 上一篇文章 %}中，分析了C语言中一个变量所具有的属性，这里再强调一下，一个变量的属性不仅仅是它的类型信息，一个变量应该具有的属性如下所示(需要注意的是，其实这里列出的属性并不完整，一个变量还有一些其他的属性信息，但是一个变量的存储模型主要是以下面这几个属性决定的，所以这里暂时先讨论这几个属性，其他的属性信息将在以后的文章中分析):

- **类型**
- **存储时期**
- **作用域**
- **链接**

这些属性的不同组合方式形成了变量的不同的**存储模型(Storage Models)**或称为**存储类(Storage Class)**，C语言中一共有5种合法的不同**存储类**，如下图所示：

{% asset_img storage-class.png 存储类 %}

# 自动变量

**自动存储类**变量的具有的属性是:

- **自动存储时期**
- **代码块作用域**
- **空链接**

默认情况下，在代码块或函数中定义的任意变量都属于自动存储类的变量，也可以使用`auto`来显示的说明一个变量是**自动存储类**的变量。

**自动存储类**的变量可以理解为局部变量，只有程序执行到代码块内的定义时，变量才会被分配空间，当程序离开代码块时，分配的空间会被回收，变量也就消失了。

关于**自动存储类**变量，唯一需要注意的是，**必须显示的进行初始化，否则变量的初始值可能是任意值。**


# 寄存器变量

**寄存器类**变量具有的属性几乎和**自动存储类**是一样的:

- **自动存储时期**
- **代码块作用域**
- **空链接**

它和**自动存储类**在形式上唯一的区别是**寄存器类**变量是用关键字`register`修饰的，如下所示

{% codeblock lang:c %}
int main(void)
{
    register int quick;
}
{% endcodeblock %}

**寄存器类**变量其实是一种优化方式，它声明了一个请求，请求编译器将这个变量存放在寄存器中，从而加速这个变量的访问速度，对于一些高频访问的变量来说，这是很有用的优化方案。

但是，需要注意的是，`register`仅仅是一个请求而已，编译器需要根据当时上下文的情况决定是否将这个变量放入寄存器，因此，当这个变量没有被放入到寄存器中时，它和**自动存储类**具有相同的属性信息。但是，由于它被声明为一个**寄存器类**变量，寄存器是没有地址，**因此不管是否真的被加载到寄存器中，都不能对寄存器类变量使用地址运算符。**


# 具有代码块作用域的静态变量

这里的"静态"是指变量的存储时期是**静态存储时期**的，这类变量具有如下的属性:

- **静态存储时期**
- **代码块作用域**
- **空链接**

之前说过，在默认情况下，代码块或函数中定义的变量都是属于**自动存储类**的变量，这里就出现了非默认情况，当一个**自动存储类**的变量被关键字`static`修饰的时候，这个变量就具有了**静态存储时期**，但仍然保持**代码块作用域**和**空链接**的属性。

{% post_link C-Memory-Management-0 上一篇文章 %}中曾经提到`static`第一种用法，这里是它的第二种用法，这里在对一个具有**代码块作用域**的变量用`static`修饰的时候，`static`改变的是这个变量的**存储时期**。


{% codeblock lang:c %}
void trystat(void)
{
    int fade = 1;
    static int stay = 1;
    printf("fade= %d and stay = %d\n",fade++,stay++);
}
{% endcodeblock %}

在上述代码中，`trystat`每次执行的时候，都会重新分配一块内存给`fade`变量，同时将其初始化为1，但是对于`stay`来说，它在一旦被创建，之后就永远不会被回收，之后每一次运行`trystat`函数，都不会为`stay`重新分配新的内存空间，因为它一直就存在着，它所占用的空间从没被回收过(**静态存储时期**的作用)，但是这个`stay`变量只能在`trystat`函数中被访问 (**代码块作用域**的作用)。

最后需要注意的是，其实`static int stay = 1;`这一句代码并不是在函数执行时被运行的,它并不是函数的一部分，当程序进入函数时，`stay`就已经就位了，把这个语句放在函数中，只是为了告诉编译器，它的**代码作用域**只存在于`trystat`函数中。


# 具有外部链接的静态变量

具有文件作用域的变量自动(也是必须)具有静态存储时期，但是却可以有用不同的链接。

**具有外部链接的静态变量**(也称为**外部存储类**)具有如下的属性:

- **静态存储时期**
- **文件作用域**
- **外部链接**

把一个变量的定义放在所有函数之外，就创建了一个**外部存储类**变量，为了是程序更加清晰，可以在使用**外部存储类**变量的函数中通过`extern`关键字来再次声明它。如果变量是在别的文件中定义的，那就必须使用`extern`来声明该变量。如下面代码所示:

{% codeblock  lang:c %}
int Errupt;         //外部存储类的变量
double Up[100];     //外部存储类的变量
extern char Coal;   //必须声明，说明Coal的定义是在其他文件中

void next(void);
int main(void)
{
    extern in Errupt;   //可选声明

    extern double Up[]; //可选声明
    ...
}
void next(void)
{
    ...
}
{% endcodeblock %}

需要注意的是，如果没有对一个**外部存储类**的变量进行初始化的话，它将会自动被赋值为0，这一点和**自动存储类**的变量是不同的，而且如果要对一个**外部存储类的变量**进行初始化，那么只能使用常量来初始化，而不能是一个变量。

{% codeblock lang:c %}
int x = 10;     //正确
int y = 3 + 20  //正确
size_t z = sizeof(int);  //正确
int x2 = 2 * x;     //不正确，x是一个变量
{% endcodeblock %}


# 具有内部链接的静态变量

这一类的**存储类**具有如下的属性:

- **静态存储时期**
- **文件作用域**
- **内部链接**

一个**具有内部链接的静态变量**是通过`static`变量进行声明的，当一个具有**文件作用域**的变量被使用`static`修饰时，这个变量就具有了内部链接，此处`static`关键字改变是变量的链接属性信息。

普通的**外部存储类**的变量可以被程序中任一文件中所包含的函数使用，而具有**内部链接的静态变量**只可以被与它在同一个文件中的函数所使用。

# 存储类说明符

C语言中一共有5个作为存储类说明符的关键字，它们是:

- `auto`
- `register`
- `static`
- `extern`
- `typedef`

其中，关键字`typedef`与内存存储无关，只是语法的原因被归入此类。需要注意的是，**这些说明符是不能同时出现在一个声明中的**

说明符`auto`表明一个变量具有**自动存储时期**，只能作用于具有**代码块作用域**的变量，由于具有**代码块作用域**的变量默认都是**自动存储时期**的，因此这个关键字主要是用来明确程序员的意图的。

说明符`register`也只能用于具有**代码块作用域**的变量。它建议编译器将该变量归入寄存器。

说明符`static`比较复杂，它的作用根据上下文有所不同，如果它作用的变量具有**代码块作用域**，则被它作用的变量将会具有**静态存储时期**，改变的是变量的**存储时期**;如果被它作用的变量具有**文件作用域**，那么被它作用的变量将具有内部的链接，此时它改变的是变量的**链接属性**。

说明符`extern`表明这是一个引用声明，具体的变量定义在其他文件中。

# 参考资料

- 《C Primer Plus》


