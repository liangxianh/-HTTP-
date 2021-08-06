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
* End-to-end Header端到端首部：会转发给请求/响应对应的最终接收目标，且必须保存在由缓存生成的响应中，另外规定他必须转发；
* Hop-by-hop Header逐跳首部：只对单次转发有效，会因通过缓存或代理而不再转发；HTTP/1.1和之后的版本，若要使用hopbyhop首部，需要提供connection首部字段；

下面列举http/1.1中的逐跳首部，除了这8个首部字段外，其他所有字段都属于端到端首部

    1. Connection
    2. Keep-Alive
    3. Proxy-Authenticate
    4. Proxy-Authorization
    5. Trailer
    6. TE
    7. Transfer-Encoding
    8. Upgrade


#### 3 HTTP/1.1 通用首部字段

1 Cache-Control：可用于请求和响应，多个指令之间用，号隔开
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

```
cache-control：no-cache  目的是为了防止从缓存中返回过期的资源，
```
客户端请求中含no-cache，表示客户端将不会接受缓存过的响应（请给我从源服务器那里拿来的资源），于是‘中间’的缓存服务器必须把客户端的请求转发给源服务器；
服务端返回的响应中包含no-cache，那么换成服务器不能对资源进行缓存，源服务器以后也将不再对缓存服务器请求中提出的资源有效性进行确认，且禁止其对响应资源进行缓存操作；
```
cache-control：no-cache=Location
```





