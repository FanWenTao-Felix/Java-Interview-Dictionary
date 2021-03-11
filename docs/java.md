# java基础

- 基础语法
- [集合](/docs/集合.md)
  - List：可重复
    - ArrayList（数组）
    - Vector（数组实现、线程同步）
    - LinkList（链表）
  - Queue
  - Set：不可重复
    - HashSet（Hash表）
    - TreeSet（二叉树）
    - LinkHashSet（HashSet+LinkedHashMap）
  - Map
    - HashMap（数组+链表+红黑树）
    - ConcurrentHashMap
    - HashTable（线程安全）
    - TreeMap（可排序）
    - LinkHashMap（记录插入顺序）
- [异常](/docs/异常.md)
- [反射](/docs/反射.md)
- [注解](/docs/注解.md)
- [内部类](/docs/内部类.md)
- [泛型](/docs/泛型.md)
- [序列化](/docs/序列化.md)

# JVM

- [JVM的运行机制](/docs/JVM的运行机制.md)
- [JVM的内存区域](/docs/JVM的内存区域.md)
- [JVM的运行时内存](/docs/JVM的运行时内存.md)
- [垃圾回收与算法](/docs/垃圾回收与算法.md)
- [Java中的4种引用类型](/docs/Java中的4种引用类型.md)
- [垃圾收集算法](/docs/垃圾收集算法.md)
- [Java网络编程模型](/docs/Java网络编程模型.md)
- [JVM的类加载阶段](/docs/JVM的类加载阶段.md)

# 并发编程

- 线程
- 线程池
- 锁
- [synchronized](/docs/synchronized.md)
- 阻塞队列
- 
- 
  - 线程的创建方式
  - 线程的生命周期
  - 线程的基本方法
- 
  - 线程池的工作原理
  - 5种常用的线程池
- 5种常用的线程池
  - newCachedThreadPool
  - newFixedThreadPool
  - newScheduledThreadPool
  - newSingleThreadExecutor
  - newWorkStealingPool
- Java中的锁
  - 乐观锁
  - 悲观锁
  - 自旋锁
  - synchronized
  - ReentrantLock
  - synchronized和ReentrantLock的比较
  - Semaphore
  - AtomicInteger
  - 可重入锁
  - 公平锁与非公平锁
  - 读写锁：ReadWriteLock
  - 共享锁和独占锁
  - 重量级锁和轻量级锁
  - 偏向锁
  - 分段锁
  - 同步锁与死锁
  - 如何进行锁优化
- 线程上下文切换
  - 上下文切换
  - 引起线程上下文切换的原因
- Java阻塞队列
  - 阻塞队列的主要操作
  - Java中的阻塞队列实现
- Java并发关键字
  - CountDownLatch
  - CyclicBarrier
  - Semaphore
  - volatile关键字的作用
- 多线程如何共享数据
  - 将数据抽象成一个类，并将对这个数据的操作封装在类的方法中
  - 将Runnable对象作为一个类的内部类，将共享数据作为这个类的成员变量
- ConcurrentHashMap并发
  - 减小锁粒度
  - ConcurrentHashMap的实现
- Java中的线程调度
  - 抢占式调度
  - 协同式调度
  - Java线程调度的实现：抢占式
  - 线程让出CPU的情况
- 进程调度算法
  - 优先调度算法
  - 高优先权优先调度算法
  - 时间片的轮转调度算法
- CAS
  - CAS的概念：比较并交换
  - CAS的特性：乐观锁
  - CAS自旋等待
- ABA问题
- AQS
  - AQS的原理
  - state：状态
  - AQS共享资源的方式：独占式和共享式

# 数据结构

- 栈
- 队列
- 链表
  - 链表的特点
  - 单向链表的操作及其Java实现
  - 双向链表及其Java实现
  - 循环链表
- 散列表
  - 常用的构造散列函数
  - Hash的应用
- 二叉排序树
  - 插入操作
  - 删除操作
  - 查找操作
  - 用Java实现二叉排序树
- 红黑树
  - 红黑树的特性
  - 红黑树的左旋
  - 红黑树的右旋
  - 红黑树的添加
  - 红黑树的删除
- 图
  - 无向图和有向图
  - 图的存储结构：邻接矩阵
  - 图的存储结构：邻接表
  - 图的遍历
- 位图
  - 位图的数据结构
  - 位图的Java实现

# Java中的常用算法

- 二分查找算法
- 冒泡排序算法
- 插入排序算法
- 快速排序算法
- 希尔排序算法
- 归并排序算法
- 桶排序算法
- 基数排序算法
- 其他算法

# 网络与负载均衡

- 网络
  - OSI七层网络模型
  - TCP/IP四层网络模型
  - TCP三次握手/四次挥手
  - HTTP的原理
  - CDN的原理
- 负载均衡
  - 四层负载均衡与七层负载均衡的对比
  - 负载均衡算法
  - LVS的原理及应用
  - Nginx反向代理与负载均衡

# 数据库及分布式事务

- 数据库的基本概念及原则
  - 存储引擎
  - 创建索引的原则
  - 数据库三范式
  - 数据库事务
  - 存储过程
  - 触发器
- 数据库的并发操作和锁
  - 数据库的并发策略
  - 数据库锁
  - 数据库分表
- 数据库分布式事务
  - CAP
  - 两阶段提交协议
  - 三阶段提交协议
  - 分布式事务

# 分布式缓存的原理及应用

- 分布式缓存介绍
- Ehcache的原理及应用
  - Ehcache的原理
  - Ehcache的应用
- Redis的原理及应用
  - Redis的原理
  - Redis的应用
- 分布式缓存设计的核心问题
  - 缓存预热
  - 缓存更新
  - 缓存淘汰策略
  - 缓存雪崩
  - 缓存穿透
  - 缓存降级

# 设计模式

- 设计模式简介
- 工厂模式
- 抽象工厂模式
- 单例模式
- 建造者模式
- 原型模式
- 适配器模式
- 装饰者模式
- 代理模式
- 外观模式
- 桥接模式
- 组合模式
- 享元模式
- 策略模式
- 模板方法模式
- 观察者模式
- 迭代器模式
- 责任链模式
- 命令模式
- 备忘录模式’
- 状态模式
- 访问者模式
- 中介者模式
- 解释器模式

# Spring

- Spring的特性
  - 轻量
  - 控制反转
  - 面向容器
  - 面向切面
  - 框架灵活
- Spring的模块
  - 核心容器层
  - 数据访问层
  - Web应用层
  - 其他重要模块
- Spring的核心JAR包
- Spring的注解
  - Spring 注解的使用
  - Spring的常用注解
- Spring loC的原理
  - Spring loC简介
  - Spring Bean的装配流程
  - Spring Bean的作用域
  - Spring Bean的生命周期
  - Spring的4种依赖注入
  - 自动装配的5种方式
- Spring AOP的原理
  - Spring AOP简介
  - AOP的核心概念
  - AOP的2种代理方式
  - AOP的5种通知类型
  - AOP的代码实现
- Spring MVC的原理
- 事务
  - 本地事务
  - 分布式事务
  - 两阶段提交协议
- MyBatis的缓存
  - MyBatis的一级缓存原理
  - MyBatis的二级缓存原理
- Spring的生态
  - Spring Data 
  - Spring的其他服务

# Spring Cloud原理及应用

- Spring Boot 
  - Spring Boot的使用
  - Spring Boot Application Starters
  - Spring Boot的常用组件及其使用
- Spring Cloud Config
  - Spring Cloud Config的原理
  - Config Server的定义和使用
  - Config Client 
- Spring Cloud Eureka
  - Spring Cloud Eueka的原理
  - Spring Cloud Eureka的使用
- Spring Cloud Consul
  - Spring Cloud Consul的原理
  - Spring Cloud Consul的使用
- Spring Cloud Feign
  - Feign的应用
  - Feign的常用注解
- Spring Cloud Hystrix
  - Hystrix的特性
  - Hystrix的服务降级流程
  - Hystrix的使用
  - 异步请求
  - Hystrix的常用配置
  - HystixDashboard
- Spring Cloud Zuul
  - Zuul的概念和特点
  - Zuul的原理
  - Zuul的使用
  - PreRequestFilter的定义和注入
  - Fallback Provider的服务容错
- Spring Cloud的链路监控
  - Sleuth+Zipkin 
  - Pinpoint

# Netty网络编程原理及应用

- Reactor线程模型
  - Java NIO 
  - Reactor单线程模型
  - Reactor多线程模型
  - Reactor主从多线程模型
- Netty的架构
  - Netty简介
  - Netty的架构设计
  - Netty的核心组件
  - Netty的原理
- Netty的特性
  - I/O多路复用模型
  - 数据零拷贝
  - 内存重用机制
  - 无锁化设计
  - 高性能的序列化框架
- Netty的使用

# ZooKeeper原理及应用

- ZooKeeper的原理
  - ZooKeeper的角色
  - ZAB协议
  - ZooKeeper的选举机制和流程
- ZooKeeper的应用
  - ZooKeeper的数据模型
  - ZooKeeper的安装
  - Zookeeper的应用场景
  - ZooKeeper节点的操作

# Kafka原理及应用

- Kafka的原理
  - Kafka的组成
  - Kafka的数据存储设计
  - 生产者并发设计
  - 消费者并发设计
- Kafka的应用
  - Kafka的安装
  - 基于SpringBoot的Kafka应用

# Hadoop原理及应用

- HDFS
  - Client
  - NameNode
  - Secondary NameNode 
  - DataNode 
- MapReduce
  - Job Client 
  - JobTracker 
  - TaskTracker 
  - Task
  - Reduce Task的执行过程
  - Hadoop MapReduce的作业生命周期
- YARN 
  - ResouceManager
  - NodeManager
  - ApplicationMaster 
  - Container 
  - YARN的任务提交和运行流程
- Hadoop的安装和应用

# HBase原理及应用

- HBase的原理
  - HBase的概念
  - 行式存储和列式存储
  - HBase列式存储的数据模型
  - HBase的架构组成
  - HBase的数据读写流程
- HBase的使用
  - HBase的安装
  - 基于SpringBoot的HBase应用

# Cassandra原理及应用

- Cassandra的原理
  - Cassandra的特点
  - Cassandra的数据模型
  - Gossip协议
  - NWR理论
  - 一致性Hash
  - Cassandra的数据副本策略
  - Cassandra数据存储
  - Cassandra的数据写入
  - Cassandra的数据读取
  - Cassandra的数据删除机制
  - Cassandra和HBase的核心差别
- Cassandra的应用

# ElasticSearch原理及应用

- ElasticSearch的概念和原理
  - Lucene的介绍
  - ElasticSearch的特点
  - ElasticSearch的应用场景
  - ElasticSearch的数据模型
  - ElasticSearch分布式架构
  - ElasticSearch的写操作流程
  - ElasticSearch的读操作流程
  - ElasticSearch中的Translog
  - ElasticSearch段合并
  - ElasticSearch的集群扩容
- ElasticSearch的应用
  - ElasticSearch的安装
  - ElasticSearch的配置和性能调优
  - ElasticSearch API的使用

# Spark原理及应用

- Spark的原理
  - Spark的特点
  - Spark的模块
  - Spark的运行原理
- Spark的使用
  - Spark的安装
  - Spark RDD的使用
  - Spark Streaming的使用
  - Spark SQL、DataFrame、DataSet的使用
  - Spark Structured Streaming的使用

# Flink原理及应用

- Flink的原理
  - Flink的特点
  - Flink的核心概念
  - Flink架构介绍
  - Flink的事件驱动模型
  - Flink的数据分析应用
  - Flink的数据清洗和数据管道
  - Flink数据流处理基本概念
  - API分类
  - Flink基于状态的内存计算
  - Flink的编程模型
  - Flink窗口计算
  - Flink故障恢复
- Flink的应用
  - Flink的安装
  - Flink实战案例

