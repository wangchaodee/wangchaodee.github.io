# WebSocket 学习


### 文档编写记录

版本    |   说明    |   日期   | 编写者 
-------| ----------| ---------| --------
 0.1   | 第一次学习记录 |  2019-04-11 |  汪超
 

## 基础概念

### 1、产生背景

HTTP协议中服务器不能主动发消息给客户端。客户端想从服务器获取消息，主要通过轮询和长轮询两种方式。  
轮询时： 客户端需要间隔地发请求到服务器，没有消息返回无，有消息则返回消息。  
长轮询时：客户端发送请求给服务器后，服务器会一直阻塞到有消息才返回。  
两种方式下服务器都是被动的。  

### 2、WebSocket是什么

WebSocket是HTML5里面的协议，弥补了上面HTTP协议缺陷。可用于解决客户端和服务器之间的实时通信。  
它提供使用一个TCP连接进行双向通讯的机制，包括网络协议和API，取代网页和服务器采用HTTP轮询进行双向通讯的机制。  

### 3、连接建立的过程
WebSocket使用HTTP的端口，因此TCP连接建立后的握手消息是基于HTTP的，由服务器判断这是一个HTTP协议，还是WebSocket协议。 WebSocket连接除了建立和关闭时的握手，数据传输和HTTP没有任何关系。  

客户端想服务端发送请求示例：  
  ```
  GET /chat HTTP/1.1
  Host: server.example.com
  Upgrade: websocket     //表明协议
  Connection: Upgrade
  Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
  Sec-WebSocket-Protocol: chat, superchat
  Sec-WebSocket-Version: 13
  Origin: http://example.com
  ```

然后服务器依据客户端的请求头信息，生成16位安全密钥（Sec-WebSocket-Accept）返回给客户端，就表示WebSocket连接成功。
```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```

一次握手成功之后，客户端和服务器之间就可以建立持久连接的双向传输数据通道，而且服务器不需要被动地等客户端的请求，服务器这边有新消息就可以通知客户端，化被动为主动。

此外，使用WebSocket时，不会像HTTP一样无状态，服务器会一直知道客户端的身份。服务器与客户端之间交换的标头信息也很小







