---
title: 'l2Overlay: 原生 JavaScript 实现 Hoverpop 效果插件'
tags: |-
  - javascript
  - 作品
permalink: l2overlay-a-javascript-based-hoverpop-plugin
id: 22
updated: 2012-05-20 05:34:39
date: 2012-05-20 03:29:21
---

最近想给自己做的项目加上类似于 Facebook 的 Namecard 效果，即鼠标移动到制定目标后浮现一个信息卡，在鼠标移开目标或者信息卡后则自动隐藏。

搜索了一下好像没有现成的插件可用，找到过几个仿 Facebook Style 的代码片段，但是感觉代码质量比我的还要糟糕，而且也不完全符合自己的要求，所以就花了一个下午的时间，用原生 JavaScript 写了一个类 Facebook Style 的 Namecard （hoverpop）插件。

<!--more-->

![l2Overlay](/images/2012/05/l2Overlay.png)

参看演示效果可以到：[l2Overlay 演示](https://lukesign.com/demo/l2Overlay "l2Overlay 演示")

## l2Overlay 的 CSS 代码

l2Overlay 的样式主要是模仿 Facebook 的 Namecard，下面是根据我的 l2Overlay 编写的 CSS 样式：

```css
.l2OverlayWrapper {
  font-size: 12px;
  color: #333333;
  line-height: 1.8;
  background-color: #FFFFFF;
  border: 1px solid #8C8C8C;
  box-shadow: 0 0 10px #C8C8C8;
  margin: 9px 0;
  position: relative;
  max-width: 300px;
  _width: 300px;
}
.l2OverlayArrow {
  position: absolute;
  width: 16px;
  height: 9px;
  background: url("arrow.png") no-repeat -50px 0px transparent;
}
.arrow-lt .l2OverlayArrow {
  left: 15px;
  top: -9px;
}
.arrow-rt .l2OverlayArrow {
  right: 15px;
  top: -9px;
}
.arrow-lb .l2OverlayArrow {
  background-position: -17px 0;
  left: 15px;
  bottom: -9px;
  _bottom: -15px;
}
.arrow-rb .l2OverlayArrow {
  background-position: -17px 0;
  right: 15px;
  bottom: -9px;
  _bottom: -15px;
}
.l2OverlayContent {
  margin: 10px;
}
.l2Action {
  padding: 0 10px;
  margin: 10px -10px -10px;
  background-color: #F2F2F2;
  border-top: 1px solid #CCCCCC;
  line-height: 34px;
  height: 34px;
}
.l2Title {
  border-bottom: 1px dotted #CCCCCC;
  font-size: 14px;
  font-weight: bold;
  margin: 0 0 10px;
}
```

## l2Overlay 的 Javascript 代码

Js 代码不压缩只有 9.88 kb，YUI 压缩后只有 4.15 kb，对于不想只为了一个效果而引用一个库的来说，绝对是很好的选择。

```js
/**
 * Plugin Name:     l2Overlay
 * Plugin Author:   LOO2K
 * Plugin Address:  https://lukesign.com/l2overlay-a-javascript-based-hoverpop-plugin/
 * Plugin Edition:  0.1
 * Last updated:    2012.05.19
 */
function l2Overlay() {
  this.initialize.apply(this, arguments);
}

l2Overlay.prototype = {
  /*
    * object   要应用 l2Overlay 的元素 对象或者数组
    * content  显示内容 'function|html|string|target'
    * option   选项
    * option   = {
    *      'title'     =   'string',                       // defalut undefied
    *      'pos'       =   'auto|lt|lb|rt|tb',             // default auto
    *      'cache'     =   'true|false'                    // default false
    * }
    */
  initialize: function(object, content, option) {
      this.id         = (new Date()).getTime();
      this.createElement();
      var _this       = this;
      this.object     = object;
      this.content    = content;
      this.option     = {
          'title' : option.title || undefined,
          'pos'   : option.pos   || 'auto',
          'cache' : option.cache || false
      };
      this.hidel2Overlay;

      this.l2Overlay.onmouseover = function() {
          _this.cancleHide();
      }
      this.l2Overlay.onmouseout = function() {
          _this.hideOverlay();
      }

      if( typeof this.object === 'object' && typeof this.object.length === 'number') {
          for( var i = 0; i < this.object.length; i++ ) {
              this.addEvent(this.object[i]);
          }
      } else if( typeof this.object == 'object' && !( this.object instanceof Array)) {
          this.addEvent(this.object);
      } else {
          alert('传入对象错误');
      }
  },
  createElement: function() {
      // 生成基本模版
      if( !document.getElementById('l2Overlay_' + this.id) ) {
          var overlay             = document.createElement('div');
          overlay.id              = 'l2Overlay_' + this.id;
          overlay.style.position  = 'absolute';
          overlay.style.zIndex    = '9999';
          overlay.style.display   = 'none';

          var wrapper = document.createElement('div');
          wrapper.className       = 'l2OverlayWrapper';

          var arrow   = document.createElement('div');
          arrow.className         = 'l2OverlayArrow';

          var content = document.createElement('div');
          content.className       = 'l2OverlayContent';

          wrapper.appendChild(arrow);
          wrapper.appendChild(content);
          overlay.appendChild(wrapper);
          document.getElementsByTagName('body')[0].appendChild(overlay);
      }
      this.l2Overlay  = document.getElementById('l2Overlay_' + this.id);
      var wrapper     = this.l2Overlay.getElementsByTagName('div')[0].getElementsByTagName('div');
      this.l2Content  = wrapper[1];
      this.l2Arrow    = wrapper[0];
  },
  getPos: function(el) {
      var ua      = navigator.userAgent.toLowerCase();
      var isOpera = (ua.indexOf('opera') != -1);
      var isIE    = (ua.indexOf('msie') != -1 && !isOpera);
      if( el.parentNode === null || el.style.display == 'none') {
          return false;
      }
      var parent  = null;
      var pos     = [];
      var box;

      if( el.getBoundingClientRect ) {
        // For IE
        box = el.getBoundingClientRect();
        var scrollTop   = Math.max(document.documentElement.scrollTop, document.body.scrollTop);
        var scrollLeft  = Math.max(document.documentElement.scrollLeft, document.body.scrollLeft);
        return {
          x:  box.left + scrollLeft,
          y:  box.top + scrollTop
        };
      } else if( document.getBoxObjectFor ) {
        // For FF
        box             = document.getBoxObjectFor(el);
        var borderLeft  = (el.style.borderLeftWidth) ? parseInt(el.style.borderLeftWidth) : 0;
        var borderTop   = (el.style.borderTopWidth) ? parseInt(el.style.borderTopWidth) : 0;
        pos             = [box.x - borderLeft, box.y - borderTop];
      } else {
        // For Safari & Opera
        pos     = [el.offsetLeft, el.offsetTop];
        parent  = el.offsetParent;
        if( parent != el ) {
          while (parent) {
            pos[0] += parent.offsetLeft;
            pos[1] += parent.offsetTop;
            parent = parent.offsetParent;
          }
        }
        if( ua.indexOf('opera') != -1 || (ua.indexOf('safari') != -1 && el.style.position == 'absolute') ) {
          pos[0] -= document.body.offsetLeft;
          pos[1] -= document.body.offsetTop;
        }
      }

      if( el.parentNode ) {
          parent = el.parentNode;
      } else {
          parent = null;
      }
      // account for any scrolled ancestors
      while (parent && parent.tagName != 'BODY' && parent.tagName != 'HTML') {
          pos[0] -= parent.scrollLeft;
          pos[1] -= parent.scrollTop;
          if (parent.parentNode) {
              parent = parent.parentNode;
          } else {
              parent = null;
          }
      }
      return {
          x:  pos[0],
          y:  pos[1]
      };
  },
  getSize: function(element) {
    if(element.offsetWidth !== 0){
      /* 元素不是display:none的情况，这个时候是能得到尺寸的 */
      return {
        'width':    element.offsetWidth,
        'height':   element.offsetHeight
      };
    }
    var old = {};
    /* 将display:none元素设成visibility:hidden */
    var options = {
      position:   "absolute",
      visibility: "hidden",
      display:    "block"
    }

    for ( var name in options ) {
      old[ name ] = element.style[ name ];
      element.style[ name ] = options[ name ];
    }
    var temp = {
      'width':    element.offsetWidth,
      'height':   element.offsetHeight
    };
    for ( var name in options ) {
      element.style[ name ] = old[ name ];
    }
    return temp;
  },
  hideOverlay: function() {
    var _this = this;
    this.hidel2Overlay = setTimeout(function() {
      _this.l2Overlay.style.display   = 'none';
    }, 100);
  },
  cancleHide: function() {
    if( this.hidel2Overlay ) {
      clearTimeout(this.hidel2Overlay);
    }
  },
  getBrowser: function() {
    var offset = {
      'top':      document.documentElement.scrollTop   || document.body.scrollTop,
      'left':     document.documentElement.scrollLeft  || document.body.scrollLeft
    }
    return offset;
  },
  addEvent: function (object) {
    var _this   = this;
    //var _object = this.object;
    var _object = object;

    _object.onmouseover = function() {
      _this.cancleHide();

      // 缓存模块
      if( _this.option.cache && this.l2cache ) {
        _this.l2Content.innerHTML = this.l2cache;
      } else if( typeof _this.content === 'function' ) {
        _this.l2Content.innerHTML = _this.content.apply(this, arguments);;
        //console.log(_this.content());
      } else {
        _this.l2Content.innerHTML = _this.content;
      }

      if( _this.option.cache && !this.l2cache ) {
        this.l2cache = _this.l2Content.innerHTML;
      }

      // 定位模块
      var position = _this.getPos(this);
      var curTop   = position.y;
      var curLeft  = position.x;
      var offset   = _this.getSize(_this.l2Overlay);
      var self     = _this.getSize(this);
      var browser  = _this.getBrowser();

      var pos = '';
      if( _this.option.pos === 'auto' ) {
          if( (curLeft - browser.left - offset.width) > 0 ) {
            pos += 'r';
          } else {
            pos += 'l';
          }

          if( (curTop - browser.top - offset.height) > 0 ) {
            pos += 'b';
          } else {
            pos += 't';
          }
      } else {
        pos = _this.option.pos;
      }
      switch(pos) {
        case 'lt':
          _this.l2Overlay.className = 'arrow-lt';
          oTop    = curTop + self.height;
          oLeft   = curLeft;
          break;
        case 'rt':
          _this.l2Overlay.className = 'arrow-rt';
          oTop    = curTop + self.height;
          oLeft   = curLeft - offset.width + self.width;
          break;
        case 'rb':
          _this.l2Overlay.className = 'arrow-rb';
          oTop    = curTop - offset.height;
          oLeft   = curLeft - offset.width + self.width;
          break;
        case 'lb':
        default:
          _this.l2Overlay.className = 'arrow-lb';
          oTop    = curTop - offset.height;
          oLeft   = curLeft;
          break;
      }

      _this.l2Overlay.style.top       = oTop + 'px';
      _this.l2Overlay.style.left      = oLeft + 'px';
      _this.l2Overlay.style.display   = '';
    }

    _object.onmouseout = function() {
      _this.hideOverlay();
    }
  }
}
```

## l2Overlay 的使用方法

l2Overlay 创建了一个函数对象 `l2Overlay`，接受三个参数。分别为：

1. `object`: 传入的必须是 HTML 元素，或者是 HTML 元素数组；
2. `content`： 显示内容，可以是函数，HTML 代码，或者字符串，作用域为 object；
3. `option`： option 对象可以设置 l2Overlay 显示的位置以及是否启用缓存；

```js
var applyLinks  = document.getElementById('demo').getElementsByTagName('a');
var content     = function() {
  var html = '<div class="l2Title">这是一个标题</div>';
  html += 'Function 内的作用域为该链接，如执行 this.href 可以得到该链接的地址：';
  html += this.href;
  html += '<div class="l2Action"><a href="#">添加</a> | <a href="#">修改</a> | <a href="#">删除</a></div>';

  return html;
}
var options     = {'pos': '', 'cache': true};
new l2Overlay(applyLinks, content, options);
```

参看演示效果可以到：[l2Overlay 演示](https://lukesign.com/demo/l2Overlay "l2Overlay 演示")

下载插件可以到：[l2Overlay 下载](http://vdisk.weibo.com/s/5CoMp "l2Overlay")

## 小总结

第一次写 JavaScript 插件，功夫不到家，代码仅供参考 :P

其实这个用 jQuery 等库实现会简单很多，而且代码行数也会少很多，但是为了熟悉原生的 JavaScript 还是自己动手写了一遍。
