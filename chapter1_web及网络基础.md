## 了解web及网络基础

### 使用http协议访问web
> web使用一种名为HTTP(HyperText Transfer Protocol,超文本传输协议[源](https://www.ituring.com.cn/article/1817) ) 的协议作为规范，完成从客户端到服务端等一系列运作流程；

### http诞生
> 为知识共享而规划web。最初理念：借助多文档之间相互关联形成超文本（HyperText）,连成可相互参阅的WWW（World Wide Web，万维网）。
> 现今已提出来3项WWW技术: 现在用来表示这一系列的集合，可也以简称为Web
  1. SGML(Standard Generalized Markup Language,标准通用标记语言)作为页面的文本标记语言的HTML
  2. 作为文档传输协议的HTTP
  3. 指定文档所在地址的URL(Uniform Resource Locator,统一资源定位符)

### 网络基础TCP/IP
> tcp/ip 协议族进行分层管理
  1. 应用层：决定了向用户提供应用服务时通信的活动,如FTP DNS HTTP
  2. 传输层：对上传应用层提供处于网络连接中的两台计算机之间的数据传输，包括TCP UDP两个协议
  3. 网络层：用来处理在网络上流动的数据包，数据包是网络传输的最小数据单位，该层规定了通过怎样的路径到达对方计算机，并把数据包传送给对方
  4. 链路层：用来处理链接网络的硬件部分，包括操作系统，硬件的的设备驱动,NIC（Network Interface Card网卡）

> TCP/IP通信传输流
> 
              客户端                         服务端
     应用层   HTTP客户端                      HTTP  
     传输层   Tcp                            TCP   
     网络层   IP                             IP   
     链路层   网络                           网络  
     
  1. 应用层http协议发出http请求
  2. 传输层从应用层获得的数据(http报文)打标记及端口号
  3. 网络层增加作为通信目的地的MAC地址后转发给链路层
  4. 接收端服务器在链路层接收到数据，按序往上层发送


### 与HTTP关系密切的协议IP TCP DNS
> 负责传输的IP协议（Internet protocol）
  > IP协议的作用是把各种数据包传递给对方，要保证能够确实传递给对方需要IP地址和MAC地址（Media Access Control Address）；IP地址指明了节点被分配到的地址，MAC地址是指网卡所属的固定地址；量地址可以进行配对，IP地址可变化，但是MAC地址基本不会更改；IP间通信依赖MAC地址,在网络上通信双方在同一个局域网内的情况是很少的，通常是经过多台计算机和网络设备中转才能连接到对方，进行中转时，会利用下一站中转设备的MAC地址来搜索下一个中转目标；ARP（Address Resolution Protocol）协议是一种用于解析地址的协议，根据通信的IP地址就反查出对应的MAC地址；
     > IP地址和MAC地址的区别主要有：
       1. 对于网络上的某一设备，如一台计算机或一台路由器，其IP地址是基于网络拓扑设计出的，同一台设备或计算机上，改动IP地址是很容易的（但必须唯一），而MAC则是生产厂商烧录好的，一般不能改动。我们可以根据需要给一台主机指定任意的IP地址，如我们可以给局域网上的某台计算机分配IP地址为192.168.0.112 ，也可以将它改成192.168.0.200。而任一网络设备（如网卡，路由器）一旦生产出来以后，其MAC地址不可由本地连接内的配置进行修改。如果一个计算机的网卡坏了，在更换网卡之后，该计算机的MAC地址就变了。.
       2. 长度不同。IP地址为32位，MAC地址为48位。 
       3. 分配依据不同。IP地址的分配是基于网络拓扑，MAC地址的分配是基于制造商。 
       4. 寻址协议层不同。IP地址应用于OSI第三层，即网络层，而MAC地址应用在OSI第二层，即数据链路层。 数据链路层协议可以使数据从一个节点传递到相同链路的另一个节点上（通过MAC地址），而网络层协议使数据可以从一个网络传递到另一个网络上（ARP根据目的IP地址，找到中间节点的MAC地址，通过中间节点传送，从而最终到达目的网络）
 
> 确保可靠性的TCP协议：位于传输层，提供可靠的字节流服务（为了方便传输，将大块数据分割成以报文段为单位的数据包进行管理）
> 负责域名解析的DNS：DNS服务和http协议一样位于应用层，提供域名到IP地址之间的解析服务（提供通过域名查找IP地址，或者逆向从IP地址反查域名的服务）

### 各种协议与HTTP协议的关系
```
客户端（想浏览http://www.baidu.com） 向DNS询问www.baidu.com的ip地址 ----> DNS
HTTP生成针对目标web服务器的HTTP请求报文（请求http://www.baidu.com） 页面的资源
TCP将HTTP请求报文分割成报文段，把每个报文段可靠地传递给对方
IP协议搜索对方的地址，一边中转一边传送
TCP从对方处接收到报文段，重组
HTTP对WEB服务器请求的内容处理，了解想要www.baidu.com 的资源，经处理的结果利用TCP/IP通信协议向用户进行回传；
```

### URI和URL
> URL(Uniform Resource Locator，统一资源定位符)及我们web浏览器输入的网址；表示资源的地点
> URI(Uniform Resource Identifier,统一资源标识符)，URL是URI的子集，用字符串表示某一互联网资源，j就是由某个协议方案表示的资源定位表示符；如下几种：
```
ftp://ftp.is.co.za/rfc/rfc1808.txt (also a URL because of the protocol)
http://www.ietf.org/rfc/rfc2396.txt (also a URL because of the protocol)
ldap://[2001:db8::7]/c=GB?objectClass?one (also a URL because of the protocol)
mailto:John.Doe@example.com (also a URL because of the protocol)
news:comp.infosystems.www.servers.unix (also a URL because of the protocol)
tel:+1-816-555-1212
telnet://192.0.2.16:80/ (also a URL because of the protocol)
urn:oasis:names:specification:docbook:dtd:xml:4.1.2

file:///~/calendar

   ftp://ftp.is.co.za/rfc/rfc1808.txt
      --文件传输协议服务的ftp方案
   gopher://spinaltap.micro.umn.edu/00/Weather/California/Los%20Angeles
      -- Gopher 和 Gopher+ 协议服务的 gopher 方案
   http://www.math.uio.no/faq/compression-faq/part1.html
      -- 超文本传输​​协议服务的http方案
   邮箱：mduerst@ifi.unizh.ch
      -- 电子邮件地址的 mailto 方案
   新闻：comp.infosystems.www.servers.unix
      -- USENET 新闻组和文章的新闻方案
   telnet://melvyl.ucop.edu/
      -- 通过 TELNET 协议进行交互式服务的 telnet 方案
```
```
URI格式由URI协议名（例如http、ftp、mailto、file），一个冒号，和协议对应的内容所构成。特定的协议定义了协议内容的语法和语义，而所有的协议都必须遵循一定的URI文法通用规则，亦即为某些专门目的保留部分特殊字符。URI文法同时也就各种原因对协议内容加以其他的限制，例如，保证各种分层协议之间的协同性。百分号编码也为URI提供附加信息。
URL的格式由下列三部分组成:第一部分是协议（或称为服务方式）；第二部分是存有该资源的主机IP地址（有时也包括端口号）；第三部分是主机资源的具体地址。
具体URI的格式如下：
[协议名]://[用户名]:[密码]@[服务器地址]:[服务器端口号]/[路径]?[查询字符串]#[片段ID]
```








备注：web服务器相关记录点 [常见的web服务器](http://www.elecfans.com/baike/computer/fuwuqi/202005061213811.html)

  > WEB服务器有哪几种
  如今的Web服务器有很多种，大家在做项目的时候根据自己的需求进行灵活的选择。下面小编就给大家分享一下目前都有哪些Web服务器。
　　1. Apache也被叫做httpd服务器，是目前使用最广泛的web服务器，它被应用于各种平台之中。Apache刚开始被推出的时候有很多的缺陷，如今已经被修复的越来越完善，如果你是web服务器的钻研者，小编建议你一定要学习一下Apache的使用。
　　2. Nginx是Linux平台下的优秀Web服务器，小编以前用过这个服务器，它让本来运行很慢的应用程序提升了很大的速度。
　　3. IIS是微软平台的Web服务器，是针对Windows平台的服务器。它和NET语言非常的搭配，新手掌握起来也比较快。
　　4. WebLogic是专门应用于企业级开发的web服务器，比较常见的是和Java语言搭配，使用起来和Apache差不多，同时它的架构也非常的优秀。
　　5. Tomcat是很多Java学习者都非常熟悉的web服务器，一般刚开始学习Java Web开发的人都会使用Tomcat作为服务器进行练习。它既有可视化的操作界面，也有命令语句，是不错的入门级服务器。
