# Netty

## 1.Netty

Netty是一个高性能、异步事件驱动的NIO框架，它基于JavaNIO提供的API实现，提供了对TCP( Transmission Control Protocol，传输控制协议）、UDP(User Datagram Protocol，用户数据包协议）和文件传输的支持。作为一个异步NIO框架，Netty的所有I/O 操作都是异步非阻塞的，通过Future-Listener机制，用户可以方便地主动获取或者通过通知机制获取I/O操作结果。

Netty是一个高性能的异步事件驱动的网络应用程序框架，主要用于客户端和服务端网络应用程序的快速搭建和开发。基于Netty API可以非常快速、方便地构建一个高性能的TCP或UDP的网络应用，不但极大地简化了网络编程的复杂度，还提高了网络应用程序的性能和稳定性。Netty支持多种网络通信协议，包括TCP、UDP、FTP、HTTP、SMTP等。

## 2.Reactor线程模型

Reactor是一种并发处理客户端请求响应的事件驱动模型。服务端在接收到客户端请求后采用多路复用策略，通过一个非阻塞的线程来异步接收所有的客户端请求，并将这些请求转发到相关的工作线程组上进行处理。

Reactor模型常常基于异步线程的方式实现，常用的Reactor线程模型有3种：Reactor单线程模型、Reactor多线程模型和Reactor主从多线程模型。

### Reactor单线程模型

Reactor单线程模型指所有的客户端I/O操作请求都在一个线程（Thread）上完成。

Reactor单线程模型的各模块组成及职责。

( 1 ) Client: NIO客户端，向服务端发起TCP连接，并发送数据。

( 2 ) Acceptor: NIO服务端，通过Acceptor接收客户端的TCP连接。

( 3 ) Dispatcher：接收客户端的数据并将数据以ByteBuffer的形式发送到对应的编解码器。

( 4 ) DecoderHandler：解码器，读取客户端的数据并进行数据解码、数据处理和消息应答。

( 5 ) EncoderHandler：编码器，将给客户端发送的数据（消息请求或消息应答）进行统一的编码处理，并写入通道。

由于Reactor模式使用的是异步非阻塞I/O，因此一个线程可以独立处理多个I/O相关的操作，Reactor单线程模型将所有I/O操作都集中在一个线程中处理。具体处理流程如下。

( 1) Acceptor接收客户端的TCP连接请求消息.

( 2）链路建立成功后通过Dispatcher将接收到的消息写入ByteBuffer，并派发到对应的DecoderHandler上进行消息解码和处理。

( 3 ）消息处理完成后调用对应的EncoderHandler将请求响应进行消息的编码和下发。

### Reactor多线程模型

Reactor多线程模型与单线程模型最大的区别就是由一组线程（Thread Poll ）处理客户端的I/O请求操作。Reactor多线程模型将Acceptor的操作封装在一组线程池中，采用线程池的方式监听服务端端口、接收客户端的TCP连接请求、处理网络I/O读写等操线。线程池一般使用标准的JDK线程池，在该线程池中包含一个任务队列和一系列NIO线程，这些NIO线程负责具体的消息读取、解码、编码和发送。

Reactor多线程模型与单线程模型的区别在于，Reactor多线程模型用于接收客户端请求的Acceptor由一个线程来负责，用于处理客户端消息的Dispatcher由一个线程池负责，这样，基于线程池的调度和线程异步执行的能力，能够在同一时间内接收和处理更多的客户端请求。

### Reactor主从多线程模型

在Reactor主从多线程模型中，服务端用于接收客户端连接的不再是一个NIO线程，而是一个独立的NIO线程池。主线程Acceptor在接收到客户端的TCP连接请求并建立完成连接后（可能要经过鉴权、登录等过程），将新创建的SocketChannel注册到子I/O线程池（Sub Reactor Pool ）的某个I/O线程上，由它负责具体的SocketChannel的读写和编解码工作。

Reactor主从多线程模型中的Acceptor线程池（Acceptor Thread Pool ）只用于客户端的鉴权、登录、握手和安全认证，一旦链路建立成功，就将链路注册到后端Sub Reactor 线程池的I/O线程上，由I/O线程负责后续的I/O操作。这样就将客户端连接的建立和消息的响应都以异步线程的方式来实现，大大提高了系统的吞吐量。

## 3.Netty的架构设计

Netty的整体架构分为Transport Services （传输服务层）、Protocol Support （传输协议层）和Core（核心层）3部分.

### 1.Transport Services

Transport Services指传输服务层，主要定义了数据的传输和通信方式，包括Socket And Datagram ( Socket协议和数据包）、HTTP Tunnel ( HTTP隧道）、In-VM Pipe （本地传输管道）。

基于Socket的协议有TCP、UDP等。其中，TCP基于报文应答机制实现，主要用于可靠数据的传输，比如移动设备状态信息的传输。UDP发出的数据不需要对方应答，主要用于对数据安全性要求不是很高但是对数据传输吞吐量要求较高的场景，比如实时视频流传输。HTTP Tunnel定义了HTTP的传输方式。In-VM Pipe定义了本地数据的传输方式。

### 2.Protocol Support

Protocol Support指传输协议层，主要定义数据传输过程中的服务类型、数据安全、数据压缩等。具体包括HTTP And WebSocket ( HTTP和WebSocket服务）、SSL And StartTLS ( SSL和StartTLS协议）、zlib/gzip Compression ( zlib/gzip压缩算法）、Large File Transfer （大文件传输）、Google ProtoBuf ( Google ProtoBuf格式）、RTSP（实时流传输协议）、Legacy Text And Binary Protocols （传统TXT和二进制数据）。

HTTP和WebSocket服务定义了客户端和服务端的数据通信方式。HTTP服务基于HTTP实现，主要用于客户端主动向服务端发起数据包请求。WebSocket主要用于服务端基于消息推送的机制实时将数据包推送到客户端。

SSL ( Secure Sockets Layer，安全套接层）主要用于传输层与应用层之间的直接网络数据的加密，是为网络通信提供安全及数据完整性的一种安全协议。SSL包含记录层( Record Layer ）和传输层（Transport Layer ），记录层协议确定传输层数据的封装格式。传输层安全协议使用X.509认证，之后利用非对称加密演算来对通信方进行身份认证，最后交换对称密钥作为此次会话的密钥。基于该会话密钥来实现通信双方数据的加密，保证两个应用程序间通信的保密性和可靠性，使客户端与服务端之间的应用程序之间的通信不被攻击者窃听。

StartTLS是一种明文通信协议的扩展，能够让明文的通信连线直接成为密连线（使用SSL或TLS加密），而不需要使用另一个特别的端口来进行加密通信，属于机会性加密。StartTLS本身是一个与应用层无关的协议，可以搭配许多应用层协议一同使用。

zlib/gzip Compression定义了消息传递过程中数据的压缩和解压缩算法，主要用于提高批量数据传输过程中网络的吞吐量。zlib是一种数据压缩格式，其使用抽象化的DEFLATE算法实现，应用十分广泛，Linux内核中使用zlib以实现网络协议的压缩、文件系统的压缩。OpenSSH、OpenSSL以zlib达到最优化的加密网络传输。gzip的基础也是DEFLATE算法，gzip也是一种数据压缩格式。

Large File Transfer定义了大文件传输过程中数据的拆包和分发策略。

Google ProtoBuf是Google发布的一款开源的数据传输格式和序列化框架。它是一种语言中立、平台无关、可扩展的序列化数据的格式，可用于通信协议、数据存储等。在序列化数据方面，它提供了灵活高效的序列化实现。ProtoBuf很适合用于数据存储或RPC数据交换格式。

RTSP ( Real Time Streaming Protocol）为实时流传输协议，是一种网络应用协议，专为娱乐和通信系统使用，以控制流媒体服务器。该协议用于创建和控制终端之间的媒体会话。媒体服务器的客户端发布VCR命令，例如，播放、录制和暂停，以便实时控制从服务器到客户端（视频点播）或从客户端到服务器（语音录音）的媒体流。

Legacy Text And Binary Protocols提供了传统文本数据格式和二进制数据格式的传输支持。

### 3.Core

Netty的Core（核心层）封装了Netty框架的核心服服和API，具体包括Extensible Event Model（可扩展事件模型）、Universal Communication API （通用通信协议API)、Zero-Copy-Capable Rich Byte Buffer （零拷贝字节缓冲区）等。可扩展事件模型为Netty灵活的事件通信提供了基础；通用通信协议API为上层提供了统一的API访问入口，提高了Netty框架的易用性；零拷贝字节缓冲区为数据的快速读取和写入提供了保障。

## 4.Netty的核心组件

Netty的核心组件包括Bootstrap、ServerBootstrap、NioEventLoop、NioEventLoopGroup、Future、ChannelFuture、Channel、Selector、ChannelHandlerContext、ChannelHandler和ChannelPipeline。

( 1 ) Bootstrap/ServerBootstrap: Bootstrap用于客户端服务的启动引导，ServerBootstrap用于服务端服务的启动引导。

( 2 ) NioEventLoop：基于线程队列的方式执行事件操作，具体要执行的事件操作包括连接注册、端口绑定和I/O数据读写等。每个NioEventLoop线程都负责多个Channel的事件处理。

( 3) NioEventLoopGroup: NioEventLoop生命周期的管理。

( 4 ) Future/ChannelFuture:Future和ChannelFuture用于异步通信的实现，基于异步通信方式可以在I/O操作触发后注册一个监听事件，在I/O操作（数据读写完成或失败）完成后自动触发监听事件并完成后续操作。

( 5 ) Channel: Channel是Netty中的网络通信组件，用于执行具体的I/O操作。Netty中所有的数据通信都基于Channel读取或者将数据写入对应的Channel。Channel的主要功能包括网络连接的建立、连接状态的管理（网络连接的打开和关闭）、网络连接参数的配置（每次接收数据的大小）、基于异步NIO的网络数据操作（数据读取、数据写出）等。

( 6 ) Selector: Selector用于多路复用中Channel的管理。在Netty中，一个Selector可以管理多个Channel，在Channel连接建立后将连接注册到Selector，Selector在内部监听每个Channel上I/O事件的变化，当Channel有网络I/O事件发生时通知ChannelHandler执行具体的I/O操作（读取字节流或字节流写入完成）。

( 7) ChannelHandlerContext: Channel上下文信息的管理。每个ChannelHandler都对应一个ChannelHandlerContext。

( 8 ) ChannelHandler: I/O事件的拦截和处理。其中，ChannelInboundHandler用于处理数据接收的I/O操作，ChannelInboundHandler用于处理数据发送的I/O操作。

( 9 ) ChannelPipeline：基于拦截器设计模式实现的事件拦截处理和转发。Netty中的每个Channel都对应一个ChannelPipeline，在ChannelPipeline中维护了一个由ChannelHandlerContext组成的双向链表，每个ChannelHandlerContext都对应一个ChannelHandler，以完成具体Channel事件的拦截和处理。其中，数据入站由Head向Tail依次传递和处理，数据出站从Tail向Head依次传递和处理。

## 5.Netty的原理

Netty的运行核心包含两个NioEventLoopGroup工作组，一个是BossGroup，用于接收客户端连接、接收客户端数据和进行数据转发；另一个是WorkerGroup，用于具体I/O事件的触发和数据处理。

### Netty Server的初始化步骤

Netty Sever的初始化过程如下。

( 1）初始化BossGroup和WorkerGroup。

( 2 ）基于SeverBootstrap配置EventLoopGroup，包括连接参数设置、Channel类型设置、编解码Handler设置等。

( 3 ）绑定端口和服务启动。

### BossGoup的职责

BossGroup为一个事件循环组，其中包含多个事件循环（NioEventLoop ），每个NioEventLoop都包含一个Selector和一个TaskQueue（事件循环线程）。每个BossNioEventLoop循环都执行以下3个步骤。

( 1 ）轮询监听Accept事件.

( 2 ）接收和处理Accept事件，包括和客户端建立连接并生成NioSocketChannel，将NioSocketChannel注册到某个WorkerNioEventLoop的Selector上。

( 3 ）处理runAllTasks的任务。

### WorkerGroup的职责

WorkerGroup为一个事件循环组，其中包含多个事件循环（NioEventLoop ），每个Work NioEventLoop循环都执行以下3个步骤。

( 1 ）轮询监听NioSocketChannel上的I/O事件（I/O 读写事件）。

( 2 ）当NioSocketChannel有I/O事件触发时执行具体的I/O操作。

( 3 ）处理任务队列中的任务。

## 6.Netty的特性

Netty之所以能在高并发的分布式网络环境下实现对数据的快速处理和分发，得益于其优秀的架构设计。Netty架构设计的主要特性有多路复用模型、数据零拷贝、内存重用机制、无锁化设计和高性能的序列化框架。

### 1.I/O多路复用模型

在高并发的I/O编程模型中，一个服务端往往要同时处理成千上万个客户端请求。传统的I/O模型会为每个Socket链路都建立一个线程专门处理该链路上数据的接收和发送。该方案的特点是，当客户端较少时，服务端的数据处理速度很快，但是当客户端的数量较多时，服务端会出现没有足够的线程资源为每个Socket链路分配一个线程的情况，服务端会出现大量的线程资源争抢和调度，性能急速下降。因此，该方案不适合高并发的场景。

当有大量客户端并发请求时，可以采用I/O多路复用模型处理该问题。I/O多路复用模型将I/O的处理分为Selector和Channel。Selector负责多个Channel的监控和管理，Channel负责真正的I/O读写操作。当Selector监控到Channel上有数据发生变化时会通知对应的Channel，Channel接收到对应的通知后处理对应的数据即可。这样一个Selector线程可以同时处理多个客户端请求。与传统的多线程I/O模型相比，I/O多路复用模型的最大优势是系统开销小，系统不需要为每个客户端连接都建立一个新的线程，也不需要维护这些线程的运行，减少了系统的维护工作量，节省了系统资源。

Netty通过在NioEventLoop（事件轮询机制）内封装Selector来实现I/O的多路复用，在一个NioEventLoop上服务端可以同时并发处理成千上万个客户端请求，同时由于Netty的I/O读写操作都是基于ByteBuffer异步非阻塞的，因此大大提高了I/O线程的运行效率，避免由于频繁I/O阻塞导致的线程挂起和系统资源的浪费。

### 2.数据零烤贝

Linux为了实现操作系统程序和应用程序的资源隔离，将系统分为内核态和用户态。操作系统的程序运行在内核态，具体包括进程管理、存储管理、文件系统、网络通信和模块机制；应用程序运行在用户态并且不能访问内核态的地址空间，如果应用程序需要访问系统内核态的资源，则需要通过系统调用接口或本地函数库（Libc）来完成，具体流程为用户程序调用系统应用接口或本地函数库，系统应用接口或本地函数库调用系统进程获取资源，并将其分配到对应的用户程序，用户程序资源在使用完成后通过系统调用接口或本地函数库释放资源。

传统的Socket服务基于JVM堆内存进行Socket读写，也就是说，申请内存资源时，需要通过JVM向操作系统申请堆内存，然后JVM将堆内存复制一份直接内存中，基于直接内存进行Socket读写。这样就存在频繁的JVM内存数据和Socket线程内存数据来回复制的问题，影响系统性能。

Netty的数据接收和发送均采用堆外直接内存进行Socket读写，堆外直接内存可以直接操作系统内存，不需要来回地进行字节缓冲区的二次复制，大大提高了系统的性能。

同时，Netty提供了组合Buffer对象，基于该对象可以聚合多个ByteBuffer对象，使得用户操作多个Buffer与操作一个Buffer一样方便，避免了传统的通过内存复制的方式将多个小Buffer合并为一个大Buffer带来的使用不便和性能损耗。

Netty的文件传输采用transferTo方法。transferTo方法可以直接将文件缓冲区的数据基于内存映射技术发送到目标Channel，避免了通过循环写方式导致的内存复制问题。

### 3.内存重用机制

JVM对于对象内存的分配和回收耗时很小，但Netty数据的接收和发送均采用堆外直接内存缓存的方式实现，而堆外直接内存缓存的分配和回收是一件耗时的操作。为了尽量重用缓冲区，Netty提供了基于内存池的缓冲区重用机制。

### 4.无锁化设计

对于一般程序来说，多线程会提高系统的并发度，但是线程数并不是越多越好，过多的线程会引起CPU的频繁切换而增加系统的负载。Netty内部采用串行无锁化设计的思想对I/O进行操作，避免多线程竞争CPU和资源锁定导致的性能下降。在具体的使用中，可以调整NIO线程池的线程参数，同时启动多个串行化的线程并行运行，这种局部无锁化的串行多线程设计比一个队列结合多个工作线程模型的性能更佳。

Netty的NioEventLoop的设计思路是Channel读取消息后，直接调用ChannelPipeline的fireChannelRead(Object msg）进行消息的处理，如果在运行过程中用户不主动切换线程，Netty的NioEventLoop则一直在该线程上进行消息的处理，这种线程绑定CPU持续执行的方式可以有效减少系统资源的竞争和切换，对于持续高并发的操作来说性能有很大的提升。

### 5.高性能的序列化框架

Netty默认基于Google ProtoBuf实现数据的序列化，通过扩展Netty的编解码接口，用户可以实现自定义的序列化框架，例如Thrift的压缩二进制编解码框架。在使用ProtoBuf序列化的时候需要注意以下几点。

( 1 ) SO_RCVBUF和SO_SNDBUF的设置：SO_RCVBUF为接收数据的Buffer大小，SO_SNDBUF为发送数据的Buffer大小，通常建议值为128KB或者256KB。

( 2) SO_TCPNODELAY的设置：将SO_TCPNODELAY 设置为true，表示开启自动粘包操作，该操作采用Nagle算法将缓冲区内字节较少的包前后相连组成一个大包一起发送，防止大量小包频繁发送造成网络的阻塞，从而提高网络的吞吐量。该算法对单条数据延迟的要求不高，但在传输数据量大的情况下能显著提高性能。

( 3 ）软中断：在开启软中断后，Netty会根据数据包的源地址、源端口、目的地址、目的端口计算一个Hash值，根据该Hash值选择对应的CPU执行软中断。即Netty将每个连接都与CPU绑定，并通过Hash值在多个CPU上均衡软中断，以提高CPU的性能。

## 7.Netty RPC实现

### 概念

RPC，即Remote Procedure Call（远程过程调用），调用远程计算机上的服务，就像调用本地服务一样。RPC可以很好的解耦系统，如WebService就是一种基于Http协议的RPC。

### 关键技术

1.服务发布与订阅：服务端使用Zookeeper注册服务地址，客户端从Zookeeper获取可用的服务地址。

2.通信：使用Netty作为通信框架。

3.Spring：使用Spring配置服务，加载Bean，扫描注解。

4.动态代理：客户端使用代理模式透明化服务调用。

5.消息编解码：使用Protostuff序列化和反序列化消息。

### 核心流程

1.服务消费方（client）调用以本地调用方式调用服务；

2.client stub接收到调用后负责将方法、参数等组装成能够进行网络传输的消息体；

3.client stub找到服务地址，并将消息发送到服务端；

4.server stub收到消息后进行解码；

5.server stub根据解码结果调用本地的服务；

6.本地服务执行并将结果返回给server stub；

7.server stub将返回结果打包成消息并发送至消费方；

8.client stub接收到消息，并进行解码；

9.服务消费方得到最终结果。

RPC的目标就是要2~8这些步骤都封装起来，让用户对这些细节透明。JAVA一般使用动态代理方式实现远程调用。

### 消息编解码

消息数据结构（接口名称+方法名+参数类型和参数值+超时时间+ requestID）

客户端的请求消息结构一般需要包括以下内容：

1.接口名称：在我们的例子里接口名是“HelloWorldService”，如果不传，服务端就不知道调用哪个接口了；

2.方法名：一个接口内可能有很多方法，如果不传方法名服务端也就不知道调用哪个方法；

3.参数类型和参数值：参数类型有很多，比如有bool、int、long、double、string、map、list，甚至如struct（class）；以及相应的参数值；

4.超时时间：

5.requestID，标识唯一请求id，在下面一节会详细描述requestID的用处。

6.服务端返回的消息：一般包括以下内容。返回值+状态code+requestID

序列化

目前互联网公司广泛使用Protobuf、Thrift、Avro等成熟的序列化解决方案来搭建RPC框架，这些都是久经考验的解决方案。

## 8.通讯过程

### 核心问题(线程暂停、消息乱序)

如果使用netty的话，一般会用channel.writeAndFlush()方法来发送消息二进制串，这个方法调用后对于整个远程调用(从发出请求到接收到结果)来说是一个异步的，即对于当前线程来说，将请求发送出来后，线程就可以往后执行了，至于服务端的结果，是服务端处理完成后，再以消息的形式发送给客户端的。于是这里出现以下两个问题：

1.怎么让当前线程“暂停”，等结果回来后，再向后执行？

2.如果有多个线程同时进行远程方法调用，这时建立在client server之间的socket连接上会有很多双方发送的消息传递，前后顺序也可能是随机的，server处理完结果后，将结果消息发送给client，client收到很多消息，怎么知道哪个消息结果是原先哪个线程调用的？如下图所示，线程A和线程B同时向client socket发送请求requestA和requestB，socket先后将requestB和requestA发送至server，而server可能将responseB先返回，尽管requestB请求到达时间更晚。我们需要一种机制保证responseA丢给ThreadA，responseB丢给ThreadB。

### 通讯流程

requestID生成-AtomicLong

1.client线程每次通过socket调用一次远程接口前，生成一个唯一的ID，即requestID（requestID必需保证在一个Socket连接里面是唯一的），一般常常使用AtomicLong从0开始累计数字生成唯一ID；

存放回调对象callback到全局ConcurrentHashMap

2.将处理结果的回调对象callback，存放到全局ConcurrentHashMap里面put(requestID, callback)；

synchronized获取回调对象callback的锁并自旋wait

3.当线程调用channel.writeAndFlush()发送消息后，紧接着执行callback的get()方法试图获取远程返回的结果。在get()内部，则使用synchronized获取回调对象callback的锁，再先检测是否已经获取到结果，如果没有，然后调用callback的wait()方法，释放callback上的锁，让当前线程处于等待状态。

监听消息的线程收到消息，找到callback上的锁并唤醒

4.服务端接收到请求并处理后，将response结果（此结果中包含了前面的requestID）发送给客户端，客户端socket连接上专门监听消息的线程收到消息，分析结果，取到requestID，再从前面的ConcurrentHashMap里面get(requestID)，从而找到callback对象，再用synchronized获取callback上的锁，将方法调用结果设置到callback对象里，再调用callback.notifyAll()唤醒前面处于等待状态的线程。

### 9.RMI 实现方式

Java 远程方法调用，即Java RMI（Java Remote Method Invocation）是Java 编程语言里，一种用于实现远程过程调用的应用程序编程接口。它使客户机上运行的程序可以调用远程服务器上的对象。远程方法调用特性使Java 编程人员能够在网络环境中分布操作。RMI 全部的宗旨就是尽可能简化远程接口对象的使用。

实现步骤

1. 编写远程服务接口，该接口必须继承 java.rmi.Remote 接口，方法必须抛出java.rmi.RemoteException 异常；
2. 编写远程接口实现类，该实现类必须继承 java.rmi.server.UnicastRemoteObject 类；
3. 运行RMI 编译器（rmic），创建客户端 stub 类和服务端 skeleton 类;
4. 启动一个RMI 注册表，以便驻留这些服务;
5. 在RMI 注册表中注册服务；
6. 客户端查找远程对象，并调用远程方法；