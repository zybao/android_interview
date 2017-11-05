### 一、 HTTP请求和响应步骤

![](http://upload-images.jianshu.io/upload_images/3985563-ef43bfa84bb68de1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上完整表示了HTTP请求和响应的7个步骤，下面从TCP/IP协议模型的角度来理解HTTP请求和响应如何传递的。

### 二、TCP/IP协议

TCP/IP协议模型（Transmission Control Protocol/Internet Protocol），包含了一系列构成互联网基础的网络协议，是Internet的核心协议，通过20多年的发展已日渐成熟，并被广泛应用于局域网和广域网中，目前已成为事实上的国际标准。TCP/IP协议簇是一组不同层次上的多个协议的组合，通常被认为是一个四层协议系统，与OSI的七层模型相对应。

HTTP协议就是基于TCP/IP协议模型来传输信息的。  


![](http://upload-images.jianshu.io/upload_images/3985563-e533b0cdd7fca359.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
\(1\). 链路层

也称作数据链路层或网络接口层（在第一个图中为网络接口层和硬件层），通常包括操作系统中的设备驱动程序和计算机中对应的网络接口卡。它们一起处理与电缆（或其他任何传输媒介）的物理接口细节。ARP（地址解析协议）和RARP（逆地址解析协议）是某些网络接口（如以太网和令牌环网）使用的特殊协议，用来转换IP层和网络接口层使用的地址。

\(2\). 网络层

也称作互联网层（在第一个图中为网际层），处理分组在网络中的活动，例如分组的选路。在TCP/IP协议族中，网络层协议包括IP协议（网际协议），ICMP协议（Internet互联网控制报文协议），以及IGMP协议（Internet组管理协议）。

IP是一种网络层协议，提供的是一种不可靠的服务，它只是尽可能快地把分组从源结点送到目的结点，但是并不提供任何可靠性保证。同时被TCP和UDP使用。TCP和UDP的每组数据都通过端系统和每个中间路由器中的IP层在互联网中进行传输。

ICMP是IP协议的附属协议。IP层用它来与其他主机或路由器交换错误报文和其他重要信息。

IGMP是Internet组管理协议。它用来把一个UDP数据报多播到多个主机。

\(3\). 传输层

主要为两台主机上的应用程序提供端到端的通信。在TCP/IP协议族中，有两个互不相同的传输协议：TCP（传输控制协议）和UDP（用户数据报协议）。

TCP为两台主机提供高可靠性的数据通信。它所做的工作包括把应用程序交给它的数据分成合适的小块交给下面的网络层，确认接收到的分组，设置发送最后确认分组的超时时钟等。由于运输层提供了高可靠性的端到端的通信，因此应用层可以忽略所有这些细节。为了提供可靠的服务，TCP采用了超时重传、发送和接收端到端的确认分组等机制。

UDP则为应用层提供一种非常简单的服务。它只是把称作数据报的分组从一台主机发送到另一台主机，但并不保证该数据报能到达另一端。一个数据报是指从发送方传输到接收方的一个信息单元（例如，发送方指定的一定字节数的信息）。UDP协议任何必需的可靠性必须由应用层来提供。  
\(4\). 应用层

应用层决定了向用户提供应用服务时通信的活动。TCP/IP 协议族内预存了各类通用的应用服务。包括 HTTP，FTP（File Transfer Protocol，文件传输协议），DNS（Domain Name System，域名系统）服务。  


![](http://upload-images.jianshu.io/upload_images/3985563-1891c256487e9d85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
当应用程序用TCP传送数据时，数据被送入协议栈中，然后逐个通过每一层直到被当作一串比特流送入网络。其中每一层对收到的数据都要增加一些首部信息（有时还要增加尾部信息），该过程如图所示。

![](http://upload-images.jianshu.io/upload_images/3985563-5d534a249b4825a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
当目的主机收到一个以太网数据帧时，数据就开始从协议栈中由底向上升，同时去掉各层协议加上的报文首部。每层协议盒都要去检查报文首部中的协议标识，以确定接收数据的上层协议。这个过程称作分用（Demultiplexing）。协议是通过目的端口号、源I P地址和源端口号进行解包的。

通过以上步骤我们从TCP/IP模型的角度来理解了一次HTTP请求与响应的过程。

下面这张图更清楚明白：

![](http://upload-images.jianshu.io/upload_images/3985563-ecf824604debcdf1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


下面具体来看如何进行一步步操作的。

### 三、TCP三次握手

TCP是面向连接的，无论哪一方向另一方发送数据之前，都必须先在双方之间建立一条连接。在TCP/IP协议中，TCP协议提供可靠的连接服务，连接是通过三次握手进行初始化的。三次握手的目的是同步连接双方的序列号和确认号并交换 TCP窗口大小信息。  


![](http://upload-images.jianshu.io/upload_images/3985563-f2fe3775bd2678c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


第一次握手：建立连接。客户端发送连接请求报文段，将SYN位置为1，Sequence Number为x；然后，客户端进入SYN\_SEND状态，等待服务器的确认；

第二次握手：服务器收到SYN报文段。服务器收到客户端的SYN报文段，需要对这个SYN报文段进行确认，设置Acknowledgment Number为x+1\(Sequence Number+1\)；同时，自己自己还要发送SYN请求信息，将SYN位置为1，Sequence Number为y；服务器端将上述所有信息放到一个报文段（即SYN+ACK报文段）中，一并发送给客户端，此时服务器进入SYN\_RECV状态；

第三次握手：客户端收到服务器的SYN+ACK报文段。然后将Acknowledgment Number设置为y+1，向服务器发送ACK报文段，这个报文段发送完毕以后，客户端和服务器端都进入ESTABLISHED状态，完成TCP三次握手。

##### 为什么要三次握手

为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。

**具体例子：**“已失效的连接请求报文段”的产生在这样一种情况下：client发出的第一个连接请求报文段并没有丢失，而是在某个网络结点长时间的滞留了，以致延误到连接释放以后的某个时间才到达server。本来这是一个早已失效的报文段。但server收到此失效的连接请求报文段后，就误认为是client再次发出的一个新的连接请求。于是就向client发出确认报文段，同意建立连接。假设不采用“三次握手”，那么只要server发出确认，新的连接就建立了。由于现在client并没有发出建立连接的请求，因此不会理睬server的确认，也不会向server发送数据。但server却以为新的运输连接已经建立，并一直等待client发来数据。这样，server的很多资源就白白浪费掉了。采用“三次握手”的办法可以防止上述现象发生。例如刚才那种情况，client不会向server的确认发出确认。server由于收不到确认，就知道client并没有要求建立连接。”

### 四、HTTP协议

##### Http是什么？

通俗来讲，他就是计算机通过网络进行通信的规则，是一个基于请求与响应，无状态的，应用层的协议，常基于TCP/IP协议传输数据。目前任何终端（手机，笔记本电脑。。）之间进行任何一种通信都必须按照Http协议进行，否则无法连接。

**四个基于：**

**请求与响应**: 客户端发送请求，服务器端响应数据

**无状态的**：协议对于事务处理没有记忆能力，客户端第一次与服务器建立连接发送请求时需要进行一系列的安全认证匹配等，因此增加页面等待时间，当客户端向服务器端发送请求，服务器端响应完毕后，两者断开连接，也不保存连接状态，一刀两断！恩断义绝！从此路人！下一次客户端向同样的服务器发送请求时，由于他们之前已经遗忘了彼此，所以需要重新建立连接。

**应用层**：Http是属于应用层的协议，配合TCP/IP使用。

**TCP/IP**：Http使用TCP作为它的支撑运输协议。HTTP客户机发起一个与服务器的TCP连接，一旦连接建立，浏览器（客户机）和服务器进程就可以通过套接字接口访问TCP。

**针对无状态的一些解决策略**：

有时需要对用户之前的HTTP通信状态进行保存，比如执行一次登陆操作，在30分钟内所有的请求都不需要再次登陆。于是引入了Cookie技术。

HTTP/1.1想出了持久连接（HTTP keep-alive）方法。其特点是，只要任意一端没有明确提出断开连接，则保持TCP连接状态，在请求首部字段中的Connection: keep-alive即为表明使用了持久连接。  
等等还有很多。。。。。。

下面开始讲解重头戏：HTTP请求报文，响应报文，对应于上述步骤的2，3，4，5，6。

HTTP报文是面向文本的，报文中的每一个字段都是一些ASCII码串，各个字段的长度是不确定的。HTTP有两类报文：请求报文和响应报文。

### 五、HTTP请求报文

一个HTTP请求报文由请求行（request line）、请求头部（header）、空行和请求数据4个部分组成，下图给出了请求报文的一般格式。  


![](http://upload-images.jianshu.io/upload_images/3985563-cd59a3899ef546e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


##### 1.请求行

请求行分为三个部分：请求方法、请求地址和协议版本

**请求方法**

HTTP/1.1 定义的请求方法有8种：GET、POST、PUT、DELETE、PATCH、HEAD、OPTIONS、TRACE。

最常的两种GET和POST，如果是RESTful接口的话一般会用到GET、POST、DELETE、PUT。

**请求地址**

URL:统一资源定位符，是一种自愿位置的抽象唯一识别方法。

组成：&lt;协议&gt;：//&lt;主机&gt;：&lt;端口&gt;/&lt;路径&gt;

**端口和路径有时可以省略（HTTP默认端口号是80）**

如下例：  


![](http://upload-images.jianshu.io/upload_images/3985563-54ce5eca048253be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
有时会带参数，GET请求

**协议版本**

协议版本的格式为：HTTP/主版本号.次版本号，常用的有HTTP/1.0和HTTP/1.1

##### 2.请求头部

请求头部为请求报文添加了一些附加信息，由“名/值”对组成，每行一对，名和值之间使用冒号分隔。

常见请求头如下：  


![](http://upload-images.jianshu.io/upload_images/3985563-539378eee14fa322.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
请求头部的最后会有一个空行，表示请求头部结束，接下来为请求数据，这一行非常重要，必不可少。

##### 3.请求数据

可选部分，比如GET请求就没有请求数据。

下面是一个POST方法的请求报文：

> POST 　/index.php　HTTP/1.1 　　 请求行  
> Host: localhost  
> User-Agent: Mozilla/5.0 \(Windows NT 5.1; rv:10.0.2\) Gecko/20100101 Firefox/10.0.2　　请求头  
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,_/_;q=0.8  
> Accept-Language: zh-cn,zh;q=0.5  
> Accept-Encoding: gzip, deflate  
> Connection: keep-alive  
> Referer:[http://localhost/](http://localhost/)  
> Content-Length：25  
> Content-Type：application/x-www-form-urlencoded  
> 　　空行  
> username=aa&password=1234　　请求数据

### 六、HTTP响应报文

![](http://upload-images.jianshu.io/upload_images/3985563-c6ee8f8526f59fc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
HTTP响应报文主要由状态行、响应头部、空行以及响应数据组成。

##### 1.状态行

由3部分组成，分别为：协议版本，状态码，状态码描述。

其中协议版本与请求报文一致，状态码描述是对状态码的简单描述，所以这里就只介绍状态码。

**状态码**

状态代码为3位数字。  
1xx：指示信息--表示请求已接收，继续处理。  
2xx：成功--表示请求已被成功接收、理解、接受。  
3xx：重定向--要完成请求必须进行更进一步的操作。  
4xx：客户端错误--请求有语法错误或请求无法实现。  
5xx：服务器端错误--服务器未能实现合法的请求。

下面列举几个常见的：

![](http://upload-images.jianshu.io/upload_images/3985563-8f3bf059bc4365e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


##### 2.响应头部

与请求头部类似，为响应报文添加了一些附加信息

常见响应头部如下：

![](http://upload-images.jianshu.io/upload_images/3985563-33ed95479f541a07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


##### 3.响应数据

用于存放需要返回给客户端的数据信息。

下面是一个响应报文的实例：

> HTTP/1.1 200 OK　　状态行  
> Date: Sun, 17 Mar 2013 08:12:54 GMT　　响应头部  
> Server: Apache/2.2.8 \(Win32\) PHP/5.2.5  
> X-Powered-By: PHP/5.2.5  
> Set-Cookie: PHPSESSID=c0huq7pdkmm5gg6osoe3mgjmm3; path=/  
> Expires: Thu, 19 Nov 1981 08:52:00 GMT  
> Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0  
> Pragma: no-cache  
> Content-Length: 4393  
> Keep-Alive: timeout=5, max=100  
> Connection: Keep-Alive  
> Content-Type: text/html; charset=utf-8  
> 　　空行
>
> &lt;html&gt;　　响应数据  
> &lt;head&gt;  
> &lt;title&gt;HTTP响应示例&lt;title&gt;  
> &lt;/head&gt;  
> &lt;body&gt;  
> Hello HTTP!  
> &lt;/body&gt;  
> &lt;/html&gt;

关于请求头部和响应头部的知识点很多，这里只是简单介绍。

通过以上步骤，数据已经传递完毕，HTTP/1.1会维持持久连接，但持续一段时间总会有关闭连接的时候，这时候据需要断开TCP连接。

### 七、TCP四次挥手

当客户端和服务器通过三次握手建立了TCP连接以后，当数据传送完毕，肯定是要断开TCP连接的啊。那对于TCP的断开连接，这里就有了神秘的“四次分手”。  


![](http://upload-images.jianshu.io/upload_images/3985563-c1c59148f8b26c43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  


  
第一次分手：主机1（可以使客户端，也可以是服务器端），设置Sequence Number，向主机2发送一个FIN报文段；此时，主机1进入FIN\_WAIT\_1状态；这表示主机1没有数据要发送给主机2了；

第二次分手：主机2收到了主机1发送的FIN报文段，向主机1回一个ACK报文段，Acknowledgment Number为Sequence Number加1；主机1进入FIN\_WAIT\_2状态；主机2告诉主机1，我“同意”你的关闭请求；

第三次分手：主机2向主机1发送FIN报文段，请求关闭连接，同时主机2进入LAST\_ACK状态；

第四次分手：主机1收到主机2发送的FIN报文段，向主机2发送ACK报文段，然后主机1进入TIME\_WAIT状态；主机2收到主机1的ACK报文段以后，就关闭连接；此时，主机1等待2MSL后依然没有收到回复，则证明Server端已正常关闭，那好，主机1也可以关闭连接了。

##### 为什么要四次分手

TCP协议是一种面向连接的、可靠的、基于字节流的运输层通信协议。TCP是全双工模式，这就意味着，当主机1发出FIN报文段时，只是表示主机1已经没有数据要发送了，主机1告诉主机2，它的数据已经全部发送完毕了；但是，这个时候主机1还是可以接受来自主机2的数据；当主机2返回ACK报文段时，表示它已经知道主机1没有数据发送了，但是主机2还是可以发送数据到主机1的；当主机2也发送了FIN报文段时，这个时候就表示主机2也没有数据要发送了，就会告诉主机1，我也没有数据要发送了，之后彼此就会愉快的中断这次TCP连接。

通过以上步骤便完成了HTTP的请求和响应，进行了数据传递，这其中涉及到需要知识点，都进行了逐一了解。

### [HTTP 1.0 1.1 2.0 区别](http://www.jianshu.com/p/52d86558ca57)
**HTTP1.1**

为了克服HTTP 1.0的这个缺陷，HTTP 1.1支持持久连接（HTTP/1.1的默认模式使用带流水线的持久连接），在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟。一个包含有许多图像的网页文件的多个请求和应答可以在一个连接中传输，但每个单独的网页文件的请求和应答仍然需要使用各自的连接。HTTP 1.1还允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照接收到客户端请求的先后顺序依次回送响应结果，以保证客户端能够区分出每次请求的响应内容，这样也显著地减少了整个下载过程所需要的时间。

在http1.1，request和reponse头中都有可能出现一个connection的头，此header的含义是当client和server通信时对于长链接如何进行处理。
在http1.1中，client和server都是默认对方支持长链接的， 如果client使用http1.1协议，但又不希望使用长链接，则需要在header中指明connection的值为close；如果server方也不想支持长链接，则在response中也需要明确说明connection的值为close。不论request还是response的header中包含了值为close的connection，都表明当前正在使用的tcp链接在当天请求处理完毕后会被断掉。以后client再进行新的请求时就必须创建新的tcp链接了。

HTTP 1.1在继承了HTTP 1.0优点的基础上，也克服了HTTP 1.0的性能问题。HTTP 1.1通过增加更多的请求头和响应头来改进和扩充HTTP 1.0的功能。如，HTTP 1.0不支持Host请求头字段，WEB浏览器无法使用主机头名来明确表示要访问服务器上的哪个WEB站点，这样就无法使用WEB服务器在同一个IP地址和端口号上配置多个虚拟WEB站点。在HTTP 1.1中增加Host请求头字段后，WEB浏览器可以使用主机头名来明确表示要访问服务器上的哪个WEB站点，这才实现了在一台WEB服务器上可以在同一个IP地址和端口号上使用不同的主机名来创建多个虚拟WEB站点。HTTP 1.1的持续连接，也需要增加新的请求头来帮助实现，例如，Connection请求头的值为Keep-Alive时，客户端通知服务器返回本次请求结果后保持连接；Connection请求头的值为close时，客户端通知服务器返回本次请求结果后关闭连接。HTTP 1.1还提供了与身份认证、状态管理和Cache缓存等机制相关的请求头和响应头。HTTP/1.0不支持文件断点续传，RANGE:bytes是HTTP/1.1新增内容，HTTP/1.0每次传送文件都是从文件头开始，即0字节处开始。RANGE:bytes=XXXX表示要求服务器从文件XXXX字节处开始传送，这就是我们平时所说的断点续传！

由上，HTTP/1.1相较于 HTTP/1.0 协议的区别主要体现在：
1. 缓存处理
2. 带宽优化及网络连接的使用
3. 错误通知的管理
4. 消息在网络中的发送
5. 互联网地址的维护
6. 安全性及完整性

#### **移动app上的困难**
一段时间内的连接复用对PC端浏览器的体验帮助很大，因为大部分的请求在集中在一小段时间以内。但对移动app来说，成效不大，app端的请求比较分散且时间跨度相对较大。所以移动端app一般会从应用层寻求其它解决方案，长连接方案或者伪长连接方案：

方案一：基于tcp的长链接

现在越来越多的移动端app都会建立一条自己的长链接通道，通道的实现是基于tcp协议。基于tcp的socket编程技术难度相对复杂很多，而且需要自己制定协议，但带来的回报也很大。信息的上报和推送变得更及时，在请求量爆发的时间点还能减轻服务器压力（http短连接模式会频繁的创建和销毁连接）。不止是IM app有这样的通道，像淘宝这类电商类app都有自己的专属长连接通道了。现在业界也有不少成熟的方案可供选择了，google的protobuf就是其中之一。

方案二：http long-polling（推送）

客户端在初始状态就会发送一个polling（轮寻）请求到服务器，服务器并不会马上返回业务数据，而是等待有新的业务数据产生的时候再返回。所以连接会一直被保持，一旦结束马上又会发起一个新的polling请求，如此反复，所以一直会有一个连接被保持。服务器有新的内容产生的时候，并不需要等待客户端建立一个新的连接。做法虽然简单，但有些难题需要攻克才能实现稳定可靠的业务框架：
和传统的http短链接相比，长连接会在用户增长的时候极大的增加服务器压力，
移动端网络环境复杂，像wifi和4g的网络切换，进电梯导致网络临时断掉等，这些场景都需要考虑怎么重建健康的连接通道。这种polling的方式稳定性并不好，需要做好数据可靠性的保证，比如重发和ack机制（ACK是一个对数据包的确认，当正确收到数据包后，接收端会发送一个ACk给发送端，里面会说明对那个数据包进行确认，每个数据包里都会有一个序列号，如果收到的数据包有误，或错序，还会申请重发，NAK是一个否定的回答，ACK是确定回答，这样保证数据的正确传输）。

polling的response有可能会被中间代理cache住，要处理好业务数据的过期机制。long-polling方式还有一些缺点是无法克服的，比如每次新的请求都会带上重复的header信息，还有数据通道是单向的，主动权掌握在server这边，客户端有新的业务请求的时候无法及时传送。

方案三：http streaming

同long-polling不同的是，server并不会结束初始的streaming请求，而是持续的通过这个通道返回最新的业务数据。显然这个数据通道也是单向的。streaming是通过在server response的头部里增加”Transfer Encoding: chunked”来告诉客户端后续还会有新的数据到来。除了和long－polling相同的难点之外，streaming还有几个缺陷：有些代理服务器会等待服务器的response结束之后才会将结果推送到请求客户端。对于streaming这种永远不会结束的方式来说，客户端就会一直处于等待response的过程中。业务数据无法按照请求来做分割，所以客户端每收到一块数据都需要自己做协议解析，也就是说要做自己的协议定制。streaming不会产生重复的header数据。

方案四：WebSocket

WebSocket和传统的tcp socket连接相似，也是基于tcp协议，提供双向的数据通道。WebSocket优势在于提供了message的概念，比基于字节流的tcp socket使用更简单，同时又提供了传统的http所缺少的长连接功能。不过WebSocket相对较新，2010年才起草，并不是所有的浏览器都提供了支持。各大浏览器厂商最新的版本都提供了支持。


### HTTP2.0
下面总结了HTTP2.0协议的几个特性:


**多路复用 (Multiplexing)**

多路复用允许同时通过单一的 HTTP/2 连接发起多重的请求-响应消息。在 HTTP/1.1 协议中浏览器客户端在同一时间，针对同一域名下的请求有一定数量限制。超过限制数目的请求会被阻塞。这也是为何一些站点会有多个静态资源 CDN 域名的原因之一，拿 Twitter 为例，http://twimg.com，目的就是变相的解决浏览器针对同一域名的请求限制阻塞问题。而 HTTP/2 的多路复用(Multiplexing) 则允许同时通过单一的 HTTP/2 连接发起多重的请求-响应消息。因此 HTTP/2 可以很容易的去实现多流并行而不用依赖建立多个 TCP 连接，HTTP/2 把 HTTP 协议通信的基本单位缩小为一个一个的帧，这些帧对应着逻辑流中的消息。并行地在同一个 TCP 连接上双向交换消息。

**二进制分帧**

HTTP/2在 应用层(HTTP/2)和传输层(TCP or UDP)之间增加一个二进制分帧层。在不改动 HTTP/1.x 的语义、方法、状态码、URI 以及首部字段的情况下, 解决了HTTP1.1 的性能限制，改进传输性能，实现低延迟和高吞吐量。在二进制分帧层中， HTTP/2 会将所有传输的信息分割为更小的消息和帧（frame）,并对它们采用二进制格式的编码 ，其中 HTTP1.x 的首部信息会被封装到 HEADER frame，而相应的 Request Body 则封装到 DATA frame 里面。

HTTP/2 通信都在一个连接上完成，这个连接可以承载任意数量的双向数据流。在过去， HTTP 性能优化的关键并不在于高带宽，而是低延迟。TCP 连接会随着时间进行自我调谐，起初会限制连接的最大速度，如果数据成功传输，会随着时间的推移提高传输的速度。这种调谐则被称为 TCP 慢启动。由于这种原因，让原本就具有突发性和短时性的 HTTP 连接变的十分低效。HTTP/2 通过让所有数据流共用同一个连接，可以更有效地使用 TCP 连接，让高带宽也能真正的服务于 HTTP 的性能提升。

这种单连接多资源的方式，减少服务端的链接压力,内存占用更少,连接吞吐量更大；而且由于 TCP 连接的减少而使网络拥塞状况得以改善，同时慢启动时间的减少,使拥塞和丢包恢复速度更快。
￼

**首部压缩（Header Compression）**

HTTP/1.1并不支持 HTTP 首部压缩，为此 SPDY 和 HTTP/2 应运而生， SPDY 使用的是通用的DEFLATE 算法，而 HTTP/2 则使用了专门为首部压缩而设计的 HPACK 算法。
￼

**服务端推送（Server Push）**

服务端推送是一种在客户端请求之前发送数据的机制。在 HTTP/2 中，服务器可以对客户端的一个请求发送多个响应。Server Push 让 HTTP1.x 时代使用内嵌资源的优化手段变得没有意义；如果一个请求是由你的主页发起的，服务器很可能会响应主页内容、logo 以及样式表，因为它知道客户端会用到这些东西。这相当于在一个 HTML 文档内集合了所有的资源，不过与之相比，服务器推送还有一个很大的优势：可以缓存！也让在遵循同源的情况下，不同页面之间可以共享缓存资源成为可能。



    | 状态代码 | 状态信息| 含义|
    |----------|--------|-----|
    |100| Continue| 初始的请求已经接受，客户应当继续发送请求的其余部分。（HTTP 1.1新）|
    |101| Switching Protocols| 服务器将遵从客户的请求转换到另外一种协议（HTTP 1.1新）|
    |200 |OK |一切正常，对GET和POST请求的应答文档跟在后面。|
    |201| Created| 服务器已经创建了文档，Location头给出了它的URL。|
    |202| Accepted |已经接受请求，但处理尚未完成。|
    |203| Non-Authoritative Information |文档已经正常地返回，但一些应答头可能不正确，因为使用的是文档的拷贝（HTTP 1.1新）。|
    |204| No Content |没有新文档，浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而Servlet可以确定用户文档足够新，这个状态代码是很有用的。|
    |205| Reset Content| 没有新的内容，但浏览器应该重置它所显示的内容。用来强制浏览器清除表单输入内容（HTTP 1.1新）。|
    |206| Partial Content| 客户发送了一个带有Range头的GET请求，服务器完成了它（HTTP 1.1新）。|
    |300| Multiple Choices| 客户请求的文档可以在多个位置找到，这些位置已经在返回的文档内列出。如果服务器要提出优先选择，则应该在Location应答头指明。|
    |301| Moved Permanently| 客户请求的文档在其他地方，新的URL在Location头中给出，浏览器应该自动地访问新的URL。|
    |302| Found| 类似于301，但新的URL应该被视为临时性的替代，而不是永久性的。注意，在HTTP1.0中对应的状态信息是“Moved Temporatily”。出现该状态代码时，浏览器能够自动访问新的URL，因此它是一个很有用的状态代码。注意这个状态代码有时候可以和301替换使用。例如，如果浏览器错误地请求http://host/~user（缺少了后面的斜杠），有的服务器返回301，有的则返回302。严格地说，我们只能假定只有当原来的请求是GET时浏览器才会自动重定向。请参见307。|
    |303| See Other| 类似于301/302，不同之处在于，如果原来的请求是POST，Location头指定的重定向目标文档应该通过GET提取（HTTP 1.1新）。|
    |304| Not Modified| 客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告诉客户，原来缓冲的文档还可以继续使用。|
    |305| Use Proxy |客户请求的文档应该通过Location头所指明的代理服务器提取（HTTP 1.1新）。|
    |307| Temporary Redirect| 和302（Found）相同。许多浏览器会错误地响应302应答进行重定向，即使原来的请求是POST，即使它实际上只能在POST请求的应答是303时 才能重定向。由于这个原因，HTTP 1.1新增了307，以便更加清除地区分几个状态代码：当出现303应答时，浏览器可以跟随重定向的GET和POST请求；如果是307应答，则浏览器只能跟随对GET请求的重定向。（HTTP 1.1新）|
    |400| Bad Request |请求出现语法错误。|
    |401| Unauthorized| 客户试图未经授权访问受密码保护的页面。应答中会包含一个WWW-Authenticate头，浏览器据此显示用户名字/密码对话框，然后在填写合适的Authorization头后再次发出请求。|
    |403| Forbidden| 资源不可用。服务器理解客户的请求，但拒绝处理它。通常由于服务器上文件或目录的权限设置导致。|
    |404| Not Found| 无法找到指定位置的资源。这也是一个常用的应答。|
    |405| Method Not Allowed| 请求方法（GET、POST、HEAD、DELETE、PUT、TRACE等）对指定的资源不适用。（HTTP 1.1新）|
    |406| Not Acceptable| 指定的资源已经找到，但它的MIME类型和客户在Accpet头中所指定的不兼容（HTTP 1.1新）。|
    |407| Proxy Authentication Required| 类似于401，表示客户必须先经过代理服务器的授权。（HTTP 1.1新）|
    |408| Request Timeout| 在服务器许可的等待时间内，客户一直没有发出任何请求。客户可以在以后重复同一请求。（HTTP 1.1新）|
    |409| Conflict| 通常和PUT请求有关。由于请求和资源的当前状态相冲突，因此请求不能成功。（HTTP 1.1新）|
    |410| Gone| 所请求的文档已经不再可用，而且服务器不知道应该重定向到哪一个地址。它和404的不同在于，返回407表示文档永久地离开了指定的位置，而404表示由于未知的原因文档不可用。（HTTP 1.1新）|
    |411| Length Required| 服务器不能处理请求，除非客户发送一个Content-Length头。（HTTP 1.1新）|
    |412| Precondition Failed| 请求头中指定的一些前提条件失败（HTTP 1.1新）。|
    |413| Request Entity Too Large| 目标文档的大小超过服务器当前愿意处理的大小。如果服务器认为自己能够稍后再处理该请求，则应该提供一个Retry-After头（HTTP 1.1新）。|
    |414| Request URI Too Long| URI太长（HTTP 1.1新）。|
    |416| Requested Range Not Satisfiable| 服务器不能满足客户在请求中指定的Range头。（HTTP 1.1新）|
    |500| Internal Server Error| 服务器遇到了意料不到的情况，不能完成客户的请求。|
    |501| Not Implemented| 服务器不支持实现请求所需要的功能。例如，客户发出了一个服务器不支持的PUT请求。|
    |502| Bad Gateway| 服务器作为网关或者代理时，为了完成请求访问下一个服务器，但该服务器返回了非法的应答。|
    |503| Service Unavailable| 服务器由于维护或者负载过重未能应答。例如，Servlet可能在数据库连接池已满的情况下返回503。服务器返回503时可以提供一个Retry-After头。|
    |504| Gateway Timeout| 由作为代理或网关的服务器使用，表示不能及时地从远程服务器获得应答。（HTTP 1.1新）|
    |505| HTTP Version Not Supported| 服务器不支持请求中所指明的HTTP版本。（HTTP 1.1新）|
    |----|------|------|----|

# HTTP协议Keep-Alive模式详解和HTTP头字段总结 

## 1. 什么是Keep-Alive模式？

我们知道HTTP协议采用“请求-应答”模式，当使用普通模式，即非KeepAlive模式时，每个请求/应答客户和服务器都要新建一个连接，完成 之后立即断开连接（HTTP协议为无连接的协议）；当使用Keep-Alive模式（又称持久连接、连接重用）时，Keep-Alive功能使客户端到服 务器端的连接持续有效，当出现对服务器的后继请求时，Keep-Alive功能避免了建立或者重新建立连接。

http 1.0中默认是关闭的，需要在http头加入"Connection: Keep-Alive"，才能启用Keep-Alive；http 1.1中默认启用Keep-Alive，如果加入"Connection: close "，才关闭。目前大部分浏览器都是用http1.1协议，也就是说默认都会发起Keep-Alive的连接请求了，所以是否能完成一个完整的Keep- Alive连接就看服务器设置情况。

## 2. 启用Keep-Alive的优点

从上面的分析来看，启用Keep-Alive模式肯定更高效，性能更高。因为避免了建立/释放连接的开销。下面是RFC 2616 上的总结：

    By opening and closing fewer TCP connections, CPU time is saved in routers and hosts (clients, servers, proxies, gateways, tunnels, or caches), and memory used for TCP protocol control blocks can be saved in hosts.
    HTTP requests and responses can be pipelined on a connection. Pipelining allows a client to make multiple requests without waiting for each response, allowing a single TCP connection to be used much more efficiently, with much lower elapsed time.
    Network congestion is reduced by reducing the number of packets caused by TCP opens, and by allowing TCP sufficient time to determine the congestion state of the network.
    Latency on subsequent requests is reduced since there is no time spent in TCP's connection opening handshake.
    HTTP can evolve more gracefully, since errors can be reported without the penalty of closing the TCP connection. Clients using future versions of HTTP might optimistically try a new feature, but if communicating with an older server, retry with old semantics after an error is reported.

RFC 2616 （P47）还指出：单用户客户端与任何服务器或代理之间的连接数不应该超过2个。一个代理与其它服务器或代码之间应该使用不超过2 * N的活跃并发连接。这是为了提高HTTP响应时间，避免拥塞（冗余的连接并不能代码执行性能的提升）。

## 3. 回到我们的问题（即如何判断消息内容/长度的大小？）

Keep-Alive模式，客户端如何判断请求所得到的响应数据已经接收完成（或者说如何知道服务器已经发生完了数据）？我们已经知道 了，Keep-Alive模式发送玩数据HTTP服务器不会自动断开连接，所有不能再使用返回EOF（-1）来判断（当然你一定要这样使用也没有办法，可 以想象那效率是何等的低）！下面我介绍两种来判断方法。
3.1、使用消息首部字段Conent-Length

故名思意，Conent-Length表示实体内容长度，客户端（服务器）可以根据这个值来判断数据是否接收完成。但是如果消息中没有Conent-Length，那该如何来判断呢？又在什么情况下会没有Conent-Length呢？请继续往下看……

3.2、使用消息首部字段Transfer-Encoding

当客户端向服务器请求一个静态页面或者一张图片时，服务器可以很清楚的知道内容大小，然后通过Content-length消息首部字段告诉客户端 需要接收多少数据。但是如果是动态页面等时，服务器是不可能预先知道内容大小，这时就可以使用Transfer-Encoding：chunk模式来传输 数据了。即如果要一边产生数据，一边发给客户端，服务器就需要使用"Transfer-Encoding: chunked"这样的方式来代替Content-Length。

chunk编码将数据分成一块一块的发生。Chunked编码将使用若干个Chunk串连而成，由一个标明长度为0 的chunk标示结束。每个Chunk分为头部和正文两部分，头部内容指定正文的字符总数（十六进制的数字 ）和数量单位（一般不写），正文部分就是指定长度的实际内容，两部分之间用回车换行(CRLF) 隔开。在最后一个长度为0的Chunk中的内容是称为footer的内容，是一些附加的Header信息（通常可以直接忽略）。 Chunk编码的格式如下：


代码如下:
```html
Chunked-Body = *<strong>chunk </strong>
"0" CRLF
footer
CRLF
chunk = chunk-size [ chunk-ext ] CRLF
chunk-data CRLF</p><p>hex-no-zero = &lt;HEX excluding "0"&gt;</p><p>chunk-size = hex-no-zero *HEX
chunk-ext = *( ";" chunk-ext-name [ "=" chunk-ext-value ] )
chunk-ext-name = token
chunk-ext-val = token | quoted-string
chunk-data = chunk-size(OCTET)</p><p>footer = *entity-header
```
    即Chunk编码由四部分组成： 1、<strong>0至多个chunk块</strong> ，2、<strong>"0" CRLF </strong>，3、<strong>footer </strong>，4、<strong>CRLF</strong> <strong>.</strong> 而每个chunk块由：chunk-size、chunk-ext（可选）、CRLF、chunk-data、CRLF组成。

## 4. 消息长度的总结

其实，上面2中方法都可以归纳为是如何判断http消息的大小、消息的数量。RFC 2616 对 消息的长度总结如下：一个消息的transfer-length（传输长度）是指消息中的message-body（消息体）的长度。当应用了 transfer-coding（传输编码），每个消息中的message-body（消息体）的长度（transfer-length）由以下几种情况 决定（优先级由高到低）：
任何不含有消息体的消息（如1XXX、204、304等响应消息和任何头(HEAD，首部)请求的响应消息），总是由一个空行（CLRF）结束。
如果出现了Transfer-Encoding头字段 并且值为非“identity”，那么transfer-length由“chunked” 传输编码定义，除非消息由于关闭连接而终止。
如果出现了Content-Length头字段，它的值表示entity-length（实体长度）和transfer-length（传输长 度）。如果这两个长度的大小不一样（i.e.设置了Transfer-Encoding头字段），那么将不能发送Content-Length头字段。并 且如果同时收到了Transfer-Encoding字段和Content-Length头字段，那么必须忽略Content-Length字段。
如果消息使用媒体类型“multipart/byteranges”，并且transfer-length 没有另外指定，那么这种自定界（self-delimiting）媒体类型定义transfer-length 。除非发送者知道接收者能够解析该类型，否则不能使用该类型。
由服务器关闭连接确定消息长度。（注意：关闭连接不能用于确定请求消息的结束，因为服务器不能再发响应消息给客户端了。）
为了兼容HTTP/1.0应用程序，HTTP/1.1的请求消息体中必须包含一个合法的Content-Length头字段，除非知道服务器兼容 HTTP/1.1。一个请求包含消息体，并且Content-Length字段没有给定，如果不能判断消息的长度，服务器应该用用400 (bad request) 来响应；或者服务器坚持希望收到一个合法的Content-Length字段，用 411 (length required)来响应。

所有HTTP/1.1的接收者应用程序必须接受“chunked” transfer-coding (传输编码)，因此当不能事先知道消息的长度，允许使用这种机制来传输消息。消息不应该够同时包含 Content-Length头字段和non-identity transfer-coding。如果一个消息同时包含non-identity transfer-coding和Content-Length ，必须忽略Content-Length 。

# 5. HTTP头字段总结

最后我总结下HTTP协议的头部字段。
1. Accept：告诉WEB服务器自己接受什么介质类型，/ 表示任何类型，type/* 表示该类型下的所有子类型，type/sub-type。
2. Accept-Charset： 浏览器申明自己接收的字符集 Accept-Encoding： 浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法（gzip，deflate） Accept-Language：浏览器申明自己接收的语言 语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等。
3. Accept-Ranges：WEB服务器表明自己是否接受获取其某个实体的一部分（比如文件的一部分）的请求。bytes：表示接受，none：表示不接受。
4. Age：当代理服务器用自己缓存的实体去响应请求时，用该头部表明该实体从产生到现在经过多长时间了。
5. Authorization：当客户端接收到来自WEB服务器的 WWW-Authenticate 响应时，用该头部来回应自己的身份验证信息给WEB服务器。
6. Cache-Control：请求：no-cache（不要缓存的实体，要求现在从WEB服务器去取） max-age：（只接受 Age 值小于 max-age 值，并且没有过期的对象） max-stale：（可以接受过去的对象，但是过期时间必须小于 max-stale 值） min-fresh：（接受其新鲜生命期大于其当前 Age 跟 min-fresh 值之和的缓存对象） 响应：public(可以用 Cached 内容回应任何用户) private（只能用缓存内容回应先前请求该内容的那个用户） no-cache（可以缓存，但是只有在跟WEB服务器验证了其有效后，才能返回给客户端） max-age：（本响应包含的对象的过期时间） ALL: no-store（不允许缓存）
7. Connection：请求：close（告诉WEB服务器或者代理服务器，在完成本次请求的响应后，断开连接，不要等待本次连接的后续请求了）。 keepalive（告诉WEB服务器或者代理服务器，在完成本次请求的响应后，保持连接，等待本次连接的后续请求）。 响应：close（连接已经关闭）。 keepalive（连接保持着，在等待本次连接的后续请求）。 Keep-Alive：如果浏览器请求保持连接，则该头部表明希望 WEB 服务器保持连接多长时间（秒）。例如：Keep-Alive：300
8. Content-Encoding：WEB服务器表明自己使用了什么压缩方法（gzip，deflate）压缩响应中的对象。例如：Content-Encoding：gzip
9. Content-Language：WEB 服务器告诉浏览器自己响应的对象的语言。
10. Content-Length： WEB 服务器告诉浏览器自己响应的对象的长度。例如：Content-Length: 26012
11. Content-Range： WEB 服务器表明该响应包含的部分对象为整个对象的哪个部分。例如：Content-Range: bytes 21010-47021/47022
12. Content-Type： WEB 服务器告诉浏览器自己响应的对象的类型。例如：Content-Type：application/xml
13. ETag：就是一个对象（比如URL）的标志值，就一个对象而言，比如一个 html 文件，如果被修改了，其 Etag 也会别修改，所以ETag 的作用跟 Last-Modified 的作用差不多，主要供 WEB 服务器判断一个对象是否改变了。比如前一次请求某个 html 文件时，获得了其 ETag，当这次又请求这个文件时，浏览器就会把先前获得的 ETag 值发送给WEB 服务器，然后 WEB 服务器会把这个 ETag 跟该文件的当前 ETag 进行对比，然后就知道这个文件有没有改变了。
14.  Expired：WEB服务器表明该实体将在什么时候过期，对于过期了的对象，只有在跟WEB服务器验证了其有效性后，才能用来响应客户请求。是 HTTP/1.0 的头部。例如：Expires：Sat, 23 May 2009 10:02:12 GMT
15. Host：客户端指定自己想访问的WEB服务器的域名/IP 地址和端口号。例如：Host：rss.sina.com.cn
16. If-Match：如果对象的 ETag 没有改变，其实也就意味著对象没有改变，才执行请求的动作。
17. If-None-Match：如果对象的 ETag 改变了，其实也就意味著对象也改变了，才执行请求的动作。
18. If-Modified-Since：如果请求的对象在该头部指定的时间之后修改了，才执行请求的动作（比如返回对象），否则返回代码304，告诉浏览器 该对象没有修改。例如：If-Modified-Since：Thu, 10 Apr 2008 09:14:42 GMT
19. If-Unmodified-Since：如果请求的对象在该头部指定的时间之后没修改过，才执行请求的动作（比如返回对象）。
20. If-Range：浏览器告诉 WEB 服务器，如果我请求的对象没有改变，就把我缺少的部分给我，如果对象改变了，就把整个对象给我。浏览器通过发送请求对象的 ETag 或者 自己所知道的最后修改时间给 WEB 服务器，让其判断对象是否改变了。总是跟 Range 头部一起使用。
21. Last-Modified：WEB 服务器认为对象的最后修改时间，比如文件的最后修改时间，动态页面的最后产生时间等等。例如：Last-Modified：Tue, 06 May 2008 02:42:43 GMT
22. Location：WEB 服务器告诉浏览器，试图访问的对象已经被移到别的位置了，到该头部指定的位置去取。例如：Location：http://i0.sinaimg.cn/dy/deco/2008/0528/sinahome_0803_ws_005_text_0.gif</a>
23. Pramga：主要使用 Pramga: no-cache，相当于 Cache-Control： no-cache。例如：Pragma：no-cache
24. Proxy-Authenticate： 代理服务器响应浏览器，要求其提供代理身份验证信息。Proxy-Authorization：浏览器响应代理服务器的身份验证请求，提供自己的身份信息。
25. Range：浏览器（比如 Flashget 多线程下载时）告诉 WEB 服务器自己想取对象的哪部分。例如：Range: bytes=1173546-
26. Referer：浏览器向 WEB 服务器表明自己是从哪个 网页/URL 获得/点击 当前请求中的网址/URL。例如：Referer：http://www.sina.com/</a>
27. Server: WEB 服务器表明自己是什么软件及版本等信息。例如：Server：Apache/2.0.61 (Unix)
28. User-Agent: 浏览器表明自己的身份（是哪种浏览器）。例如：User-Agent：Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.8.1.14) Gecko/20080404 Firefox/2、0、0、14
29. Transfer-Encoding: WEB 服务器表明自己对本响应消息体（不是消息体里面的对象）作了怎样的编码，比如是否分块（chunked）。例如：Transfer-Encoding: chunked
30. Vary: WEB服务器用该头部的内容告诉 Cache 服务器，在什么条件下才能用本响应所返回的对象响应后续的请求。假如源WEB服务器在接到第一个请求消息时，其响应消息的头部为：Content- Encoding: gzip; Vary: Content-Encoding那么 Cache 服务器会分析后续请求消息的头部，检查其 Accept-Encoding，是否跟先前响应的 Vary 头部值一致，即是否使用相同的内容编码方法，这样就可以防止 Cache 服务器用自己 Cache 里面压缩后的实体响应给不具备解压能力的浏览器。例如：Vary：Accept-Encoding
31. Via： 列出从客户端到 OCS 或者相反方向的响应经过了哪些代理服务器，他们用什么协议（和版本）发送的请求。当客户端请求到达第一个代理服务器时，该服务器会在自己发出的请求里面添 加 Via 头部，并填上自己的相关信息，当下一个代理服务器收到第一个代理服务器的请求时，会在自己发出的请求里面复制前一个代理服务器的请求的Via 头部，并把自己的相关信息加到后面，以此类推，当 OCS 收到最后一个代理服务器的请求时，检查 Via 头部，就知道该请求所经过的路由。

例如：Via：1.0 236.D0707195.sina.com.cn:80 (squid/2.6.STABLE13)
=============================================================================== 

HTTP 请求消息头部实例： Host：rss.sina.com.cn User-Agent：Mozilla/5、0 (Windows; U; Windows NT 5、1; zh-CN; rv:1、8、1、14) Gecko/20080404 Firefox/2、0、0、14 Accept：text/xml,application/xml,application/xhtml+xml,text/html;q=0、9,text/plain;q=0、8,image/png,/;q=0、5 Accept-Language：zh-cn,zh;q=0、5 Accept-Encoding：gzip,deflate Accept-Charset：gb2312,utf-8;q=0、7,*;q=0、7 Keep-Alive：300 Connection：keep-alive Cookie：userId=C5bYpXrimdmsiQmsBPnE1Vn8ZQmdWSm3WRlEB3vRwTnRtW &lt;-- Cookie If-Modified-Since：Sun, 01 Jun 2008 12:05:30 GMT Cache-Control：max-age=0 HTTP 响应消息头部实例： Status：OK - 200 -- 响应状态码，表示 web 服务器处理的结果。 Date：Sun, 01 Jun 2008 12:35:47 GMT Server：Apache/2.0.61 (Unix) Last-Modified：Sun, 01 Jun 2008 12:35:30 GMT Accept-Ranges：bytes Content-Length：18616 Cache-Control：max-age=120 Expires：Sun, 01 Jun 2008 12:37:47 GMT Content-Type：application/xml Age：2 X-Cache：HIT from 236-41.D07071951.sina.com.cn -- 反向代理服务器使用的 HTTP 头部 Via：1.0 236-41.D07071951.sina.com.cn:80 (squid/2.6.STABLE13) Connection：close