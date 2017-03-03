---
layout: post
title: "基于微积分算法的手势库eTouch"
date: 2017-02-28 13:16:40 +0800
tags: tag
---

### eTouch是一个移动端无依赖js手势库

众所周知，原生js的touch事件不能很好的识别移动端手势，在日常开发中往往依赖hammer，或者使用zepto、vue等框架的touch插件。

但即便小如hammer，很多手势日常中也不必使用。想要自己造个轮子、或者是更小的手势库？不妨看看MeCKodo巨巨的eTouch。

eTouch源码不到250行，使用微积分算法识别手势路径，应用于最常见的点击、上下左右滑动。

### demo

[eTouch演示和说明](http://meckodo.github.io/eTouch/clock)

### eTouch使用

事件代理的方式：

``` html
<body>
  <div id="pox">
    <ul>
      <li>Lorem ipsum dolor sit amet.1</li>
      <li>Lorem ipsum dolor sit amet.2</li>
      <li>Lorem ipsum dolor sit amet.3</li>
    </ul>
  </div>

  <script type="text/javascript">
    //事件代理 !!!!!第一个参数仅支持id!!!!!!
    //支持复杂选择器代理
    etouch('#pox','ul li',function(e,touch) {
      // e为事件对象，touch为触摸返回对象
      e.clock = true;  //给div加锁,完全阻止默认事件

      alert(this.innerHTML);
      console.log('点击（tap）');
    }).on('swiper',function(e,touch) {
      console.log('实时获取滑动（swiper）');
    }).on('up',function(e,touch) {
      console.log('上滑回调');
    }).on('down',function(e,touch) {
      console.log('下滑回调');
    }).on('left',function(e,touch) {
      console.log('左滑回调');
    }).on('right',function(e,touch) {
      console.log('右滑回调');
    });
  </script>
</body>
```

批量绑定的方式：

``` html
<body>
  <div id="pox">
    <ul>
      <li>Lorem ipsum dolor sit amet.1</li>
      <li>Lorem ipsum dolor sit amet.2</li>
      <li>Lorem ipsum dolor sit amet.3</li>
    </ul>
  </div>

  <script>
    // 直接事件批量绑定
    etouch('li', function(e,touch) {
      console.log(this,e,touch);
    }).on('left',function() {})
    // e为事件对象，touch为触摸返回对象
  </script>
</body>
```

注意事项：

- 支持AMD和CMD加载
- `e.clock = true` 给div加锁，会调用 `e.preventDefault()` 阻止默认事件
- 事件代理第一个参数仅支持id作为参数

### 源码分析

eTouch将一个触摸状态抽象成了 `eTouch.touchObj` ，包含7个属性：`status,pageX,pageY,clientX,clientY,distanceX,distanceY` 。

status描述触控类型：swiper滑动中、tap轻触、up、right、down、left。

实际用于计算distance的是pageX/Y，即触发点相对文档区域左上角距离（clientX/Y获取到的是触发点相对浏览器可视区域左上角距离，不随页面滚动而改变）。

在处理swiper滑动事件中使用了微积分公式，适时调用 `e.preventDefault()` ，避免移动中的手势调用移动端浏览器的一些默认行为（如页面后退、滑动）。

当触摸时间小于150ms或触摸位移小于2px时，设status为tap轻触事件，返回触发的dom。

复杂选择器实现使用的是querySelector和遍历。


### 项目中的应用

[滑动删除选项](http://meckodo.github.io/eTouch/list.html)

demo里左右滑动的距离使用了eTouch的返回对象touch，计算 `touch.distanceX` 来得到css效果需要的像素值。

<!--
1. 是什么，解决什么问题
2. 什么样子，预览
3. 如何使用
4. 如何工作的（源码分析，基本流程）
5. 总结，收获-->
