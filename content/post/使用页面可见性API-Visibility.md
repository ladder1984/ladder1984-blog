---
title: 使用页面可见性API Visibility
date: 2015-09-25 18:38:43
tags: 
 - JavaScript
---

## 介绍

可见性API的好处是判断当前页面是否可见，并基于此执行不同的操作，通常可以节省不少资源，比如轮询，比如视频播放。

可见性API包括两个属性**document.hidden**、**document.visibilityState**和一个事件**visibilitychange**。

## 说明

HTML5扩展了document的接口，增加了`hidden`和`visibilityState`两个属性，它们的含义也比较接近。

`hidden`返回一个布尔值，表示页面是否完全不可见。`visibilityState`表示可见状态，返回一个字符串，可能的返回值是`visible`、`hidden`、`prerender`、`uploaded`，具体含义可以看MDN的介绍[使用页面可见性API](https://developer.mozilla.org/zh-CN/docs/Web/Guide/User_experience/Using_the_Page_Visibility_API)

`visibilitychange`事件当页面可见性改变的时候会触发。

## 示例

结合两个属性和事件，可以很好的根据页面可见性进行一些操作，比如
```JavaScript
document.addEventListener("visibilitychange", func, false);

if(document.visibilityState == 'visible'){  
    //do sth.
}
else if(document.visibilityState == 'hidden'){
    //do sth.

}
```

参考文档：
- http://www.w3.org/TR/page-visibility/
- https://developer.mozilla.org/en-US/docs/Web/Events/visibilitychange
- https://developer.mozilla.org/zh-CN/docs/Web/Guide/User_experience/Using_the_Page_Visibility_API
- http://blogs.msdn.com/b/ie/archive/2011/07/08/using-pc-hardware-more-efficiently-in-html5-new-web-performance-apis-part-2.aspx

