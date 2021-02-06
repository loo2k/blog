---
title: 完美的 jQuery 网页进度条
tags: |-
  - jQuery
  - 教程
permalink: jquery-loading-bar
id: 8
updated: 2011-07-10 08:19:18
date: 2010-05-05 20:43:21
---

给自己的博客加上一个 LOADING... 进度条，这好像是一个很老很老的话题，两年前就已经有人用各种方法去实现它了。结合之前的前辈写的代码，我重写了一次。

<!--more-->

> 最初我在 AW 的博客上看到的[给页面加上Loading效果最简单实用的办法](http://www.awflasher.com/blog/archives/1589)，他介绍的是使用纯的 CSS 跟 JavaScript 实现的效果。

原理是：在页面中写一个 LOADING... 的提示，利用 JavaScript 在 HTML 加载完时给 LOADING... 这个 DIV 设置隐藏。

> 后来在 iShawn 的博客上看到[jQuery 页面载入进度条](http://ishawn.net/tips/loading-status-bar.html)，用了 jQuery 动画漂亮很多。

原理是：在页面的不同位置放置多个 JavaScript 代码，每个 JavaScript 代码逐步增加 LOADING... 的宽度，到最后加载完隐藏。

AW 跟 iShawn 都介绍了 LOADING... 的原理，明白了之后自己动手改造一下吧。我发现 iShawn 的博客的**进度条不仅是在载入的时候能出现，而且在点击外部链接以及网页能的 JavaScript 事件的时候都能出现**

## 为什么要网页进度条

如果你只想实现效果，这个小层次可以忽略过去。

> Analytics的统计数据表明，加入了这一效果之后，用户的“平均停留时间”的确有所提高。可见，一个“正在加载”让许多用户都有更多的耐心等待，而不会因为屏幕空白太久而不耐烦地离开。
> via [AW](http://www.awflasher.com/blog/archives/1589)

还有一段时来自刘未鹏的：

> 进度条的设计是一个很多人都知道的故事：**同样的耗时，如果不给任何进度提示，只是在完成之后才弹出一个完成消息，中间没有任何动态变化，那么整个过 程就会让人等得非常焦急，导致一些人干脆把程序关了了事。**如果有进度不断更新，那么对整个过程耗时的心理感受就会远低于实际值，用户也不会郁闷到把程序关 了。（你有多少次在银行处理手续的时候，看着工作人员把一堆材料不停地倒腾来去，心里多希望他们可以在柜台小窗口上投影一个进度条？）
> 这里的原因在于，没有进度提示的话，我们无法判断这个等待什么时候才是个尽头。如果有不断增长的进度条，那么我们对于什么时候会达到100%就会有 一个粗略的估计，这个估计是一剂定心丸，让我们知道这事情总会并且会在不久的将来完成。
> via [Mind Hacks](http://mindhacks.cn/2009/10/05/im-a-tiny-bird-book-review/)

**网页进度条可以增加网站的用户体验，不过用户体验不是增加一个网页进度条。**

## 网页进度条实现原理

![](/images/2010/05/loading.png "LOADING... 进度条原理")

原理一：在页面中放一个用固定定位的 LOADING... 的提示，这个提示可以是图片、文字或者一段加载的动画，利用浏览器是先加载完成 HTML 文档之后加载 CSS 最后加载 JavaScript 的原理，在加载到最后用 JavaScript 把 LOADING... 所在的 DIV 设置隐藏即可。

[预览效果](https://lukesign.com/demo/loadingbar1.html)

![](/images/2010/05/loading1.png "LOADING... 进度条原理")

原理二：根据网页加载时按照 HTML 文档中 JavaScript 的先后顺序执行，给进度条显示百分比数字，但这个效果并不完美和准确。

[预览效果](https://lukesign.com/demo/loadingbar2.html)

![](/images/2010/05/loading2.png "LOADING... 进度条")

原理三：利用 JavaScript 实现，凡页面能的 JavaScript 被触发也显示 LOADING... ，待事件完成之后隐藏。

[预览效果](https://lukesign.com/demo/loadingbar3.html)

## 网页进度条的实现办法

网页进度条的实现办法 AW 跟 iShawn 已经在他们的博客上说得很清楚了，效果也即是上面小节的原理一和原理二。我会重新实现一次原理一、二，而且实现原理三的效果，即点击站内链接、JavaScript 事件显示 LOADING... 的办法。

准备工作没有什么，最主要的是给网页加载 jQuery 库（其实不一定非要 jQuery 实现，但能方便很多）：

在网页的`</body>`标签前插入下面的代码：

<script type='text/javascript' src='http://ajax.googleapis.com/ajax/libs/jquery/1.2.6/jquery.min.js'></script>

在网页的`<body>...</body>`之间插入`<div id="status">LOADING...</div>`

CSS 文件可以这样写（后有[针对 IE6 的 CSS HACK](https://lukesign.com/ie6-position-fixed/)）：

```css
#status{
  background:none repeat scroll 0 0 #27BCEF;
  border-bottom:1px solid #888888;
  border-right:1px solid #888888;
  color:#FFFFFF;
  float:right;
  font-size:11px;
  left:45%;
  letter-spacing:2px;
  padding:6px 12px;
  position:fixed;
  text-align:center;
  top:20%;
  z-index:999;
  \_position:absolute;
  \_bottom:auto;
  \_top:expression(eval(document.documentElement.scrollTop));
  \_margin-top:20%;
}
```

JavaScript 文件这样写（务必放在 jQuery 库后面）：

```html
<script type="text/javascript">
$('#status').fadeOut(800);

$("a[rel!='nofollow'],a[rel!='external'][target!='_blank'],a[class!='load']").click(function() {
  $("#status").fadeIn(400);
  setTimeout(function() {
    $("#status").fadeOut(400)
  },
  4000)
});

$("a[href*='#'],a[rel='external nofollow'],a[href='javascript:void(0)']").click(function() {
  $("#status").fadeOut(400)
});
</script>
```

err...代码我也不多解释了；

## 总结

其实这是个很简单的东西，但是**我的表达确实太烂，烂到了居然用那么长的一篇文章来说完这个效果**，简单的说无非就是隐藏。

* 我觉得，如果看了这篇文章你能举一反三，那你就真理解了；
* 用户体验很重要，网页进度条可以增加网站的用户体验，不过用户体验不是增加一个网页进度条；
