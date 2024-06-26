# 概览

「从浏览器地址栏输入网址，到网页彻底打开，中间都发生了什么？」

为了让文章更有趣味性，我们先思考这样一个问题，**「从进入餐厅落座后拿到菜单点完菜，到用餐结束离开餐厅，中间都发生了什么？」**

读完[一则经典技术面试题目的解读](http://mp.weixin.qq.com/s?__biz=MzI0MjA1Mjg2Ng==&amp;mid=209679438&amp;idx=1&amp;sn=d68c1512ad23f6e164f69bd351a18c62&amp;) （下文统一称为**解读**），我们应该有了一个大概的认知，即上网这个过程分为三个大的区块，一块是客户端，一块是网络传输层，一块是服务端。

这种先总后分的思路有助于我们更好地理解整个过程。就拿学习开车来说，我心中的好教练会告诉学员：「不要想太多，一个方向盘，四个轮子，有啥好怕的」。对一个学车的新手来说，上来就研究发动机的运行原理，大概率不会对掌握开车技能有实际的帮助。

那我们先确保理解这三个抽象的概念：

- 客户端
- 网络传输层
- 服务端

在前面「用餐」的例子里，**客户端**即「消费者」，**传输层**的角色由「服务员」扮演，而**服务端**则是「厨师」。

## 概念：名词解释

### 客户端

在当前的语境下，我们首先想到的应该是「浏览器」。我们知道，浏览器有很多种，比较知名的有 **谷歌的 Chrome、Mozilla Firefox、苹果的 Safari、微软的 Microsoft Edge** 等。

当然，我们知道浏览器是作为一个应用程序运行在操作系统上的，所以我们在理解「客户端」这个概念的时候，还要考虑到诸如 macOS/iOS/iPadOS、Linux/Android、Windows 等操作系统。再举个例子，当我们在微信里打开一个链接的时候，微信就是一个客户端。

除了上面这些具有现代化好看的用户界面的客户端，我们还应该了解诸如 `wget`, `curl` 一类的命令行工具，也可以扮演客户端的角色，这里先放一个示例：

```console
% curl -vvv http://localhost
* Host localhost:80 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:80...
* connect to ::1 port 80 from ::1 port 55294 failed: Connection refused
*   Trying 127.0.0.1:80...
* Connected to localhost (127.0.0.1) port 80
> GET / HTTP/1.1
> Host: localhost
> User-Agent: curl/8.7.1
> Accept: */*
>
* Request completely sent off
< HTTP/1.1 200 OK
< Server: nginx/1.25.4
< Date: Tue, 16 Apr 2024 05:25:47 GMT
< Content-Type: text/html
< Content-Length: 615
< Last-Modified: Wed, 14 Feb 2024 16:03:35 GMT
< Connection: keep-alive
< ETag: "65cce457-267"
< Accept-Ranges: bytes
<
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
* Connection #0 to host localhost left intact
```

简单解释一下，上面的命令是使用 `curl` 这个「客户端」去请求 `http://localhost` 这个「服务端」，我使用 `-vvv` 参数，把这个请求过程的详细信息打印了出来，关于请求过程，我们后续再讨论，目前先有个印象即可。

现在我们先认为客户端就是一个普通的浏览器，为了方便后续的抓包演示，如果不做特别说明，本书将默认「客户端」是「运行在 macOS 上的 Chrome 浏览器」。

### 网络传输层

提到「网络传输层」，大家肯定会想到 `TCP/IP` 协议。是的，传输层主要是 `TCP` 和 `UDP`，这绝对是一个大话题。

但在当前语境下，这个「网络传输层」可能并不局限于对 `OCI` 的七层模型、`TCP/IP` 的四层/五层模型 等模型的理解，我们还需要了解 `DNS`、`HTTP/HTTPS`、`CDN` 甚至 `QUIC` 等**非传输层**的协议。

同样地，这些概念太多了，读者大不必为此挠头，我们先混个眼熟，后面遇到了再详细展开。


### 服务端

这可能是三个概念里最容易理解的，提供服务的嘛。也就是我们常说的 `Web` 服务器。

## 对[解读](http://mp.weixin.qq.com/s?__biz=MzI0MjA1Mjg2Ng==&amp;mid=209679438&amp;idx=1&amp;sn=d68c1512ad23f6e164f69bd351a18c62&amp;)的解读

开头 caoz 老师的文章已经总结得很好了，起码对于一个非技术从业者来说，在思考「从浏览器地址栏输入网址，到网页彻底打开，中间都发生了什么？」这样一个问题时，我们知道可以从 **客户端、网络传输、服务端** 三块来分析。其实这正是一个典型的计算机程序的执行过程：**输入、处理、输出**。这里的**输入**可以是我们在浏览器里敲好网址按下回车，也可以是找到浏览器收藏夹里保存好的网页后，轻轻地点击鼠标；**输出**则是浏览器最终为我们展现的页面。而重点就在于中间的**处理**的过程。

1. 浏览器的地址栏先解析你的输入的网址。有的读者可能会留意到，有时候我手动输入一个网址，但回车后浏览器却打开了搜索引擎给出的结果而不是目标网站。这正是因为浏览器在解析你的输入。这种情况下，极有可能是你输入的地址不合法，可能是里面没有 `http://` 这样的开头，也可能是地址里混入了全角标点等。

1. 网址被浏览器判定为合法之后，流程就来到了域名的解析。一般来说，域名解析要经过浏览器的缓存、系统的缓存、系统的 `hosts` 文件、本地 DNS 服务器、远程的 DNS 服务器等。[^dns]

[^dns]: [浅谈从输入 URL 到页面展现发生了什么 #17](https://github.com/yd160513/blog/issues/17)

1. 解析完成后，浏览器作为**客户端**就开始了正式向**服务端**发起请求，索要数据。中间不可避免地需要**网络传输层**的大力支持。

1. **服务端**收到请求，给予响应，将数据传回给**客户端**；收到数据的客户端把结果在**浏览器**进行渲染，呈现给几秒前按下回车键的你。

上面的解释并不完全正确，至少读者应该保持一个质疑的态度，因为我们还有很多疑惑:

1. 为什么域名解析的顺序是你讲的这样，依据是什么？
1. [解读](http://mp.weixin.qq.com/s?__biz=MzI0MjA1Mjg2Ng==&amp;mid=209679438&amp;idx=1&amp;sn=d68c1512ad23f6e164f69bd351a18c62&amp;) 中提到的 DNS 劫持是怎么实现的？
1. CDN 的原理和机制，它到底是怎么做到针对不同地区用户智能解析的？
1. 浏览器是怎么渲染的？
1. 访问一个网页往往在几秒之内就有结果了，这么神奇的吗？
1. 但有时候网页卡很久才出结果，这又是怎么回事呢？

看起来这会是一次奇妙的旅程，接下来我们会有理有据地揭开这个谜团。请读者自行尝试回答以上问题，以及你想到的但没有列出在这里的疑问，我们下一章再见。
