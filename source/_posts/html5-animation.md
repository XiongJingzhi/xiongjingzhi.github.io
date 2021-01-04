---
title: html5 animation
date: 2018-06-24 09:11:22
tags: front-end
---

## animation

动画，一帧一帧地播放静态页面。
在前端领域无非两种方式实现：
 - JavaScript用DOM API的setInterval函数实现
 - 用CSS3的transition、animation属性实现

### setInterval方式
``` javascript
  function animate(element) {
    //调用此函数先清除timeId，防止重复调用错误
    clearInterval(element.timeId);
    element.timeId = setInterval(function () {
        todoing()
        //完成todoing应清除timeId clearInterval(element.timeId);
      }
    }, 10);
  }
```

### CSS transition animation
``` css
transition: property duration curve  delay;
<!-- 事件、伪类触发，element property的变化，transition自动脑补 -->
transform: translate(x, y) || scale(x, y) || rotate(180deg) || skew(180deg, deg);
<!-- 同时transform也可以操控3d，x左负右正；y上负下正；z里负外正 -->
```
``` css
animation: name duration curve delay count direction;
@keyframes name {
    /* percent represent during time*/
    0%{}
    50%{}
    100%{}
}
```