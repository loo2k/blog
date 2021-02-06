---
title: 为什么尽量避免使用 CSS 表达式
tags: |-
  - css
  - IE
permalink: why-avoid-css-expression
id: 15
updated: 2013-03-16 20:26:25
date: 2011-07-12 18:28:59
---

以前曾经发表过一篇文章 [解决 IE6 position:fixed 固定定位问题](https://lukesign.com/ie6-position-fixed/)，讲的是使用 CSS 表达式来解决 IE6 无法使用 positio: fixed; 固定定位的问题；在文章发表之后的到现在，一些朋友告诉我 CSS 表达式可能会影响到网站的性能； 经过这些朋友们的提醒，我更多的留意了关于 CSS 表达式方面的知识，而这篇文章，是我在看 《[高性能网站建设指南](http://book.douban.com/subject/3132277/)》后，以及之前做的一些学习，我想总结下 “**为什么尽量避免使用 CSS 表达式**”；

<!--more-->

## 什么是 CSS Expression？

CSS Expression （CSS 表达式），是一种使用动态设置 CSS 属性的方式，并且被 IE5 以上的版本所支持，但是 IE8 的标准模式已不再支持 CSS 表达式了[[1]](#expressions)；

#### 一个简单的 CSS 表达式

```css
body {
  background-color: expression( (new Date()).getHours()%2 ? "#B8D4FF" : "#F08A00" );
}
```

这段代码的作用是能让你页面中 body 的背景色每隔一小时变换一次；

## CSS Expression 带来的性能问题

是的，参考 MSDN “[关于动态属性](http://msdn.microsoft.com/en-us/library/ms537634.aspx)” 的文档，你会发现，其实 CSS 表达式还是非常强大的，比如你可以使用 CSS 表达式实现 min-width 属性，隔行换色，模拟 :hover, :before, :after 等伪类； 但是，正式因为 CSS 表达式太强大了，以至于 CSS 表达式带来的严重的性能问题：“**为了确保有效性，CSS 表达式会进行频繁的求值**”，到底有多频繁？就是在你改变窗口大小，滚动页面甚至移动鼠标都会触发表达式进行求值，如此频繁的求值以至于浏览器的性能收到严重的影响；

## 关于对 CSS Expression 的性能测试

这个测试的方法是来自于最近一段时间在看的《[高性能网站建设指南](http://book.douban.com/subject/3132277/)》中的规则7；

```css
P {
  width: expression( setCntr(), document.body.clientWidth<600 ? "600px" : "auto" );
  min-width: 600px;
  border: 1px solid;
}
```

这个方法通过绑定一个 setCntr() 函数到 CSS 表达式上，统计页面执行了多少次的 CSS 表达式，并显示在一个文本框里面；你也可以通过 IE5 ~ IE6 访问 [http://stevesouders.com/hpws/expression-counter.php](http://stevesouders.com/hpws/expression-counter.php) 进行测试；

### 测试结果

页面内有 10 个段落，加载完页面大概执行了 40 次的 CSS 表达式，然后在你改变页面大小，滚动页面，甚至移动鼠标，在我的测试里不动鼠标仍然会执行 CSS 表达式，几万次的求值根本不在话下，而且在点击文本框之后，IE 就已经卡死了；

## 避免使用 CSS Expression

好吧，这是一个总结；虽然还有对 CSS 表达式进行优化的方法（你可以在参考链接中找到），但是这不是这篇文章要总结的，这篇文章要总结的是为什么尽量避免使用 CSS 表达式； CSS 表达式虽然强大，但是会给浏览器带来很严重的性能问题，并拖慢网页的加载速度；**在可能的前提下，尽量避免使用 CSS 表达式！**

## 参考链接

1. [CSS Expression 用法总结](http://www.cnblogs.com/rubylouvre/archive/2009/07/29/1534330.html)
2. [CSS Expression 的优化](http://www.planabc.net/2009/09/21/optimization_of_css_eexpression/)

## 脚注

1. IE 8 的标准模式不再支持 CSS 表达式: http://msdn.microsoft.com/en-us/library/cc304082(v=vs.85).aspx#expressions
