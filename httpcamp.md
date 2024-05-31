# http in Camp

# 资源

## 资源与 URI

在 HTTP 中，使用 URI 指定唯一资源。

### URI

URI，`Uniform Resource Identifiers`，统一资源标识符。它适用于多种协议，以下都属于 URI。

```Bash
https://developer.mozilla.org/en-US/docs/Learn

git@github.com:shfshanyue/node-examples.git

ftp://shanyue.tech/http-train.zip

data:,Hello%2C%20World!
```

![img](https://s2.loli.net/2024/05/31/H1bEDfXwoR62mzF.png)

**而 URL 一般指 HTTP 协议的** **URI****。**

![img](https://s2.loli.net/2024/05/31/OKN6eakmrLo4cvJ.png)

### URI 组成部分

URI 由以下部分组成，详见 [MDN: Identifying resources on the Web](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web)，[RFC 9110: URI](https://httpwg.org/specs/rfc9110.html#uri) 及 [wiki: URI](https://zh.wikipedia.org/wiki/统一资源标志符)

- `protocol`。包含 http/ssh/ftp 等。
- `authority`。包含 `userinfo`/`host`/`port`，其中 `userinfo` 与 `port` 可选。
- `path`
- `query`，在 URI 的后缀有 `?a=3&b=4` 被称为 `query`，也被称为 `search`，可通过 `URLSearchParams API` 进行解析。
- `fragment`，在 URI 的后缀有 `#app` 被称为 `fragment`，也被称为 `hash`

### URL API

在浏览器/node.js 中，通过 [URL API](https://developer.mozilla.org/zh-CN/docs/Web/API/URL_API) 可解析 URL 的各个部分。

```JavaScript
new URL('https://q.shanyue.tech/engineering/?a=3&b=4')
```

![img](https://s2.loli.net/2024/05/31/myQnVRwjJozpLEK.webp)

### URLSearchParams API

在浏览器/node.js 中，[URLSearchParams API](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams) 用以处理 URL 的查询字符串。

```JavaScript
const params = new URLSearchParams({ a: 3, b: 4 })



// 也可以接收字符串 

// const params = new URLSearchParams('a=3&b=4')



//=> 3

params.get('a')



//=> a=3&b=4

params.toString()



//=> {a: '3', b: '4'}

Object.fromEntries(params.entries())
```

其中，通过 `URL API` 也可以获取到 `query`，代码如下：

```JavaScript
const url = new URL('https://q.shanyue.tech/engineering/?a=3&b=4')



//=> {a: '3', b: '4'}

Object.fromEntries(url.searchParams.entries())
```

### URI 与 URL 的区别

一般来讲，URL 是 URI 的子集，但在目前的 [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110.html) 中，都使用 URI 这个词来表示 HTTP 上的资源。见 [RFC 9110 Identifiers in HTTP](https://www.rfc-editor.org/rfc/rfc9110.html#name-identifiers-in-http)。

- [HTTP 协议中 URI 和 URL 有什么区别？](https://www.zhihu.com/question/21950864)
- [What is the difference between a URI, a URL, and a URN?](https://stackoverflow.com/questions/176264/what-is-the-difference-between-a-uri-a-url-and-a-urn)

### 作业

1. 什么是 URI
2. 了解 `URL API` 与 `URLSearchParams API`
3. 在 `react router` 等路由库中内置了 `useSearchParams` 及 [useLocation](https://reactrouter.com/en/main/hooks/use-location) 的 hooks，如果脱离路由库，如何实现(React/Vue 均可，推荐使用 codesandbox)

## MIME

资源类型通过 MIME（`Multipurpose Internet Mail Extensions`）进行表示，以此为基础的 npm 库 [mime-db](https://github.com/jshttp/mime-db) 也常用在各个 Node.js 服务器框架。

我们常见的文本、图像与视频，皆有其特有的 `MIME Type`，常见文件类型的拓展名与 MIME Type 可见 [MIME Types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)。

可在以下链接找到 `mime types` 大全。

- https://[www.iana.org/assignments/media-types/media-types.xhtml](http://www.iana.org/assignments/media-types/media-types.xhtml)
- https://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
- https://hg.nginx.org/nginx/raw-file/default/conf/mime.types

对于浏览器应该如何加载该类型资源，取决于其 MIME Type，而非其后缀名。

### 常见的 MIME Type 及其后缀

#### 二进制

- `.bin`，Any kind of binary data，`application/octet-stream`

#### 图片

- `.avif`，AVIF image，`image/avif`
- `.webp`，WEBP image，`image/webp`
- `.gif`，Graphics Interchange Format (GIF)，`image/gif`
- `.ico`，Icon format，`image/vnd.microsoft.icon`
- `.jpeg`，`.jpg`，JPEG images，`image/jpeg`
- `.png`，Portable Network Graphics，`image/png`
- `.svg`，Scalable Vector Graphics (SVG)，`image/svg+xml`
- `.tif`， `.tiff`，Tagged Image File Format (TIFF)，`image/tiff`

#### 音视频

- `.avi`，AVI: Audio Video Interleave，`video/x-msvideo`
- `.mp3`，MP3 audio，`audio/mpeg`
- `.mp4`，MP4 video，`video/mp4`
- `.mpeg`，MPEG Video，`video/mpeg`
- `.wav`，Waveform Audio Format，`audio/wav`

#### 文档

- `.csv`，Comma-separated values (CSV)，`text/csv`
- `.doc`，Microsoft Word，`application/msword`
- `.docx`，Microsoft Word (OpenXML)，`application/vnd.openxmlformats-officedocument.wordprocessingml.document`
- `.epub`，Electronic publication (EPUB)，`application/epub+zip`
- `.pdf`，Adobe Portable Document Format (PDF)，`application/pdf`
- `.ppt`，Microsoft PowerPoint，`application/vnd.ms-powerpoint`
- `.pptx`，Microsoft PowerPoint (OpenXML)，`application/vnd.openxmlformats-officedocument.presentationml.presentation`
- `.vsd`，Microsoft Visio，`application/vnd.visio`
- `.xls`，Microsoft Excel，`application/vnd.ms-excel`
- `.xlsx`，Microsoft Excel (OpenXML)，`application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

#### 压缩

- `.gz`，GZip Compressed Archive，`application/gzip`
- `.rar`，RAR archive，`application/vnd.rar`
- `.tar`，Tape Archive (TAR)，`application/x-tar`

#### 字体

- `.ttf`，TrueType Font，`font/ttf`
- `.woff`，Web Open Font Format (WOFF)，`font/woff`
- `.woff2`，Web Open Font Format (WOFF)，`font/woff2`

#### 编程

- `.htm`，`.html`，HyperText Markup Language (HTML)，`text/html`
- `.js`，`JavaScript`，`text/javascript`
- `.json`，JSON format，`application/json`
- `.jsonld`，JSON-LD format，`application/ld+json`
- `.mjs`，JavaScript module，`text/javascript`
- `.php`，Hypertext Preprocessor (Personal Home Page)，`application/x-httpd-php`
- `.sh`，Bourne shell script，`application/x-sh`
- `.txt`，Text，`text/plain`
- `.xhtml`，XHTML，`application/xhtml+xml`
- `.xml`，XML，`application/xml` is recommended as of RFC 7303 (section 4.1)， but `text/xml` is still used sometimes

#### 作业

1. 有哪些图片常见的 MIME Type
2. 有哪些前端常见的 MIME Type

# 报文

## http报文

HTTP，Hyper Text Transfer Protocol 简写，超文本传输协议。在前端最重要的体现在于，浏览器（HTTP Client）与服务器（HTTP Server）之间的通信。
HTTP 是前后端沟通的桥梁，了解 HTTP 协议及报文相当重要。

HTTP 报文
HTTP Message，也叫 HTTP 报文，用于在客户端与服务器间传送数据。HTTP 由请求（Request）及响应（Response）构成。
访问百度首页的报文格式如下，报文每行由 \r\n 换行，详见 LF 与 CRLF 的区别。
从报文可以看出，HTTP 是基于文本的协议。

```bash
# 请求报文​
2
​
3
# 首行由 Method Path Version 构成​
4
# 每一行结尾是 \r\n​
5
GET / HTTP/1.1​
6
# 以下是请求头，Host 是请求的域名​
7
Host: www.baidu.com​
8
User-Agent: curl/7.79.1​
9
Accept: */*​
10
​
11
# 响应报文​
12
# 相隔两个 \r\n，将会收到响应报文​
13
​
14
# 首行由 Version StatusCode StatusText 组成​
15
# 一般在 HTTP/2 中，没有 StatusText​
16
HTTP/1.1 200 OK​
17
# 以下是响应头​
18
Accept-Ranges: bytes​
19
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform​
20
Connection: keep-alive​
21
Content-Length: 2443​
22
Content-Type: text/html​
23
Date: Wed, 31 Aug 2022 09:23:42 GMT​
24
Etag: "58860402-98b"​
25
Last-Modified: Mon, 23 Jan 2017 13:24:18 GMT​
26
Pragma: no-cache​
27
Server: bfe/1.0.8.18​
28
Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/​
29
​
30
# 响应体​
31
# 相隔两个 \r\n，将会收到响应体​
32
<!DOCTYPE html>​
33
...​
```

通过 `curl -v https://www.baidu.com` 可以获得完整的响应报文以及响应体。

![image-20240531084539209](https://s2.loli.net/2024/05/31/8x15BaC6ylOJbqM.png)

以下是请求方法为 GET 时的 HTTP 报文：

![image-20240531084604705](https://s2.loli.net/2024/05/31/wU9becm5ti83TA1.png)

如果是 POST 请求，则请求头与请求体中间隔两个 \r\n，而请求体与响应头中间没有 \r\n。

![image-20240531084640810](https://s2.loli.net/2024/05/31/J5Sm4xiZNEaOCQL.png)

>  图片来自 [HTTP Messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages)

以下是请求方法为 POST 时的 HTTP 报文：

![image-20240531084712967](https://s2.loli.net/2024/05/31/qLaH2tV9yEJbwhv.png)

## HTTP Client/Server

HTTP 由请求以及响应组成，负责请求的被称为 `HTTP Client`，即 HTTP 客户端，而负责响应的被称为 `HTTP Server`，即HTTP 服务器端。

在后端中，他们的 `nginx/django/express/koa` 等，便是扮演 HTTP 服务器端的角色，**接收 HTTP 客户端的请求，分析路由、请求方法请求体，并返回对应的响应报文**。

在前端中，浏览器便是扮演 HTTP 客户端的角色，从代码层面来说，我们使用的 `fetch`/`axios` 就是 HTTP 客户端，各种编程语言的请求库以及 `curl` 都可以视为 HTTP 客户端。

在前后端联调 API 时，团队经常使用的 API 管理工具，[Apifox](https://www.apifox.cn/a1shanyue) 与 [Postman](https://www.postman.com/) 也属于 HTTP 客户端。

## 作业

1. 什么是 `\r\n`
2. 如何找到文件中的 `\r\n`
3. HTTP 报文格式是什么样的
4. 我们如何查看某次请求的 HTTP 报文



# 头部

## 1. header



HTTP 中的 Header 虽然接触很多，但是一些规则容易忽略：

1. HTTP Header 名称不区分大小写。因此 `Content-Type` 与 `content-type` 并无差别
2. HTTP Header 名称与值由 `:` 分割，值首部空格将被忽略，更严格地说，是被 `/:\s+/` 分割。因此 `A: 3` 与 `A:    3` 并无差别。
3. HTTP Header 中的非标准自定义首部由 `X-` 作为前缀，虽[已被废弃](https://datatracker.ietf.org/doc/html/rfc6648)，但仍然有大量使用。比如 `X-Powered-By`，仍被大量服务器框架所使用。

HTTP Header 虽然不区分大小写，但有时也希望获取到原始的 Header，因此在 Node.js 中提供了两个 API：

1. [message.headers](https://nodejs.org/api/http.html#messageheaders)：对头部全部转化为小写形式返回
2. [message.rawHeaders](https://nodejs.org/api/http.html#messagerawheaders)：对头部不做大小写转化进行返回

## pseudo-header (伪头)

![image-20240531085012364](https://s2.loli.net/2024/05/31/eN29Cd7pGbHAEc3.png)

在 HTTP/2 协议中，以 `:` 开头，被称为伪头，他们用于传递 HTTP 报文初识行数据。见 [RFC9113](https://www.rfc-editor.org/rfc/rfc9113.html#name-request-pseudo-header-field)

尽管**伪头不属于 HTTP 头部字段**，但仍然在这里列出一下，毕竟在浏览器控制台网络面板中也将他们置于一起。

有以下 4+1 个伪头，其中四个用于请求，一个用于响应。

- `:authority`，同 Host
- `:method`，同 METHOD
- `:path`，同 PATH
- `:scheme`，同 SCHEME，即 HTTPS/HTTP
- `:status`，同 Status Code

## 测试

`httpbin.org/headers` 可返回请求头，可与 `curl` 结合使用。

```bash
# 请求头无关大小写，在 httpbin 中会全部作大写处理
$ curl "http://httpbin.org/headers" -H "A: 3" -H "b: 4" -H "c:     5"
{
  "headers": {
    "A": "3", 
    "Accept": "*/*", 
    "B": "4", 
    "C": "5", 
    "Host": "httpbin.org", 
    "User-Agent": "curl/7.79.1", 
    "X-Amzn-Trace-Id": "Root=1-63214e3e-5b4c2b280bc00e7e39b568fc"
  }
}
```

## 作业

1. HTTP 响应头中 `Cache-Control`，有时为首字母大写，有时为小写，哪个是正确写法
2. 什么是伪头
3. 如何自定义 HTTP 头部
4. 观察自己常逛网站的 HTTP 请求头与响应头
5. 通过 `curl` 与 `httpbin` 测试请求头部

## 2. 请求头列表

请求头一般有以下分类，本篇文章仅大致介绍下常见的请求头，部分重要的请求头将在以后文章重点讲解。

## 控制相关

- `Host`：一般一个 IP 地址对应 N 个应用，通过 `Host` 即可定位到对应的应用。
- `Cache-Control`：发送请求时，如何控制客户端的缓存策略。
- `Expect`：与 100 状态码相关。
- `Range`：指定范围请求，与 206 状态码相关。

## 条件相关

条件请求相关头部，前两者与 304 状态码相关，`If-Range` 与 206 状态码相关。

- `If-Match`
- `If-Modified-Since`
- `If-Range`

## 内容协商

内容协商，告知服务器端我需要什么样的资源，比如语言以及压缩编码，如果服务器无法返回对应的资源，则返回 406 状态码。

![image-20240531085146210](https://s2.loli.net/2024/05/31/LrBStcR7Fp9Dxk3.png)

Accept：我（客户端）需要什么样的资源，比如 json 与 html
Accept-Encoding：我需要什么样的压缩编码，比如 gzip 与 br，如果不配置则可能不进行压缩
Accept-Language：我需要什么样的语言，比如 en-US 和 zh-CN

认证相关
Authorization：每次发送请求时，使用该头部携带 token 信息，维护客户端的认证状态

来源相关
通过来源相关，我们可以更好地统计用户信息，也可以以此为依据用来防止爬虫。
Referer：当前页面的上一个页面是哪里，或者说该页面是由哪个页面跳转而来的
User-Agent：用户代理是什么，或者说该页面是由哪个客户端（比如浏览器版本号之类）跳转而来的

作业
浏览常见网站，打开浏览器控制台网络面板，查看其请求头

## 3. 响应头列表

响应头一般有以下分类，本篇文章仅大致介绍下常见的响应头，部分重要的响应头将在以后文章重点讲解。

控制相关
Date：HTTP 报文在源服务器产生的时间
Age：HTTP 报文在缓存服务器，比如 CDN 中的存储时间，以秒作为单位。一般来说，当前时间减去 Date，大约就是 Age 的秒数。
Cache-Control：HTTP 缓存策略，后续讲到，与 304 状态码相关
Location：新建资源与重定向资源的路径，与 201/30x 状态码相关
Vary：一般作为缓存的键（key），与内容协商相关。如 vary: Accept-Encoding

条件相关
以下均与缓存策略相关，将在后续文章重点讲到
ETag：Entity Tag，用以标志实体的唯一性，与缓存策略 304 状态码相关
Last-Modified：资源上次修改时间，与缓存策略 304 状态码相关

作业
Date 与 Age 响应头代表什么意思
浏览常见网站，打开浏览器控制台网络面板，查看其响应头

## 4. Host 与 :authority

在一个的服务器中，可能拥有多个 Host 的应用服务，即同一个 IP 地址，可能对应多个域名，此时仅仅通过 IP 无法访问到对应的服务，可通过 Host 来进行定位。
如果某 IP 对应多个域名，则 Host 是必须携带的请求头，如果缺失了该请求头则会返回 400 状态码。在 HTTP/2 以及 HTTP/3 中，以一个伪头 :authority 代替。
在浏览器中使用 fetch，或者使用 curl/httpie 发送请求时，将会自动携带该请求头。

nc
可通过 nc 命令，直接以发送报文的形式来请求数据，进行测试：

```
$ nc httpbin.org 80​
2
GET /get HTTP/1.1​
3
Host: httpbin.org​
4
​
5
HTTP/1.1 200 OK​
6
Date: Wed, 21 Sep 2022 05:49:27 GMT​
7
Content-Type: application/json​
8
Content-Length: 200​
9
Connection: keep-alive​
10
Server: gunicorn/19.9.0​
11
Access-Control-Allow-Origin: *​
12
Access-Control-Allow-Credentials: true​
13
​
14
{​
15
  "args": {}, ​
16
  "headers": {​
17
    "Host": "httpbin.org" ​
18
  }, ​
19
  "origin": "122.222.222.222", ​
20
  "url": "http://httpbin.org/get"​
21
}​
```

如果不携带 `Host` 请求头，则直接返回 400 状态码。

```
$ nc httpbin.org 80
GET /get HTTP/1.1

HTTP/1.1 400 Bad Request
Server: awselb/2.0
Date: Wed, 21 Sep 2022 05:51:39 GMT
Content-Type: text/html
Content-Length: 122
Connection: close

<html>
<head><title>400 Bad Request</title></head>
<body>
<center><h1>400 Bad Request</h1></center>
</body>
</html>
```

如果某 IP 地址仅对应一个域名，并且通过 IP 地址可直接访问应用，则可不加 `Host` 请求头，**比如百度**。**但是这种情况很少。**

```
$ dig +short www.baidu.com
www.a.shifen.com.
14.215.177.38
14.215.177.39

# 直接访问 IP 地址成功
$ curl --head 14.215.177.39
HTTP/1.1 200 OK
Accept-Ranges: bytes
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Connection: keep-alive
Content-Length: 277
Content-Type: text/html
Date: Sun, 23 Oct 2022 03:01:08 GMT
Etag: "575e1f71-115"
Last-Modified: Mon, 13 Jun 2016 02:50:25 GMT
Pragma: no-cache
Server: bfe/1.0.8.18

# 此时 nc 时，可不发送 HOST 请求头
$ nc www.baidu.com 80
GET / HTTP/1.1

HTTP/1.1 200 OK
Accept-Ranges: bytes
Cache-Control: no-cache
```

## curl

通过 `curl` 发送 HTTP 请求时，将会自动携带 `HOST` 请求头。那如何通过 `curl` 模拟 `HOST` 的功能呢？

1. 通过 `-H HOST:` 删除 HOST 请求头。**在** `**curl**` **中，如果** `**-H**` **指定的参数以** `**:**` **结尾，代表不发送该请求头。**
2. 通过请求 IP 地址，并通过 `-H` 参数明确指定 Host 头部。

```Bash
$ curl -i http.devtool.tech -H HOST:

HTTP/1.1 404 Not Found

Connection: keep-alive

Content-Length: 67

Content-Type: text/plain; charset=utf-8

Date: Sun, 23 Oct 2022 04:18:47 GMT

Server: Vercel

X-Vercel-Error: DEPLOYMENT_NOT_FOUND

X-Vercel-Id: hnd1::cgcvn-1666498727096-1e18fb504bcb



The deployment could not be found on Vercel.



DEPLOYMENT_NOT_FOUND
```

以下是通过 IP + HOST 的方式：

```Bash
# 请求成功

$ curl http.devtool.tech



# 获取到其 IP 地址

$ dig +short http.devtool.tech

76.223.126.88



# 直接请求 IP，导致找不到该应用

$ curl 76.223.126.88



# 请求成功

$ curl 76.223.126.88 -H "Host: http.devtool.tech"
```

## 作业

1. 通过 `nc` 命令直接发送报文控制 Host 请求头的发送
2. 如何删除 `curl` 默认携带的请求头
3. 通过浏览器控制台，查看各个网站的 `Host`/`:authority` 请求头

## 内容协商

内容协商，客户端与服务器端协商需要什么样的资源，比如语言（中文/英文）、压缩编码以及媒体类型，如果服务器无法返回对应的资源，则返回 406 状态码。

![image-20240531085926961](https://s2.loli.net/2024/05/31/vYkOGAMJpXSf2ql.png)



## 请求头

关于内容协商的请求头有以下几个：

- `Accept`：我（客户端）需要什么样的 MIME 资源，比如 json 与 html，**不较常见**
- `Accept-Language`：我需要什么样的语言，比如 en-US 和 zh-CN，**较为常见**
- `Accept-Encoding`：我需要什么样的压缩编码，比如 gzip 与 br，如果不配置则可能不进行压缩，**非常常见**

## Accept 格式

```Bash
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, */*;q=0.8



# 为了便于理解，我使用括号进行分组，括号仅仅是为了理解！

Accept: (text/html), (application/xhtml+xml), (application/xml;q=0.9), (*/*;q=0.8)
```

其中 `q` 为 `quality`，意为权重，默认为 1。比如以上 `Accept` 优先级如下。

| **Content-Type**      | **Quality** |
| --------------------- | ----------- |
| text/html             | 1.0         |
| application/xhtml+xml | 1.0         |
| application/xml       | 0.9         |
| */*                   | 0.8         |

注意，比较反直觉的是，在 HTTP Header 中，`**,**` **拥有比** `**;**` **更高的优先级**，根据`,` 分组，而不是根据 `;` 分组，这有可能让一些人无所适从。

## Accept

`Accept` 协商 Content-Type，在国外 API 文档较为常见，比如 [Github API](https://docs.github.com/en/rest/overview/media-types)，它允许配置以下两种 MIME Type

- `application/vnd.github.text+json`：仅仅返回 Markdown
- `application/vnd.github.html+json`：仅仅返回 HTML

比如，在 Github Issue 中，通过 markdown 进行书写 Issue，可通过内容协商控制返回 Markdown/HTML 的一种。

> `vnd`，`vendor` 简写，由供应商自定义 MIME Type，见 [RFC 6838](https://www.rfc-editor.org/rfc/rfc6838.html#section-3.2)，随后紧跟供应商名称与其自定义的类型，比如 Github 的 `application/vnd.github.text+json` 以及微软的 `application/vnd.ms-excel`。

可见示例

```Bash
# jq 为 JSON 处理工具，以下语句意为只打印 body body_html body_text 三个字段

# 选择 html 内容进行返回，则可以看到返回的 .body_html 字段

$ curl -s \

  -H "Accept: application/vnd.github.html+json" \

  https://api.github.com/repos/shfshanyue/Daily-Question/issues/1 | jq "{ body, body_html, body_text }"

{

  "body": null,

  "body_html": "<p dir=\"auto\">网站开发中，如何实现图片的懒加载，随着 web 技术的发展，他有没有一些更好的方案</p>",

  "body_text": null

}



# 选择 text 内容进行返回，则可以看到返回的 .body_text 字段

$ curl -s \

  -H "Accept: application/vnd.github.text+json" \

  https://api.github.com/repos/shfshanyue/Daily-Question/issues/1 | jq "{ body, body_html, body_text }"

{

  "body": null,

  "body_html": null,

  "body_text": "网站开发中，如何实现图片的懒加载，随着 web 技术的发展，他有没有一些更好的方案"

}
```

## Accept-Language

`Accept Language` 协商语言，常用作国际化。

比如，MDN 会通过判断请求中的 `Accept`，来判断给出那种语言的版本。

```Bash
# 如果请求的语言是中文版本，则重定向至 /zh-CN/ 中文版页面

$ curl -I https://developer.mozilla.org/ -H 'Accept-Language: zh'

HTTP/2 302 

content-length: 0

location: /zh-CN/



# 如果请求的语言是英语版本，则重定向至 /en-US/ 英文版页面

$ curl -I https://developer.mozilla.org/ -H 'Accept-Language: en'

HTTP/2 302 

content-length: 0

location: /en-US/
```

而在浏览器中发送请求时，通过配置页面的语言配置，来控制发送 `Accept Language` 请求头，

![image-20240531085948589](https://s2.loli.net/2024/05/31/28mOJK9IeuYoBXG.png)

语言配置也可以通过 API 拿到，通过此可进行 i18n 配置，作为其默认语言，这比通过前端界面选择语言要友好许多：

```JavaScript
navigator.language
```

## Accept-Encoding

控制压缩编码，比如 gzip 与 br，如果不配置则可能不进行压缩。可通过响应头 `Content-Encoding` 查看压缩编码。

```Bash
$ curl -I https://juejin.cn

HTTP/2 200 

content-type: text/html; charset=utf-8

content-length: 58565

vary: Accept-Encoding



$ curl -I https://juejin.cn -H 'Accept-Encoding: gzip'

HTTP/2 200 

server: Tengine

content-type: text/html; charset=utf-8

content-encoding: gzip

vary: Accept-Encoding



$ curl -I https://juejin.cn -H 'Accept-Encoding: br'

HTTP/2 200 

content-type: text/html; charset=utf-8

vary: Accept-Encoding

content-encoding: br
```

## 反爬

```Bash
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9

accept-encoding: gzip, deflate, br

accept-language: zh-CN,zh;q=0.9,en;q=0.8
```

在浏览器中会自动发送 `Accept`、`Accept-Encoding` 以及 `Accept-Language` 三个请求头，如上所示。

因此一些安全性要求较高的网站，**将通过以上三个请求头是否存在判断请求方是浏览器还是爬虫**。如果不存在以上请求头，则直接拒绝请求。

## 作业

1. 什么是内容协商
2. Accept 如何配置权重
3. Accept/Accept-Language/Accept-Encoding 三个请求头的应用场景
4. 如何得知某资源是否配置了 gzip/brotli 压缩
5. 为什么在浏览器中发送的请求大都是 gzip 经压缩数据，而在 curl 直接发送请求时返回的是原始数据

## Content-type

`Content-Type` 指定 Body 的媒体资源类型，如果是请求头，则代表请求体的资源类型，如果是响应头，则代表响应体的资源类型。

资源类型通过 MIME（`Multipurpose Internet Mail Extensions`）进行表示，以此为基础的 npm 库 [mime-db](https://github.com/jshttp/mime-db) 也常用在各个 Node.js 服务器框架。

我们常见的文本、图像与视频，皆有其特有的 MIME Type，常见文件类型的拓展名与 MIME Type 可见 [MIME Types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)。

对于浏览器应该如何加载该类型资源，取决于其 MIME Type，而非其后缀名。

## 请求头中的 Content-Type

当请求头中含有 `Content-Type` 时，它指明 Request Body 的媒体资源类型，此时一般为 POST 请求。

当前端向后端请求 API 接口时，请求体一般为 JSON 数据类型，此时需要配置 `Content-Type: application/json`。

除此之外，在 API 中常见以下几种请求头中的 Content-Type：

- `application``/json`：请求体为 JSON
- `application/x-www-form-urlencoded`：请求体为以 `&` 分割的字符串，如 `a=3&b=4`
- `multipart/form-data`：请求体以 Boundary 分割

在浏览器中通过 Form 表单发送请求体会自动携带 `application/x-www-form-urlencoeded` 请求头，通过 `FormData API` 会自动携带 `multipart/form-data` 请求头。但是如果需要通过 `application/json` 发送 JSON 数据，则需要**手动携带请求头**。

当服务器接收到客户端请求时，将会**根据 Content-Type 来决定如何解析 Request Body**，这就是 Body Parser 所做的事情，比如 [koajs/bodyparser](https://github.com/koajs/bodyparser)。

```Bash
# 使用 Form 发送请求

$ curl -X POST httpbin.org/post -d a=3

{

  "args": {}, 

  "data": "", 

  "files": {}, 

  "form": {

    "a": "3"

  }, 

  "headers": {

    "Accept": "*/*", 

    "Content-Length": "3", 

    "Content-Type": "application/x-www-form-urlencoded", 

    "Host": "httpbin.org", 

    "User-Agent": "curl/7.79.1", 

    "X-Amzn-Trace-Id": "Root=1-63454557-3cd5f9a53682be3a327dd0a3"

  }, 

  "json": null, 

  "url": "http://httpbin.org/post"

}



# 使用 JSON 发送请求

$ curl -X POST httpbin.org/post -H "content-type: application/json" -d '{"a": 3}'

{

  "args": {}, 

  "data": "{\"a\": 3}", 

  "files": {}, 

  "form": {}, 

  "headers": {

    "Accept": "*/*", 

    "Content-Length": "8", 

    "Content-Type": "application/json", 

    "Host": "httpbin.org", 

    "User-Agent": "curl/7.79.1", 

    "X-Amzn-Trace-Id": "Root=1-63454532-1aa1d82138215128077cd85d"

  }, 

  "json": {

    "a": 3

  }, 

  "url": "http://httpbin.org/post"

}
```

## 响应头中的 Content-Type

当响应头中含有 `Content-Type` 时，它指明 Response Body 的媒体资源类型。

因为我们可以通过 HTTP 去请求各种各样的资源，因此 Content-Type 基本上可以是所有 MIME 类型。

而在前端中，涉及到的响应头中的 Content-Type 为以下几种：

- `text/html`
- `text/css`
- `application/javascript`
- `image/png`
- `image/jpeg`
- `image/webp`
- `image/svg+xml`

## 作业

1. 你接触过哪些 MIME Type
2. 你在 HTTP Header 中见过那些 Content-Type



## User-Agent

`User-Agent` 请求头是用以表明客户端的特征字符串，一般也会简称为 `UA`。

对于一般的 HTTP 客户端来说，使用客户端的应用加版本号即可作为 `User-Agent`

```Bash
# curl 的 User-Agent 为 curl/7.79.1

$ curl httpbin.org/headers

{

  "headers": {

    "Accept": "*/*", 

    "Host": "httpbin.org", 

    "User-Agent": "curl/7.79.1", 

    "X-Amzn-Trace-Id": "Root=1-63413b72-69ed41753e5826a46b5f573e"

  }

}



# httpie 的 User-Agent 为 HTTPie/3.2.1

http --body httpbin.org/headers

{

  "headers": {

    "Accept": "*/*",

    "Accept-Encoding": "gzip, deflate",

    "Host": "httpbin.org",

    "User-Agent": "HTTPie/3.2.1",

    "X-Amzn-Trace-Id": "Root=1-63413b60-054f0ff42a87407e6316b91e"

  }

}
```

## 浏览器的 User-Agent

但是对于浏览器来说，其包含的信息就比较多了，他不仅可以表示出你所用的浏览器，浏览器版本号，还可以看出渲染引擎，**操作系统信息**等

![image-20240531090142747](https://s2.loli.net/2024/05/31/TlSYZxCuadUBQJh.png)

比如在以下 UA 中，MacOS 的浏览器 UA 中包含字符 `Mac OS`，iPhone 的浏览器 UA 中包含字符串 `iPhone`

```JavaScript
// 在 MacOS 中的 Chrome 浏览器

'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36'



// 在 iPhone 中的 Chrome 浏览器

'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1'



// 在我手机中的夸克浏览器

// 同时可看出我的手机是 小米10、Android 11 的操作系统，后边可以合理推测，该版本的夸克浏览器基于 Chrome78，而现在 Chrome 最新版本是 Chrome106，肯定有不少兼容性问题

'Mozilla/5.0 (Linux; U; Android 11; zh-CN; Mi 10 Build/RKQ1.200826.002) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/78.0.3904.108 Quark/5.9.3.228 Mobile Safari/537.36'
```

在浏览器中，可通过 API `navigator.userAgent` 查看其 UA，但是在移动端浏览器不方便打开控制台，可使用山月自制小工具 [UA-Parser](https://devtool.tech/ua-parser) 查看浏览器的 UA

## 浏览器如何判断 PC/Mobile

既然在 UA 中包含了操作系统信息，便可使用它判断是否移动端。

通过判断 API `navigator.userAgent` 可获取该浏览器发送请求时的 User-Agent 请求头，对于 Android/iPhone 可以匹配以下正则。

```JavaScript
const appleIphone = /iPhone/i;

const appleIpod = /iPod/i;

const appleTablet = /iPad/i;

const androidPhone = /\bAndroid(?:.+)Mobile\b/i; // Match 'Android' AND 'Mobile'

const androidTablet = /Android/i;
```

当然，不要重复造轮子，推荐一个库: <https://github.com/kaimallea/isMobile>

```JavaScript
import isMobile from "ismobilejs";



const mobile = isMobile();
```

## 打点统计

基于 User-Agent 结合打点服务，可统计某网站的浏览器、操作系统、PC/Mobile 占比等数据。

下图是在 Google Analytics 中，山月的网站基于 User-Agent 分析的各个浏览器版本号占比。

> 目前 20221012，Chrome 最新版本是 106

![image-20240531090240598](https://s2.loli.net/2024/05/31/BQ3MjxvcWbhimo2.png)

可基于网站的浏览器份额占比，配置合适的 [browserslist](https://github.com/browserslist/browserslist) 规则，可减小垫片代码体积，提高网站性能。

关于如何减小垫片代码体积请参考[browserslist 与垫片体积](https://xunlianying.feishu.cn/wiki/wikcnZco6nERwgPF2OfZFH5oblb) 。

## 内容协商

同时 UA 请求头，也可用作内容协商，见 [User-Agent](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Content_negotiation#user-agent_首部)。

HTTP 服务器端根据请求头中的 `User-Agent` 返回不同的内容。有以下实际应用场景

- UA 判断是否移动端，据此返回移动端网站或者PC网站
- UA 判断是否搜索引擎爬虫，据此返回是否经 SEO 优化好的内容，比如 [prerender](https://github.com/prerender/prerender)
- UA 判断是否浏览器环境，据此返回不同内容

试举一例，比如 <https://ifconfig.me> 用于返回请求端的公网 IP 地址，通过 curl 与浏览器访问，完全是不同的页面。

```Bash
# 判断 UA 是否浏览器环境，如果是，返回 html 页面，这与直接打开该网址效果相同

$ curl https://ifconfig.me -H "User-Agent: chrome"



# 如果不是浏览器环境，直接返回 IP 地址

$ curl https://ifconfig.me

171.221.136.154
```

## 作业

1. 在浏览器中如何判断当前环境是移动端还是 PC 端
2. 在浏览器中如何判断当前环境是否在 iPhone 中
3. 打点服务如何统计自己产品用户的各个浏览器版本的占比 



## Referer

> `Referer` 的正确拼法应该是 `Referrer`，但刚开始拼错，后来一直都没改。

`Referer` 请求头指当前请求页面的来源地址。比如

1. 你在谷歌搜索页面进入掘金，那该掘金网页的 Referer 就是谷歌搜索页面
2. 你在知乎文章页面进入掘金，那该掘金网页的 Referer 就是知乎文章页面
3. 你直接在地址栏输入掘金，那该掘金网页的 Referer 请求头不存在

而对于 JS/CSS/Image 来说，Referer 字段一般是对应的文档页面，即 HTML 页面，因为它是通过 HTML 页面中的链接而发起的请求。

由于 `Referer` 可以拿到网站的来源页面，在打点服务中可以进行**打点统计分析**，取得本网站的来源网站的流量占比。如果是商业网站，则可以通过此信息分析流量投放。举一个最简单的例子，比如某网站每天有 1000 条请求是通过网站甲进来，则向网站甲每天支付 1000 元。

## 防盗链

除了打点统计分析外，`**Referer**` **最大的功能是做图片防盗链**。

试举一例，当你访问掘金的图片时，请求图片的请求头中的 `Referer` 将是图片所在的掘金文章地址。**如果掘金服务器端，判断请求图片的请求头** `**Referer**` **并不是掘金的域名，则禁止访问，返回 403 状态码。**

由于掘金图片可以任意上传，如果被人当做图床，这对于掘金来说是笔不少的服务器费用，因此才有了图片防盗链的技术。

## 防盗链示例

解决这种问题，除了图片防盗链，还有图片水印的方式，比如掘金就选择了后者。哔哩哔哩选择了图片防盗链，以哔哩哔哩首页轮播图及 curl 命令行为例说明：

**直接访问该图片地址，访问成功，从以下几点可以看出**

1. 200 状态码
2. content-type 为 image/png，证明是图片格式 
3. content-length 为 2182486，2MB 左右大小，证明不可能是占位符图片
4. 最后下载提示你为二进制图片格式，需要下载到本地

**而添加一个其它域名的 Referer 去请求该图片，则直接返回 403 状态码，被防盗链**

```Bash
# 直接访问该图片地址，访问成功，从以下几点可以看出

$ curl -i https://i0.hdslb.com/bfs/banner/728f45f115c9b99752bb162664600a183b23c8da.png

HTTP/2 200

date: Wed, 12 Oct 2022 02:47:09 GMT

content-type: image/png

content-length: 2182486

server: openresty



Warning: Binary output can mess up your terminal. Use "--output -" to tell 

Warning: curl to output it to your terminal anyway, or consider "--output 

Warning: <FILE>" to save to a file.



# 添加一个其它域名的 Referer 去请求该图片，则直接返回 403 状态码，防盗链

$ curl -i -H "referer: https://shanyue.tech" https://i0.hdslb.com/bfs/banner/728f45f115c9b99752bb162664600a183b23c8da.png

HTTP/2 403 

server: kngx/1.10.2

date: Wed, 12 Oct 2022 02:52:52 GMT

content-type: text/html

content-length: 168



<html>

<head><title>403 Forbidden</title></head>

<body bgcolor="white">

<center><h1>403 Forbidden</h1></center>

<hr><center>kngx/1.10.2</center>

</body>

</html>
```

## 防防盗链

那如何防止防盗链呢？

既然防盗链的原理是判断 `Referer` 请求头，那岂不是不发既可以了？

确实如此，并且浏览器可以通过 `Referrer-Policy` 响应头控制是否发送 `Referer` 请求头。

```Bash
# 不发送 referer 请求头

Referrer-Policy: no-referrer
```

也可以将它置于 HTML 中

```HTML
<meta name="referrer" content="origin" />
<meta name="referrer" content="no-referrer" />
```

在山月的工具网站 [MDTU](https://markdown.devtool.tech/app) 中便通过 `no-referrer` 策略来避免防盗链，你可以将某个防盗链的图片使用 Markdown 格式添加至该工具编辑器中，发现任何防盗链图片都可以正常显示。

![img](https://s2.loli.net/2024/05/31/FblJeXzD5rxtuGc.webp)

## 作业

1. Referer 请求头有那些使用场景
2. 图片防盗链的技术原理是什么
3. 图片防盗链图片为何直接在浏览器**新标签页手动输入地址**可以打开
4. 你见过那些添加了防盗链的网站
5. 如何防止防盗链

# 缓存

# 跨域

# 曲奇

# 状态吗

# Body

# 安全

# HTTPS

# HTTP/2

# 抓包

# websocket

