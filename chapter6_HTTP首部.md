## HTTP首部

HTTP协议的请求和响应报文中必定包含HTTP首部；

#### 1 HTTP 报文首部
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

#### 2 HTTP 首部字段
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
|Upgrade | 升级为其他协议|
|Via  | 代理服务器相关信息 |
|Warning  | 错误通知 |

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
* End-to-end Header端到端首部
* Hop-by-hop Header逐跳首部

