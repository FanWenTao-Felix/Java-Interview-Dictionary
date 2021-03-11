# Docker

## 1.概念

| Docker 镜像(Images)    | Docker 镜像是用于创建Docker 容器的模板。                     |
| ---------------------- | ------------------------------------------------------------ |
| Docker 容器(Container) | 容器是独立运行的一个或一组应用。                             |
| Docker 客户端(Client)  | Docker 客户端通过命令行或者其他工具使用Docker API与Docker 的守护进程通信。 |
| Docker 主机(Host)      | 一个物理或者虚拟的机器用于执行Docker 守护进程和容器。        |
| Docker 仓库(Registry)  | Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。Docker Hub 提供了庞大的镜像集合供使用。 |
| Docker Machine         | Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、Digital Ocean、Microsoft Azure。 |

Docker 的出现一定是因为目前的后端在开发和运维阶段确实需要一种虚拟化技术解决开发环境和生产环境环境一致的问题，通过Docker 我们可以将程序运行的环境也纳入到版本控制中，排除因为环境造成不同运行结果的可能。但是上述需求虽然推动了虚拟化技术的产生，但是如果没有合适的底层技术支撑，那么我们仍然得不到一个完美的产品。本文剩下的内容会介绍几种Docker 使用的核心技术，如果我们了解它们的使用方法和原理，就能清楚Docker 的实现原理。Docker 使用客户端-服务器(C/S) 架构模式，使用远程API来管理和创建Docker容器。Docker 容器通过Docker 镜像来创建。

![](D:\workspace\Java-Interview-Offer\images\docker001.png)

## 2.Namespaces

命名空间（namespaces）是Linux 为我们提供的用于分离进程树、网络接口、挂载点以及进程间通信等资源的方法。在日常使用Linux 或者macOS 时，我们并没有运行多个完全分离的服务器的需要，但是如果我们在服务器上启动了多个服务，这些服务其实会相互影响的，每一个服务都能看到其他服务的进程，也可以访问宿主机器上的任意文件，这是很多时候我们都不愿意看到的，我们更希望运行在同一台机器上的不同服务能做到完全隔离，就像运行在多台不同的机器上一样。

Linux 的命名空间机制提供了以下七种不同的命名空间，包括CLONE_NEWCGROUP、CLONE_NEWIPC、CLONE_NEWNET、CLONE_NEWNS、CLONE_NEWPID、CLONE_NEWUSER 和CLONE_NEWUTS，通过这七个选项我们能在创建新的进程时设置新进程应该在哪些资源上与宿主机器进行隔离。

## 3.进程(CLONE_NEWPID 实现的进程隔离)

docker 创建新进程时传入CLONE_NEWPID 实现的进程隔离，也就是使用Linux 的命名空间实现进程的隔离，Docker 容器内部的任意进程都对宿主机器的进程一无所知。当我们每次运行docker run 或者docker start 时，都会在创建一个用于设置进程间隔离的Spec，同时会设置进程相关的命名空间，还会设置与用户、网络、IPC 以及UTS 相关的命名空间，所有命名空间相关的设置Spec 最后都会作为Create 函数的入参在创建新的容器时进行设置。

## 4.Libnetwork与网络隔离

如果Docker 的容器通过Linux 的命名空间完成了与宿主机进程的网络隔离，但是却有没有办法通过宿主机的网络与整个互联网相连，就会产生很多限制，所以Docker 虽然可以通过命名空间创建一个隔离的网络环境，但是Docker 中的服务仍然需要与外界相连才能发挥作用。

Docker整个网络部分的功能都是通过Docker 拆分出来的libnetwork 实现的，它提供了一个连接不同容器的实现，同时也能够为应用给出一个能够提供一致的编程接口和网络层抽象的容器网络模型。

libnetwork 中最重要的概念，容器网络模型由以下的几个主要组件组成，分别是Sandbox、Endpoint 和Network。在容器网络模型中，每一个容器内部都包含一个Sandbox，其中存储着当前容器的网络栈配置，包括容器的接口、路由表和DNS 设置，Linux 使用网络命名空间实现这个Sandbox，每一个Sandbox 中都可能会有一个或多个Endpoint，在Linux 上就是一个虚拟的网卡veth，Sandbox 通过Endpoint 加入到对应的网络中，这里的网络可能就是我们在上面提到的Linux 网桥或者VLAN。

每一个使用docker run 启动的容器其实都具有单独的网络命名空间，Docker 为我们提供了四种不同的网络模式，Host、Container、None 和Bridge模式。

在这一部分，我们将介绍Docker 默认的网络设置模式：网桥模式。在这种模式下，除了分配隔离的网络命名空间之外，Docker 还会为所有的容器设置IP 地址。当Docker 服务器在主机上启动之后会创建新的虚拟网桥docker0，随后在该主机上启动的全部服务在默认情况下都与该网桥相连。在默认情况下，每一个容器在创建时都会创建一对虚拟网卡，两个虚拟网卡组成了数据的通道，其中一个会放在创建的容器中，会加入到名为docker0 网桥中。

![](D:\workspace\Java-Interview-Offer\images\docker002.png)

## 5.资源隔离与CGroups

Control Groups（简称CGroups）能够隔离宿主机器上的物理资源，例如CPU、内存、磁盘I/O 和网络带宽。每一个CGroup 都是一组被相同的标准和参数限制的进程，不同的CGroup 之间是有层级关系的，也就是说它们之间可以从父类继承一些用于限制资源使用的标准和参数。

## 6.镜像与UnionFS

Linux 的命名空间和控制组分别解决了不同资源隔离的问题，前者解决了进程、网络以及文件系统的隔离，后者实现了CPU、内存等资源的隔离，但是在Docker 中还有另一个非常重要的问题需要解决-也就是镜像。

Docker 镜像其实本质就是一个压缩包，我们可以使用命令将一个Docker 镜像中的文件导出，你可以看到这个镜像中的目录结构与Linux 操作系统的根目录中的内容并没有太多的区别，可以说Docker 镜像就是一个文件。

## 7.存储驱动

Docker 使用了一系列不同的存储驱动管理镜像内的文件系统并运行容器，这些存储驱动与Docker 卷（volume）有些不同，存储引擎管理着能够在多个容器之间共享的存储。

当镜像被docker run 命令创建时就会在镜像的最上层添加一个可写的层，也就是容器层，所有对于运行时容器的修改其实都是对这个容器读写层的修改。

容器和镜像的区别就在于，所有的镜像都是只读的，而每一个容器其实等于镜像加上一个可读写的层，也就是同一个镜像可以对应多个容器

UnionFS 其实是一种为Linux 操作系统设计的用于把多个文件系统『联合』到同一个挂载点的文件系统服务。而AUFS 即Advanced UnionFS 其实就是UnionFS 的升级版，它能够提供更优秀的性能和效率。

AUFS 只是Docker 使用的存储驱动的一种，除了AUFS 之外，Docker 还支持了不同的存储驱动，包括aufs、devicemapper、overlay2、zfs 和vfs 等等，在最新的Docker 中，overlay2 取代了aufs 成为了推荐的存储驱动，但是在没有overlay2 驱动的机器上仍然会使用aufs 作为Docker 的默认驱动。