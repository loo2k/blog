---
title: 禁用微信 webview 调整字体大小
tags: ['wechat']
date: 2016-07-09 16:42:05
---

微信 webview 内置了**调整字体大小**的功能，对于网页的可用性来说是一个很实用的功能。一些网页的字体设置过小导致用户看不清文字，调整字体大小即可解决这个问题。

<!-- more -->

![resize_font](/images/resize_font.png)

但是对于一些追求显示效果的移动端页面来说，字体大小的调整可能会导致部分文字无法显示甚至是页面布局出错。如图，大众点评案例。

![resize_font_compare](/images/resize_font_compare.png)

## 解决方案

微信的 iOS 版的调整字体大小使用的是通过给 `body` 设置 `-webkit-text-size-adjust:120%` 属性实现的，Android 则是用过 Java 调用 webview 的 API 设置字体大小。

因为这两个系统实现调整字体大小的原理不同，所以需要两种方法来限制微信对网页字体大小的调整。

### iOS

在 iOS 下，对网页的 `body` 元素设置 `-webkit-text-size-adjust: 100% !important;` 可以覆盖掉微信的样式。

```CSS
body {
  -webkit-text-size-adjust: 100% !important;
}
```

### Android

在 Android 下，需要通过 `WeixinJSBridge` 对象将网页的字体大小设置为默认大小，并且重写设置字体大小的方法，让用户不能在该网页下设置字体大小。

```javascript
(function() {
  if (typeof WeixinJSBridge == "object" && typeof WeixinJSBridge.invoke == "function") {
    handleFontSize();
  } else {
    if (document.addEventListener) {
      document.addEventListener("WeixinJSBridgeReady", handleFontSize, false);
    } else if (document.attachEvent) {
      document.attachEvent("WeixinJSBridgeReady", handleFontSize);
      document.attachEvent("onWeixinJSBridgeReady", handleFontSize);
    }
  }

  function handleFontSize() {
    // 设置网页字体为默认大小
    WeixinJSBridge.invoke('setFontSizeCallback', { 'fontSize' : 0 });
    // 重写设置网页字体大小的事件
    WeixinJSBridge.on('menu:setfont', function() {
      WeixinJSBridge.invoke('setFontSizeCallback', { 'fontSize' : 0 });
    });
  }
})();
```

注意：**如果用户之前已经设置过了字体大小，访问网页时会先看到字体被放大后的效果再恢复正常**，因为在 `WeixinJSBridge` 对象初始化完成之后才能通过 `WeixinJSBridge` 对象的方法设置为默认大小。

Android 延迟生效问题录屏：

![resize_font_android](/images/resize_font_android.gif)

## 测试用例

[http://lukesign.com/demo/resize_font.html](http://lukesign.com/demo/resize_font.html)

## 总结

由于实现原理的不同，目前的解决方案只在 iOS 上表现完美，Android 下仍然有延迟生效问题。如果你有更好的解决方案，欢迎在评论里补充。
