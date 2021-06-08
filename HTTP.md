# HTTP

## 1 什么~是HTTP？

HTTP，全称 **Hyper Text Transfer Protocol**，中文 **超文本传输协议**。

它是一种 <font color=red>**非对称**</font>(asymmetric) request-response client-server的协议。同时，它还是 <font color = red>无状态(stateless)</font>的。允许对数据类型和表示进行协商(Permits negotiation of data type and representation，~~当然了，这句话我没看懂~~bushi)

## 2 HTTP 工作流程示例

![](D:\BTH011\自主整理\HTTP\工作流程.png)

1. 用户在浏览器里输入请求的URL
2. **浏览器**<font color = red>(客户端)</font>向服务器发送一个 <font color = red>**request**</font> message
3. 服务器把这个请求的URL在自己的目录下映射到对应的资源文件夹
4. 服务器回送一个 <font color=red>**response**</font> message
5. 浏览器对这个response信息进行格式转换并显示

> 说了那么多，可到底什么是URL？
>
> URL = **Uniform Resource Locator**
> $$
> protocol://hostname:port/path-and-file-name
> $$
> 上面的公式就是URL的标准格式
>
> 1. Protocol: 客户端和服务器使用的<font color = red>应用层</font>协议，如HTTP、FTP、telnet。
>
> 2. Hostname: 可以是DNS域名，也可以是服务器IP地址
>
> 3. Port: 端口, <font color = red>服务器端</font>的<font color = blue>TCP</font>监听请求队列的端口，划重点，**TCP**！！！(为什么呢？因为HTTP的传输层协议就是<font color = red>**TCP**</font>！)
>
>    > 这里结合一张图看：
>    >
>    > ![](D:\BTH011\自主整理\HTTP\HTTP底层.png)
>
> 4. Path-and-file-name: 请求资源的名称和位置，位于服务器文档基目录下。
>
> URL大概就是这样，但是！还有URI，URN，以及 它们和URL互相之间，是个什么区别？
>
> 哦还有个URC。
>
> |   URL   |    Uniform Resource Locator     | Contains information about how to fetch a resource from its location 关于如何从这些资源的位置中抓取它们 |
> | :-----: | :-----------------------------: | :----------------------------------------------------------: |
> | **URI** | **Uniform Resource Identifier** | URI 是使用数字、字母和符号组成的短字符串来标识文档的标准[RFC 3986]。<font color = red>“我是URL, URN,URC的爸爸(bushi)"</font> <br/>**URLs, URNs, and URCs are all types of URI.** |
> | **URN** |    **Uniform Resource Name**    | 通过唯一和持久的名称标识资源，但不一定告诉您如何在internet上找到它。(~~其实，我也没懂~~)<br/>通常它以<font color = red>URN:</font> 这样的前缀开头 |
> | **URC** |  **Uniform Resource Citation**  | 统一资源引用<br/>关于文档而不是文档本身的元数据。(~~更迷了~~) |

------

> 再谈谈<font color = red>HTML</font>(疯狂注水)
>
> HTML 是 **Hyper Text Markup Language**<font color = red>超文本标记语言</font> 的简称 (想起了那日老师在课上问 HTML中文翻译 我脱口而出 超文本传输协议 ~~不是~~)
>
> HTML描述了一个使用标记的网页结构
>
> HTML 元素 是 构成HTML页面的块/构建单位，它们由标签来表示
>
> HTML标签可以对内容块进行标签
>
> 浏览器不显示HTML标记，而是使用它们来呈现页面的内容

## 3 HTTP request 和 response

### 3.1 request

一个例子

```http
http://www.nowhere123.com/doc/index.html
```

Client Browser 将会向 Server 发送如下内容:

```http
GET /docs/index.html HTTP/1.1 
Host: www.nowhere123.com 
Accept: image/gif, image/jpeg, */* 
Accept-Language: en-us Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1) 
(blank line) 
```

![](D:\BTH011\自主整理\HTTP\REQUEST结构.png)

我们把具体内容和Request结构结合在一起看，则:

```http
(2，4-8行位Request Message Header)
GET /docs/index.html HTTP/1.1 (request line)
(4-8行为Request Headers)
Host: www.nowhere123.com 
Accept: image/gif, image/jpeg, */* 
Accept-Language: en-us Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1) 
(blank line) 
(以下为Request Message Body, 不过这是可选的)
```

#### 3.1.1 Request Line

$$
request-method-name~request-URI~HTTP-version
$$

以上公式就是请求行(<font color = blue>request line</font>)的格式<br/>

- Request-method-name: HTTP协议定义了一组请求方法, 比如, GET, POST, HEAD, OPTIONS. 这些方法被客户端用于发送一个请求给服务器。
- request-URI: 指定请求的资源
- HTTP-Version: 最近比较常用的有 HTTP/1.0，HTTP/1.1，HTTP/2.0

例子:

```http
GET /test.html HTTP/1.1
```

```http
HEAD /query.html HTTP/1.0
```

```http
POST /index.html HTTP/1.1
```

#### 3.1.2 Request Headers

这里面都是 name: value的对子(pairs)

可以指定多个值，不过由逗号分开(Accept 和 Accept - Language)

例子:

```http
//request line在第一行,但它不是request header
Host: www.xyz.com 
Connection: Keep-Alive 
Accept: image/gif, image/jpeg, */* 
Accept-Language: us-en, fr, cn 
```

------



总结request的一个例子:

```http
GET /marc/ HTTP/1.1[CRLF]
Host: 127.0.0.1[CRLF]
Connection: keep-alive[CRLF]
Upgrade-Insecure-Requests: 1[CRLF]
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36[CRLF]
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8[CRLF]
Accept-Encoding: gzip, deflate[CRLF]
Accept-Language: sv-SE,sv;q=0.9,enUS;q=0.8,en;q=0.7,de;q=0.6,ru;q=0.5[CRLF]
Cookie: PHPSESSID=u2vkjd7lxyxyxyxyxyxyxyxyx30[CRLF]
[CRLF]
```

------



### 3.2 Response  

![](D:\BTH011\自主整理\HTTP\Response结构.png)

#### 3.2.1 Status Line

$$
HTTP-version~status-code~reason-phrase
$$

- HTTP-version: 此会话中使用的HTTP版本。HTTP/1.0或HTTP/1.1。
- status-code: 由服务器生成的反映请求结果的3位数字。
- reason-phrase: 给出状态代码的简短解释。

> 看看 status code:<br/>1xx Informational response
> 2xx Success
> 	200 OK
> 3xx Redirection
> 	301 Moved Permanently
> 4xx Client Error
> 	400 Bad Request
> 	404 Not Found
> 5xx Server Error
> 	500 Internal Server Error

#### 3.2.2 Response Headers

也是 name:value 的对子:white_check_mark:<br/>多个值也是用逗号隔开

一个小例子:

```http
//这里空出来，放status line
Content-Type: text/html 
Content-Length: 35 
Connection: Keep-Alive 
Keep-Alive: timeout=15, max=100
```

------

response 示例:

```http
HTTP/1.1 200 OK[CRLF]
Date: Thu, 06 Dec 2018 08:37:06 GMT[CRLF]
Server: Apache/2.4.7 (Ubuntu)[CRLF]
X-Powered-By: PHP/5.5.9-1ubuntu4.6[CRLF]
Expires: Thu, 06 Dec 2018 08:42:06 GMT[CRLF]
Cache-Control: max-age=300,public, must-revalidate[CRLF]
Content-Length: 159[CRLF]
Keep-Alive: timeout=5, max=95[CRLF]
Connection: Keep-Alive[CRLF]
Content-Type: image/png[CRLF]
[CRLF]
```

==Date==是什么！！！是你==客户端去请求这个服务器资源的时间==！==不是==这个服务器文件==修改的时间==！

==Last-Modified==才是服务器上文件的最后修改时间！

### 3.3 HTTP Method

- GET: Request to get a web resource from the server.
- HEAD: Request to get the header that a GET request would have obtained.
- POST: Used to post data up to the web server.
- PUT: Ask the server to store the data. 请求服务器存储数据
- DELETE: Ask the server to delete the data. 请求服务器删除数据
- CONNECT: Tell a proxy to make a connection to another host and simply reply the content, without attempting to parse or cache it. 告诉代理建立到另一个主机的连接，并简单地回复内容，而不试图解析或缓存它。<font color = red>不懂(bushi)</font>
- TRACE: Ask the server to return a diagnostic trace of the actions it takes. 要求服务器返回它所采取的操作的诊断跟踪。<font color = red>不懂(bushi)</font>
- OPTIONS: Ask the server to return the list of request methods it supports. 要求服务器返回它支持的请求方法列表。

一个小表格，总结这些方法的异同:

| HTTP Method |   RFC    | Request has body | Response has body |  Safe  | Idempotent | Cacheable |
| :---------: | :------: | :--------------: | :---------------: | :----: | :--------: | :-------: |
|     GET     | RFC 7231 |      Option      |        Yes        |  Yes   |    Yes     |    Yes    |
|    HEAD     | RFC 7231 |      ==No==      |      ==No==       |  Yes   |    Yes     |    Yes    |
|    POST     | RFC 7231 |       Yes        |        Yes        | ==No== |   ==No==   |    Yes    |
|     PUT     | RFC 7231 |       Yes        |        Yes        | ==No== |    Yes     |  ==No==   |
|   DELETE    | RFC 7231 |      ==No==      |        Yes        | ==No== |    Yes     |  ==No==   |
|   CONNECT   | RFC 7231 |       Yes        |        Yes        | ==No== |   ==No==   |  ==No==   |
|   OPTIONS   | RFC 7231 |      Option      |        Yes        |  Yes   |    Yes     |  ==No==   |
|    TRACE    | RFC 7231 |      ==No==      |        Yes        |  Yes   |    Yes     |  ==No==   |
|    PATCH    | RFC 5789 |       Yes        |        Yes        | ==No== |   ==No==   |  ==No==   |

HTTP PUT 与 POST 的一些小对比:

==PUT==:

PUT将文件或资源放在特定的URI上，确切地说是在该URI上。如果该URI上已经有一个文件或资源，PUT将替换该文件或资源。如果没有文件或资源，PUT将创建一个。PUT是==幂等==的，但矛盾的是，PUT响应是不可缓存的。

==POST==:

POST将数据发送到特定的URI，并期望该URI上的资源来处理请求。此时，web服务器可以决定在指定资源的上下文中如何处理数据。POST方法不是==幂等==的;但是POST响应是可缓存的，只要服务器设置了适当的缓存控制和Expires头。<font color=red>不懂</font>

>什么是**幂等** ==idempotent==?
>idempotent: 数学和计算机科学中某些运算的性质，它们可以多次应用而不改变最初应用之外的结果
>举个例子:<br/>
>
>```cpp
>a = 1; //幂等的,对这个操作重复，a的值还是1
>a++; //非幂等的,对这个操作重复,a的值会不停变化

## 4 HTTP Based Services

1. All service invocations use HTTP: HTTP is always the outer envelope of an invocation request and response
2. HTTP is always the outer envelope of an invocation request and response: The URI is the location of the service
3. All service invocations are a form of RPC: 
   - Regardless of technology
   - Some services hide behind other abstractions (REST)

> [REST Architectural](https://en.wikipedia.org/wiki/Representational_state_transfer)
>
> - Client-Server: Separation of concerns. 侧重==分离==
> - Stateless: No client context being stored on the server between requests. 在请求之间无法存储客户端的上下文
> - Cacheability: Responses must therefore, implicitly or explicitly, define themselves as cacheable or not to prevent clients from getting stale or inappropriate data in response to further requests. 因此，响应必须隐式或显式地将自己定义为可缓存或不可缓存，以防止客户端在响应进一步的请求时获得过时或不恰当的数据。
> - Layered System: 中介服务器通过负载平衡和提供共享缓存来提高可伸缩性。
> - Uniform interface: Simplifies Architecture, enables each part to evolve independently. 简化架构，使每个部分能够独立发展。
> - Code on Demand: 服务器可以通过传输代码(比如JavaScript)来扩展或定制客户端的功能

> Use HTTP Verbs to their meaning!
>
> - GET: Retrieving a resource. 检索资源
> - POST: Creating a resource. 创建资源
> - PUT: Updating a resource. 更新资源
> - DELETE: deleting a resource. 删除资源
>
> ==注意==
>
> GET不能改变任何请求的数据<br/>GET requests must not change any underlying resource data. Measurements and tracking which update data may still occur, but the resource data identified by the URI should not change.

