---
title: 判断微信客户端的那些坑
tags: ['wechat']
date: 2015-05-04 00:24:09
---

最近的一个项目中需要在前端侧判断用户是否使用微信客户端，最开始是根据 UserAgent 去判断，后来不少的用户反馈在微信客户端内仍然提示非微信客户端访问。

这里总结了当时遇到的一些问题和解决方案供大家参考，以免后续踩坑。

<!-- more -->

## UserAgent 判断微信客户端

大部分开发者都会想到检测 UserAgent 中是否含有 MicroMessenger 字符来判断是否微信客户端。

微信的 UserAgent:

```
Mozilla/5.0 (iPhone; CPU iPhone OS 8_3 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Mobile/12F70 MicroMessenger/6.1.5 NetType/WIFI
```

使用正则表达式就可以方便的判断：

```js
function isWechat() {
  var ua = navigator.userAgent;
  return /micromessenger/i.test(ua);
}
```

看起来很正常，**但是**根据我的访问数据中得到的结果是有不少手机（**Windows Phone 以及一些奇怪机型**）的微信客户端 UserAgent 是不带 MicroMessenger 字符的，所以使用 UserAgent 并不能覆盖所有的机型。

测试使用 NOKIA 1520 获得的结果是：

```
Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 1520)
```

在 Windows Phone 系统上的微信客户端是没有 MicroMessenger 字符串的；

## Windows Phone 的判断

起初以为 Windows Phone 中 UserAgent 中不包含 MicroMessenger 字符串是一个 bug，后来反馈给微信 Windows Phone 开发的同学确认之后得知这不是一个 bug 而是 **Windows Phone 的 Webview 无法直接修改 UserAgent**，所以客户端开发的同学针对 WindowsPhone 做了一些特殊处理。

Windows Phone 的微信客户端中的 HTTP 请求中 header 信息中会包含：

```
wxuserAgent:MicroMessenger
```

并且，Windows Phone 中的 navigator 对象中存在一个 wxuserAgent 属性。这个属性是在页面加载完成后异步注入的，所以你要能取到 navigator.wxuserAgent 对象需要 1~2s 的延迟。

所以建议增加一个 setTimeout 来做判断：

```js
function isWechat() {
  var ua = navigator.userAgent.toLowerCase();
  return /micromessenger/i.test(ua) || typeof navigator.wxuserAgent !== 'undefined';
}

// 延迟判断
setTimeout(function() {
  if(isWechat()) {
    // handle is wechat...
  } else {
    // handle is not wechat...
  }
}, 2000);
```

## WeixinJSBridge 对象判断

~~之前微信客户端会内置一个 WeixinJSBridge 对象，用于 JS 调用微信内置的一些功能，但是随着微信 JSSDK 推出，该对象已经在新的版本中被废弃。~~

微信的JS API建立在客户端浏览器内置JS对象 WeixinJSBridge 上。然而 WeixinJSBridge 并不是 WebView 一打开就有了，客户端需要初始化这个对象，当这个对象准备好的时候，客户端会抛出事件 "WeixinJSBridgeReady"。因此，WeixinJSBridge 存在与否可以作为判断是否微信浏览器的一种办法。

```js
// callback 函数为微信浏览器的回调
if (typeof WeixinJSBridge == "object" && typeof WeixinJSBridge.invoke == "function") {
  callback();
} else {
  if (document.addEventListener) {
    document.addEventListener("WeixinJSBridgeReady", callback, false);
  } else if (document.attachEvent) {
    document.attachEvent("WeixinJSBridgeReady", callback);
    document.attachEvent("onWeixinJSBridgeReady", callback);
  }
}
```

## OAuth 授权判断

通过 UserAgent 或者 WeixinJSBridge 对象来判断微信客户端并不能保证 100% 的正确，因为客户端可能会模拟 UserAgent 或者伪造 WeixinJSBridge 对象。

在一些并不是很重要的页面，可以考虑使用上面提到的 UserAgent 和对象判断的方法节省工作量。但是，在一些比较重要，限制必须在微信客户端内访问的网页可以使用微信的 OAuth 授权来做判断会更保险。

在微信公众号的[网页授权获取用户基本信息](http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html)里面有提到 scope=snsapi\_base 的授权方式不需要用户确认就能自动跳转并且回调获得用户的 open_id。

使用网页 oauth 授权（**需要拥有一个认证过的微信公众帐号**），简单的梳理一下OAuth 流程大概是：

![微信Oauth授权](/images/wechat_oauth.png)

**利用这个 OAuth 的接口，我们可以根据后端是否能正确的获取到用户的 open_id 来判断用户是否使用微信客户端，并且还能有效的区分不同的微信用户。**

## 第三方授权判断

有不少人是没有认证过的微信公众帐号的，所以也没有办法使用 OAuth 授权的方法判断，所以有人用自己的微信公众号做了个跟使用 OAuth 授权判断微信用户一样的思路的第三方 [weixingate](http://www.weixingate.com/)。

不过使用这种第三方的方法并不推荐，有一定风险。如果有已经认证过的微信公众号还是自己实现一个会比较安全。

## 总结

- 前端做微信客户端判断建议使用 WeixinJSBridge 对象进行判断
- 服务端做微信客户端判断建议使用 OAuth 静默授权进行判断
- 如没有 oauth 权限可以考虑使用 UserAgent 和 HTTP 头信息进行判断
