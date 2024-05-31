# HTTP

## 训练营作业

### 1. 资源

#### 1. 什么是URI

URI 是 Uniform Resource Identifier 的缩写，“URI 就是由某个协议方案表示的资源的定位标识符。协议方案是指访问资源所使用的协议类型名称。URI 用字符串标识某一互联网资源，而 URL 表示资源的地点（互联网上所处的位置）。可见 URL 是 URI 的子集。”

```
ftp://ftp.is.co.za/rfc/rfc1808.txt
http://www.ietf.org/rfc/rfc2396.txt
ldap://[2001:db8::7]/c=GB?objectClass?one
mailto:John.Doe@example.com
news:comp.infosystems.www.servers.unix
tel:+1-816-555-1212
telnet://192.0.2.16:80/
urn:oasis:names:specification:docbook:dtd:xml:4.1.2
```

![image-20240514091050771](https://s2.loli.net/2024/05/14/wPMtQFa9nLNl8BJ.png)

#### 2. 了解 `URL API` 与 `URLSearchParams API`

#### 3. 在 `react router` 等路由库中内置了 `useSearchParams` 及 [useLocation](https://reactrouter.com/en/main/hooks/use-location) 的 hooks，如果脱离路由库，如何实现(React/Vue 均可)

-----

## http图解

### 一、基础知识

“根据 Web 浏览器地址栏中指定的 URL，Web 浏览器从 Web 服务器端获取文件资源（resource）等信息，从而显示出 Web 页面。像这种通过发送请求获取服务器资源的 Web 浏览器等，都可称为客户端（client）。”

“Web 使用一种名为 HTTP（HyperText Transfer Protocol，超文本传输协议 1
 ）的协议作为规范，完成从客户端到服务器端等一系列运作流程。而协议是指规则的约定。可以说，Web 是建立在 HTTP 协议上通信的。”

#### IP、TCP 和 DNS

IP（Internet Protocol）网际协议，“IP”其实是一种协议的名称，IP 协议的作用是把各种数据包传送给对方。而要保证确实传送到对方那里，则需要满足各类条件。其中两个重要的条件是 IP 地址和 MAC 地址（Media Access Control Address）。
IP 地址指明了节点被分配到的地址，MAC 地址是指网卡所属的固定地址。IP 地址可以和 MAC 地址进行配对。IP 地址可变换，但 MAC 地址基本上不会更改。

“TCP 位于传输层，提供可靠的字节流服务。” Hypertext Transfer Protocol，TCP 协议为了更容易传送大数据才把数据分割，而且 TCP 协议能够确认数据最终是否送达到对方。“为了准确无误地将数据送达目标处，TCP 协议采用了三次握手（three-way handshaking）策略。”“用 TCP 协议把数据包送出去后，TCP 不会对传送后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了 TCP 的标志（flag） —— SYN（synchronize） 和 ACK（acknowledgement）。
发送端首先发送一个带 SYN 标志的数据包给对方。接收端收到后，回传一个带有 SYN/ACK 标志的数据包以示传达确认信息。最后，发送端再回传一个带 ACK 标志的数据包，代表“握手”结束。
若在握手过程中某个阶段莫名中断，TCP 协议会再次以相同的顺序发送相同的数据包。”“除了上述三次握手，TCP 协议还有其他各种手段来保证通信的可靠性。”

DNS（Domain Name System）服务是和 HTTP 协议一样位于应用层的协议。它提供域名到 IP 地址之间的解析服务。“DNS 协议提供通过域名查找 IP 地址，或逆向从 IP 地址反查域名的服务。

<img src="https://s2.loli.net/2024/05/15/4XIZ2oC16ranMGQ.png" alt="image-20240515085739024" style="zoom:33%;" />

<img src="https://s2.loli.net/2024/05/15/64ofs7WBr2mzxgb.png" alt="image-20240515085752837" style="zoom:33%;" />

<img src="https://s2.loli.net/2024/05/15/oTQpvR8OsSPcLf1.png" alt="image-20240515085823551" style="zoom:33%;" />

#### URI & URL

与 URI（统一资源标识符） Uniform Resource Identifier 相比，我们更熟悉 URL（Uniform Resource Locator，统一资源定位符）。URL：正是使用 Web 浏览器等访问 Web 页面时需要输入的网页地址。比如 http://hackr.jp/
URI：就是由某个协议方案表示的资源的定位标识符。协议方案是指访问资源所使用的协议类型名称。

采用 HTTP 协议时，协议方案就是 http。除此之外，还有 ftp、mailto、telnet、file 等。标准的 URI 协议方案有 30 种左右”

### http协议

“HTTP 协议和 TCP/IP 协议族内的其他众多的协议相同，用于客户端和服务器之间的通信。
请求访问文本或图像等资源的一端称为客户端，而提供资源响应的一端称为服务器端。”	

![image-20240515090746231](https://s2.loli.net/2024/05/15/ZBl9LSRad5u1w24.png)

```
HTTP/1.1 200 OK
Date: Tue, 10 Jul 2012 06:50:15 GMT
Content-Length: 362
Content-Type: text/html

// 响应报文
```

#### http请求方法

get post put,head,delete,options,trace,connect

“在 HTTP/1.1 中，所有的连接默认都是持久连接,这样就能够做到同时并行发送多个请求，而不需要一个接一个地等待响应了”

“保留无状态协议这个特征的同时又要解决类似的矛盾问题，于是引入了 Cookie 技术。Cookie 技术通过在请求和响应报文中写入 Cookie 信息来控制客户端的状态。

Cookie 会根据从服务器端发送的响应报文内的一个叫做 Set-Cookie 的首部字段信息，通知客户端保存 Cookie。当下次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入 Cookie 值后发送出去。
服务器端发现客户端发送过来的 Cookie 后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息。”

“响应报文（服务器端生成 Cookie 信息）

HTTP/1.1 200 OK
Date: Thu, 12 Jul 2012 07:12:20 GMT
Server: Apache
＜Set-Cookie: sid=1342077140226724; path=/; expires=Wed,
10-Oct-12 07:12:20 GMT＞
Content-Type: text/plain; charset=UTF-8


请求报文（自动发送保存着的 Cookie 信息）

GET /image/ HTTP/1.1
Host: hackr.jp
Cookie: sid=1342077140226724

cookie

Cookie 技术通过在请求和响应报文中写入 Cookie 信息来控制客户端的状态。
Cookie 会根据从服务器端发送的响应报文内的一个叫做 Set-Cookie 的首部字段信息，通知客户端保存 Cookie。当下次客户端再往该服务器发送请求时，客户端会自动在请求报文中加入 Cookie 值后发送出去。
服务器端发现客户端发送过来的 Cookie 后，会去检查究竟是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息。

### http报文

用于 HTTP 协议交互的信息被称为 HTTP 报文。请求端（客户端）的 HTTP 报文叫做请求报文，响应端（服务器端）的叫做响应报文。HTTP 报文本身是由多行（用 CR+LF 作换行符）数据构成的字符串文本。
HTTP 报文大致可分为报文首部和报文主体两块。两者由最初出现的空行（CR+LF）来划分。通常，并不一定要有报文主体。

// 插图 请求报文/响应报文的图例

HTTP 报文的主体用于传输请求或响应的实体主体。
通常，报文主体等于实体主体。只有当传输中进行编码操作时，实体主体的内容发生变化，才导致它和报文主体产生差异。

HTTP 协议中有一种被称为内容编码的功能会将报文内容进行体积压缩

gzip（GNU zip），compress（UNIX 系统的标准压缩），deflate（zlib），identity（不进行编码）

把实体主体分块的功能称为分块传输编码（Chunked Transfer Coding）。

#### 发送多种数据的多部分对象集合

HTTP 协议中也采纳了多部分对象集合，发送的一份报文主体内可含有多类型实体。通常是在图片或文本文件等上传时使用。

multipart/form-data： 在 Web 表单文件上传时使用。

multipart/byteranges：状态码 206（Partial Content，部分内容）响应报文包含了多个范围的内容时使用。

### HTTP 状态码

#### 状态码的类别

| 类别 | 原因短语                         |
| ---- | -------------------------------- |
| 1XX  | Informational（信息性状态码）    |
| 2XX  | Success（成功状态码）            |
| 3XX  | Redirection（重定向状态码）      |
| 4XX  | Client Error（客户端错误状态码） |
| 5XX  | Server Error（服务器错误状态码） |
