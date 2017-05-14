---
title: Vim个人配置文件
tags:
  - Vim配置
  - Vim
categories:
  - vim
keywords:
  - Vim配置
  - Vim
description: Vim的个人配置文件，主要针对Python、Latex的配置。
date: 2017-05-14 16:15:07
---




本篇博文对自己的Vim配置文件进行了说明.

# 插件配置

一共使用了13个第三方插件，主要列表和说明如下:

1. [UltiSnips][]: 代码片段的插件，能够根据你输入的代码片段进行自动不足。还能设置自定义代码片段进行补足。
2. [vim-snippets][]: 同样是代码插件，包含了大量预定义的代码片段。
3. [The-NERD-tree][]: 老牌的文件管理插件，在我的配置里快捷键配置为F10和F9，F10打开树形文件浏览窗口，F9关闭窗口。
4. [VOoM][]: 文档目录浏览插件，支持绝大部分文件类型，包括Python，markdown等文件格式。在我的配置文件中，常用的Python、markdown、tex等文件类型自动启用该插件。另外设置了快捷键F8，对插件进行启用和关闭。
5. [indentLine][]: 显示各级缩进的插件，对于像Python这样利用缩进排版的语言特别有用，不可或缺。
6. [vim-airline][]: 显示各个打开文件的状态插件，对于同时编辑多个文件的情况下，非常有用。
7. [nerdcommenter][]: 快速注释和解开注释。
9. [syntastic][]: 这也是一个神器插件，能够自动对各种语言进行语法检查，对保持一个良好的代码风格很有帮助。
10. [bbye][]: 管理文件缓冲区，可以同时打开各个不同的文件。
11. [supertab][]: 代码自动补全插件，但是只能补全已经存在代码。
12. [dash][]: Dash的辅助插件，和Vim结合之后能够自动搜索文档内容。
13. [vim-javascript][]: Javascript的语法辅助插件。

这13个插件都是用[Vundle][]进行管理的，非常方便。

# 个人配置

关于个人配置上面，我主要针对Python、markdown和tex文件进行了一系列的配置。
详细配置可以在{% post_link Vim-Insert-Title Vim自动插入文件头部 %}中和具体的配置文件中查看。

# 配置效果

{% asset_img vim.png %}

# 一键安装

我将我所有的配置都放在了Github上面，并且写了相应的shell，可以做到一键安装配置。
具体地址在: [https://github.com/flyaway1217/dotfiles](https://github.com/flyaway1217/dotfiles)

# 参考资料

# 更新日志

- 2017年5月14日写作并发表初稿。

[UltiSnips]: https://github.com/SirVer/ultisnips
[vim-snippets]: https://github.com/honza/vim-snippets
[The-NERD-tree]: https://github.com/vim-scripts/The-NERD-tree
[VOoM]: http://www.vim.org/scripts/script.php?script_id=2657
[indentLine]: https://github.com/Yggdroot/indentLine
[vim-airline]: https://github.com/vim-airline/vim-airline
[nerdcommenter]: https://github.com/scrooloose/nerdcommenter
[syntastic]: https://github.com/vim-syntastic/syntastic
[bbye]: https://github.com/moll/vim-bbye
[supertab]: https://github.com/ervandew/supertab
[dash]: https://github.com/rizzatti/dash.vim
[vim-javascript]: https://github.com/pangloss/vim-javascript
[Vundle]: https://github.com/VundleVim/Vundle.vim
