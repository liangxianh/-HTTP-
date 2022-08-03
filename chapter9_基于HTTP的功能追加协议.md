## 基于HTTP的功能追加协议

### 1 基于http协议

http功能的不足可以通过创建一套全新的协议来补充；但是鉴于目前HTTP的web浏览器使用环境已遍布全球，无法完全抛弃http；一些新协议的规则时基于http的，并在此基础上添加了新功能；

### 2 消除http瓶颈的SPDY

* http的瓶颈

当服务器上的海量内容被更新就需要直接将内容反馈到客户端的界面上；而http想探知服务器上内容是否更新就需要频繁的到服务器确认（无论内容是否更新，都要把数据重新发送一份）；若没有更新，就会产生徒劳的通信；

```
 客户端                                                     服务端
 请求（发送确认更新情况的请求） ---------------------------->                    
    <------（无论内容是否更新，都要把数据重新发送一份）------    响应
    
  请求 ---------------（每次都互相发送相同的首部）---------->                    
    <--------------------------------------   响应 (有时不压缩) 
     
```

1. 一条链接上只可以发送一个请求；
2. 请求只能从客户端开使，客户端不可以接收除响应意外的指令；
3. 请求、响应首部未经压缩就发送，首部信息越多延迟越大；
4. 发送冗长的首部，每次发送相同的首部造成的浪费较多；
5. 可任意选择数据压缩格式，非强制压缩发送；

> Ajax的解决方法

1. 可以实现只更新局部页面
2. 未解决协议本身问题，可能导致大量请求产生；

> comet的解决方法

Comet是一种用于web的推送技术；能使服务器实时地将更新的信息传送到客户端，而无须客户端发出请求，目前有两种实现方式，长轮询和iframe流。
1. 长轮询，是在打开一条连接以后保持，等待服务器推送来数据再关闭的方式。
2. iframe流方式是在页面中插入一个隐藏的iframe，利用其src属性在服务器和客户端之间建立一条长链接，服务器向iframe传输数据（通常是HTML，内有负责插入信息的javascript），来实时更新页面。 iframe流方式的优点是浏览器兼容好，Google公司在一些产品中使用了iframe流，如Google Talk。
在HTML5标准中，定义了客户端和服务器通讯的WebSocket方式，在得到浏览器支持以后，WebSocket将会取代Comet成为服务器推送的方法，目前Google Chrome、Firefox、Opera、Safari等主流版本均支持，Internet Explorer从10开始支持。

参考：[Comet-维基百科](https://zh.m.wikipedia.org/zh-hans/Comet_(web%E6%8A%80%E6%9C%AF))

1. 通过延迟应答，来模拟实现服务端向客户端推送的功能；通常服务端接收到请求，在处理完毕会立即返回响应，但是为了实现推送功能，Comet会将响应置于挂起状态，当服务器端有内容更新时在返回响应；
2. 内容上虽然说可以做到实时更新，但是为了保留响应，一次连接的时间也变长了；会消耗更多的资源；
3. Comet也未解决协议本身问题
 
> SPDY的目标

SPDY（发音如英语：speedy），一种开放的网路传输协定，由Google开发，用来传送网页内容。基于传输控制协议（TCP）的应用层协议。SPDY也就是HTTP/2的前身。Google最早是在Chromium中提出的SPDY协议[1]。被用于Google Chrome浏览器中来访问Google的SSL加密服务。SPDY并不是首字母缩略字，而仅仅是"speedy"的缩写。SPDY现为Google的商标[2]。HTTP/2的关键功能主要来自SPDY技术，换言之，SPDY的成果被采纳而最终演变为HTTP/2。

参考： [SPDY——维基百科](https://zh.m.wikipedia.org/zh-hans/SPDY)

陆续出现的ajax和Comet，一定程度上使http得到了改善；但http本身的限制需要在协议层面做些改动
 
* SPDY的设计与功能

```
HTTP  应用层
SPDY  会话层

SSL   表示层

TCP   传输层
```
SPDY 还是采用HTTP建立通信连接，可以照常使用http的get post cookie http报文等

使用SPDY后，http协议额外获得如下功能：

1. 多路复用流：通过单一的tcp连接，可以无限制的处理多个http请求；
2. 赋予请求优先级：不仅可以并发处理请求，对这些请求会逐个的分配优先级；
3. 压缩http首部：压缩请求和响应的首部；
4. 推送功能：支持服务器向客户端推送数据的功能；
5. 服务器提示功能：s端可以主动提示客户端请求所需要的资源；

* SPDY消除了web瓶颈么？

因SPDY基本上只是将单个域名（IP地址）的通信多路复用，所以当一个网站使用多个域名下的资源，改善效果就会受到限制；

### 3 使用浏览器进行全双工通信的WebSocket

weblsocket网络技术是一套新协议及API

* websocket的设计与功能

是web浏览器和web服务器之间的全双工通信标准，

* websocket协议

有如下特点：
1. 推送功能：全双工；
2. 减少通信量：对比http，只要建立连接后保持连接状态，减少http每次连接时的开销，而且websocket首部信息也时很小的；

> 握手请求

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection:Upgrade
Sec-Websocket-Key: dghjkJKJKKJKDJFKDJF==   // (Sec-Websocket-Key记录握手过程中必不可少的简直)
Origin: http://example.com 
Sec-WebSocket-Protocol: chat,superchat    //(Sec-WebSocket-Protocol记录使用的子协议)
Sec-WebSocket-Version: 13
```

> 握手响应

```
HTTP/1.1 101 SwitchingProtocols
Upgrade: websocket
Connection:Upgrade
Sec-WebSocket-Accept: djkJKJKJFKkjkdjf= 
Sec-WebSocket-Protocol: chat,superchat 
```
成功建立连接之后，通信时将不在使用HTTP的数据帧，而时采用websocket独立的数据帧；

> websocket api

详见：[serve-websockey api](https://www.npmjs.com/package/koa-websocket)

websocket---一个同事开发的zip包上传解析记录实时展示的实例
web端利用new Websocket(api url接口全路径地址) 
server端接收请求 ping返回pong等保持心跳
web init
server init
开始数据连接
服务端向web端发送消息
web端关注信息内容 可能是pong  init  data  close等
比如web端接收到的是data数据（比如是文件解析记录）, 将数据实时展示到页面列表里面
参考：

1. [web-websocket api](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

2. [serve-websockey api](https://www.npmjs.com/package/koa-websocket)

3. [简单的示例](https://juejin.cn/post/6844904005919834119)


### 4 期待已久的HTTP2.0

参考文档：

[1.http1.mdn](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Connection_management_in_HTTP_1.x)

[2.协议如何升级mdn](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Protocol_upgrade_mechanism)

[3.http进化史](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)
