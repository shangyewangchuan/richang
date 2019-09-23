# websocket反向代理

> ## websocket

?> WebSocket是一种在单个TCP连接上进行全双工通信的协议。WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

### websocket 特点 {docsify-ignore}

- 建立在 TCP 协议之上，服务器端的实现比较容易。
- 与 HTTP 协议有着良好的兼容性。
- 数据格式比较轻量，性能开销小，通信高效。
- 可以发送文本，也可以发送二进制数据。
- 没有同源限制，客户端可以与任意服务器通信。
- 协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。

      ws://example.com:80/some/path
- 数据分片模式是有序的，不支持多路复用。
- 不支持压缩
- 扩展头部
  - Sec-WebSocket-Version ：协商WebSocket版本
  - Sec-WebSocket-Key ：客户端自动生成的key，验证服务器支持请求的协议版本
  - Sec-WebSocket-Accept ：服务器响应Sec-WebSocket-Key的协议版本，包含客户端生成的key
  - Sec-WebSocket-Protocol ：协商应用子协议
  - Sec-WebSocket-Extensions ：协商WebSocket扩展

### [websocket 数据帧](https://segmentfault.com/a/1190000017792977)

> ## Nginx反向代理websocket

### 功能 {docsify-ignore}

- 反向代理websocket

### 模块 {docsify-ignore}

- ngx_http_proxy_module

### 代理websocket前提

- HTTP连接版本为HTTP1.1
- HTTP Header中包含以下头部值
  - Upgrade: websocket
  - Connection: upgrade

> ## 实例 ：Nginx反向代理websocket

  <center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="https://raw.githubusercontent.com/shangyewangchuan/material/master/img/nginx_websocket.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">代理websocket流程</div>
  </center>

### 在线测试websocket {docsify-ignore}

由于websocket搭建比较繁琐，客户端采用[在线模拟websocket请求工具客户端](http://www.bejson.com/httputil/websocket/)，服务端采用公开的websocket测试服务端 ：`echo.websocket.org`。

### Nginx.conf

```nginx
server {
    listen 81;
    server_name localhost;

    location / {
        #上游服务器
        proxy_pass http://echo.websocket.org;
        # 设置HTTP版本
        proxy_http_version 1.1;
        #设置Upgrade头部
        proxy_set_header Upgrade $http_upgrade;
        #设置Connection头部
        proxy_set_header Connection "upgrade";
    }
}
```

### 测试 {docsify-ignore}

#### 1.测试websocket请求工具和echo.websocket.org服务端是否正常

![测试1](https://raw.githubusercontent.com/shangyewangchuan/material/master/img/nginx_websocket_2.png)

#### 2.测试Nginx反向代理websocket

![测试2](https://raw.githubusercontent.com/shangyewangchuan/material/master/img/nginx_websocket_3.png)