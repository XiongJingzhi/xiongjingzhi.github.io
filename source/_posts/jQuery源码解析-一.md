---
title: jQuery源码解析(一)
date: 2018-07-1 21:58:32
tags: front-end
---

# jQuery 结构

`jQuery version 1.3 2.2`

jQuery的设计哲学：write less, do more. 在browser端，引入即可使用。
 - 一切皆对象，jQuery也是对象
 - jQuery 是一个不需要new的普通函数
 - jQuery 返回jQuery对象
## 最基本构造函数方式
``` javascript
var $ = jQuery = function() {
  // todo
}
// 别名
jQuery.fn = jQuery.prototype = {
  // 扩展原型对象
  method: todofunction,
}
//使用
var $instance = new $()
$instance.method()
```
这是一般生成对象方式，需要实例化，使用时需声明变量，用起来麻烦。

## 去掉外部new
第一个想法要去掉外部的new，直接`return jQuery()`，必然会出现无限循环。
因此，可以`return jQuery.init()`。
但是，同时考虑返回的jQuery对象需要有`jQuery.prototype`里的属性方法。
`return jQuery.prototype.init()`
``` javascript
var $ = jQuery = function() {
  // new 返回实例化jQuery对象
  return jQuery.prototype.init()
}
// 别名，对象方法属性
jQuery.fn = jQuery.prototype = {
  // 扩展原型对象
  init: function(){
    return this
  },
  method: todofunction,
}
//使用
var $instance = $()
$instance.method()
```
上述方式，init里this属性将覆盖掉外部属性，返回的jQuery对象将公用一条作用链，破坏作用域的独立性。
需要将jQuery.prototype.init()作用域独立出来，即`return new jQuery.prototype.init()`

### 重新建立连接，跨域访问
决定用`return new jQuery.prototype.init()`，便要考虑如何访问`jQuery.prototype`的属性。
方法便是覆盖，重新建立连接。
``` javascript
var $ = jQuery = function() {
  // new 返回实例化jQuery对象
  return new jQuery.prototype.init()
}
// 别名, 对象方法属性
jQuery.fn = jQuery.prototype = {
  // 扩展原型对象
  init: function(){
    return this
  },
  method: todofunction,
}
// 覆盖,重新建立连接
jQuery.prototype.init.prototype = jQuery.fn
//使用
var $instance = $()
$instance.method()
```

如此一来，new出来的jQuery对象，__prototype__ 便指向 $.prototype。
jQuery对象便继承了jQuery.fn的方法。

## 避免全局污染
直接写成代码块的形式显然不合适。
这里采用了立即调用函数的方式进行封装，同时将window当做参数传入，减少变量链的长度。
传入形参undefined，是因为在早期ECMA标准中，undefined不是关键字，可是重写。为了避免这个因素，undefined当做形参，不传入实参，undefined 就等于真的undefined。防止了重写。

``` javascript
(function(window, undefined) {
    // 对外暴露的工厂函数
    function jQuery() {
        return new jQuery.fn.init();
    }
    // 给原型提供一个简写方式
    jQuery.fn = jQuery.prototype = {
        constrcutor: jQuery
    };
    // init是jQuery中真正的构造函数 ==> 入口函数
    var init = jQuery.fn.init = function() {
    };
    // 替换构造函数的原型 为 jQuery工厂的原型
    // 为了实现插件机制，让外界可以透过jQuery.fn扩充方法。
    init.prototype = jQuery.fn;
    // 把工厂通过两个变量暴露出去
    w.jQuery = w.$ = jQuery;
}(window));
```
为了兼容Node.js，可以在这个外面包一层`(function( global, factory )`,用于判断所处环境。

