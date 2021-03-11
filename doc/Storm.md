# Storm

## 1.概念

Storm是一个免费并开源的分布式实时计算系统。利用Storm可以很容易做到可靠地处理无限的数据流，像Hadoop批量处理大数据一样，Storm可以实时处理数据。

## 2.集群架构

![](D:\workspace\Java-Interview-Offer\images\storm001.png)

### Nimbus（master-代码分发给Supervisor）

Storm集群的Master节点，负责分发用户代码，指派给具体的Supervisor节点上的Worker节点，去运行Topology对应的组件（Spout/Bolt）的Task。

### Supervisor（slave-管理Worker进程的启动和终止）

Storm集群的从节点，负责管理运行在Supervisor节点上的每一个Worker进程的启动和终止。通过Storm的配置文件中的supervisor.slots.ports配置项，可以指定在一个Supervisor上最大允许多少个Slot，每个Slot通过端口号来唯一标识，一个端口号对应一个Worker进程（如果该Worker进程被启动）。

### Worker（具体处理组件逻辑的进程）

运行具体处理组件逻辑的进程。Worker运行的任务类型只有两种，一种是Spout任务，一种是Bolt任务。

### Task

worker中每一个spout/bolt的线程称为一个task. 在storm0.8之后，task不再与物理线程对应，不同spout/bolt的task可能会共享一个物理线程，该线程称为executor。

### ZooKeeper

用来协调Nimbus和Supervisor，如果Supervisor因故障出现问题而无法运行Topology，Nimbus会第一时间感知到，并重新分配Topology到其它可用的Supervisor上运行

## 3.编程模型（spout->tuple->bolt）

strom在运行中可分为spout与bolt两个组件，其中，数据源从spout开始，数据以tuple的方式发送到bolt，多个bolt可以串连起来，一个bolt也可以接入多个spot/bolt.运行时原理如下图：

![](D:\workspace\Java-Interview-Offer\images\storm002.png)

### Topology

Storm中运行的一个实时应用程序的名称。将Spout、Bolt整合起来的拓扑图。定义了Spout和Bolt的结合关系、并发数量、配置等等。

### Spout

在一个topology中获取源数据流的组件。通常情况下spout会从外部数据源中读取数据，然后转换为topology内部的源数据。

### Bolt

接受数据然后执行处理的组件,用户可以在其中执行自己想要的操作。

### Tuple

一次消息传递的基本单元，理解为一组消息就是一个Tuple。

### Stream

Tuple的集合。表示数据的流向。

## 4.Topology运行

在Storm中,一个实时应用的计算任务被打包作为Topology发布，这同HadoopMapReduce任务相似。但是有一点不同的是:在Hadoop中，MapReduce任务最终会执行完成后结束；而在Storm中，Topology任务一旦提交后永远不会结束，除非你显示去停止任务。计算任务Topology是由不同的Spouts和Bolts，通过数据流（Stream）连接起来的图｡一个Storm在集群上运行一个Topology时，主要通过以下3个实体来完成Topology的执行工作：

![](D:\workspace\Java-Interview-Offer\images\storm003.png)

### Worker(1个worker进程执行的是1个topology的子集)

1个worker进程执行的是1个topology的子集（注：不会出现1个worker为多个topology服务）。1个worker进程会启动1个或多个executor线程来执行1个topology的component(spout或bolt)。因此，1个运行中的topology就是由集群中多台物理机上的多个worker进程组成的。

### Executor(executor是1个被worker进程启动的单独线程)

executor是1个被worker进程启动的单独线程。每个executor只会运行1个topology的1个component(spout或bolt)的task（注：task可以是1个或多个，storm默认是1个component只生成1个task，executor线程里会在每次循环里顺序调用所有task实例）。

### Task(最终运行spout或bolt中代码的单元)

是最终运行spout或bolt中代码的单元（注：1个task即为spout或bolt的1个实例，executor线程在执行期间会调用该task的nextTuple或execute方法）。topology启动后，1个component(spout或bolt)的task数目是固定不变的，但该component使用的executor线程数可以动态调整（例如：1个executor线程可以执行该component的1个或多个task实例）。这意味着，对于1个component存在这样的条件：#threads<=#tasks（即：线程数小于等于task数目）。默认情况下task的数目等于executor线程数目，即1个executor线程只运行1个task。

### Storm Streaming Grouping

Storm中最重要的抽象，应该就是Stream grouping了，它能够控制Spot/Bolt对应的Task以什么样的方式来分发Tuple，将Tuple发射到目的Spot/Bolt对应的Task.

目前，Storm Streaming Grouping支持如下几种类型：

#### huffle Grouping 

随机分组，尽量均匀分布到下游Bolt中将流分组定义为混排。这种混排分组意味着来自Spout的输入将混排，或随机分发给此Bolt中的任务。shuffle grouping对各个task的tuple分配的比较均匀。

#### Fields Grouping 

按字段分组，按数据中field值进行分组；相同field值的Tuple被发送到相同的Task这种grouping机制保证相同field值的tuple会去同一个task。

#### All grouping ：广播

广播发送，对于每一个tuple将会复制到每一个bolt中处理。

#### Global grouping

全局分组，Tuple被分配到一个Bolt中的一个Task，实现事务性的Topology。Stream中的所有的tuple都会发送给同一个bolt任务处理，所有的tuple将会发送给拥有最小task_id的bolt任务处理。

#### None grouping ：不分组

不关注并行处理负载均衡策略时使用该方式，目前等同于shuffle grouping,另外storm将会把bolt任务和他的上游提供数据的任务安排在同一个线程下。

#### Directgrouping ：直接分组指定分组

由tuple的发射单元直接决定tuple将发射给那个bolt，一般情况下是由接收tuple的bolt决定接收哪个bolt发射的Tuple。这是一种比较特别的分组方法，用这种分组意味着消息的发送者指定由消息接收者的哪个task处理这个消息。只有被声明为Direct Stream的消息流可以声明这种分组方法。而且这种消息tuple必须使用emitDirect方法来发射。消息处理者可以通过TopologyContext来获取处理它的消息的taskid (OutputCollector.emit方法也会返回taskid)。

