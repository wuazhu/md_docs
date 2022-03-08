HTTP 不是一个孤立的协议。

俗话说“一个好汉三个帮”，HTTP 也是如此。在互联网世界里，HTTP 通常跑在 TCP/IP 协议栈之上，依靠 IP 协议实现寻址和路由、TCP 协议实现可靠数据传输、DNS 协议实现域名查找、SSL/TLS 协议实现安全通信。此外，还有一些协议依赖于 HTTP，例如 WebSocket、HTTPDNS 等。这些协议相互交织，构成了一个协议网，而 HTTP 则处于中心地位。

- http2(SPDY-chrome)

  1. 二进制协议不再是纯文本
  2. 可发起多个请求
  3. 使用专用算法压缩头部, 减少数据传输量
  4. 允许服务器主动向客户端推送数据
  5. 增强安全性,"事实上"要求加密通信

- http3(QUIC-chrome)

  

  

#### DNS

“域名系统”（Domain Name System）在 DNS 中，“域名”（Domain Name）又称为“主机名”（Host），级别从左到右逐级升高，最右边的被称为“顶级域名”.但想要使用 TCP/IP 协议来通信仍然要使用 IP 地址，所以需要把域名做一个转换，“映射”到它的真实 IP，这就是所谓的“域名解析”。

#### SSL/TLS

Secure Socket Layer 发展到 3.0 时被标准化改名为 TLS Transport Layer Security 因为历史原因还是有很多人简称为 SSL

#### 代理

中转站,即可转发客户端也可以转发服务端的应答

1. 匿名代理, 外界看到的知识代理服务器
2. 透明代理: 外界即知道代理也知道客户端
3. 正向代理: 靠近客户端, 代表客户端向服务器发送请求
4. 反向代理: 靠近服务端, 代表服务器响应客户端的请求

CDN 就是透明代理+反向代理的角色, 由于加入代理这个中间层就可以做很多的事情:

1. 负载均衡
2. 内容缓存: 暂存上下行数据,减轻后端的压力
3. 安全防护: 隐匿 IP 使用 WAF 等工具抵御攻击, 保护被代理的机器
4. 数据处理: 提供压缩加密等而外的功能

#### TCP/IP 网络分层模型

![img](https://static001.geekbang.org/resource/image/2b/03/2b8fee82b58cc8da88c74a33f2146703.png)

第一层: **连接层** 负责在以太网、WiFi 这样的底层网络上发送原始数据包，工作在网卡这个层次，使用 MAC 地址来标记网络上的设备，所以有时候也叫 MAC 层。

第二层: **网际层** 或者 **网络互连层**（internet layer），IP 协议就处在这一层。因为 IP 协议定义了“IP 地址”的概念，所以就可以在“链接层”的基础上，用 IP 地址取代 MAC 地址，把许许多多的局域网、广域网连接成一个虚拟的巨大网络，在这个网络里找设备时只要把 IP 地址再“翻译”成 MAC 地址就可以了。

第三层: **传输层**（transport layer），这个层次协议的职责是保证数据在 IP 地址标记的两点之间“可靠”地传输，是 TCP 协议工作的层次，另外还有它的一个“小伙伴”UDP。

第四层: **应用层** 由于下面的三层把基础打得非常好，所以在这一层就“百花齐放”了，有各种面向具体应用的协议。例如 Telnet、SSH、FTP、SMTP 等等，当然还有我们的 HTTP。

MAC 层的传输单位是帧（frame），IP 层的传输单位是包（packet），TCP 层的传输单位是段（segment），HTTP 的传输单位则是消息或报文（message）。但这些名词并没有什么本质的区分，可以统称为数据包。



#### OSI 网络分层模型

![img](https://static001.geekbang.org/resource/image/3a/dc/3abcf1462621ff86758a8d9571c07cdc.png)



第一层:物理层，网络的物理形式，例如电缆、光纤、网卡、集线器等等

第二层：数据链路层，它基本相当于 TCP/IP 的链接层；

第三层：网络层，相当于 TCP/IP 里的网际层；

第四层：传输层，相当于 TCP/IP 里的传输层；

第五层：会话层，维护网络中的连接状态，即保持会话和同步；

第六层：表示层，把数据转换为合适、可理解的语法和语义；

第七层：应用层，面向具体的应用传输数据。



如上就是我们常说的四层跟七层结构



#### 请求头, 响应头

由空行将请求头与 body 区分开

host 是请求头里唯一的必须字段,其他字段都非必须



#### HTTP 状态码

1xx: 状态码属于提示信息，是协议处理的中间状态，实际能够用到的时候很少。

2xx: 成功状态, 204 成功,但是没有 body 信息, 206分块下载, 通常还有头字段Content-Range:比如 bytes 0-99/2000 意思请求到了 2000bytes 中的第 0-99 这个 chunk

3xx: 资源重定向, 通常会在响应头字段里用 Location 指明后续要跳转的 URI, 301 永久重定向, 302临时重定向, 304用于 if-modified-since 等条件请求, 表示资源未修改, 用于控制缓存

4xx: 客户端请求报文有误, 服务器无法处理了

![image-20220307224312395](https://imgs.wuazhu.cn/2022/0307/m1Lx1I_bNWo2f.png)

5xx: 表示客户端请求报文正确, 但服务端处理时内部发生了错误, 无法返回响应数据



#### body 的数据类型与编码

- MIME type

最早是用在邮件系统里的, http 取了其中一部分来标记 body的数据类型.形式是“type/subtype”的字符串

1. text：即文本格式的可读数据，我们最熟悉的应该就是 text/html 了，表示超文本文档，此外还有纯文本 text/plain、样式表 text/css 等。
2. image：即图像文件，有 image/gif、image/jpeg、image/png 等。
3. audio/video：音频和视频数据，例如 audio/mpeg、video/mp4 等。
4. application：数据格式不固定，可能是文本也可能是二进制，必须由上层应用程序来解释。常见的有 application/json，application/javascript、application/pdf 等，另外，如果实在是不知道数据是什么类型，像刚才说的“黑盒”，就会是 application/octet-stream，即不透明的二进制数据。

- 编码格式“Encoding type”

  1. gzip：GNU zip 压缩格式，也是互联网上最流行的压缩格式；
  2. deflate：zlib（deflate）压缩格式，流行程度仅次于 gzip；
  3. br：一种专门为 HTTP 优化的新压缩算法（Brotli）。

- 客户端用 Accept告诉服务器希望接收什么样的数据而服务器用 Content头告诉客户端 实际发送了什么样的数据. Accept字段标记的是客户端可理解的 MIME type，可以用“,”做分隔符列出多个类型，让服务器有更多的选择余地，例如下面的这个头：

  ```
  Accept: text/html,application/xml,image/webp,image/png
  
  // 服务器返回
  Content-Type: text/html
  Content-Type: image/png
  ```

- Accept-Encoding 字段标记的是客户端支持的压缩格式

  ```
  Accept-Encoding: gzip, deflate, br
  // 服务端返回
  Content-Encoding: gzip
  ```

- Accept-Language 字段标记了客户端可理解的自然语言

  ```
  Accept-Language: zh-CN, zh, en
  ```

- 字符集在 HTTP 里使用的请求头字段是 Accept-Charset，但响应头里却没有对应的 Content-Charset，而是在 Content-Type 字段的数据类型后面用“charset=xxx”来表示，这点需要特别注意。

  ```
  Accept-Charset: gbk, utf-8
  Content-Type: text/html; charset=utf-8
  ```

  

