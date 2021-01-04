---
title: WebSockets && SEE
date: 2019-04-03 10:11:37
tags: front-end back-end
---

## WebSockets
  ws 协议
### 解决问题
 - 解决问题：因为HTTP协议是一个client->请求->响应->client协议，所以原有实现，实时聊天，多人在线等即时通讯只能hack：Ajax短轮询或者Comet。

  1. 轮询：指浏览器通过JavaScript启动一个定时器，然后以固定的间隔给服务器发AJax请求，询问服务器有没有新消息。
  2. comet: Comet本质上也是轮询，但是在没有消息的情况下，服务器先拖一段时间，等到有消息了再回复。
    - 基于Ajax的长轮询（long-polling）方式： 
      浏览器发出XMLHttpRequest 请求，服务器端接收到请求后，会阻塞请求直到有数据或者超时才返回，浏览器JS在处理请求返回信息（超时或有效数据）后再次发出请求，重新建立连接。在此期间服务器端可能已经有新的数据到达，服务器会选择把数据保存，直到重新建立连接，浏览器会把所有数据一次性取回
    - 基于 Iframe 及 htmlfile 的流（http streaming）方式
      Iframe是html标记，这个标记的src属性会保持对指定服务器的长连接请求，服务器端则可以不停地返回数据，相对于第一种方式，这种方式跟传统的服务器推则更接近
 - WebSockets用途：它可以在用户的浏览器和服务器之间打开交互式通信会话。

### API实现
  #### socket client端

    ``` javascript
    const socket = new WebSocket('ws://localhost:3000/path/any')
    // Connection opened
    socket.addEventListener('open', function (event) {
      // send data
      socket.send('Hello Server!')
    })
    // Listen for messages
    socket.addEventListener('message', function (event) {
      console.log('Message from server ', event.data)
    })
    // connection close
    WebSocket.close([code[, reason]])
    WebSocket.addEventListener('close', function (event) {

    })
    ```
    1. new WebSocket 建立链接
    2. open之后send发送数据
    3. addEventListener('message')用于指定当从服务器接受到信息时的回调函数
    4. 可以手动关闭close addEventListener('close') 处理关闭的回调函数

  #### node端

  ``` javascript
  const WebSocket = require('ws')

  const WebSocketServer = WebSocket.Server

  // 实例化:
  const wss = new WebSocketServer({
    port: 3000
  })

  wss.on('connection', function(ws) {
    console.log(`[SERVER] connection()`)
    ws.on('message', function(message) {
      console.log(`[SERVER] Received: ${message}`)
      ws.send(`ECHO: ${message}`, (err) => {
        if (err) {
          console.log(`[SERVER] error: ${err}`)
        }
      })
    })
  })
  ```
  0. [wx](https://www.npmjs.com/package/ws)
  1. 使用ws的WebSocketServer, 建立服务监控
  2. connection -> message
  3. ws.send(data), 发送数据

## SEE(Server-Sent Event)
 - SSE能在现有的HTTP/HTTPS协议上运作，所以它能直接运行于现有的代理服务器和认证技术。
 - SSE服务器向客户端声明，接下来要发送的是流信息。本质上，这种通信就是以流信息的方式，完成一次用时很长的下载。

### API实现

  #### client端
  ``` javascript
  // 判断是否支持
  if ('EventSource' in window) {
    // 建立链接, 跨域带凭证
    var source = new EventSource(url[, withCredentials: true])
    
    var div = document.getElementById('example')
    
    source.onopen = function (event) {
      div.innerHTML += '<p>Connection open ...</p>'
    }
    
    source.onerror = function (event) {
      div.innerHTML += '<p>Connection close.</p>'
    }
    
    source.addEventListener('connecttime', function (event) {
      div.innerHTML += ('<p>Start time: ' + event.data + '</p>')
    }, false)
    
    source.onmessage = function (event) {
      div.innerHTML += ('<p>Ping: ' + event.data + '</p>')
    }
  }
  ```

  #### node端
  ``` javascript
  var http = require("http");

  http.createServer(function (req, res) {
    var fileName = "." + req.url;

    if (fileName === "./stream") {
      res.writeHead(200, {
        // 关键在这里，SSE声明
        "Content-Type":"text/event-stream",
        "Cache-Control":"no-cache",
        "Connection":"keep-alive",
        "Access-Control-Allow-Origin": '*',
      });
      res.write("retry: 10000\n");
      res.write("event: connecttime\n");
      res.write("data: " + (new Date()) + "\n\n");
      res.write("data: " + (new Date()) + "\n\n");

      interval = setInterval(function () {
        res.write("data: " + (new Date()) + "\n\n");
      }, 1000);

      req.connection.addListener("close", function () {
        clearInterval(interval);
      }, false);
    }
  }).listen(8844, "127.0.0.1");
  ```


  