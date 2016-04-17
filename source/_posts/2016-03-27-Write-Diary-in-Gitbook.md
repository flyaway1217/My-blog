---
title: 在Gitbook中使用Markdown写日记
date: 2016-03-27 14:23:10
categories:
    - misc
tags: 
    - Gitbook
    - Diary
    - Markdown
description: 在Gitbook中使用Markdown写日记。
---

我平时有写日记的习惯，而且特别想要使用[Markdown][]来写日记，可惜没能发现比较合适的工具。

对于这类工具，我主要有如下几点要求:

1. 便于组织，能够打开即写
2. 支持[Markdown]()格式
3. 远程同步，由于写日记这是个长期的习惯，我可不希望换机器的时候，连日记一起丢失了。

过去一段时间内，折腾过的工具主要有如下三种:

# Evernote

原来是一直使用[Evernote][]作为日记工具的，它的组织和同步功能堪称一流，唯一的不足就是不支持[Markdown][]的语法，而且在可预见的未来，[Evernote][]似乎也不太可能支持[Markdown][].

虽然如此，我却一直苦于没有合适的工具，只能一直屈从于Evernote的富文本协议。

# Hexo

[Hexo][]本身是一个静态博客框架，基于[Node.js]构建的，在各个静态博客工具中，属于做的比较好的。由于它天生就支持[Markdown][]，使用它来构建我自己的日记系统似乎也是一种比较好的选择。

然而，Hexo不足的一点在于对于日记的组织能力不够，当然这也许可以通过一些[插件][hexo plugins]来解决，但这又和我简约的要求相违背了。


# Gitbook

[Gitbook][]其实之前就看到过有人推荐这个工具了，但是一直没有仔细深入研究过。借着这次机会，我深入了解了一些[Gitbook][]，它本身是一个用来写作电子书的工具，它的目的在于让大家更方面的写作属于自己的电子书.它的一些特性对我来说，完美的满足了我的需求。

## 便于组织

整本电子书是按章节分割的，每一个章节都可以独立成单个文件。对我写日记的需求来说，我可以把把每月、每周、每天都作为一个独立章节来对待，每一天都是独立的一个文件。

而这些文件完全不需要手动建立，需要你做的只需要编辑项目根目录下的``SUMMARY.md``文件，其他的工作就可以统统交给[Gitbook][]来完成了。

## 支持Markdown

这是最重要的特性，如果没有这个特性我是不会选择[Gitbook][]的。它不仅支持普通的[Markdown][],还支持[GFW][]，也就是我们能够在日记中使用表格.

## 简约

利用[Gitbook][]来写日记，只需要掌握两个命令就可以了:

- ``gitbook init``: 初始化一个项目
- ``gitbook serve``: 运行一个项目

## 远程托管

其实[Gitbook][]是支持远程托管的，但是对于免费用户来说，所有托管的电子书都是公开的，这显然是不适合日记这类写作目的的。

其实，还有别的方案的:托管的功能可以交给Dropbox同步就可以了。

[Gitbook][]在我本地只是扮演一个组织者和渲染工具的角色，云端保存的任务交给Dropbox就可以了。

最后贴一张``SUMMARY.md``的样例:

{% asset_img summary.png SUMMARY.md的样例 %}


[Markdown]: http://wowubuntu.com/markdown/
[Evernote]: https://evernote.com/intl/zh-cn/
[Hexo]: https://hexo.io/
[node.js]: https://nodejs.org/en/
[hexo plugins]: https://hexo.io/plugins/
[Gitbook]: https://www.gitbook.com/
[GFW]: http://help.gitbook.com/format/markdown.html




