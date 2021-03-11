# YARN

## 1.概念

YARN是一个资源管理、任务调度的框架，主要包含三大模块：ResourceManager（RM）、NodeManager（NM）、ApplicationMaster（AM）。其中，ResourceManager负责所有资源的监控、分配和管理；ApplicationMaster负责每一个具体应用程序的调度和协调；NodeManager负责每一个节点的维护。对于所有的applications，RM拥有绝对的控制权和对资源的分配权。而每个AM则会和RM协商资源，同时和NodeManager通信来执行和监控task。几个模块之间的关系如图所示。

![](D:\workspace\Java-Interview-Offer\images\YARN001.png)

## 2.ResourceManager

1.ResourceManager负责整个集群的资源管理和分配，是一个全局的资源管理系统。

2.NodeManager以心跳的方式向ResourceManager汇报资源使用情况（目前主要是CPU和内存的使用情况）。RM只接受NM的资源回报信息，对于具体的资源处理则交给NM自己处理。

3.YARN Scheduler根据application的请求为其分配资源，不负责application job的监控、追踪、运行状态反馈、启动等工作。

## 3.NodeManager

1.NodeManager是每个节点上的资源和任务管理器，它是管理这台机器的代理，负责该节点程序的运行，以及该节点资源的管理和监控。YARN集群每个节点都运行一个NodeManager。

2.NodeManager定时向ResourceManager汇报本节点资源（CPU、内存）的使用情况和Container的运行状态。当ResourceManager宕机时NodeManager自动连接RM备用节点。

3.NodeManager接收并处理来自ApplicationMaster的Container启动、停止等各种请求。

## 4.ApplicationMaster

用户提交的每个应用程序均包含一个ApplicationMaster，它可以运行在ResourceManager以外的机器上。

1.负责与RM调度器协商以获取资源（用Container表示）。

2.将得到的任务进一步分配给内部的任务(资源的二次分配)。

3.与NM通信以启动/停止任务。

4.监控所有任务运行状态，并在任务运行失败时重新为任务申请资源以重启任务。

5.当前YARN自带了两个ApplicationMaster实现，一个是用于演示AM编写方法的实例程序DistributedShell，它可以申请一定数目的Container以并行运行一个Shell命令或者Shell脚本；另一个是运行MapReduce应用程序的AM—MRAppMaster。

注：RM只负责监控AM，并在AM运行失败时候启动它。RM不负责AM内部任务的容错，任务的容错由AM完成。

## 5.YARN运行流程

1.client向RM提交应用程序，其中包括启动该应用的ApplicationMaster的必须信息，例如ApplicationMaster程序、启动ApplicationMaster的命令、用户程序等。

2.ResourceManager启动一个container用于运行ApplicationMaster。

3.启动中的ApplicationMaster向ResourceManager注册自己，启动成功后与RM保持心跳。

4.ApplicationMaster向ResourceManager发送请求，申请相应数目的container。

5.ResourceManager返回ApplicationMaster的申请的containers信息。申请成功的container，由ApplicationMaster进行初始化。container的启动信息初始化后，AM与对应的NodeManager通信，要求NM启动container。AM与NM保持心跳，从而对NM上运行的任务进行监控和管理。

6.container运行期间，ApplicationMaster对container进行监控。container通过RPC协议向对应的AM汇报自己的进度和状态等信息。

7.应用运行期间，client直接与AM通信获取应用的状态、进度更新等信息。

8.应用运行结束后，ApplicationMaster向ResourceManager注销自己，并允许属于它的container被收回。