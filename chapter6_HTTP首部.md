## HTTP首部

HTTP协议的请求和响应报文中必定包含HTTP首部；

#### 一 HTTP 报文首部
* 请求报文首部：包括方法、URI、HTTP版本、HTTP首部字段
```
GET / HTTP/1.1
Host: hackr.jp
Accept: text/html,application/xhtml+xml,****
```
* 响应报文首部：包括HTTP版本、状态码（数字和原因短语）、HTTP首部字段
```
HTTP/1.1 304 Not Modified
Server: Apache
Etag: '3234-34a-2342343'
```

#### 二 HTTP 首部字段
HTTP首部字段传递重要信息，可以提供报文主体大小，所使用的语言，认证信息等
* 格式
 > 字段名: 字段值(可以是多个值用,号隔开) eg Content-Type: text/html； Keep-Alive: timeout=15，max=100；
 ```
 字段名重复的话，不同浏览器处理结果不同，有的会是处理第一次出现的，有的处理第二次出现的；
 ```
 * 四种HTTP首部字段类型,根据实际用途分为四种
 > 通用首部字段(General Header Fields): 请求报文和响应报文都会使用的首部；
 > 请求首部字段(Request Header Fields): 客户端请求使用的；补充了请求的附加内容，客户端信息，响应内容相关优先级等信息；
 > 通用首部字段(Response Header Fields): 响应使用的；补充了附加内容，也会要求客户端附加额外的内容信息；
 > 实体首部字段(Entity Header Fields): 针对请求报文和响应报文的实体部分使用的首部，补充了资源的内容更新时间等与实体有关的信息

* HTTP/1.1规范了如下47中首部字段
1. 通用首部字段

|  首部字段名   |  说明    |
|      :-      |    :-   |
|Cache-Control | 控制缓存的行为 |
|Connection | 逐跳首部、连接的管理|
|Date      | 创建报文的日期时间 |
|Pragma | 报文指令 |
|Trailer | 报文末端的首部一览 |
|Transfer-Encoding  | 指定报文主体的传输编码方式 |
|Upgrade | 检测是否可以使用其他协议|
|Via  | 代理服务器相关信息，为了追踪传输路径，经常和TRACE方法一起使用 |
|Warning  | 告知用户一些与缓存相关的问题的警告 |

2. 请求首部字段

|  首部字段名   |  说明    |
|      :-      |    :-    |
|Accept | 用户代理可处理的媒体类型 |
|Accept-Charset | 优先的字符集 |
|Accept-Encoding  | 优先的内容编码 |
|Accept-Language  | 优先的语言 |
|Authorization  | Web认证信息|
|Expect  | 期待服务器的特定行为 |
|Form  | 用户的电子邮箱地址 |
|Host  | 请求资源所在服务器 |
|If-Match  | 比较实体标记（Etag）|
|If-Modified-Since  | 比较资源的更新时间 |
|If-None-Match  | 比较实体标记 （与If-Match相反）|
|If-Range  | 资源未更新时发送实体Byte的范围请求 |
|If-Unmodified-Since  | 比较资源的更新时间(与If-Modified-Since相反) |
|Max-Forwards  | 最大传输逐跳数 |
|Proxy-Authorization  | 代理服务器要求客户端的认证信息 |
|Range  | 字体的字节范围请求 |
|Referer  | 对请求中URI的原始获取方 |
|TE  | 传输编码的优先级 |
|User-Agent  | HTTP客户端程序的信息 |

3. 响应首部字段

|  首部字段名   |  说明    |
|      :-      |    :-    |
|Accept-Ranges | 是否接受字节范围请求 |
|Age | 推算资源创建经过时间 |
|ETag  | 资源的匹配信息 |
|Location  | 令客户端重定向至指定的URI |
|Proxy-Authorization  | 代理服务器对客户端的认证信息 |
|Retry-After  | 对再次发起请求的时机要求 |
|Server  | HTTP服务的安装信息 |
|Vary  | 代理服务器缓存的管理信息 |
|WWW-Authenticate  | 服务器对客户端的认证信息 |

4. 实体首部字段

|  首部字段名   |  说明    |
|      :-      |    :-    |
|Allow  | 资源可支持的HTTP方法 |
|Content-Encodng  | 实体主体的使用的编码方式 |
|Content-Language  | 实体主体的自然语言 |
|Content-Length  | 实体主体的大小（单位：字节） |
|Content-Location  | 替代对应资源的URI |
|Content-MD5  | 实体主体的报文摘要 |
|Content-Range  | 实体主体的位置范围|
|Content-Type  | 实体主体的媒体类型 |
|Expires  | 实体主体过期的日期时间 |
|Last-Modified  | 资源的最后修改日期时间 |

5. 非HTTP/1.1首部字段
除了这47种首部字段，还有Cookie、Set-Cookie和Content-Disposition等

6. End-to-end首部和Hop-by-hop首部
HTTP首部字段将定义成缓存代理和非缓存代理的行为，分成两种类型
* End-to-end Header端到端首部：会转发给请求/响应对应的最终接收目标，且必须保存在由缓存生成的响应中，另外规定他必须转发；
* Hop-by-hop Header逐跳首部：只对单次转发有效，会因通过缓存或代理而不再转发；HTTP/1.1和之后的版本，若要使用hop-by-hop首部，需要提供connection首部字段；

下面列举http/1.1中的逐跳首部，除了这8个首部字段外，其他所有字段都属于端到端首部

    1. Connection
    2. Keep-Alive
    3. Proxy-Authenticate
    4. Proxy-Authorization
    5. Trailer
    6. TE
    7. Transfer-Encoding
    8. Upgrade


#### 三 HTTP/1.1 通用首部字段

1 Cache-Control：可用于请求和响应，多个指令之间用，号隔开;可以详见[mdn](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)
* 缓存请求指令

|  首部字段名    |   参数   |    说明    |
|      :-      |    :-    |   :-      |
|no-cache      | 无       |强制向原服务器再次验证   |
|no-store      | 无       |不缓存请求或响应的任何内容   |
|max-age = [秒] |  必需    |响应的最大Age值   |
|max-stale     | 可省略    |接收已过期的响应   |
|min-fresh = [秒] | 必需   |期望在指定时间内的响应仍有效   |
|no-transform   | 无      |代理不可更改媒体类型  |
|only-if-cached | 无      |从缓存获取资源   |
|cache-extension| -       |新指令标记（token）   |

* 缓存响应指令

|  首部字段名    |   参数   |    说明    |
|      :-      |    :-    |   :-      |
|public        | 无       | 可向任一方提供响应的缓存  |
|private       | 可省略    | 仅向特定用户返回响应，缓存服务器会对该特定用户提供资源缓存的服务  |
|no-cache      | 可省略    | 缓存前必须先确认其有效性  |
|no-store      | 无       | 不缓存请求或响应的任何内容 |
|no-transform   | 无      | 代理不可更改媒体类型   |
|must-revalidate | 无      | 可缓存但是必须再向源服务器进行确认  |
|proxy-revalidate| -       | 要求中间缓存服务器对缓存的响应有效性在进行确认  |
|max-age = [秒] |  必需    | 响应的最大Age值   |
|s-maxage = [秒] |  必需    |公共缓存服务器响应的最大Age值   |
|cache-extension| -       |新指令标记（token）   |

**表示是否能缓存的指令**
```
cache-control：public
cache-control：private
```
```
cache-control：no-cache  目的是为了防止从缓存中返回过期的资源，并不是不缓存，no-store才是真正的不缓存
```
+ 客户端请求中含no-cache，表示客户端将不会接受缓存过的响应（请给我从源服务器那里拿来的资源），于是‘中间’的缓存服务器必须把客户端的请求转发给源服务器；
+ 服务端返回的响应中包含no-cache，那么换成服务器不能对资源进行缓存，源服务器以后也将不再对缓存服务器请求中提出的资源有效性进行确认，且禁止其对响应资源进行缓存操作；
```
cache-control：no-cache=Location。只能在响应指令中指定该参数
```
+ 由服务器返回的响应中若该参数被字段名no-cache指定（该）参数值了，那么客户端就不能使用缓存


**控制可执行缓存的对象的指令**
```
cache-control：no-store 暗示请求（和对应的响应）和响应中包含机密信息，该指令规定缓存不能在本地存储请求或响应的一部分，是真正的不缓存
```

**制定缓存期限喝认证的指令**
```
cache-control：s-maxage=604800(单位：秒)；
```
+ s-maxage指令只适用于供多位用户使用的公共缓存服务器，对于向同一用户重复返回响应的服务器来说，该指令没有任何作用；当使用s-maxage指令后，则直接忽略对expires首部字段及max-age指令的处理；

```
cache-control：max-age=604800(单位：秒)；
```
+ 客户端----- 缓存服务器----- 源服务器
+ 对客户端来说，相当于要是缓存没超过一周，就把他给我；当max-age=0时，缓存服务器通常需要将请求转发给源服务器；
+ 对服务端来说，一周之内不必再向我确认，你直接支配缓存好了；返回响应中max-age数值代表资源保存为缓存的最长时间
+ HTTP/1.1中缓存同时遇到max-age和Expires时会优先处理max-age，忽略Expires；
+ HTTP/1.0中相反，缓存同时遇到max-age和Expires时会优先处理Expires，忽略max-age；

```
cache-control：min-fresh=60(单位：s)，客户端请求首部，这缓存在过60s还算是新鲜的么？如果这60s内有超过有效期的资源都无法作为响应返回了；
```
+ min-fresh指令要求缓存服务器返回至少还未过指定时间的缓存资源；
```
cache-control：max-stale=3600(单位：s)，使用max-stale可指示缓存资源，即使过期也照常接受；
```
+ 若指令为指定参数之，那么无论经过多久，客户端都会接收响应；
+ 若指令指定具体数值，那么即使过期，只要仍处于max-stale指定的时间内，仍旧被客户端接收；
```
cache-control：only-if-cached表示客户端仅在缓存服务器本地缓存目标资源的情况下才会要求其返回
```
+ 该指令要求缓存服务器不重新加载响应，也不会再次确认资源的有效性；
+ 若发生请求缓存服务器的本地缓存无响应，则状态码504Gate Timeout;

```
cache-control：must-revalidate 代理会向源服务器再次验证即将返回的响应缓存目前是否仍然有效；
```
+ 如果代理无法连通源服务器再次获取有效资源的话缓存必须给客户端一条504状态码；
+ 使用must-revalidate指令会忽略请求的max-stale指令；

```
cache-control：proxy-revalidate 该指令要求所有缓存服务器在接收到客户端带有该指令的请求返回响应之前，必须再次验证缓存的有效性；
```

```
cache-control：no-transform 该指令规定无论在请求还是响应中，缓存都不能改变实体主体的媒体类型；
```
+ **这样做可以防止缓存或者代理压缩图片等类似的操作**

**Cache-Control扩展**
**cache-extension token**
```
Cache-Control:private, community="UTC". cache-extension标记token，可以扩展Cache-Control首部字段内的指令；
```
+ Cache-Control首部字段本身没有community这个指令；借助extension tokens实现了该指令的添加，如果缓存服务器不理解community这个新指令，就会直接忽略；因此，extension tokens仅对能理解它的缓存服务器来说是有意义的；

2 Connection 首部字段具备如下两个作用；
* 控制不在转发给代理的首部字段；

```
GET / HTTP/1.1
Upgrade: / HTTP/1.1
Connection: Upgrade(此处是不在转发的首部字段名)
```
将Upgrade这个首部字段删除后在进行转发，经过代理转发后，在服务端收到的将是:
```
GET / HTTP/1.1
```

* 管理持久连接
```
Connection: close
```
HTTP/1.1版本的默认连接是持久化的，当服务端想明确断开连接时则指定Connection首部字段的值为close

```
Connection: Keep-Alive
```
HTTP/1.1之前的HTTP版本的默认连接都是非持久化的；想在就版本的HTTP协议上维持持续连接，则需要指定Connection首部字段的值为Keep-Alive

```
客户端发送如下请求：
GET / HTTP/1.1
Connection: Keep-Alive

服务端会加上Keep-Alive和Connection这两个首部字段，返回如下：
HTTP/1.1 200 OK
...
Keep-Alive: timeout=0, max=500
Connection: Keep-Alive
...
```

3 Date表明创建HTTP报文的日期和时间
```
在响应报文中，格式如下（RFC1123）：
date: Thu, 12 Aug 2021 08:49:12 GMT
```
响应date有个作用是可以判断是否是缓存：
HTTP没有为用户提供一种手段来区分响应是缓存命中的，还是访问原始服务器得到的,参考[文章](https://www.cnblogs.com/saolv/p/7828529.html)

> 客户端有一种方法能判断响应是否来自缓存，就是使用**Date首部**。将响应中Date首部的值与当前时间进行比较，如果响应中的日期值比较早，客户端通常就可以认为是来自缓存的；

> 客户端也可以通过**Age首部**来检测缓存的响应，通过这个首部可以分辨出这条响应的使用期。否则，则认为是来自原始服务器中的。Age 消息头里包含对象在缓存代理中存贮的时长，以秒为单位。Age的值通常接近于0。表示此对象刚刚从原始服务器获取不久；其他的值则是表示代理服务器当前的系统时间与此应答中的通用头 Date 的值之差。
<font color='red'> (应该date的值加上age的值等于现在的时间,但是看了很多网站和资源都不是这样，有大神路过可以指点一下么？？？) </font>

4 Pragma是HTTP/1.1之前版本历史遗留字段，仅作为与HTTP/1.0的向后兼容而定义
```
Pragma: no-catch;通用首部字段，仅用于客户端发送的请求中；no-cache 与 Cache-Control: no-cache 效果一致。强制要求缓存服务器在返回缓存的版本之前将请求提交到源头服务器进行验证
```
> 因为不保证所有中间服务器都能一HTTP/1.1为基准，有可能存在HTTP/1.0及以下，故发送请求时一般同时含有下么两个首部
```
Cache-Control: no-catch
Pragma：no-cache
```

5 Trailer事先说明在报文主体后记录了哪些首部字段，是一个响应首部，允许发送方在分块发送的消息后面添加额外的元信息，这些元信息可能是随着消息主体的发送动态生成的，比如消息的完整性校验，消息的数字签名，或者消息经过处理之后的最终状态等。提醒客户端在报文的最后写了很多重要的东西记得要仔细阅读；
```
HTTP/1.1 200 OK
Date：Thu, 12 Sep 2021 08:49:12 GMT
Content-Type：text/html
...
Transfer-Encoding：chunked
Trailer：Expires

...报文主体...
0 
Expires: Tue, 28 Sep 2021 23:59:59 GMT //Expires 首部出现在分块信息的结尾，作为挂载（trailer）首部
```

6 Transfer-Encoding规定传输报文主体时采用的编码方式；是一个逐跳传输消息首部；一个多节点连接中的每一段都可以应用不同的Transfer-Encoding 值；如果你想要将压缩后的数据应用于整个连接，那么请使用端到端传输消息首部  Content-Encoding；当这个消息首部出现在 HEAD 请求的响应中，而这样的响应没有消息体，那么它其实指的是应用在相应的  GET 请求的应答的值。
```
HTTP/1.1 200 OK
date: Mon, 16 Aug 2021 03:43:30 GMT
ache-control: no-cache, no-store, must-revalidate
content-type: text/plain
expires: Fri, 01 Jan 1990 00:00:00 GMT
content-Encoding: gzip
Transfer-Encoding: chunked
connection: keep-alive
cf0  // 16进制（转换为10进制为3312）

...3312字节分块数据...

392 // 16进制（转换为10进制为914）
...914字节分块数据...

0
```

7 Upgrade用于检测http协议及其他协议时候可使用更高的版本进行通信，其参数值可以指定一个完全不同的通信协议；
```
request
GET /index.htm HTTP/1.1
Upgrade：TSL/1.0
Connection：Upgrade   //Connection字段被指定为Upgrade了，产生作用仅限于客户端和临近的服务器之间，

Response
HTTP/1.1 101 Switching Protocols
Upgrade：TSL/1.0，HTTP/1.1
Connection：Upgrade

```
一个很好的应用实例，利用upgrade实现websocket的连接，详见[文章一：websocket服务器怎么向客户端推送消息？](https://blog.csdn.net/weixin_39608988/article/details/111102328) [文章2:简单聊聊 WebSocket](https://blog.csdn.net/weixin_39939303/article/details/111164090)

> Note: Connection: upgrade must be set whenever Upgrade is sent.

8 via 为了追踪client和server之间的请求和响应报文的传输路径
```
via: 1.1 fae59a141259f8fbe249f8056c44162e.cloudfront.net (CloudFront) //其中1.1代表接收请求的服务器上应用的HTTP协议版本；
```

9 warning 属于http/1.1 是从http/1.0的响应首部retry-after演变而来，告知用户与缓存相关的问题的警告；
```
格式：
warning：[警告码][警告的主机：端口号]”[警告内容]“([日期时间])
eg：
warning: 113 gw.hackr.jp:8080 "Heuristic expiration" Mon, 16 Aug 2021 07:26:47 GMT
```
> HTTP/1.1 定义了7中警告码，的警告码
|  警告码 |   警告内容  |    说明    |
|  :-    |    :-    |   :-      |
|110     | Response is stale(响应已过期)   | 代理返回已过期的资源  |
|111     | Revalidation failed(再验证失败) | 代理再次验证有效性时失败（服务器无法到达等原因）  |
|112     | Disconnection operation(断开连接操作) | 代理与互联网连接估计切断 |
|113     | Heuristic expiration(试探性过期) | 如果缓存服务器采用启发式方法，将缓存的有效时间设定为24小时，而在该响应的age超过24小时发送 |
|199     | Miscellaneous Warning(杂项警告)  | 任意的、未明确指定的警告信息  |
|214     | Transformation Applied(使用了转换) | 由代理服务器添加，如果它对返回的展现内容进行了任何转换，比如改变了内容编码、媒体类型等  |
|299     | Miscellaneous Warning(持久杂项警告) | 与199类似，只不过指代的是持久化警告 |


#### 四 请求首部字段

1. Accept通知服务器，用户代理可处理的媒体类型及媒体类型相对优先级
```
accept：text/plain；q=0.3，text/html //那份资源最后给我html的，如果没有给我text的也可以
accept：类型1；类型1对应的优先级q（0～1，1权重最高，默认为1.0，可精确到小数点后3位），类型2；类型2的优先级
在eg：在网站https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Warning发现一个doc资源如下：
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
```
下面列举几种媒体类型
* 文本文件
```
text/html, text/plain,text/css...
application/xhtml+xml,application/xml...
```
* 图片文件
```
image/jpeg,image/gif,image/png...
```
* 视频文件
```
video/mpeg,video/quicktime...
```
* 应用程序使用的二进制文件
```
application/octet-stream,application/zip...
```

2. Accept-Charset 通知服务器，用户代理支持的的字符集及字符集相对优先顺序
```
accpet-charset:iso-8859-5,unicode-1-1;q=0.8
```

3. Accept-Encoding 通知服务器，用户代理支持的的内容编码及内容编码优先顺序,可一次性指定多种；可以使用q设置优先级，也可以使用 * 通配符指定任意编码格式
```
accpet-Encoding:gzip,deflate
```
下面列举几种内容编码
* gzip：由文件压缩程序gzip生成编码格式（RFC1952），采用lempei-ziv（lz77）算法及32位循环冗余校验（cyclic redundancy check，通称CRC）
* compress:由UNIX文件压缩程序compress生成的编码格式，采用lempel-ziv-welch算法（lzw）
* deflate：组合使用zlib格式及由deflate压缩算法生成的编码格式 
* identity：不执行压缩或不会变化默认的编码格式 
* br：表示采用 Brotli 算法的编码方式 

4. Accept-Language 通知服务器，用户代理可处理的自然语言集及自然语言集相对优先级
```
accpet-Language:zh-cn，zh；q=0.7，en-us，en;q=0.3
```

5. Authorization 告知服务器用户代理的认证信息
当去请求资源时，有时会返回401 Unauthorized，当客户端接收到该状态码响应时，会将首部字段Authorization加入请求中，共用缓存在接收到含有Authorization的请求时操作处理会有差异；

6. Expect 告知服务器，期待服务器的特定行为；
目前规范中只规定了 "100-continue" 这一个期望条件
```
Expect: 100-continue //通知接收方客户端要发送一个体积可能很大的消息体，期望收到状态码为100 (Continue)  的临时回复。因服务器无法理解c端的期望做出回应而发生错误时，会返回417 Expectation Failed
```
* 100 如果消息头中的期望条件可以得到满足，使得请求可以顺利进行的话，
* 417 (Expectation Failed) 如果服务器不能满足期望条件的话；也可以是其他任意表示客户端错误的状态码（4xx）。

7. Form 告知服务器使用用户代理的用户的电子邮箱地址（如果有事请联系这个电子邮箱）
使用代理时应尽可能的包含from首部，但可能会因为代理的不同，将电子邮件的地址记录在User-Agent首部里；

8. Host  告知服务器，请求资源所在服务器主机名和端口号
```
host: www.hackr.jp //一个域名指向一个ip和对应的端口号
```
* 如果没有包含端口号，会自动使用被请求服务的默认端口（比如HTTPS URL使用443端口，HTTP URL使用80端口）。
* 在http/1。1规范中唯一一个必须被包含在请求内的首部字段，对于缺少Host头或者含有超过一个Host头的HTTP/1.1 请求，可能会收到400（Bad Request）状态码。
* 当相同的ip地址下部署运行多个域名，就需要host来明确指出请求的主机名；若服务器未设置主机名直接发送一个空值即可

？？？但是我们平时做web开发时发起请求并没有指定host啊？？？？web开发过程中如何设置请求头host
？？？是通过devserver的proxy target指定的么？
？？？或者可以利用nginx的代理转发指定的？
```
   #接口
   location /apiroute/ {
      proxy_pass  http://www.***.com/apiroute/;
   }
```

**形如If-xxx这种样式的请求首部字段，都可称为条件请求。服务器接收到附加条件请求后，只有判断指定条件为真时才会执行请求；**

9. If-Match If-match告知服务器匹配资源所用的实体标记Etag，当请求的If-Match值和服务器的资源的实体标记（Etag）值一致时才会执行请求，；否则返回412 Precondition Failed的响应；当值为 * 号时，服务器会忽略Etag的值；
```
Request 
GET /index.html
If-Match: "12345"

Response
//当服务器资源是 index.html 实体标记（Etag）为12345时  // 实体标记（Etag）是与特定资源关联的确定值，资源跟新后Etag也会随之更新；
200 OK

//当服务器资源是 index.html 实体标记（Etag）为567890时
412 Precondition Failed
```

10. If-Modified-Since 告知服务器若If-Modified-Since时间早于资源更新时间，则希望能处理该请求；**用于确认代理或客户端拥有本地资源的有效性**
```
GET /index.htm
If-Modified-Since:Tue, 17 Aug 2021 09:07:44 GMT

//对服务器，因为是21年8月17之后更新过的资源所以我可以接受
200 OK
Last-Modified：Tue, 19 Aug 2021 09:07:44 GMT

//对服务器，因为在21年8月17之后未更新过，所以不能接受返回304
304 Not Modified
Last-Modified：Tue, 19 Aug 2021 09:07:44 GMT
```

11. If-None-Match 只有在If-None-Match值与实体标记（Etag）值不一致时，可以处理该请求 （与If-Match相反）；**在GET或HEAD方法中使用可以获取最新的资源**；
```
Request 
GET /sample.html
If-None-Match: *

Response
//当服务器没有sample.html资源时，也就没有对应的Etag故可以处理你的请求
200 OK
```

12. If-Range 告知服务器若指定的If-Range字段值（ETag或者时间）和请求资源的Etag值或者时间一致时，则作为范围请求处理；反之则返回全体资源；
```
Request 
GET /index.html
If-Range: “12345”
Range：bytes=5001-10000

Response
//当服务器有index.html资源时，且Etag=12345 将返回范围内容
206 Partial Content
content-range：bytes 5001-10000/10000
content-length:5000

//当服务器有index.html资源时，但是Etag=67890不一致时 将返回全部内容
200 OK
Etag=“67890”

当请求不使用if-range时，则需要两次处理
Request 
GET /index.html
If-Match: “12345”
Range：bytes=5001-10000

Response
//如果服务器的资源更新，那客户端持有的资源中的一部分也会随之无效，范围请求作为前提是无效的
412 Precondition Failed //你要的部分资源已经没有了，你重新在发送一个请求吧；这样与使用if-range比，就需要花费两倍的功夫；

Request
GET /
Response
200 OK
Etag=“67890”

```

13. If-Unmodified-Since 告知服务器只有在If-Unmodified-Since值指定的时间日期之后，未发生资源更新情况下，才能处理该请求(与If-Modified-Since相反)；若在指定时间内发生了更新则返回412 Precondition Failed 

14. Max-Forwards 最大传输逐跳数，通过TRANCE或OPTIONS方法，发送含有max-forwards的请求，该字段以十进制证书形式指定可经过的服务器最大数目；服务器在往下一个服务器转发请求前将值-1重新赋值，当服务器收到max-forwards值为0的请求时，则不在进行转发，而是直接返回响应；使用http协议通信时，可能会进过代理等多台服务器，途中若代理服务器由于某些原因导致请求失败，客户端将等不到服务器的响应；因当值为0时，服务器立即返回响应；可以利用此来对那台问题服务器为终点的传输路径的通信状态有所把握；
> max-forwards 大于转发代理服务器个数时怎么处理????


15. Proxy-Authorization 接受到从代理服务器发来的认证质询时，客户端会发送包含首部字段proxy-authorization的请求，以告知服务器认证所需要的信息；这个首部通常是在服务器返回了 407 Proxy Authentication Required 响应状态码及 Proxy-Authenticate 首部后发送的。认证行为发生在客户端和代理之间；客户端和服务器之间的认证使用authrization即可；

16. Range 对于只需要获取部分资源的范围请求，包含首部字段染革即可告知服务器资源的指定范围
 ```
 range: bytes=5001-10000
 
 response
 // 会处理
 206 partial content
 
 //无法处理时
 200 ok 及全部资源
 ```

17. Referer 告知服务器请求的原始资源的URI,请求头包含了当前请求页面的来源页面的地址，即表示当前页面是通过此来源页面里的链接进入的。服务端一般使用 Referer 请求头识别访问来源，可能会以此进行统计分析、日志记录以及缓存优化等。
```
Referer: https://www.baidu.com/?tn=62095104_19_oem_dg
```

18. TE 告知服务器客户端能够处理的响应的**传输编码**方式及相对优先级;同Accept-Encoding（用于内容编码）相似；
```
TE: gzip, deflate;q=0.5

TE: trailers //表示客户端期望在采用分块传输编码的响应中接收挂载字段。
```

19. User-Agent 会将创建请求的浏览器和用户代理名称等信息传达给服务器（HTTP客户端程序的信息）；不同浏览器及爬虫和机器人的ua内容不一致详见[mdn](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent)
```
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36
```

#### 五 响应首部字段

1. Accept-Ranges 服务器使用 HTTP 响应头 Accept-Ranges 标识自身支持范围请求(partial requests)。字段的具体值用于定义范围请求的单位。
```
Accept-Ranges: bytes // 范围请求的单位是 bytes （字节）。
Accept-Ranges: none // 不支持任何范围请求单位，由于其等同于没有返回此头部，因此很少使用。不过一些浏览器，比如IE9，会依据该头部去禁用或者移除下载管理器的暂停按钮。
```

2. Age 告知客户端源服务器在多久前创建了响应,消息头里包含对象在缓存代理中存贮的时长，以秒为单位;Age的值通常接近于0。表示此对象刚刚从原始服务器获取不久；其他的值则是表示代理服务器当前的系统时间与此应答中的通用头 Date 的值之差。

3. ETag 告知客户端实体标识；服务器会为每份资源分配对应的etag值，当资源更新时也随之更新
```
//强etag值，不论实体发生多么细微的变化都会改变其值
ETag:'usagi-1234'

//弱etag值，只有资源发生了根本改变，产生差异时才会改变etag值
ETag：W/'usagi-1234'
```

4. Location 令客户端重定向至指定的URI；几乎所有的浏览器在接收到包含首部字段location的响应后，都会强制性地尝试对已提示的重定向资源的访问；

5. Proxy-Authorization  由代理服务器所要求的认证信息发送给客户的

6. Retry-After 告知客户端应该在多久之后再次发送请求；主要配合状态码503 Service Unavaliable和3xx redirect响应一起使用；字段值可以是具体的时间（Tue, 19 Aug 2021 09:07:44 GMT）或者创建响应后的秒数；
```
Retry-After:120 
```
7. Server 告知客户端当前服务器上安装的HTTP服务器应用程序信息，除了软件应用名称，还可能有版本号和安装时启用的可选项；
```
Server: Apache/2.4.1 (Unix)
Server: Apache/2.4.1 (Unix) PHP/5.2.5
```
8. Vary 可以对缓存进行控制，它决定了对于未来的一个请求头，应该用一个缓存的回复(response)还是向源服务器请求一个新的回复；大部分情况下是用在客户端缓存机制或者是缓存服务器在做缓存操作的时候，会使用到Vary头，会读取响应头中的Vary的内容，进行一些缓存的判断
```
// 客户端
GET /sample.html
Accept-Language:en-us

// 代理服务器A
GET /sample.html
Accept-Language:en-us

// 源服务器返回给代理服务器,告知代理只能低持相同自然语言（Accept-Language）的请求返回缓存
Vary：Accept-Language 

// 代理服务器A收到源服务器返回的包含vary指定项的响应之后，若再要进行缓存，仅对请求中含有相同vary指定首部字段的请求返回缓存；
// 客户端即使对相同资源发起请求，但由于vary指定的首部字段不同，因此必须要从源服务器重新获取资源
```
还可以是下面的值,对于User-Agent 头部信息，例如你提供给移动端的内容是不同的，可用防止你客户端误使用了用于桌面端的缓存。 并可帮助Google和其他搜索引擎来发现你的移动端版本的页面，同时告知他们不需要Cloaking。
```
Vary: Accept-Encoding
Vary: Accept-Encoding,User-Agent
Vary: X-Some-Custom-Header,Host
Vary: *
```
9. WWW-Authenticate  服务器对客户端的认证信息

#### 六 实体首部字段
1. Allow 资源可支持的HTTP方法，若服务器返回状态码 405 Method Not Allowed，则该首部字段亦需要同时返回给客户端。如果 Allow  首部字段的值为空，说明资源不接受使用任何 HTTP 方法的请求。这是可能的，比如服务器需要临时禁止对资源的任何访问。

2. Content-Encodng  告知客户端服务器对实体的主体部分的使用的内容编码方式，内容编码是指在不丢失实体信息的前提下所进行的压缩,主要有以下4种内容编码的方式；
* gzip
* compress
* deflate
* identity

3. Content-Language  告知客户端实体的主体使用的自然语言
4. Content-Length  实体主体的大小（单位：字节）
5. Content-Location  给出与报文主体部分相对应的URI，与location不同，content-location表示的是报文主体返回资源对应的uri
6. Content-MD5 客户端会对接收的报文主体执行相同的md5算法，然后与首部字段content-md5的字段值比较；
7. Content-Range 针对范围请求，返回响应时使用该字段告知客户端实体主体的位置范围
8. Content-Type  实体主体的媒体类型
9. Expires  告知客户端实体主体过期的日期时间
10. Last-Modified  资源的最后修改日期时间


#### 七 为cookie服务的首部字段

|  首部字段名    |   说明   |    首部类型    |
|      :-      |    :-    |   :-      |
|  Set-Cookie  | 开始状态管理所使用的cookie信息 | 响应首部字段   |
|   Cookie     | 服务器接收到的cookie信息     | 请求首部字段   |

1. Set-Cookie， 响应首部 Set-Cookie 被用来由服务器端向客户端发送 cookie；当服务器准备开始管理客户端的状态时，会事先告知客户端各种信息，如下时set-cookie字段属性

|    属性    |   说明   |
|      :-      |    :-    |
|  NAME=VALUE  | 赋予cookie的名称和其值（必须项） |
| expires=DATE | cookie的有效期（若不明确指明则默认为浏览器关闭前为止）  |
| Max-Age=NUMBER | 在cookie失效之前需要经过的秒数。秒数为0或-1将会使cookie直接过期。一些老的浏览器(ie6/ie7和ie8)不支持这个属性。对于其他浏览器来说，假如二者(指 Expires 和Max-Age)均存在,那么Max-Age优先级更高|
| path=PATH    | 将服务器上的文件目录作为cookie的适用对象（若不指定则默认为文档所在的文件目录）  |
| domain=域名  |  作为cookie适用对象的域名（不指定默认为创建cookie的服务器域名） |
| secure     | 仅在HTTPS安全通信时才会发送cookie     |
| httponly   | 加以限制，是cookie不能被js脚本访问;设置了 HttpOnly 属性的 cookie 不能使用 JavaScript 经由  Document.cookie 属性、XMLHttpRequest 和  Request APIs 进行访问，以**防范跨站脚本攻击**（XSS (en-US)） |

```
set-cookie: uuid_tt_dd=10_9892166660-1629444067412-283315; Expires=Thu, 01 Jan 2025 00:00:00 GMT; Path=/; Domain=.csdn.net;
```

2. Cookie,会告知服务器，当客户想获得http状态管理支持时，就会在请求中包含从服务器接收到的cookie

#### 八 其他首部字段

http首部字段时可以自行扩展的，所以在web服务器和浏览器的应用上，会出现各种非标准的首部字段；

1. X-Frame-Options：属于http响应首部，用于控制网站内容在其他web网站的frame标签内的显示问题，其主要目的是**防止点击劫持（clickjacking）攻击**
```
// 有两个值，且仅部分浏览器支持（有兼容性）
X-Frame-Options：DENY // 拒绝
X-Frame-Options：SAMEORIGIN  //仅同源域名下的页面匹配时许可，如当指定http://hackr.js/sample.html页面为SAMEORIGIN时，hackr.js上所有页面的frame都被允许可加载该页面，而example.com等其他域名的页面就不可以
```

2. X-XSS-Protection：属于http响应首部，针对跨站脚本攻击的一种对策，用户控制浏览器XSS防护机制的开关
```
X-XSS-Protection：0 //将xss过滤设置成无效状态
X-XSS-Protection：1 //将xss过滤设置成有效状态
```

3. DNT:属于http请求首部，DNT（DO NOT TRACK的简称）意为拒绝个人信息被手机，表示拒绝被精准广告追踪的一种方法，
```
DNT：0 //同意被追踪
DNT：1 //拒绝被追踪
```

4. P3P：属于http响应首部，利用P3P(the platform for provacy preferences,在线隐私偏好平台）技术，可以让web网站上的个人隐私变成一种仅供程序可理解的形式，以达到保护用户隐私的目的；要进行P3P的设定，需进行如下操作：
* 创建P3P隐私
* 创建P3P隐私对照文件后，保存命名在/w3c/p3p.xml
* 从P3P隐私中新建compact policies后，输出到http响应中

注意：协议总对X-开头的做法表示建议停止，但是已经在用的X-前缀，不应该要求其变更；








