---
title: Sublime Text 2 便携版工具包
tags: |-
  - 工具
  - 技巧
permalink: sublime-text-2-portable-version-tool
id: 25
updated: 2015-05-05 16:21:32
date: 2012-08-16 00:05:37
---

> Note: 这是一篇比较久的文章，当时 Sublime Text 的最新版本是2，但是该工具也能正常兼容 Sublime Text 3.

<!--more-->

前段时间 Sublime Text 2 正式版发布之后就从 Notepad++ 转到了 Sublime Text 2，用了一段时间，被 ST2 深深吸引（比 NPP 漂亮的界面，优秀的用户体验），好吧，总之 ST2 是一款编辑神器；

不过 ST2 不是一个免费软件

> Sublime Text 2 may be downloaded and evaluated for free, however a license must be purchased for continued use. There is currently no enforced time limit for the evaluation.

ST2 可以免费下载使用，但是必须要购买一个授权才能继续使用。不过现在只是在使用过程中会弹出窗口提示是否购买授权，不影响使用，但是 ST 也有可能在以后的版本要求必须购买授权才能使用。一个 ST 的授权是 $59，好吧，对于一个这么强大的编辑器来说，还能接受吧；

![Sublime Text 2](/images/2012/08/st2.png)

## Sublime Text 2 便携版工具包

因为目前用的 ST2 的版本是 Portable Version，但是 Portable 版本的 ST2 不能像 Notepad++ 一样直接在选项设置里面设置文件关联，而且也不能添加右键菜单；所以就自动动手写了个批处理脚本，自动添加右键菜单和自动关联文件扩展名了；

![Sublime Text 2 便携版工具包](/images/2012/08/st2tool.png)

## 使用 Sublime Text 2 便携版工具包的时候有几个注意事项：

- 工具包必须复制到 Sublime Text 2 的文件夹中执行；
- 确保 Sublime Text 2 的可执行文件名为 sublime_text.exe；
- 在工具包执行文件的同目录下，请新建一个 ext.txt 文件用于保存需绑定的扩展名；
- ext.txt 文件中，每行只输入一个扩展名；
- ext.txt 文件中在每行的开头输入 ; 分号可注释该行；

## 效果预览：

![Sublime Text2 右键菜单](/images/2012/08/st2menu.png)

## 便携版工具包下载地址：

[Sublime Text 2 便携版工具包](https://github.com/loo2k/Sublime-Text-Portable-Tool);

## 便携版工具包版本：

- 2012.09.23：修复操作序号错误问题，修复关联 txt 文件后无法在新建 txt 文件的问题；

- 2012.08.15：第一个版本发布；

## 相关链接：

好吧，如果你也想开始使用 ST2 的话，可以看看下面列出来的一些资源：

Sublime Text 2 简介 via @大城小胖

<embed src="http://player.youku.com/player.php/sid/XMzU5NzQ5ODgw/v.swf" type="application/x-shockwave-flash" width="480" height="400" quality="high" />

## 其他链接：

- [Sublime Text 2 官方网站](http://www.sublimetext.com/);
- [Sublime Package Control](http://wbond.net/sublime_packages/package_control);
- [Sublime Text 2 Tips and Tricks](http://net.tutsplus.com/tutorials/tools-and-tips/sublime-text-2-tips-and-tricks/);
- [Essential Sublime Text 2 Plugins and Extensions](http://net.tutsplus.com/tutorials/tools-and-tips/essential-sublime-text-2-plugins-and-extensions/);
- [Sublime Text 2 入门及技巧](http://lucifr.com/139225/sublime-text-2-tricks-and-tips/);

其实网上关于 Sublime Text 2 的介绍已经足够多了，也没必要在这里介绍，Google 吧；
