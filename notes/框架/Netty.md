
* [Netty是什么](#netty%E6%98%AF%E4%BB%80%E4%B9%88)
* [Netty高性能的表现](#netty%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84%E8%A1%A8%E7%8E%B0)
* [TCP粘包/拆包的问题解决](#tcp%E7%B2%98%E5%8C%85%E6%8B%86%E5%8C%85%E7%9A%84%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3)
* [Reactor模式](#reactor%E6%A8%A1%E5%BC%8F)
* [Netty的Reactor模式](#netty%E7%9A%84reactor%E6%A8%A1%E5%9E%8B)


### Netty是什么

Netty是一款基于NIO开发的**高性能和异步事件驱动**的网络通信框架，支持高并发，传输速度快，封装好。

**高并发：**
对比于BIO，并发性能得到了很大提高。

**传输快：**
Netty的传输依赖于零拷贝特性，尽量减少不必要的内存拷贝，实现了更高效率的传输。

**封装好：**
Netty封装了NIO操作的很多细节，提供了易于使用调用接口。

典型的应用有：阿里分布式服务框架Dubbo，默认使用Netty作为基础通信组件，还有
RocketMQ也是使用Netty作为通讯的基础。

### Netty高性能的表现

**IO线程模型：**
同步非阻塞，用最少的资源做更多的事。

**内存零拷贝：**
尽量减少不必要的内存拷贝，实现了更高效率的传输。

**内存池设计：**
申请的内存可以重用，主要指直接内存。内部实现是用一颗二叉查找树管理内存分配情况。

**串形化处理读写：**
避免使用锁带来的性能开销。

**高性能序列化协议**：
支持protobuf等高性能序列化协议。



###  TCP粘包/拆包的问题解决

TCP是一个“流”协议，所谓流，就是没有界限的一长串二进制数据。TCP作为传输层协议并不不了解上层业务数据的具体含义，它会根据TCP缓冲区的实际情况进行数据包的划分，所以在业务上认为是一个完整的包，可能会被TCP拆分成多个包进行发送，也有可能把多个小的包封装成一个大的数据包发送，这就是所谓的TCP粘包和拆包问题。

**粘包问题的解决策略：**
由于底层的TCP无法理解上层的业务数据，所以在底层是无法保证数据包不被拆分和重组的，这个问题只能通过上层的应用协议栈设计来解决。业界的主流协议的解决方案，可以归纳如下：

1.消息定长，报文大小固定长度，例如每个报文的长度固定为200字节，如果不够空位补空格

2.包尾添加特殊分隔符，例如每条报文结束都添加回车换行符（例如FTP协议）或者指定特殊字符作为报文分隔符，接收方通过特殊分隔符切分报文区分；

3.将消息分为消息头和消息体，消息头中包含表示信息的总长度（或者消息体长度）的字段；

4.更复杂的自定义应用层协议。

**Netty粘包和拆包解决方案：**
Netty提供了多个解码器，可以进行分包的操作，分别是：

LineBasedFrameDecoder（回车换行结尾）

DelimiterBasedFrameDecoder（添加特殊分隔符报文来分包）

FixedLengthFrameDecoder（使用定长的报文来分包）

LengthFieldBasedFrameDecoder（按照应用层数据包的大小，拆包）


### Reactor模式

反应器设计模式(Reactorpattern)是一种为处理并发服务请求，并将请求提交到一个或者多个服务处理程序的事件设计模式。当客户端请求抵达后，服务处理程序使用多路分配策略，由一个非阻塞的线程来接收所有的请求，然后派发这些请求至相关的工作线程进行处理。

Reactor模式是事件驱动的，有一个或者多个并发输入源，有一个Server Handler和多个Request Handlers，这个Service Handler会同步的将输入的请求多路复用的分发给相应的Request Handler。

![](https://raw.githubusercontent.com/LLLRS/git_resource/master/4rvfsvfdh1223ttyy.png)

Reactor模式从结构上有点类似生产者和消费者模型，即一个或多个生产者将事件放入一个Queue中，而一个或者多个消费者主动的从这个队列中poll事件来处理；而Reactor模式则没有Queue来做缓冲，每当一个事件输入到Service Handler之后，该Service Handler会主动根据不同的Evnent类型将其分发给对应的Request Handler来处理。

### Netty的Reactor模型
Netty中Reactor模式的参与者主要有下面一些组件：

* Selector
* EventLoopGroup/EventLoop
* ChannelPipeline

#### EventLoopGroup / EventLoop
当系统在运行过程中，如果频繁的进行线程上下文切换，会带来额外的性能损耗。多线程并发执行某个业务流程，业务开发者还需要时刻对线程安全保持警惕，哪些数据可能会被并发修改，如何保护？这不仅降低了开发效率，也会带来额外的性能损耗。

为了解决上述问题，Netty采用了串行化设计理念。从消息的读取、编码以及后续Handler的执行，始终都由IO线程EventLoop负责，这就意味着整个流程不会进行线程上下文的切换，数据也不会面临被并发修改的风险。

EventLoopGroup是一组EventLoop的抽象，EventLoopGroup提供next接口，可以从一组EventLoop里面按照一定规则获取其中一个EventLoop来处理任务。对于EventLoopGroup这里需要了解的是在Netty中，在Netty服务器编程中我们需要BossEventLoopGroup和WorkerEventLoopGroup两个EventLoopGroup来进行工作。通常一个服务端口即一个ServerSocketChannel对应一个Selector和一个EventLoop线程，也就是说BossEventLoopGroup的线程数参数为1。
BossEventLoop负责接收客户端的连接并将SocketChannel交给WorkerEventLoopGroup来进行IO处理。
EventLoop的实现充当Reactor模式中的分发（Dispatcher）的角色。

#### ChannelPipeline

ChannelPipeline其实是担任着Reactor模式中的请求处理器这个角色。ChannelPipeline的默认实现是DefaultChannelPipeline，DefaultChannelPipeline本身维护着一个用户不可见的tail和head的ChannelHandler，他们分别位于链表队列的头部和尾部。tail在更上层的部分，而head在靠近网络层的方向。

在Netty中关于ChannelHandler有两个重要的接口，ChannelInBoundHandler和ChannelOutBoundHandler。inbound可以理解为网络数据从外部流向系统内部，而outbound可以理解为网络数据从系统内部流向系统外部。用户实现的ChannelHandler可以根据需要实现其中一个或多个接口，将其放入Pipeline中的链表队列中，ChannelPipeline会根据不同的IO事件类型来找到相应的Handler来处理。

同时链表队列是责任链模式的一种变种，自上而下或自下而上所有满足事件关联的Handler都会对事件进行处理。ChannelInBoundHandler对从客户端发往服务器的报文进行处理，一般用来执行半包/粘包，解码，读取数据，业务处理等；ChannelOutBoundHandler对从服务器发往客户端的报文进行处理，一般用来进行编码，发送报文到客户端。

下图是对ChannelPipeline执行过程的说明：

![](https://raw.githubusercontent.com/LLLRS/git_resource/master/123df3535dczfffc.png)


#### Buffer
Netty提供的经过扩展的Buffer相对NIO中的有个许多优势，作为数据存取非常重要的一块。

**ByteBuf读写指针** ：在ByteBuffer中，读写指针都是position，而在ByteBuf中，读写指针分别为readerIndex和writerIndex。直观看上去ByteBuffer仅用了一个指针就实现了两个指针的功能，节省了变量，但是当对于ByteBuffer的读写状态切换的时候必须要调用flip方法，而当下一次写之前，必须要将Buffe中的内容读完，再调用clear方法。每次读之前调用flip，写之前调用clear，这样无疑给开发带来了繁琐的步骤，而且内容没有读完是不能写的，这样非常不灵活。相比之下使用ByteBuf读写指针，读的时候仅仅依赖readerIndex指针，写的时候仅仅依赖writerIndex指针，不需每次读写之前调用对应的方法，而且没有必须一次读完的限制。


**零拷贝** ：Netty的接收和发送ByteBuffer采用DIRECT BUFFERS，使用堆外直接内存进行Socket读写，不需要进行字节缓冲区的二次拷贝。如果使用传统的堆内存（HEAP BUFFERS）进行Socket读写，JVM会将堆内存Buffer拷贝一份到直接内存中，然后才写入Socket中。相比于堆外直接内存，消息在发送过程中多了一次缓冲区的内存拷贝。Netty提供了组合Buffer对象，可以聚合多个ByteBuffer对象，用户可以像操作一个Buffer那样方便的对组合Buffer进行操作，避免了传统通过内存拷贝的方式将几个小Buffer合并成一个大的Buffer。Netty的文件传输采用了transferTo方法，它可以直接将文件缓冲区的数据发送到目标Channel，避免了传统通过循环write方式导致的内存拷贝问题。

**引用计数与池化技术** ：在Netty中，每个被申请的Buffer对于Netty来说都可能是很宝贵的资源，因此为了获得对于内存的申请与回收更多的控制权，Netty自己根据引用计数法去实现了内存的管理。Netty对于Buffer的使用都是基于直接内存（DirectBuffer）实现的，大大提高I/O操作的效率。然而DirectBuffer和HeapBuffer相比之下除了I/O操作效率高之外还有一个天生的缺点，即对于DirectBuffer的申请相比HeapBuffer效率更低。因此Netty结合引用计数实现了PolledBuffer，即池化的用法，当引用计数等于0的时候，Netty将Buffer回收致池中，在下一次申请Buffer的没某个时刻会被复用。


[李林峰：Netty系列之Netty高性能之道](https://www.infoq.cn/article/netty-high-performance)
