---
title: 基于 SAE 的图片管理系统 Migs
tags: |-
  - php
  - 作品
  - 设计
permalink: sae-migs
id: 18
updated: 2014-06-18 19:24:12
date: 2011-09-06 04:01:08
---

Migs 是我今年暑假的一个作品，基于 SAE 写的一款图片管理系统（可以作为图床使用）；

Migs 托管与 Sina App Engin，按照 SAE 目前的免费配额的话（每天免费补充云豆到 1,000），使用 Migs 做图床能储存你 10G 的图片，以及每天免费的 2G ~ 4G 的流量（100GB/月）；

<!--more-->

## 介绍 Introduction

Migs 是一款基于 SAE 开发的免费图片管理系统，主要特性包括：

![友好的图片画廊展示](http://migs-1.stor.sinaapp.com/original/d7e9c08de1509cf0bce80a0483687bbe.png) **友好的图片画廊展示**

漂亮的 CSS3 效果展示，你可以自定义展示在首页的图片供来访者查看；

![多钟上传方式的支持](http://migs-1.stor.sinaapp.com/original/cf351aef80c4a73ed81bf7bf32e9f0c9.png) **多钟上传方式的支持**

HTML 的上传方式能兼容几乎所有符合标准的浏览器， FLASH 上传能让你一次上传多个文件并进行批量编辑；

![AJAX 即时编辑](http://migs-1.stor.sinaapp.com/original/f42d07ea36740bb653d8b9b865c05406.png) **AJAX 即时编辑**

只要移动鼠标点击标题或者描述，即可 AJAX 编辑图片的标题和描述，方便、快捷，免于繁琐的提交过程；

![安全的防盗链设置](http://migs-1.stor.sinaapp.com/original/a3204cf78814d0c3ebe058c55f4a2557.png) **安全的防盗链设置**

担心图片被盗链？不怕，你可以启用防盗链的白名单，只允许白名单下的域名才能链接到你的网站，防止网站流量的突升；

![优雅的 Lightbox 效果](http://migs-1.stor.sinaapp.com/original/f829b724fc88ab0ab078c3a61b1e1d7d.png) **优雅的 Lightbox 效果**

Migs 自带了优雅的 Lightbox 效果，方便完整的展示图片，无需打开新的页面即可查看原图；

![方便的网页快捷键](http://migs-1.stor.sinaapp.com/original/30ec39c00d8275f9da79377c9057f8c7.png) **方便的网页快捷键**

点击键盘上面的 ← 和 → 即可快速翻到上一页或者下一页；

![完美兼容所有现代浏览器](http://migs-1.stor.sinaapp.com/original/185f922be86e1b4358e8b1ae2164e3b3.png) **完美兼容所有现代浏览器**

完美兼容所有现代浏览器(IE8/Chrome/Firefox/Safair/Opera)，当然至于 IE6 这些古代浏览只能保证正常的浏览；

![方便的外链提取](http://migs-1.stor.sinaapp.com/original/a0fd3432672054c983a18f7377dc7b98.png) **方便的外链提取**

直接提供图片的外链地址以及图片的大小尺寸，让你方便的提取图片链接并贴到自己的网店或者博客上；

![强大的云计算服务支持](http://migs-1.stor.sinaapp.com/original/5634d7896c3a05324e9e48fd867476b1.png) **强大的云计算服务支持**

Migs 基于 Sina App Engine 开发，享受到高速稳定的云储存；

更多特性等待你的发掘...

## 下载 Download

*   最新版本： Migs Beta 0.1
*   发布时间： 2011 年 9 月 5 日
*   语言版本： 简体中文版
*   下载地址： [Download Migs](http://code.google.com/p/migs/downloads/detail?name=migs%20beta%200.1.zip)

## 安装 Installation

*   第一步： [注册新浪 SAE 用户](http://sae.sina.com.cn/) 如果你没有 SAE 帐号请点击这里注册以表示支持
*   第二步： 创建新的 SAE 应用
*   第三步： 初始化 MYSQL
*   第四步： 创建至少一个 Storage Domain 用来填写设置
*   第五步： 下载最新版本的 Migs 程序
*   第六步： 解压下载的 Migs 压缩文件并使用 SVN 部署代码
*   第七步： 直接进入你的应用首页进行安装

## 注意事项 Notes

1.  本程序只适用于 SAE 平台，其他 PHP 的主机无法正常使用；
2.  请在安装前创建至少一个 Storage Domain，而且至少填写至少一个 Domian；
3.  安装和进行站点设置时，务必按顺序正确地填写 Storage 的 Domain，否则会出现严重的错误；
4.  开启防盗链功能后必须在白名单上填写一条自己应用的域名，如 migs.sinaapp.com；
5.  防盗链白名单内填写允许访问的来源域名，千万不要带 http://；
6.  防盗链白名单支持通配符 \* 和 ?；
7.  防盗链跳转地址仅允许 yourapp.sinaapp.com；

## 关于 About

*   演示站点： [Migs](http://migs.sinaapp.com/)
*   SAE 注册： [注册新浪 SAE 用户](http://sae.sina.com.cn/)
*   报告 Bug： 请在本页留言
*   说明： 如果没有非常特殊的情况，Migs 会一直保持开发
*   说明： 本篇介绍的图片全部托管于 SAE 的 Migs

## 更新记录 Update

*   Beta 0.1: 发布 Migs 程序