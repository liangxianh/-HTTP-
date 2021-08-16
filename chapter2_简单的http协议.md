## 简单的http协议

1. HTTP协议用于在客户端和服务端之间的通信

2. 通过请求和响应的交换达成通信,从客户端开始建立通信，服务端在没有接收到请求之前不会发送响应
  > 请求报文包括：请求方法（eg：POST）、请求URI(eg:/form/list)、协议版本(eg:TPPT/1.1)、可选的请求首部字段(eg:HOST:* Connection: keep-alive等)和内容实体（请求参数）
  > 响应保本包括：协议版本、状态码（eg： 200）、用于解释状态码的原因短语（eg: ok）、可选的响应首部字段以及实体主体内容
  
3. HTTP是不保存状态的协议，即http协议自身不对请求和响应之间的通信状态进行保存，对于发送过的请求或响应不做持久化处理；HTTP/1.1虽然无状态，但是为了实现期望的保持状态功能，引入了Cookie技术；

4. 请求URI定位资源，HTTP协议使用URI定位互联网上的资源

5. 告知服务器意图的HTTP方法

  |  方法名   | 作用  | 支持的HTTP协议版本 | 详细说明 |
  |  :----:  | :----:  | :----: | :----: |
  | GET    | 获取资源 | 1.0、1.1 | 用了请求访问已被URI识别的资源 |
  | POST  | 传输实体主体 | 1.0、1.1 | 用了传输实体主体 |
  | PUT   | 传输文件 | 1.0、1.1 | 用于传输文件 |
  | HEAD  | 获得报文首部 | 1.0、1.1 | 和GET方法一样不返回报文主体内容，用于确认URI的有效性及资源更新的日期时间等 |
  | DELETE  | 删除文件 | 1.0、1.1 | 与put方法相反，delete方法按照请求URI删除指定的资源 |
  | OPTIONS  | 询问支持的方法 | 1.1 | 用来查询针对请求URI指定资源支持的方法（eg Allow：GET,POST,HEAD,OPTIONS） | 
  | TRACE  | 追踪路径 | 1.1 | 请求要连接到源目标服务器可能会通过代理中转（中间会被加工或者篡改），TRACE就是用来确认连接过程中发生的一系列操作；请求时设置max-forwards的值，每经过一个服务器端就-1，当数值刚好为0停止传输，当前的服务器或者代理服务器返回请求的响应；容易引起XST（Cross-site tracing,跨站追踪）攻击 |
  | CONNECT  | 要求用隧道协议连接代理 | 1.1 | 要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信；主要使用SSL(Secure Sockets Layer,AN安全套接层)和TLS(Transport Layer Security,传输层安全)协议把通信内容加密后经网络隧道传输 |
  | LINK  | 建立和资源之间的联系 | 1.0 | -- |
  | UNLINK  | 断开连接关系 | 1.0 | -- |
  
6. 使用方法下达命令

7. 持久连接节省通信量，HTTP初版本中没进行一次HTTP通信都要断开一次TCP连接，HTTP/1.1和一份HTTP/1.0想出了持久连接（HTTP Persistent Connections，也称HTTP keep-alive或HTTP connection reuse）；其特点是只要任意一端没有明确提出断开连接，则保持TCP连接状态；

8. 管线化，可以使得不用等待上一次的响应亦可直接发送下一个请求，这样可以同时并行发送多个请求
9. 使用Cookie的状态管理：http是无状态的，为了让服务器记住各个客户端的状态引入了Cookie。Cookie通过在请求和响应报文中写入Cookie信息来控制客户端的状态
   > a 客户端进行无cookie请求； b 服务器端生成Cookie信息并在响应报文中添加set-cookie； c 再次请求报文里面会自动发送保存着的cookie；我们获取到响应报文的cookie信息也可以使用Cookies.set('KEYNAME', VALUE)形式按照自己的医院去设置存储
   ```
   响应报文
   set-cookie: rememberMe=deleteMe; Path=/dd; Max-Age=0; Expires=Sun, 20-Jun-2021 08:15:42 GMT
   set-cookie: JSESSIONID=dfasdffdffa2341231234dfAsdf4; Path=/dd; Max-Age=0; Expires=Sun, 20-Jun-2021 08:15:44 GMT
   
   请求报文
   cookie: JSESSIONID=dfasdffdffa2341231234dfAsdf4; experimentation_subject_id=IjBhZjhlMTg0LTc0MjctNDIyYy05M2I1LTFkMzQyMjg1NmI0OCI%3D--dfasdffdffa2341231234dfAsdf4
   ```
   

1: 前端缓存那些事:[https://juejin.cn/post/6844904105329033230]
