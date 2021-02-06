---
title: 强化你的 WordPress 后台编辑器
tags: |-
  - WordPress
  - 技巧
  - 教程
permalink: wordpress-editor
id: 10
updated: '2011-07-10 08:00:15'
date: 2010-06-06 23:16:56
---

通常在 WordPress 的后台编辑器上面编辑文章的时候显示的是 WordPress 的默认样式，而我们发布出去的文章内容使用的是 WordPress 主题上的样式。我想在 WordPress 的默认编辑器里面显示 WordPress 主题的文章内容样式。

<!--more-->

简单一点通过下面两张图片了解这是什么：

WordPress 编辑器默认的效果（这是在没有自定义的情况下显示的效果）：

![](/images/2010/06/editor.png "WordPress 默认编辑器")

通过自定义 CSS 文件实现的 WordPress 编辑器所见即所得效果：

![](/images/2010/06/custom-css-editor.png "WordPress 所见即所得编辑器")

图示的两张图片是我在我的博客上的一篇《[给新手的 HTML 进阶秘籍](../30-tips-for-html-beginner/)》文章在编辑时的效果，我把 WordPress 主题自带的文章内容样式搬到了 WordPress 后台编辑器上。这样当我想预览编辑的内容在主题上面显示的效果的时候就不用一次一次的去点预览了。**在 Windows live writer 上也有类似的功能，不过这篇文章讨论的是如何将这个功能加到你自己的 WordPress 主题上。**

## 实现 WordPress 后台编辑器预览效果

这次操作需要修改你的 WordPress 主题，所以修改之前请注意备份。主要内容是修改 `functions.php` 以及创建一个 `editor.css` 的文件。

## 创建给 WordPress 后台编辑器的 editor.css 文件

这个文件时专门为 WordPress 后台编辑器写的样式文件，你可以从你的 WordPress 主题中的 `style.css` 文件中提取出你文章内容的样式（**重要：提取时请注意修改 CSS 的选择器**）。由于 WordPress 的后台编辑器的编辑区用的是 Ifram ，所以编辑样式表的时候只需要直接给类似于 `<p>` `<blockquote>` `<cite>` `<h2>` `<hr />`...等标签样式即可。

下面是我博客的一个实例：（需要根据主题编写，请勿照搬）

**我的主题在 `style.css` 中文章内容的样式规则是：**

```css
.post .entry {
  font-size: 13px;
  margin: 6px 0;
  line-height: 1.8;
  word-spacing: 3px;
}

.entry p {
  margin-bottom: 10px;
}

.entry blockquote {
  border-left: 2px dashed #DDDDDD;
  color: #999999;
  margin: 5px 10px 10px;
  padding: 0 15px;
}
```

**根据我原来的代码，我新建了一个 editor.css 文件在主题目录下：**

```css
body {
  font-size: 13px;
  margin: 6px 0;
  line-height: 1.8;
  word-spacing: 3px;
}

p {
  margin-bottom: 10px;
}

blockquote {
  border-left: 2px dashed #DDDDDD;
  color: #999999;
  margin: 5px 10px 10px;
  padding: 0 15px;
}
```

其中，我去掉了选择器里一些多于的内容。（以上代码仅供参考）

## 把样式表添加到 WordPress 编辑器上

既然已经写好了样式表，现在就只剩最后一步——把样式表链接 WordPress 到编辑器上。这个时候需要编辑你的 `functions.php` ：

```php
function editor\_styles($url) {
  if (!empty($url)) $url .= ',';
    $url .= trailingslashit(get\_stylesheet\_directory\_uri()).'editor.css';
    return $url;
  }

  if(current\_user\_can('edit\_posts')):

  add\_filter('mce\_css', 'editor\_styles');

  endif;
}
```

按照上面的形式将代码添加到 `functions.php` 文件中即可。

## WordPress 后台编辑器的其他强化

根据上面的办法，你可以打开你的 WordPress 后台编辑器测试下效果了。这样的预览效果估计能让你的 WordPress 后台编辑文章的时候更方便一点。

还有几个想法：

* 不知 WordPress 的编辑器，其他的编辑器也可以这样弄
* 制作 WordPress 主题的朋友可以参考下
* 不一定要修改主题代码，也可以用 Firefox 的 [Stylish](https://addons.mozilla.org/zh-CN/firefox/addon/2108/) 插件实现
* 还可以用 [Greasemonkey](https://addons.mozilla.org/zh-CN/firefox/addon/748/) 这个插件实现
* ...

但是现在还有几个问题：

* WordPress 后台编辑器功能太烂
* WordPress 后台编辑器的 HTML 代码编辑器很不方便
* 或许可以使用外部的编辑器，如 notepad++

下个星期三就要参加高二的学业水平测试，考完之后我就是个高三生了，至于这些问题就留到下篇文章吧 :P
