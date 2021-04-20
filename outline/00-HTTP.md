<!--
 * @Author: your name
 * @Date: 2021-04-08 15:51:43
 * @LastEditTime: 2021-04-12 17:50:41
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /Interview Files/大纲/00-HTTP.md
-->

#### 1. HTTP与HTTPS的区别

1. https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

2. http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

3. http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

4. http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

HTTPS优点：

尽管HTTPS并非绝对安全，掌握根证书的机构、掌握加密算法的组织同样可以进行中间人形式的攻击，但HTTPS仍是现行架构下最安全的解决方案，主要有以下几个好处：

1. 使用HTTPS协议可认证用户和服务器，确保数据发送到正确的客户机和服务器；

2. HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，要比http协议安全，可防止数据在传输过程中不被窃取、改变，确保数据的完整性。

3. HTTPS是现行架构下最安全的解决方案，虽然不是绝对安全，但它大幅增加了中间人攻击的成本。

4. 谷歌曾在2014年8月份调整搜索引擎算法，并称“比起同等HTTP网站，采用HTTPS加密的网站在搜索结果中的排名将会更高”。

HTTPS缺点：

1. HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；

2. HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；

3. SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。

4. SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。

5. HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。

#### 2. 状态码

- | 类别 | 原因短语
---|---|---
1xx | Informational（信息性状态码）|接受的请求正在处理
2xx | Success（成功状态码） | 请求正常处理完毕
3xx | Redirection（重定向） | 需要进行附加操作以完成请求
4xx | Client error（客户端错误） | 客户端请求出错，服务器无法处理请求
5xx | Server Error（服务器错误）|服务器处理请求出错

#### 3. TCP为什么不能两次握手进行连接

两次握手只能保证单向连接是畅通的。

Step1 A => B : 你好，B。

Step2 A <= B : 收到。你好，A。

这样的两次握手过程， A 向 B 打招呼得到了回应，即 A 向 B 发送数据，B 是可以收到的。

但是 B 向 A 打招呼，A 还没有回应，B 没有收到 A 的反馈，无法确保 A 可以收到 B 发送的数据。

只有经过第三次握手，才能确保双向都可以接收到对方的发送的 数据。

Step3 A => B : 收到，B。

这样 B 才能确定 A 也可以收到 B 发送给 A 的数据。

#### 4. HTTP 简单请求是什么意思

1. 只能使用get/post/head请求方式
2. 不能手动设置以下集合之外的请求头信息
  - accept
  - accept-language
  - content-language
  - content-type
3. content-type只能设置以下内容：
  - text/plain
  - multipart/form-data
  - application/x-www-http-urlencoded
4. 不能为XMLHttpRequestUpdate注册监听器
5. 请求中没有使用readableStream对象

#### 5. TCP 和 UDP 的区别

##### TCP的优点：

可靠，稳定
TCP的可靠体现在TCP在传递数据之前，会有三次握手来建立连接，而且在数据传递时，有确认、窗口、重传、拥塞控制机制，在数据传完后，还会断开连接用来节约系统资源。

##### TCP的缺点：

慢，效率低，占用系统资源高，易被攻击
TCP在传递数据之前，要先建连接，这会消耗时间，而且在数据传递时，确认机制、重传机制、拥塞控制机制等都会消耗大量的时间，而且要在每台设备上维护所有的传输连接，事实上，每个连接都会占用系统的CPU、内存等硬件资源。
而且，因为TCP有确认机制、三次握手机制，这些也导致TCP容易被人利用，实现DOS、DDOS、CC等攻击。

##### UDP的优点：

快，比TCP稍安全
UDP没有TCP的握手、确认、窗口、重传、拥塞控制等机制，UDP是一个无状态的传输协议，所以它在传递数据时非常快。没有TCP的这些机制，UDP较TCP被攻击者利用的漏洞就要少一些。但UDP也是无法避免攻击的，比如：UDP Flood攻击……

##### UDP的缺点：

不可靠，不稳定
因为UDP没有TCP那些可靠的机制，在数据传递时，如果网络质量不好，就会很容易丢包。


##### 什么时候应该使用TCP：

当对网络通讯质量有要求的时候，比如：整个数据要准确无误的传递给对方，这往往用于一些要求可靠的应用，比如HTTP、HTTPS、FTP等传输文件的协议，POP、SMTP等邮件传输的协议。
在日常生活中，常见使用TCP协议的应用如下：

1. 浏览器，用的HTTP
2. FlashFXP，用的FTP
3. Outlook，用的POP、SMTP
4. Putty，用的Telnet、SSH
5. QQ文件传输
6. ...

##### 那么什么时候应该使用UDP：

当对网络通讯质量要求不高的时候，要求网络通讯速度能尽量的快，这时就可以使用UDP。
比如，日常生活中，常见使用UDP协议的应用如下：

1. QQ语音
2. QQ视频
3. TFTP
4. ...

#### 6. GET 和 POST 的区别

1. get是从服务器上获取数据，post是向服务器传送数据。

2. get是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。post是通过HTTPpost机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址。用户看不到这个过程。

3. 对于get方式，服务器端用Request.QueryString获取变量的值，对于post方式，服务器端用Request.Form获取提交的数据。

4. get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。但理论上，IIS4中最大量为80KB，IIS5中为100KB。（这里有看到其他文章介绍get和post的传送数据大小跟各个浏览器、操作系统以及服务器的限制有关）

5. get安全性非常低，post安全性较高。

标准答案：

- GET在浏览器回退时是无害的，而POST会再次提交请求。

- GET产生的URL地址可以被Bookmark，而POST不可以。

- GET请求会被浏览器主动cache，而POST不会，除非手动设置。

- GET请求只能进行url编码，而POST支持多种编码方式。

- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。

- GET请求在URL中传送的参数是有长度限制的，而POST么有。 

- 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。

- GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。

- GET参数通过URL传递，POST放在Request body中。

#### 7. 浏览器缓存策略

1. 缓存可以说是性能优化中简单高效的一种优化方式了。一个优秀的缓存策略可以缩短网页请求资源的距离，减少延迟，并且由于缓存文件可以重复利用，还可以减少带宽，降低网络负荷。

2. 浏览器优先会把一些小文件放到浏览器Memory中存储，大文件直接放到disk中，内存占用不多的时候，会优先放Memory中，但到底哪些资源放内存，哪些放disk，由Browse自己决定。

3. 强缓存：不会向服务器发送请求，直接从缓存中读取资源，在chrome控制台的Network选项中可以看到该请求返回200的状态码，并且Size显示from disk cache或from memory cache。强缓存可以通过设置两种 HTTP Header 实现：Expires 和 Cache-Control。

http1.0中的响应头里会有Expires和Last-Modified 字段，expires告诉Browse这个时间之前不向服务器发新请求。若过期再发请求，这时请求头会有if-modified-since头。

缺点：它受限于本地时间，如果修改了本地时间，可能会造成缓存失效。

- 如果本地打开缓存文件，即使没有对文件进行修改，但还是会造成 Last-Modified 被修改，服务端不能命中缓存导致发送相同的资源
- 因为 Last-Modified 只能以秒计时，如果在不可感知的时间内修改完成文件，那么服务端会认为资源还是命中了，不会返回正确的资源

浏览器的缓存规则是在 http 协议头和 html 页面的 meta 标签中定义的。主要分为两部分：强缓存和协商缓存。

强缓存是指缓存的副本在有效期内，浏览器直接获取这个副本并渲染。

强缓存主要涉及的 http 协议报头有：Expires(到期时间)(http1.0规则下的响应头)，Cache-Control(缓存控制)(http1.1规则下的响应头)。

不被缓存的请求：

- 包含cache-control：no-cache，pragma：no-cache 或者 cache-control：max-age=0等。
- 需要根据cookie，认证信息等决定输入内容的动态请求是不能被缓存的。
- post 请求。

基于缓存策略：

- 同一个url保证稳定性。
- 给 css 、js 、图片等资源增加 HTTP 缓存头（对于不常修改的静态资源，设置一个较长的时间），入口 html 不建议设置缓存。
- 减少对 cookie 的依赖，每次 get 和 post 请求，都带上 cookie 增加网络传输流量，导致增长交互时间，同时cache 是很难缓存的。

#### 8. 浏览器内核

IE=Trident

Firefox=Gecko

Safari=webkit

Chrome=Blink

Opera=Blink

Edge=Chromium

#### 9. 常见Http方法有哪些？使用场景分别是什么？

1. GET
2. HEAD
3. PUT
4. DELETE
5. POST
6. OPTIONS

参考:https://www.cnblogs.com/susanhonly/p/8508596.html

#### 10. 在HTML的form 标签里，method支持哪些类型？

get和post

#### 11. web安全领域常见的攻击方式

1. SQL注入攻击
2. 跨站脚本攻击 - XSS
3. 跨站伪造请求攻击 - CSRF
4. 文件上传漏洞攻击
5. 分布式拒绝服务攻击 - DDOS

#### 12. 一次完整的HTTP请求所经历的7个步骤

![avatar](https://img-blog.csdnimg.cn/20200713091626381.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDQ3ODM3OA==,size_16,color_FFFFFF,t_70)

HTTP通信机制是在一次完整的HTTP通信过程中，Web浏览器与Web服务器之间将完成下列7个步骤：

1. 建立TCP连接
在HTTP工作开始之前，Web浏览器首先要通过网络与Web服务器建立连接，该连接是通过TCP来完成的，该协议与IP协议共同构建Internet，即著名的TCP/IP协议族，因此Internet又被称作是TCP/IP网络。HTTP是比TCP更高层次的应用层协议，根据规则，只有低层协议建立之后才能进行更高层协议的连接，因此，首先要建立TCP连接，一般TCP连接的端口号是80。

2. Web浏览器向Web服务器发送请求命令
一旦建立了TCP连接，Web浏览器就会向Web服务器发送请求命令。例如：GET/sample/hello.jsp HTTP/1.1。

3. Web浏览器发送请求头信息
浏览器发送其请求命令之后，还要以头信息的形式向Web服务器发送一些别的信息，之后浏览器发送了一空白行来通知服务器，它已经结束了该头信息的发送。

4. Web服务器应答
客户机向服务器发出请求后，服务器会客户机回送应答， HTTP/1.1 200 OK ，应答的第一部分是协议的版本号和应答状态码。

5. Web服务器发送应答头信息
正如客户端会随同请求发送关于自身的信息一样，服务器也会随同应答向用户发送关于它自己的数据及被请求的文档。

6. Web服务器向浏览器发送数据
Web服务器向浏览器发送头信息后，它会发送一个空白行来表示头信息的发送到此为结束，接着，它就以Content-Type应答头信息所描述的格式发送用户所请求的实际数据。

7. Web服务器关闭TCP连接
一般情况下，一旦Web服务器向浏览器发送了请求数据，它就要关闭TCP连接，然后如果浏览器或者服务器在其头信息加入了这行代码：Connection:keep-alive

TCP连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求建立新连接所需的时间，还节约了网络带宽

参考:https://blog.csdn.net/weixin_44478378/article/details/107309754

#### 13. websocket和http的区别:

![avatar](https://pic3.zhimg.com/80/v2-7473ab83669c31a09c2b2814c7f48fca_1440w.jpg)

参考:https://zhuanlan.zhihu.com/p/113286469

#### 14. 前后端常见的几种鉴权方式

1. HTTP Basic Authentication
2. session-cookie
3. Token 验证
4. OAuth(开放授权)

参考:https://blog.csdn.net/wang839305939/article/details/78713124/

#### 15. 浏览器缓存是如何控制的

![avatar](https://upload-images.jianshu.io/upload_images/3685006-eb43e1669d46f0ea.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/743/format/webp)

浏览器在请求已经访问过的URL时，会判断是否使用缓存，判断是否是由缓存的主要依据是缓存是否在有效期内，如果在有效期内，则会直接使用缓存(如上图中的情况，注意status code)这一部分主要通过response header中的两个字段来判断：

- Expires： 表示有效期，是一个GMT时间，以客户端为基准，与服务器时间可能存在一定时间差
- Cache-Control中的max-age值： 表示最大有效时间，单位是s，优先级比expires高，为了解决expires时间差的问题而出现的. 当超过缓存期时，浏览器不会直接请求资源，而是判断缓存是否有更新，能否继续使用，判断方法有两种：

Last-Modified和 if-Modified-Since：当浏览器第一次请求某个资源时，服务器的响应中有一个Last-Modified字段，表示最近一个修改缓存的时间，当缓存过期后，浏览器就会把这个时间放在请求的If-Modified-Since字段中并发送给服务器，由服务器来判断缓存是否有更新

Etag和If-Node-Match：当浏览器第一次请求某个资源时，服务器的响应中有一个ETag字段，是用于表示文件的字符串，一旦文件更新，该字段就会发生变化；当缓存过期后，浏览器会把该字段的内容放在请求的If-None-Match字段中，并发送给服务器，由服务器来判断缓存是否有更新。

注意：Etag比Last-Modified的更高（因为Etag更加准确，而Last-Modified只能精确到秒）

参考:https://www.jianshu.com/p/1b276233689e



























































