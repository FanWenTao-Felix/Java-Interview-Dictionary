# Hadoop

## 1.Hadoop架构原理

Hadoop是目前大数据领域最主流的一套技术体系，包含了多种技术。包括HDFS（分布式文件系统），YARN（分布式资源调度系统），MapReduce（分布式计算系统）等等。

## 2.HDFS

HDFS全称是Hadoop Distributed File System，是Hadoop的分布式文件系统。它由很多机器组成，每台机器上运行一个DataNode进程，负责管理一部分数据。然后有一台机器上运行了NameNode进程，NameNode大致可以认为是负责管理整个HDFS集群的这么一个进程，他里面存储了HDFS集群的所有元数据。然后有很多台机器，每台机器存储一部分数据！

## **3.HDFS的NameNode架构原理**

NameNode有一个很核心的功能：**管理整个HDFS集群的元数据**，比如说文件目录树、权限的设置、副本数的设置，等等。

每次内存里改完了，写一条edits log，元数据修改的操作日志到磁盘文件里，不修改磁盘文件内容，就是顺序追加，这个性能就高多了。

每次Active NameNode（主节点）修改一次元数据都会生成一条edits log，**除了写入本地磁盘文件，还会写入JournalNodes集群。**

然后Standby NameNode就可以从JournalNodes集群拉取edits log，应用到自己内存的文件目录树里，跟Active NameNode保持一致。

然后每隔一段时间，Standby NameNode都把自己内存里的文件目录树写一份到磁盘上的fsimage，这可不是日志，这是完整的一份元数据。**这个操作就是所谓的checkpoint检查点操作。**

然后把这个fsimage上传到到Active NameNode，接着清空掉Active NameNode的旧的edits log文件，这里可能都有100万行修改日志了！

然后Active NameNode继续接收修改元数据的请求，再写入edits log，写了一小会儿，这里可能就几十行修改日志而已！

如果说此时，Active NameNode重启了，bingo！没关系，只要把Standby NameNode传过来的fsimage直接读到内存里，**这个fsimage直接就是元数据**，不需要做任何额外操作，纯读取，效率很高！

然后把新的edits log里少量的几十行的修改日志回放到内存里就ok了！

这个过程的启动速度就快的多了！因为不需要回放大量上百万行的edits log来恢复元数据了！

## 4.HDFS TB级文件上传

### **Chunk缓冲机制**

首先，数据会被写入一个chunk缓冲数组，这个**chunk是一个512字节大小的数据片段**，然后这个缓冲数组可以容纳多个chunk大小的数据在里面缓冲。

这个缓冲，首先就可以让客户端快速的写入数据了，不至于说几百字节就要进行一次网络传输.

### **Packet数据包机制**

接着，当chunk缓冲数组都写满了之后，就会把这个chunk缓冲数组进行一下**chunk切割**，切割为一个一个的chunk，一个chunk是一个数据片段。然后多个chunk会直接一次性写入另外一个内存缓冲数据结构，就是**Packet数据包**。

一个Packet数据包，设计为可以容纳127个chunk，大小大致为64mb。所以说大量的chunk会不断的写入Packet数据包的内存缓冲中。

通过这个Packet数据包机制的设计，又可以在内存中容纳大量的数据，**进一步避免了频繁的网络传输影响性能。**

### **内存队列异步发送机制**

**当一个Packet被塞满了chunk之后，就会将这个Packet放入一个内存队列来进行排队。**

然后有一个DataStreamer线程会不断的获取队列中的Packet数据包，通过网络传输直接写一个Packet数据包给DataNode。

如果一个Block默认是128mb的话，那么一个Block默认会对应两个Packet数据包，每个Packet数据包是64MB。

也就是说，**传送两个Packet数据包给DataNode之后，就会发一个通知说，一个Block的数据都传输完毕。**

这样DataNode就知道自己收到一个Block了，里面包含了人家发送过来的两个Packet数据包。

 

### 

