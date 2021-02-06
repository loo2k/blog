---
title: 解决 IE6 position:fixed 固定定位问题
tags: |-
  - css
  - ie6
  - 技巧
permalink: ie6-position-fixed
id: 5
updated: 2013-04-02 02:29:01
date: 2010-04-15 21:48:05
---

就像你所遇到的问题一样， IE6 有太多的 bug 让制作网页的人头疼。这篇文章介绍的是介绍我的**如何解决 IE6 不支持 position:fixed; 属性的办法**。

![](/images/2010/04/ie6-position-fixed.png "Internet explorer 6 position fixed")

<!--more-->

## 关于 `position: fixed;` 属性

> 生成绝对定位的元素，相对于浏览器窗口进行定位。 元素的位置通过 `left` / `top` / `right` / `bottom` 属性进行规定。

`position: fixed;` 可以让网页上的某个元素固定在一个绝对的位置，即使拉动滚动条位置也不发生变化。（在 LOO2K 博客右下角的那个置顶的小按钮就是用了这个 CSS 属性实现的）

## 一般的 `position: fixed;` 实现方法

以我的博客为例，在右下角 `<div id="top">...</div>` 这个 HTML 元素使用的 CSS 代码如下：

```css
#top {
  position:fixed;
  bottom:0;
  right:20px;
}
```

实现让`<div id="top">...</div>`元素固定在浏览器的底部和距离右边的20个像素。

## 在 IE6 中实现 `position: fixed;` 的办法

刚刚提过，**在 IE6 中是不能直接使用 position: fixed;** 。你需要一些 CSS Hack 来解决它。（当然，IE6 的问题也不仅仅 `position: fixed;`） 相同的还是让 `<div id="top">...</div>` 元素固定在浏览器的底部和距离右边的 20 个像素，这次的代码是：

```css
#top {
  position:fixed;
  \_position:absolute;
  bottom:0;
  right:20px;
  \_bottom:auto;
  \_top:expression(eval(document.documentElement.scrollTop+document.documentElement.clientHeight-this.offsetHeight-(parseInt(this.currentStyle.marginTop,10)||0)-(parseInt(this.currentStyle.marginBottom,10)||0)));
}
```

`right` 跟 `left` 属性可以用绝对定位的办法解决，而 `top` 跟 `bottom` 就需要用上面的表达式来实现。其中在 `_position: absolute;` 中的 `_` 符号只有 IE6 才能识别，**目的是为了区分其他浏览器**。 上面的只是一个例子，下面的才是最重要的代码片段：

##### 使元素固定在浏览器的顶部

```css
#top {
  \_position:absolute;
  \_bottom:auto;
  \_top:expression(eval(document.documentElement.scrollTop));
}
```

##### 使元素固定在浏览器的底部

```css
#top{
  \_position:absolute;
  \_bottom:auto;
  \_top:expression(eval(document.documentElement.scrollTop+document.documentElement.clientHeight-this.offsetHeight-(parseInt(this.currentStyle.marginTop,10)||0)-(parseInt(this.currentStyle.marginBottom,10)||0)));
}
```

这两段代码只能实现在最底部跟最顶部，你可以使用 `_margin-top:10px;` 或者 `_margin-bottom:10px;` 修改其中的数值控制元素的位置。

## position:fixed; 闪动问题

现在，问题还没有完全解决。在用了上面的办法后，你会发现：**被固定定位的元素在滚动滚动条的时候会闪动**。解决闪动问题的办法是在 CSS 文件中加入：

```css
\*html{
  background-image:url(about:blank);
  background-attachment:fixed;
}
```

其中 `*` 是给 IE6 识别的。 到此，IE6 的 `position: fixed;` 问题已经被解决了。现在 LOO2K 这个博客上的固定定位就是使用的这个办法解决 IE6 固定定位问题的。
