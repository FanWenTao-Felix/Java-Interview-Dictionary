# YARN

### 1.概念

YARN 是一个资源管理、任务调度的框架，主要包含三大模块：ResourceManager（RM）、NodeManager（NM）、ApplicationMaster（AM）。其中，ResourceManager 负责所有资源的监控、分配和管理； ApplicationMaster 负责每一个具体应用程序的调度和协调；NodeManager 负责每一个节点的维护。对于所有的applications，RM 拥有绝对的控制权和对资源的分配权。而每个AM 则会和RM 协商资源，同时和NodeManager 通信来执行和监控task。

### 2.ResourceManager

1. ResourceManager 负责整个集群的资源管理和分配，是一个全局的资源管理系统。
2. NodeManager 以心跳的方式向ResourceManager 汇报资源使用情况（目前主要是CPU 和内存的使用情况）。RM 只接受NM 的资源回报信息，对于具体的资源处理则交给NM 自己处理。
3. YARN Scheduler 根据application 的请求为其分配资源，不负责application job 的监控、 追踪、运行状态反馈、启动等工作。

### 3.NodeManager

1. NodeManager 是每个节点上的资源和任务管理器，它是管理这台机器的代理，负责该节点程序的运行，以及该节点资源的管理和监控。YARN集群每个节点都运行一个NodeManager。
2. NodeManager 定时向ResourceManager 汇报本节点资源（CPU、内存）的使用情况和Container 的运行状态。当ResourceManager 宕机时NodeManager 自动连接RM 备用节点。
3. NodeManager 接收并处理来自ApplicationMaster 的Container 启动、停止等各种请求。

### 4.ApplicationMaster

用户提交的每个应用程序均包含一个ApplicationMaster，它可以运行在ResourceManager 以外的机器上。

1. 负责与RM 调度器协商以获取资源（用Container 表示）。
2. 将得到的任务进一步分配给内部的任务(资源的二次分配)。
3. 与NM 通信以启动/停止任务。
4. 监控所有任务运行状态，并在任务运行失败时重新为任务申请资源以重启任务。
5. 当前YARN 自带了两个ApplicationMaster 实现，一个是用于演示AM 编写方法的实例程序DistributedShell，它可以申请一定数目的Container 以并行运行一个Shell 命令或者Shell脚本；另一个是运行MapReduce 应用程序的AM—MRAppMaster。

注：RM 只负责监控AM，并在AM 运行失败时候启动它。RM 不负责AM 内部任务的容错，任务的容错由AM 完成。

### 5.YARN 运行流程

1. client 向RM 提交应用程序，其中包括启动该应用的ApplicationMaster 的必须信息，例如ApplicationMaster 程序、启动ApplicationMaster 的命令、用户程序等。
2. ResourceManager 启动一个container 用于运行ApplicationMaster。
3. 启动中的ApplicationMaster 向ResourceManager 注册自己，启动成功后与RM保持心跳。
4. ApplicationMaster 向ResourceManager 发送请求，申请相应数目的container。
5. ResourceManager 返回ApplicationMaster 的申请的containers 信息。申请成功的container，由ApplicationMaster 进行初始化。container 的启动信息初始化后，AM 与对应的NodeManager 通信，要求NM 启动container。AM 与NM 保持心跳，从而对NM 上运行的任务进行监控和管理。
6. container 运行期间，ApplicationMaster 对container 进行监控。container 通过RPC 协议向对应的AM 汇报自己的进度和状态等信息。
7. 应用运行期间，client 直接与AM 通信获取应用的状态、进度更新等信息。
8. 应用运行结束后，ApplicationMaster 向ResourceManager 注销自己，并允许属于它的container 被收回。