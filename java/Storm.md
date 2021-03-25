# Storm

### 1.概念

Storm 是一个免费并开源的分布式实时计算系统。利用Storm 可以很容易做到可靠地处理无限的数据流，像Hadoop 批量处理大数据一样，Storm 可以实时处理数据。

### 2.storm的特点是什么？

（1）支撑各种实时类的项目场景：实时处理消息以及更新数据库，基于最基础的实时计算语义和API（实时数据处理领域）；对实时的数据流持续的进行查询或计算，同时将最新的计算结果持续的推送给客户端展示，同样基于最基础的实时计算语义和API（实时数据分析领域）；对耗时的查询进行并行化，基于DRPC，即分布式RPC调用，单表30天数据，并行化，每个进程查询一天数据，最后组装结果。

storm做各种实时类的项目都ok。

（2）高度的可伸缩性：如果要扩容，直接加机器，调整storm计算作业的并行度就可以了，storm会自动部署更多的进程和线程到其他的机器上去，无缝快速扩容

扩容起来，超方便。

（3）数据不丢失的保证：storm的消息可靠机制开启后，可以保证一条数据都不丢。

数据不丢失，也不重复计算。

（4）超强的健壮性：从历史经验来看，storm比hadoop、spark等大数据类系统，健壮的多的多，因为元数据全部放zookeeper，不在内存中，随便挂都不要紧。

特别的健壮，稳定性和可用性很高。

（5）使用的便捷性：核心语义非常的简单，开发起来效率很高。

用起来很简单，开发API还是很简单的。

### 3.集群架构

Nimbus(master-代码分发给 Supervisor)

Storm 集群的Master 节点，负责分发用户代码，指派给具体的Supervisor 节点上的Worker 节点，去运行Topology 对应的组件（Spout/Bolt）的Task。

负责元数据的维护，资源调度，提交实时计算作业的入口。

Supervisor(slave-管理Worker 进程的启动和终止)

Storm 集群的从节点，负责管理运行在Supervisor 节点上的每一个Worker 进程的启动和终止。通过Storm 的配置文件中的supervisor.slots.ports 配置项，可以指定在一个Supervisor 上最大允许多少个Slot，每个Slot 通过端口号来唯一标识，一个端口号对应一个Worker 进程（如果该Worker 进程被启动）。

Worker(具体处理组件逻辑的进程)

运行具体处理组件逻辑的进程。Worker 运行的任务类型只有两种，一种是Spout 任务，一种是Bolt 任务。

Task

worker 中每一个spout/bolt 的线程称为一个task. 在storm0.8 之后，task 不再与物理线程对应，不同spout/bolt 的task 可能会共享一个物理线程，该线程称为executor。

ZooKeeper

用来协调Nimbus 和Supervisor，如果Supervisor 因故障出现问题而无法运行Topology，Nimbus 会第一时间感知到，并重新分配Topology 到其它可用的Supervisor 上运行

### 4.编程模型（spout->tuple->bolt）

strom 在运行中可分为spout 与bolt 两个组件，其中，数据源从spout 开始，数据以tuple 的方式发送到bolt，多个bolt 可以串连起来，一个bolt 也可以接入多个spot/bolt。

Topology

Storm 中运行的一个实时应用程序的名称。将 Spout、 Bolt 整合起来的拓扑图。定义了 Spout 和Bolt 的结合关系、并发数量、配置等等。

Spout

在一个topology 中获取源数据流的组件。通常情况下spout 会从外部数据源中读取数据，然后转换为topology 内部的源数据。

数据源的一个代码组件，就是我们可以实现一个spout接口，写一个java类，在这个spout代码中，我们可以自己尝试去数据源获取数据，比如说从kafka中消费数据。

Bolt

接受数据然后执行处理的组件,用户可以在其中执行自己想要的操作。

一个业务处理的代码组件，spout会将数据传送给bolt，各种bolt还可以串联成一个计算链条，java类实现了一个bolt接口。

一堆spout+bolt，就会组成一个topology，就是一个拓扑，实时计算作业，spout+bolt，一个拓扑涵盖数据源获取/生产+数据处理的所有的代码逻辑，topology。

Tuple

一次消息传递的基本单元，理解为一组消息就是一个Tuple。

就是一条数据，每条数据都会被封装在tuple中，在多个spout和bolt之间传递。

Stream

Tuple 的集合。表示数据的流向。

就是一个流，抽象的概念，源源不断过来的tuple，就组成了一条数据流。

### 5.Topology 运行

在Storm 中,一个实时应用的计算任务被打包作为Topology 发布，这同Hadoop MapReduce任务相似。但是有一点不同的是:在Hadoop 中，MapReduce 任务最终会执行完成后结束；而在Storm 中，Topology 任务一旦提交后永远不会结束，除非你显示去停止任务。计算任务Topology 是由不同的Spouts 和Bolts，通过数据流（Stream）连接起来的图｡一个Storm 在集群上运行一个Topology 时，主要通过以下3 个实体来完成Topology 的执行工作：

Worker(1 个worker 进程执行的是1 个topology 的子集)

1 个worker 进程执行的是1 个topology 的子集（注：不会出现1 个worker 为多个topology服务）。1 个worker 进程会启动1 个或多个executor 线程来执行1 个topology 的component(spout 或bolt)。因此，1 个运行中的topology 就是由集群中多台物理机上的多个worker 进程组成的。

Executor(executor 是1 个被worker 进程启动的单独线程)

executor 是1 个被worker 进程启动的单独线程。每个executor 只会运行1 个topology 的1 个component(spout 或bolt)的task（注：task 可以是1 个或多个，storm 默认是1 个component 只生成1 个task，executor 线程里会在每次循环里顺序调用所有task 实例）。

Task(最终运行spout 或bolt 中代码的单元)

是最终运行spout 或bolt 中代码的单元（注：1 个task 即为spout 或bolt 的1 个实例，executor 线程在执行期间会调用该task 的nextTuple 或execute 方法）。topology 启动后，1个component(spout 或bolt)的task 数目是固定不变的，但该component 使用的executor 线程数可以动态调整（例如：1 个executor 线程可以执行该component 的1 个或多个task 实例）。这意味着，对于1 个component 存在这样的条件：#threads<=#tasks（即：线程数小于等于task 数目）。默认情况下task 的数目等于executor 线程数目，即1 个executor 线程只运行1 个task。

### 6.Storm Streaming Grouping

Storm 中最重要的抽象，应该就是Stream grouping 了，它能够控制Spot/Bolt 对应的Task 以什么样的方式来分发Tuple，将Tuple 发射到目的Spot/Bolt 对应的Task.

目前，Storm Streaming Grouping 支持如下几种类型：

huffle Grouping

随机分组，尽量均匀分布到下游Bolt 中将流分组定义为混排。这种混排分组意味着来自Spout 的输入将混排，或随机分发给此Bolt 中的任务。shuffle grouping 对各个task 的tuple 分配的比较均匀。

Fields Grouping

按字段分组，按数据中field 值进行分组；相同field 值的Tuple 被发送到相同的Task 这种grouping 机制保证相同field 值的tuple 会去同一个task。

All grouping ：广播

广播发送， 对于每一个tuple 将会复制到每一个bolt 中处理。

Global grouping

全局分组，Tuple 被分配到一个Bolt 中的一个Task，实现事务性的Topology。Stream 中的所有的tuple 都会发送给同一个bolt 任务处理，所有的tuple 将会发送给拥有最小task_id 的bolt任务处理。

None grouping ：不分组

不关注并行处理负载均衡策略时使用该方式，目前等同于shuffle grouping,另外storm 将会把bolt 任务和他的上游提供数据的任务安排在同一个线程下。

Direct grouping ：直接分组 指定分组

由tuple 的发射单元直接决定tuple 将发射给那个bolt，一般情况下是由接收tuple 的bolt 决定接收哪个bolt 发射的Tuple。这是一种比较特别的分组方法，用这种分组意味着消息的发送者指定由消息接收者的哪个task 处理这个消息。 只有被声明为Direct Stream 的消息流可以声明这种分组方法。而且这种消息tuple 必须使用emitDirect 方法来发射。消息处理者可以通过TopologyContext 来获取处理它的消息的taskid (OutputCollector.emit 方法也会返回taskid)。

### 7.Storm的并行度以及流分组

并行度：Worker->Executor->Task。

对于一个拓扑来说，并行度就是task。

流分组：Task与Task之间的数据流向关系。

Shuffle Grouping：随机发射，负载均衡。 Fields Grouping：根据某一个，或者某些个，fields，进行分组，那一个或者多个fields如果值完全相同的话，那么这些tuple，就会发送给下游bolt的其中固定的一个task。

你发射的每条数据是一个tuple，每个tuple中有多个field作为字段。

比如tuple，3个字段，name，age，salary。

{"name": "tom", "age": 25, "salary": 10000} -> tuple -> 3个field，name，age，salary