---
title: HTML5相关代码
date: 2018-09-26 23:55:31
tags: front-end
---

## 拖曳

```javascript
var box = document.querySelector('.box')
var container = document.querySelector('.container')

// 整个拖拽都会执行
box.addEventListener('drag', function(e) {
  console.log('drag')
})

// 拖拽的点离开当前盒子
box.addEventListener('dragleave', function() {
  console.log('dragleave')
})

// 拖拽开始
box.addEventListener('dragstart', function() {
  this.style.backgroundColor = 'red'
  console.log('dragstart')
})

// 拖拽结束
box.addEventListener('dragend', function(ev) {
  this.style.backgroundColor = ''
  console.log('dragend')
})
//在目标元素上移动
container.addEventListener('dragover', function(e) {
  this.style.backgroundColor = 'yellow'
  console.log('目标dragover')
  e.preventDefault()
})
//在目标元素松开
container.addEventListener('drop', function(e) {
  this.style.backgroundColor = 'black'
  console.log('目标drop')
  e.preventDefault()
})
//在目标元素离开
container.addEventListener('dragleave', function(e) {
  this.style.backgroundColor = ''
  console.log('目标dragleave')
  e.preventDefault()
})
```

## 文件读取

```javascript
/*获取到了文件表单元素*/
var file = document.querySelector('.file')
/*选择文件后触发*/
file.onchange = function() {
  /*初始化了一个文件读取对象*/
  var reader = new FileReader()
  /*读取文件数据  this.files[0] 文件表单元素选择的第一个文件 */
  reader.readAsDataURL(this.files[0])
  /*读取的过程就相当于 加载过程 */
  /*读取完毕  预览 */
  reader.onload = function() {
    /*读取完毕 base64位数据  表示图片*/
    console.log(this.result)
    document.querySelector('#img').src = this.result
  }
}
```

## 网络状态

```javascript
// 通过window.navigator.onLine可以返回当前的网络状态
alert(window.navigator.onLine)

window.addEventListener('online', function() {
  //alert('online')
  $('.tips')
    .text('网络已连接')
    .fadeIn(500)
    .delay(1000)
    .fadeOut()
})

window.addEventListener('offline', function() {
  //alert('offline')
  $('.tips')
    .text('网络已断开')
    .fadeIn(500)
    .delay(1000)
    .fadeOut()
})
```

## geolocation

```javascript
if (navigator.geolocation) {
  /*getCurrentPosition 获取当前定位信息*/
  navigator.geolocation.getCurrentPosition(
    function(position) {
      /*获取定位信息成功*/
      /*position 定位信息对象*/
      /*position 属性 coords 坐标对象*/
      /*coords 有 经纬度 海拔 ... */
      /* latitude 维度  longitude 经度 */
    },
    function(PositionError) {
      /*获取定位信息失败*/
      /*PositionError 失败信息*/
    }
  )
}
```

## storage

```javascript
$(function() {
  /*1.使用json数据存储搜索历史记录*/
  /*2.预设一个key   historyList */
  /*3.数据格式列表 存的是json格式的数组*/
  /*4. [电脑，手机，。。。。]*/

  /*1.默认根据历史记录渲染历史列表*/
  var historyListJson = localStorage.getItem('historyList') || '[]'
  var historyListArr = JSON.parse(historyListJson)
  /*获取到了数组格式的数据*/
  var render = function() {
    /*$.each(function(i,item){}) for() for in */
    /* forEach 遍历函数  只能数组调用  回到函数（所有对应的值，索引）*/
    var html = ''
    historyListArr.forEach(function(item, i) {
      html +=
        '<li><span>' +
        item +
        '</span><a data-index="' +
        i +
        '" href="javascript:">删除</a></li>'
    })
    html = html || '<li>没有搜索记录</li>'
    $('ul').html(html)
  }
  render()

  /*2.点击搜索的时候更新历史记录渲染列表*/
  $('[type="button"]').on('click', function() {
    var key = $.trim($('[type=search]').val())
    if (!key) {
      alert('请输入搜索关键字')
      return false
    }
    /*追加一条历史*/
    historyListArr.push(key)
    /*保存*/
    localStorage.setItem('historyList', JSON.stringify(historyListArr))
    /*渲染一次*/
    render()
    $('[type=search]').val('')
  })

  /*3.点击删除的时候删除对应的历史记录渲染列表*/
  $('ul').on('click', 'a', function() {
    var index = $(this).data('index')
    /*删除*/
    historyListArr.splice(index, 1)
    /*保存*/
    localStorage.setItem('historyList', JSON.stringify(historyListArr))
    /*渲染一次*/
    render()
  })
  /*4.点击清空的时候清空历史记录渲染列表*/
  $('div a').on('click', function() {
    /*清空*/
    historyListArr = []
    /*慎用  清空网上的所有本地存储*/
    //localStorage.clear()
    //localStorage.removeItem('historyList')
    localStorage.setItem('historyList', '')
    render()
  })
})
```

## history

``` javascript
// window.history.length
// history
// history.back()  去上一条历史
// history.forward() 去下一条历史
// history.go() 相对当前  跳多少条记录   正 前走  负 后退
// history.pushState();
// history.replaceState();
// window.onpopstate = function(){}
/*1.存数据  null*/
/*2.存标题  null*/
/*3.追叫的历史记录的地址*/
history.pushState(null,null,'test.html')
history.replaceState(null,null,'test.html')
```