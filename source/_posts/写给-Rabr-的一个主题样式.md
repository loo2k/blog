---
title: 写给 Rabr 的一个主题样式
tags: |-
  - 作品
  - 设计
permalink: theme-for-rabr
id: 4
updated: 2011-07-10 08:53:08
date: 2010-04-05 21:18:38
---

![Rabr](/images/2010/04/rabr.png)

还在用 Twitter 的应该大部分都知道 #Rabr 是什么吧？@disinfeqt 的一个开源项目，用户体验做得很不错。没必要做太多的介绍，**这篇文章是想分享我做的一个 Rabr 的界面**。

<!--more-->

## 预览效果

![预览效果](/images/2010/04/rabr_full.png)

CSS 代码：

```css
body {
  background:url("http://a1.twimg.com/profile_background_images/77200708/bg.gif") repeat scroll 0 0 #FFFFFF;
}
a{
  color:#7F6644;
}
#nav li:hover {
  background-color: #DDDDDD;
}
#side_base {
  background-color:#DBDBDB;
  border-left:1px solid #CCCCCC;
}
ul.sidebar-menu li a:hover {
  background-color:#EBEBEB;
}
#side .promotion {
  background-color:#EBEBEB;
}
#side .promotion .definition strong, .side\_name {
  color:#7F6644;
}
#side hr {
  color:#D0D0D0;
  background:none repeat scroll 0 0 #D0D0D0;
}
ul.sidebar-menu li.active a, a.active {
  background-color:#EBEBEB;
}
#plz_dont_block_us{
  display:none;
}
```

## 使用方法

1. 复制代码里面的所有内容；
2. 打开 Rabr – Settings；
3. 在 Customize CSS 下面的文本框内输入刚刚复制的代码；
4. Save;

我把广告设置成 `display: none;` 了，所以广告是显示不出来的，想用的推友就用吧。

## 更新日志

2010-07-30：解决了背景图片在 Firefox 滚动时出现卡滞的问题；
