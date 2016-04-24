---
title: Python剖析0
tags:
  - Python
categories:
  - Python
description: 本文总结了Python源代码文件的组织结构，为后续学习Python源代码打好基础。
keywords:
  - Python-Source-Code
  - Python源代码目录
  - Python源代码框架
date: 2016-04-23 20:26:26
---


# 写在前面

之前一直在纠结该不该把「算法导论」的读书笔记系列写完，后来考虑到下半年出国读书时，高级算法课依然将会是一门必修课，因此决定把「算法导论」读书笔记系列安排到上课时一起进行，而不需要分两次学习算法了。

于是，马上切换到下一个比较感兴趣的点上——Python源码剖析。

陈儒先生曾经出过一本书，就叫「Python源码剖析」，但是这本书出得比较早，是2008年出版的，现在基本已经买不到了，早就停印了。
曾经在大学期间和研究生期间看过，确实对理解Python的内部实现有很大帮助，可惜随着时间的推移，这本书:

1. 太古老了，Python语言每年都在进行变化，很多内部结构发生了变化，「Python源码剖析」中的内容很多都和现在的源码不一致了。
2. 只涵盖了Python2的部分，对于目前主流的Python3并没有涉及。

因为上述两个问题，使得我们很难真正理解Python的底层实现是什么样的，于是乎我就产生了好好研究一下Python源码的想法。

虽然在很多人看来，这也许是一个没有太大意义的事情，只要懂得用就行了，何必花大力气去看源码呢？而且在众多的语言中 Python的实现绝不是最好的。

对于这些疑问，我只能说，这就是我的兴趣，我喜欢Python，我更加深入地了解它，仅此而已！

在正式开始之前，有个研究PLT的朋友向我推荐了15年新出的一本书: 「 [Fluent Python][] 」，这也是一本针对Python的高阶书籍，主要是对Python的设计哲学进行论述，感觉还不错。

接下去的计划是这样的:

1. 首先把陈儒先生的「Python源码剖析」和Luciano Ramalho的「Fluent Python」过一遍，做出读书笔记，对Python实现有个大概了解
2. 在陈儒先生的基础之上，自己阅读目前Python3的源码，分析提炼出自己觉得有价值的内容。

# 何谓Python

这里所说的[Python][]其实包含了两个意思，第一层意思是指Python语言，这是一种具体的编程语言，它有它相应的规范和文法；第二层意思是指Python这个编程语言的具体实现，最常用的一个实现就是CPython，用C语言实现的Python版本，其他还有JPython，PyPython等等。
这就好比是C语言有[ANSI C][]这个规范，同时有很多符合这个规范的实现(编译器)，比如GCC系列和Borland系列.
而对于Python这种动态语言来说，它的具体实现称作解释器。
我比较感兴趣的是CPython的具体实现，因此本文以及后续所有的文章都是围绕CPython这个实现来展开的。

# Python总体框架

{% asset_img python-structure.jpg  %}

Python的整体框架如上图所示，主要有三个组成部分:

1. 文件系统: 包含了Python本身的代码文件、内置的库文件已经用户自定义的模块。
2. Python核心: 本质其实就是Python解释器，通过词法分析、语法分析、生成字节码等流程，将写好的Python程序转化成为Python虚拟机可以执行的字节码。
3. Python运行时环境，包括了对象/类型系统(Object/Type structures)，内存分配器(Memory Allocator)和运行时状态(Current State of Python).对象/类型系统负责管理Python运行过程中的各种内建对象以及用户自定义的类型和对象；内存分配器负责自动管理内存的申请和释放；运行时状态则维护了程序执行过程中各种不同状态的切换。

# Python源代码组织

Python 3.5.1的源代码组织结构如下所示:

{% asset_img source-code-structure.png  %}

其中各个目录主要作用如下:


- Doc: 不用说，这个目录是存放Python文档的目录.Python的文档是基于[Sphinx][]制作的，采用reStructuredText进行写作。
- Grammar: 该目录只有一个Grammar文件，包含了描述Python整个文法，以BNF的形式写在了Grammar文件中。
- Include: 该目录下包含了Python提供的所有头文件。
- Lib: 该目录包含了Python自带的所有标准库，Lib中的库都是用Python写成的。
- Mac: 针对Mac系统的配置代码
- Misc: 一些不太好归类的文件都放在这里了。
- Models: 该目录包含了所有用C语言编写的模块，这些模块对速度有比较高的要求，因此都是用C写成的，而对速度没有太高要求的模块都是用Python写成放入Include目录。
- Objects: 该目录中包含了Python的内建对象的实现。
- Parser: Python的词法分析器和语法分析器。
- PC: 比较老的Windows和OS2的Port的项目以及Port用到的一些公用文件放在这里，PCBuild要用到这个目录的内容.
- PCBuild: Python用于VS 2015的Project文件
- Programms: 可执行文件的源代码。
- Python: 该目录下包含了Python解释器和虚拟机，是Python运行的核心。
- Tools: 扩展Python所需要的工具。

# 参考资料

- [Python源码分析4 – Grammar文件和语法分析](http://blog.csdn.net/atfield/article/details/1457035)

[ANSI C]: https://zh.wikipedia.org/wiki/ANSI_C
[Python]: http://python.org/
[Fluent Python]: https://www.amazon.cn/Fluent-Python-Ramalho-Luciano/dp/1491946008/ref=sr_1_sc_1?ie=UTF8&qid=1461411698&sr=8-1-spell&keywords=fluent+ptython

[Sphinx]: http://sphinx-doc.org/

