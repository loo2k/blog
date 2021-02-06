---
title: Gravatar China for WordPress
tags: |-
  - WordPress
  - 作品
  - 技巧
permalink: gravatar-cache-reset
id: 16
updated: 2011-08-05 08:50:37
date: 2011-08-04 19:33:10
---

这是一个关于 Gravatar Cache 的重要更新，之前（2010.10.15） Gravatar 由于一些众所未知的原因不能访问，所以当初制作了一个 Gravatar 头像的本地缓存插件，但是由于当时编写的比较匆忙，遗留下了一些问题，包括但不限于：无法使用默认图片、无法缓存不同大小的头像等；

<!--more-->

最近一段时间（2011.08.02），Gravatar 再次无法访问，所以重新写了一个插件 Gravatar China for WordPress，并解决的之前存在的所有已知的问题；

## Gravatar China for WordPress 特性

1. Gravatar 头像防墙补丁：替换 Gravatar 头像能正常访问的地址；
2. Gravatar 本地缓存：对特殊的网络环境下给头像进行本地缓存；
3. 自定义设置缓存过期时间；
4. 国内、国外主机用户通用；
5. 完美兼容 WordPress；

#### Gravatar China for WordPress 后台界面：

![Gravatar China](/images/2011/08/2011-08-04_113426.png)

## Gravatar China for WordPress 说明

本插件针对中国大陆的网络环境制作；

一般情况下，你可以在 Gravatar 头像不能正常访问的时候启用本插件的 “Gravatar 补丁”，它能帮助你的 WordPress 访客连接到正常的头像地址上；

通常，根据网页前端的性能优化来说，不推荐用户启用 “Gravatar 本地缓存”，因为它对 WordPress 的性能有一定的影响，当然这个影响仅限于生成本地缓存的时候；（启用缓存前请确认你的 WordPress 目录 `wp-content/plugins/gravatar-cn/cache` 可写）

本插件使用 GPL2 协议进行授权；

如果你喜欢这个插件的话，可以[赞助作者](http://loo2k.com/donate/)以表示支持 :)

### Gravatar China for WordPress 下载

[WordPress 官方下载](http://wordpress.org/extend/plugins/gravatar-china/) | [Google Code](http://code.google.com/p/gravatar-china/downloads/list)

最初发布于：2010 年 10 月 16 日 11:33
