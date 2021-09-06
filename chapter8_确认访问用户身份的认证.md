### 确认访问用户身份的认证
某些web页面只想让特定的人浏览，或者干脆仅本人可见，为达到这个目标，必不可少的就是认证功能；

1. 何为认证
计算机无法确认网络的那头究竟是谁，不知道使用者的身份，就需要使用者自报家门，并核对该人是否有系统的权限（核对登录者本人才知道的信息，才会有的信息）
* 密码：只有本人才会知道的字符串信息；
* 动态令牌：仅限本人持有的设备内显示的一次性密码；
* 数字认证：仅限本人（终端）持有的信息
* 生物认证：指纹和虹膜等本人的生理信息；
* IC卡等：仅限本人持有的信息

http使用的认证方式
* BASIC认证（基本认证）
* DIGEST认证（摘要认证）
* SSl客户端认证
* FormBase认证（基于表单认证）

2. BASIC认证
basic认证是http/1.0就定义的认证方式，是web服务器与通信客户端之间进行的认证方式；现在仍有一部分网站会使用这种认证方式；如下的认证步骤：

```
客户端                                                         服务器
    1 ----> 发送请求GET/private/http/1。1    ---->
            host：hackr.jp
            
     <----- 返回状态码401告知客户端需要进行认证,附带WWW-Authenticate首部字段    <----  2
            http/1.1 401 Authorization Required
            Date:Mon,19,Sep 2012 08:21:32 GMT
            Server:Apache/2.2.3(Unix)
            WWW-Authenticate:Basic realm="input Your ID and Password"
            (WWW-Authenticate保护认证方式和RequestURI安全域字符串（realm）)
            
    3 ----> 用户ID和密码以base64方式编码后发送       ---->  
            ID:密码----> 经base64编码处理 --->结果为Z3V\X3Q6Z3VLC3Q写入到Authorization首部字段中
            GET /private/HTTP/1.1
            Host:hackr.jp
            Authorization:Basic Z3V\X3Q6Z3VLC3Q
            
     <----- 认证成功返回状态码200（包含Request-URI），失败则401（接收到包含Authorization首部的请求）  <----  4
            HTTP/1.1 200 OK
            Date:Mon,19,Sep 2012 08:21:32 GMT
            Server:Apache/2.2.3(Unix)

```
basic认证采用base64编码，并不是加密 传递的id和password在http等非加密通信的线路傻姑娘进行basic认证的过程中可能被盗或被监听； 

3. DIGEST认证
为了弥补basic认证的弱点，从http/1.1起就有了digest认证，同样使用质询/响应的方式；与basic不同的是发送的内容是摘要及质询码产生的计算结果不是id和密码，故而降低了密码泄漏的可能性；详细步骤如下：
```
客户端                                                         服务器
    1 ----> 发送请求GET/digest/http/1。1    ---->
            host：hackr.jp  
            
     <----- 发送临时的质询码（随机数，nonce）以及告知需要认证的状态码401,附带WWW-Authenticate首部字段    <----  2
            http/1.1 401 Authorization Required
            WWW-Authenticate:Digest realm="DIGEST"
            nonce:"MQSQZoit=343jdfk34",algorithm=MD5,qop="auth" 
            (WWW-Authenticate字段内必须包含realm和nonce，客户端就是依靠向服务端回送这两个值进行认证的
            nonce是一种每次随返回的401响应生成的任意随机字符串，该字符串通常推荐由Base64编码的16进制数的组成形式)
            
    3 ----> 返回响应中包含Authorization信息，发送摘要以及由质询码计算出的响应码       ---->  
            ID:密码----> 经base64编码处理 --->结果为Z3V\X3Q6Z3VLC3Q写入到Authorization首部字段中
            GET /digest/HTTP/1.1
            Host:hackr.jp
            Authorization:Digest username“guest”，realm=”DIGEST“
            nonce:"MQSQZoit=343jdfk34",uri="/digest/",algorithm=MD5,response="df563*****"
            ,qop="auth",nc=000000001,cnonce="08dfjkdj 740"
            (Authorization内必须包含username，realm，nonce，uri，response信息，其中
            realm和nonce是从服务器的响应字段；
            username是realm限定范围内可进行认证的用户名
            uri（digest-uri）即Request-URI的值，代理转发后改值可能会变化，因此事先会复制一份副本保存在uri内，
            response也可叫做Request-Digest，存放经过MD5运算后的秘码字符串，形成响应码)
            
     <----- 认证成功返回状态码200（包含Request-URI并且会在首部字段Authorization-Info写入一些认证成功的相关信息），失败则401（接收到包含Authorization首部的请求）  <----  4
            HTTP/1.1 200 OK
            Authorization-Info:rspauth="**",cnonce="**",nc=0000001,qop=auth
           
```
digest存在密码被窃听的保护机制，但是并不存在防止用户伪装的保护机制；

4. SSl客户端认证
如果从用户id和密码方面来看，两者都正确即可认证是本人行为，若id和密码被盗用，很可能被第三者冒充；利用SSL客户端认证可以避免该情况发生；
达到SSl客户端认证，需要事先将客户端证书分发给客户端，且客户端必须安装此证书；
> 认证步骤
* 步骤一：接收到需要认证资源的请求，服务器会发送certificate Request报文，要求客户端提供客户端证书；
* 步骤二：用户选择将发送的客户端证书（包含公开密钥）后，客户端会把证书信息以Client Certificate报文方式发送给服务器；
* 步骤三：服务器验证客户端证书通过后方可领取证书内客户端的公开密钥，然后开始https加密通信；

> SSL认证采用双因素认证
多数情况下，ssl客户端认证不会仅依靠证书完成认证，一般会和基于表单认证组合形成一种双因素认证；
即 第一个认证因素的ssl客户端证书用来认证客户端计算机；另一个认证因素的密码则用来确定这是用户本人的行为；

> SSL客户端认证必要的费用


5. FormBase认证（基于表单认证）
该方法不是在http协议中定义的；客户端会向服务器上的web应用程序发送登录信息（credential），按登录信息的验证结果认证；现在认证多半为基于表单的验证；但是没有统一的标准规范；
由于使用上的便利性及安全性问题，http协议标准提供的basic认证和digest认证几乎不怎么使用，而ssl客户端认证虽然具有较高的安全性，但是因导入及费用问题还尚未普及；

> session管理和cookie应用
一般会使用cookie来管理session（会话），1以弥补http协议中不存在的状态管理功能（之前已认证成功过的用户无法通过协议层面保存下来）；
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
> 步骤
* 步骤一：client端将id和密码放入报文实体以post方式发送给服务端，
* 步骤二：服务器发送用以识别用户的sessionid，验证客户端发送过来的信息，把用户的认证状态和sessionid绑定后记录在服务端；sessionid被盗的话，对方就可以伪装成你的身份进行恶意操作了；为了防止跨站脚本攻击（xss）造成的损失，见识现在cookie内加上httponly属性
* 步骤三：客户端接收到信息后会将其作为cookie保存在本地；下次向服务器发送请求时，浏览器会自动发送cookie；





















