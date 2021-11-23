---
title: 面试问到HTTP就这么回答
cover: >-
  HTTPs://img.showydream.com/img/SoFF52-ryunosuke-kikuno-hyWy0z4UayE-unsplash.jpg
description: HTTP和HTTPs的区别是什么？为什么HTTPS比HTTP安全?什么是强缓存和协商缓存？常见的HTTP状态码有哪些，都代表了什么含义？
tags: 面经
keywords: HTTP和HTTPs的区别是什么？为什么HTTPS比HTTP安全?什么是强缓存和协商缓存？常见的HTTP状态码有哪些，都代表了什么含义？
categories:
  - 前端
abbrlink: dc44fbe2
date: 2021-11-12 19:53:53
---

## HTTP和HTTPs的区别

- HTTP 是明文传输协议，HTTPS 协议是由 SSL+HTTP 协议构建的可进行加密传输、身份认证的网络协议，比 HTTP 协议安全。

​		<img src="https://image.fundebug.com/2019-0424-00010.png" alt="img"  />

- HTTPS比HTTP更加安全，对搜索引擎更友好，利于SEO，谷歌、百度优先索引HTTPS网页;
- HTTPS需要用到SSL证书，而HTTP不用;
- HTTPS标准端口443，HTTP标准端口80;
- HTTPS基于传输层，HTTP基于应用层;
- HTTPS在浏览器显示绿色安全锁，HTTP没有显示;

## 为什么HTTPS比HTTP安全?

### HTTP的问题

HTTP是明文协议，它有以下缺点：

- 通信使用明文（不加密），内容可能被窃听
- 无法证明报文的完整性，所以可能遭篡改
- 不验证通信方的身份，因此有可能遭遇伪装

HTTP明文协议的缺陷是导致数据泄露、数据篡改、流量劫持、钓鱼攻击等安全问题的重要原因。

### HTTPS如何解决HTTP上述问题？

HTTPS并非是应用层的一种新协议。只是HTTP通信接口部分用SSL（Secure Socket Layer）和TLS（Transport Layer Security）协议代替而已。通常，HTTP直接和TCP通信。当使用SSL时，则演变成先和SSL通信，再由SSL和TCP通信了。简言之，**所谓HTTPS，其实就是身披SSL协议这层外壳的HTTP**。

HTTPS 协议的主要功能基本都依赖于 TLS/SSL 协议，TLS/SSL 的功能实现主要依赖于三类基本算法：散列函数 、对称加密和非对称加密，**其利用非对称加密实现身份认证和密钥协商，对称加密算法采用协商的密钥对数据加密，基于散列函数验证信息的完整性**。

![img](https://img.showydream.com/img/FDDoZq-2019-0424-0004.png)

#### 使用加密解决内容可能被窃听的问题

具体做法是：**发送密文的一方使用对方的公钥进行加密处理“对称的密钥”，然后对方用自己的私钥解密拿到“对称的密钥”，这样可以确保交换的密钥是安全的前提下，使用对称加密方式进行通信**。所以，HTTPS采用对称加密和非对称加密两者并用的混合加密机制。

#### 使用数字签名解决报文可能遭到篡改的问题

网络传输过程中需要经过很多中间节点，虽然数据无法被解密，但可能被篡改，那如何校验数据的完整性呢？—-校验数字签名。

**数字签名有两种功效**：

- 能确定消息确实是由发送方签名并发出来的，因为别人假冒不了发送方的签名。
- 数字签名能确定消息的完整性，证明数据是否未被篡改过。

**数字签名如何生成:**

![img](https://img.showydream.com/img/pnTh5y-2019-0424-0006.png)

将一段文本先用Hash函数生成消息摘要，然后用发送者的私钥加密生成数字签名，与原文文一起传送给接收者。接下来就是接收者校验数字签名的流程了。

![img](https://img.showydream.com/img/7Y4uzP-2019-0424-0007.png)

接收者只有用发送者的公钥才能解密被加密的摘要信息，然后用HASH函数对收到的原文产生一个摘要信息，与上一步得到的摘要信息对比。如果相同，则说明收到的信息是完整的，在传输过程中没有被修改，否则说明信息被修改过，因此数字签名能够验证信息的完整性。

#### 使用数字证书解决通信方身份可能被伪装的问题

**数字证书认证机构（Certificate Authority，简称CA）**处于客户端与服务器双方都可信赖的第三方机构的立场上。我们来介绍一下数字证书认证机构的业务流程。

1. 服务器的运营人员向数字证书认证机构提出公开密钥的申请
2. 数字证书认证机构在判明提出申请者的身份之后，会对已申请的公开密钥做数字签名
3. 然后分配这个已签名的公开密钥，并将该公开密钥放入公钥证书后绑定在一起
4. 服务器会将这份由数字证书认证机构颁发的公钥证书发送给客户端，以进行非对称加密方式通信。公钥证书也可叫做数字证书或直接称为证书。
5. 接到证书的客户端可使用数字证书认证机构的公开密钥，对那张证书上的数字签名进行验证，一旦验证通过，客户端便可明确两件事：

​	一、认证服务器的公开密钥的是真实有效的数字证书认证机构。

​	二、服务器的公开密钥是值得信赖的。

## 什么是强缓存和协商缓存

**强缓存：**浏览器不会像服务器发送任何请求，直接从本地缓存中读取文件并返回Status Code: 200 OK

Expires：过期时间，如果设置了时间，则浏览器会在设置的时间内直接读取缓存，不再请求

Cache-Control：当值设为max-age=300时，则代表在这个请求正确返回时间（浏览器也会记录下来）的5分钟内再次加载资源，就会命中强缓存。

cache-control：除了该字段外，还有下面几个比较常用的设置值：

```
（1） max-age：用来设置资源（representations）可以被缓存多长时间，单位为秒；
（2） s-maxage：和max-age是一样的，不过它只针对代理服务器缓存而言；
（3）public：指示响应可被任何缓存区缓存；
（4）private：只能针对个人用户，而不能被代理服务器缓存；
（5）no-cache：强制客户端直接向服务器发送请求,也就是说每次请求都必须向服务器发送。服务器接收到     请求，然后判断资源是否变更，是则返回新内容，否则返回304，未变更。这个很容易让人产生误解，使人误     以为是响应不被缓存。实际上Cache-Control:     no-cache是会被缓存的，只不过每次在向客户端（浏览器）提供响应数据时，缓存都要向服务器评估缓存响应的有效性。
（6）no-store：禁止一切缓存（这个才是响应不被缓存的意思）。
复制代码
```

> cache-control是http1.1的头字段，expires是http1.0的头字段,如果expires和cache-control同时存在，cache-control会覆盖expires，建议两个都写。

**协商缓存：** 向服务器发送请求，服务器会根据这个请求的request header的一些参数来判断是否命中协商缓存，如果命中，则返回304状态码并带上新的response header通知浏览器从缓存中读取资源

Last-Modifed/If-Modified-Since和Etag/If-None-Match是分别成对出现的，呈一一对应关系

##### Etag/If-None-Match：

Etag：

> Etag是属于HTTP 1.1属性，它是由服务器（Apache或者其他工具）生成返回给前端，用来帮助服务器控制Web端的缓存验证。 Apache中，ETag的值，默认是对文件的索引节（INode），大小（Size）和最后修改时间（MTime）进行Hash后得到的。

If-None-Match:

> 当资源过期时，浏览器发现响应头里有Etag,则再次像服务器请求时带上请求头if-none-match(值是Etag的值)。服务器收到请求进行比对，决定返回200或304



![img](https://img.showydream.com/img/GJCuOP-16a8c60fb0ef49f0~tplv-t2oaga2asx-watermark.awebp)



##### Last-Modifed/If-Modified-Since：

Last-Modified：

> 浏览器向服务器发送资源最后的修改时间

If-Modified-Since：

> 当资源过期时（浏览器判断Cache-Control标识的max-age过期），发现响应头具有Last-Modified声明，则再次向服务器请求时带上头if-modified-since，表示请求时间。服务器收到请求后发现有if-modified-since则与被请求资源的最后修改时间进行对比（Last-Modified）,若最后修改时间较新（大），说明资源又被改过，则返回最新资源，HTTP 200 OK;若最后修改时间较旧（小），说明资源无新修改，响应HTTP 304 走缓存。

> - Last-Modifed/If-Modified-Since的时间精度是秒，而Etag可以更精确。
> - Etag优先级是高于Last-Modifed的，所以服务器会优先验证Etag
> - Last-Modifed/If-Modified-Since是http1.0的头字段

## 常见的HTTP状态码有哪些，都代表了什么含义？

### 2XX(Success 成功状态码)

2XX 响应的结果标明请求被正常处理了

#### 200 OK



![图片摘取自HTTP图解](https://img.showydream.com/img/soRTPw-16029efa6af2ea6c~tplv-t2oaga2asx-watermark.awebp)



> 表示从客户端发来的请求在服务器端被正常处理了

在响应报文内，随状态码一起返回的信息会因方法的不同而发生改变。比如，使用 GET 方法时，对应请求资源的实体会作为响应返回;而使用 HEAD 方法 时，对应请求资源的实体首部不随报文主体作为响应返回(即在响应中只返回首部，不会返回实体的主体部分)。

#### 204 No Content



![图片摘取自HTTP图解](https://img.showydream.com/img/Jf2qvI-16029efa697ec682~tplv-t2oaga2asx-watermark.awebp)



该状态码代表服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分。另外，也不允许返回任何实体的主体。比如，当从浏览器发出请求 处理后，返回 204 响应，那么浏览器显示的页面不发生更新。

> 一般在只需要从客户端往服务器发送信息，而对客户端不需要发送新信息内容的情况下使用

#### 206 Partial Content



![图片摘取自HTTP图解](https://img.showydream.com/img/Hkua7T-16029efb76d2b65f~tplv-t2oaga2asx-watermark.awebp)



> 该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的 GET 请求
>  响应报文中包含由 **Content-Range** 指定范围的实体内容

### 3XX(Redirection 重定向状态码)

3XX 响应结果表明浏览器需要执行某些特殊的处理以正确处理请求

#### 301 Moved Permanently



![图片摘取自HTTP图解](https://img.showydream.com/img/u9uGwU-16029efa6a4ba749~tplv-t2oaga2asx-watermark.awebp)



> 永久性重定向

该状态码表示请求的资源已被分配了新的 URI，以后应使用资源现在所指的 URI。也就是说，如果已经把资源对应的 URI 保存为书签了，这时应该按 Location 首部字段提示的 URI 重新保存

#### 302 Found



![图片摘取自HTTP图解](https://img.showydream.com/img/wwXNW3-16029efba78b2b7e~tplv-t2oaga2asx-watermark.awebp)



> 临时性重定向

该状态码表示请求的资源已被分配了新的 URI，希望用户(本次)能使用新的 URI 访问。 和 301 Moved Permanently 状态码相似，但 302 状态码代表的资源不是被永久移动，只是临时性质的。换句话说，已移动的资源对应的 URI 将来还有可能发生改 变。比如，用户把 URI 保存成书签，但不会像 301 状态码出现时那样去更新书签，而是仍旧保留返回 302 状态码的页面对应的 URI。

#### 303 See Other



![图片摘取自HTTP图解](https://img.showydream.com/img/kr6eVz-16029efbc1b5cfce~tplv-t2oaga2asx-watermark.awebp)



> 该状态码表示由于请求对应的资源存在着另一个 URI，应使用 GET 方法定向获取请求的资源。

303 状态码和 302 Found 状态码有着相同的功能，但 303 状态码明确表示客户端应当采用 GET 方法获取资源，这点与 302 状态码有区别。 比如，当使用 POST 方法访问 CGI 程序，其执行后的处理结果是希望客户端能以 GET 方法重定向到另一个 URI 上去时，返回 303 状态码。虽然 302 Found 状态码也可以实现相同的功能，但这里使用 303 状态码是最理想的

> 当 301、302、303 响应状态码返回时，几乎所有的浏览器都会把 POST 改成 GET，并删除请求报文内的主体，之后请求会自动再次发送
>  301、302 标准是禁止将 POST 方法改变成 GET 方法的，但实际使用时大家都会这么做

#### 304 Not Modified



![图片摘取自HTTP图解](https://img.showydream.com/img/Hl5964-16029efaa07f726e~tplv-t2oaga2asx-watermark.awebp)



该状态码表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但未满足条件的情况。304 状态码返回时，不包含任何响应的主体部分。304 虽 然被划分在 3XX 类别中，但是和重定向没有关系。

> 附带条件的请求是指采用 GET 方法的请求报文中包含 If-Match，If-Modified-Since，If-None-Match，If-Range，If-Unmodified-Since 中任一首部

#### 307 Temporary Redirect

临时重定向。该状态码与 302 Found 有着相同的含义。尽管 302 标准禁止 POST 变换成 GET，但实际使用时大家并不遵守

307 会遵照浏览器标准，不会从 POST 变成 GET。但是，对于处理响应时的行为，每种浏览器有可能出现不同的情况

### 4XX(Client Error 客户端错误状态码)

4XX 的响应结果表明客户端是发生错误的原因所在

#### 400 Bad Request



![图片摘取自HTTP图解](https://img.showydream.com/img/Q4HWzU-16029efaa064bdb6~tplv-t2oaga2asx-watermark.awebp)



该状态码表示请求报文中存在语法错误。当错误发生时，需修改请求的内容后再次发送请求。另外，浏览器会像 200 OK 一样对待该状态码。

#### 401 Unauthorized



![图片摘取自HTTP图解](https://img.showydream.com/img/64QWKf-16029efacc2e6a4c~tplv-t2oaga2asx-watermark.awebp)



该状态码表示发送的请求需要有通过 HTTP 认证(BASIC 认证、DIGEST 认证)的认证信息。另外若之前已进行过 1 次请求，则表示用 户认证失败

返回含有 401 的响应必须包含一个适用于被请求资源的 WWW-Authenticate 首部用以质询(challenge)用户信息。当浏览器初次接收到 401 响应，会弹出认证用的对话窗口

#### 403 Forbidden



![图片摘取自HTTP图解](https://img.showydream.com/img/2drMZR-16029efaa0cce655~tplv-t2oaga2asx-watermark.awebp)



该状态码表明对请求资源的访问被服务器拒绝了。服务器端没有必要给出拒绝的详细理由，但如果想作说明的话，可以在实体的主体部分对原因进行描述，这样就能让用户看到了

未获得文件系统的访问授权，访问权限出现某些问题(从未授权的发送源 IP 地址试图访问)等列举的情况都可能是发生 403 的原因

#### 404 Not Found



![图片摘取自HTTP图解](https://img.showydream.com/img/e5L4we-16029efac97cd782~tplv-t2oaga2asx-watermark.awebp)



该状态码表明服务器上无法找到请求的资源。除此之外，也可以在服务器端拒绝请求且不想说明理由时使用

#### 405 Method Not Allowed

该状态码标明，客户端请求的方法虽然能被服务器识别，但是服务器禁止使用该方法

> GET 和 HEAD 方法，服务器应该总是允许客户端进行访问

客户端可以通过 OPTIONS 方法来查看服务器允许的访问方法, 如下

```
Access-Control-Allow-Methods →GET,HEAD,PUT,PATCH,POST,DELETE
复制代码
```

### 5XX(Server Error 服务器错误状态码)

5XX 的响应结果表明服务器本身发生错误

#### 500 Internal Server Error



![图片摘取自HTTP图解](https://img.showydream.com/img/X3x3VD-16029efae2420839~tplv-t2oaga2asx-watermark.awebp)



该状态码表明服务器端在执行请求时发生了错误。也有可能是 Web 应用存在的 bug 或某些临时的故障

#### 502 Bad Gateway

该状态码表明扮演网关或代理角色的服务器，从上游服务器中接收到的响应是无效的

> 502 错误通常不是客户端能够修复的，而是需要由途径的 Web 服务器或者代理服务器对其进行修复

#### 503 Service Unavailable



![图片摘取自HTTP图解](https://img.showydream.com/img/Pjgq3P-16029efaf01b284a~tplv-t2oaga2asx-watermark.awebp)



该状态码表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。如果事先得知解除以上状况需要的时间，最好写入 RetryAfter 首部字段再返回 给客户端

> 状态码和状况的不一致
>  不少返回的状态码响应都是错误的，但是用户可能察觉不到这点。比如 Web 应用程序内部发生错误，状态码依然返回 200 OK，这种情况也经常遇到。


