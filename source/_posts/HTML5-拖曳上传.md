---
title: HTML5 拖曳上传
date: 2018-08-2 10:47:09
tags: front-end
---

# HTML5 拖曳上传

``` javascript
$(function(){ 
    //阻止浏览器默认行
    $(document).on({ 
        dragleave: function(e){                       //拖离 
            e.preventDefault()
        },
        drop: function(e){                            //拖后放 
            e.preventDefault()
        },
        dragenter: function(e){                       //拖进 
            e.preventDefault()
        },
        dragover: function(e){                        //拖来拖去 
            e.preventDefault()
        }
    })

    var box = document.getElementById('drop_area')    //拖拽区域
    box.addEventListener("drop",function(e){
        e.preventDefault()                            //取消默认浏览器拖拽效果 
        var fileList = e.dataTransfer.files           //获取文件对象
        if(fileList.length == 0){                     //检测是否是拖拽文件到页面的操作
            return false
        }
        //检测文件是不是图片 
        if(fileList[0].type.indexOf('image') === -1){
            alert("您拖的不是图片！")
            return false
        }
        
        //拖拉图片到浏览器，可以实现预览功能 
        var img = window.URL.createObjectURL(fileList[0])
        var filename = fileList[0].name               //图片名称
        var filesize = Math.floor((fileList[0].size) / 1024)
        if(filesize > 5000){
            alert("上传大小不能超过5M")
            return false
        }
        var str = "<img src='" + img + " width='60xp' height='60px''><p>图片名称：" 
        + filename+"</p><p>大小：" + filesize + "KB</p>"
        
        $("#preview").html(str)
        
        //上传 
        xhr = new XMLHttpRequest
        xhr.open("post", "upload.php", true)
        xhr.onreadystatechange = function(){
            if (xhr.readyState !== 4) return
            // 说明响应数据下载完成
            console.log(xhr.responseText)
        }
        xhr.setRequestHeader("X-Requested-With", "XMLHttpRequest")
        var fd = new FormData
        fd.append('pic', fileList[0])
        xhr.send(fd)
    }, false)
})
```
