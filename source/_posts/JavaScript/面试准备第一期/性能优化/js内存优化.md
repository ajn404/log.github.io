---
title: 前端
---

# 性能优化	*[参考](https://segmentfault.com/a/1190000022205291)*

##  1.减少http请求

**一个完整的http请求：域名解析，与服务器建立连接（三次握手），发起http请求，服务器相应http请求浏览器得到html代码，浏览器解析html并请求资源，浏览器进行页面的渲染，断开连接（四次挥手）**

1. 域名解析

   搜索浏览器自身的DNS缓存，搜索操作系统自身的DNS缓存，Windows系统尝试读取host文件，发起dns系统调用

2. 与服务器建立连接

   TCP三次握手四次挥手

   TCP连接的基础：

   - TCP连接的端点叫套接字或者插口，套接字定义为端口号拼接到ip地址
   - 每条TCP连接唯一地被通信两端的两个端点（即两个套接字）所确定
   - 同一个名词Socket可以表示多种不同的意思
   - 停止等待协议
   - 滑动窗口协议
   - TCP拥塞控制：慢开始与拥塞避免，快重传与快恢复
   - TCP的主要特点是：
     - 面向连接
     - 每条TCP连接只能是点对点，一对一的
     - 提供可靠交付的服务
     - 提供全双工通信
     - 面向字节流
   - TCP连接建立，服务器确认客户的连接请求后，客户要对服务器的确认再进行确认
   - TCP连接释放，任何一方都可以在数据传送结束后发出连接释放通知，待对方确认后进入半关闭状态，当另一方也没有数据再发送时，则发送连接释放通知，对方确认后关闭TCP连接

3. 发起http请求

   8中请求方法：GET，POST，PUT，DELETE，PATCH，HEAD，OPTIONS，TRACE

   RESTful api常用GET POST DELETE　PUT

   默认端口号

4. 服务器响应http请求，浏览器获得html代码

   服务器负载均衡：位于网站的最前端，把短时间较高的访问量分摊到不同机器上处理，方案分为硬件和软件

   状态码：

   | 1**     | 服务器收到请求，需要请求者继续执行操作                       |
   | ------- | ------------------------------------------------------------ |
   | 100     | continue：继续，客户端应继续其请求                           |
   | 101     | Switching Protocols：切换协议，服务器根据客户端的请求切换协议，只能切换到更高级的协议 |
   |         |                                                              |
   | **2**** | **成功，操作被成功接收并处理**                               |
   | 200     | OK：请求成功，一般用于GET或者POST请求                        |
   | 201     | Created：成功请求并创建了新的资源                            |
   | 202     | Accepted：已接收，已接收请求，但未处理完成                   |
   | 203     | 非授权信息                                                   |
   | 204     | 无内容                                                       |
   | 205     | 重置内容                                                     |
   | **3**** | **重定向，需要进一步的操作以完成请求**                       |
   | 300     | 多种选择                                                     |
   | 301     | 永久移动                                                     |
   | 302     | 临时移动                                                     |
   | 303     | 查看其它地址                                                 |
   | 304     | Not Modified：未修改：所请求的资源未修改，服务器返回此状态码时，不会返回任何资源，客户端通常会缓存所访问过的资源，通过提供一个头信息指出客户端希望只返回指定日期之后修改的资源<br>如果客户端返回一个带条件的GET请求且该请求已被允许，而文档的内容并没有改变，则服务器应当返回这个状态码。304响应禁止包含消息体，因此始终以消息头后的第一个空行结尾 |
   | 305     | Use Proxy:使用代理，所请求的资源必须通过代理访问             |
   | 306     | Unused                                                       |
   | 307     | 临时重定向                                                   |
   | **4**** | **客户端错误，请求包含语法错误**                             |
   | 400     | Bad Request                                                  |
   | 401     | Unauthorized，请求要求用户的身份认证                         |
   | 402     |                                                              |
   | 403     | Forbidden：服务器理解请求客户端的请求，但是决绝执行此请求    |
   | 404     | Not Found                                                    |
   | 405     | 请求中的方法被禁止                                           |
   | 406     | 服务器无法根据请求的内容特性完成请求                         |
   | 407     | 请求要求代理的身份认证                                       |
   | 408     | 服务器等待                                                   |
   | 5**     |                                                              |
   | 500     | 服务器遇到了一个未曾预料到的状况                             |
   | 501     |                                                              |
   | 502     | 作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应 |
   | 503     |                                                              |
   | 504     |                                                              |
   | 509     | 服务器达到带宽限制                                           |
   | 510     | 获取资源所需要的策略并没有被满足                             |
|         |                                                              |
   
   一个http请求实例
   
   {% asset_img https://segmentfault.com/img/remote/1460000037788872  存储流程 %}

其中：

- Queueing：在请求队列中的时间
- Stalled: 从TCP 连接建立完成，到真正可以传输数据之间的时间差，此时间包括代理协商时间。
- Proxy negotiation: 与代理服务器连接进行协商所花费的时间。
- DNS Lookup: 执行DNS查找所花费的时间，页面上的每个不同的域都需要进行DNS查找。
- Initial Connection / Connecting: 建立连接所花费的时间，包括TCP握手/重试和协商SSL。
- SSL: 完成SSL握手所花费的时间。
- Request sent: 发出网络请求所花费的时间，通常为一毫秒的时间。
- Waiting(TFFB): TFFB 是发出页面请求到接收到应答数据第一个字节的时间。
- Content Download: 接收响应数据所花费的时间。

建议将多个小文件合并成为一个大文件，从而减小http请求的次数

## 2.使用HTTP2

#### http1.0与http1.1的区别

- 缓存处理：http1.1引入了更多的缓存控制策略
- 带宽优化以及网络连接的使用，http1.1在请求头引入了range头域，它允许只请求资源的某个部分，即返回码206（Partial Content）
- 错误通知管理：http1.1新增了24个错误状态响应码
- Host头处理：http1.1的请求消息和响应消息都支持Host头域，且请求消息中如果没有Host头域会报400
- 长连接：HTTP 1.1支持长连接（PersistentConnection）和请求的流水线（Pipelining）处理，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟，在HTTP1.1中默认开启Connection： keep-alive，一定程度上弥补了HTTP1.0每次请求都要创建连接的缺点。

#### https与http

- https需要到CA申请证书
- http协议运行在tcp之上，所有传输的内容都是明文，https运行在ssl/tls之上，而ssl/tls运行在tcp之上，所有的传输内容都经过加密的
- http与https使用完全不同的连接方式，用的端口也不一样，http是80，https是443
- https可以有效地防止运营商劫持（dns,ip路由,cdn,http,portal)

#### SPDY

### http2.0

- 解析速度快
- 多路复用：多个请求同时共用一个tcp连接
- 首部压缩
- 优先级：http2.0可以对比较紧急的情况设置一个较高的优先级，服务器收到这样的请求后，可以优先处理
- 流量控制
- 服务器推送：除了对最初请求的响应外，服务器还可以额外向客户端推送资源，而无需客户端明确地请求。

## 3.使用服务端渲染

服务端渲染是指服务端返回HTML文件，客户端只需解析HTML

优点在于：首屏渲染快，SEO好

缺点在于：配置复杂，增加服务器的计算压力

##### [veu-ssr实践（初识NUXTJS）](https://github.com/ajn404/vue-ssr.git)

## 4.静态资源使用CDN

CDN（内容分发网络）是一组分布在多个不同地理位置的Web服务器

我们都知道，当服务器离用户越远时，延迟越高。CDN 就是为了解决这一问题，在多个位置部署服务器，让用户离服务器更近，从而缩短请求时间。

用户访问的网站部署了 CDN，过程是这样的：

1. 浏览器要将域名解析为 IP 地址，所以需要向本地 DNS 发出请求。
2. 本地 DNS 依次向根服务器、顶级域名服务器、权限服务器发出请求，得到全局负载均衡系统（GSLB）的 IP 地址。
3. 本地 DNS 再向 GSLB 发出请求，GSLB 的主要功能是根据本地 DNS 的 IP 地址判断用户的位置，筛选出距离用户较近的本地负载均衡系统（SLB），并将该 SLB 的 IP 地址作为结果返回给本地 DNS。
4. 本地 DNS 将 SLB 的 IP 地址发回给浏览器，浏览器向 SLB 发出请求。
5. SLB 根据浏览器请求的资源和地址，选出最优的缓存服务器发回给浏览器。
6. 浏览器再根据 SLB 发回的地址重定向到缓存服务器。
7. 如果缓存服务器有浏览器需要的资源，就将资源发回给浏览器。如果没有，就向源服务器请求资源，再发给浏览器并缓存在本地。

**在我看来，CDN的本质是缓存，而内核中支撑它的互联网精神则是共享**

#### 关于http缓存和浏览器缓存		*[参考](https://segmentfault.com/a/1190000021661656#:~:text=%E6%B5%8F%E8%A7%88%E5%99%A8%E7%BC%93%E5%AD%98%E5%88%86%E4%B8%BA,%E4%B9%8B%E9%97%B4%E5%AD%98%E5%9C%A8%E4%B8%80%E6%AC%A1%E9%80%9A%E4%BF%A1%E3%80%82)*

- http缓存
  - 缓存位置：Service Worker,Memory Cache,Disk Cache,Push Cache
  - 用户行为影响：
    - 地址栏输入：disk cache,没有匹配则发送网络请求
    - 普通刷新：优先使用memory cache,其次是disk cache
    - 强制刷新：浏览器不使用缓存
- 浏览器缓存
  - 强缓存
  - 协商缓存
  - 区别
    - 如果浏览器命中强缓存，则不需要给服务器发送请求；协商缓存最终由服务器决定是否使用缓存，客户端与服务器之间存在一次通信
    - 在 `chrome` 中强缓存（虽然没有发出真实的 `http` 请求）的请求状态码返回是 `200 (from cache)`；而协商缓存如果命中走缓存的话，请求的状态码是 `304 (not modified)`。 不同浏览器的策略不同，在 `Fire Fox`中，`from cache` 状态码是 304.

## 5.将css放在文件头部，JavaScript放在底部

## 6.使用字体图标iconfont代替图片图表

## 7.webpack善用文件缓存，不重复加载相同的资源

## 8.webpack插件压缩文件

## 9.图片优化

### 延迟加载（懒加载）

首先将页面上的图片的 src 属性设为 loading.gif，而图片的真实路径则设置在 data-src 属性中，页面滚动的时候计算图片的位置与滚动的位置，当图片出现在浏览器视口内时，将图片的 src 属性设置为 data-src 的值，这样，就可以实现延迟加载。

实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lazyload 1</title>
    <style>
        img {
	    display: block;
	    margin-bottom: 50px;
	    height: 200px;
	}
    </style>
</head>
<body>
    <img src="images/loading.gif" data-src="images/1.png">
    <img src="images/loading.gif" data-src="images/2.png">
    <img src="images/loading.gif" data-src="images/3.png">
    <img src="images/loading.gif" data-src="images/4.png">
    <img src="images/loading.gif" data-src="images/5.png">
    <img src="images/loading.gif" data-src="images/6.png">
    <img src="images/loading.gif" data-src="images/7.png">
    <img src="images/loading.gif" data-src="images/8.png">
    <img src="images/loading.gif" data-src="images/9.png">
    <img src="images/loading.gif" data-src="images/10.png">
    <img src="images/loading.gif" data-src="images/11.png">
    <img src="images/loading.gif" data-src="images/12.png">
    <script>
        function lazyload() {
	    var images = document.getElementsByTagName('img');
	    var len    = images.length;
	    var n      = 0;      //存储图片加载到的位置，避免每次都从第一张图片开始遍历		
	    return function() {
		var seeHeight = document.documentElement.clientHeight;
		var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
		for(var i = n; i < len; i++) {
		    if(images[i].offsetTop < seeHeight + scrollTop) {
		        if(images[i].getAttribute('src') === 'images/loading.gif') {
			     images[i].src = images[i].getAttribute('data-src');
			}
			n = n + 1;
		     }
		}
	    }
	}
	var loadImages = lazyload();
	loadImages();          //初始化首页的页面图片
	window.addEventListener('scroll', loadImages, false);
    </script>
</body>
</html>
```

### 响应式图片

浏览器根据屏幕的大小自动加载合适的图片

### 调整图片大小

### 降低图片质量

### 使用CSS３效果

### 使用webp

## 10.通过 webpack 按需加载代码，提取第三库代码，减少 ES6 转为 ES5 的冗余代码

## 11.减少重绘重排