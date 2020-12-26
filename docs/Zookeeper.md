# Zookeeper

## 1.zookeeper 的使用场景

分布式协调

这个其实是 zookeeper 很经典的一个用法，简单来说，就好比，你 A 系统发送个请求到 mq，然后 B 系统消息消费之后处理了。那 A 系统如何知道 B 系统的处理结果？用 zookeeper 就可以实现分布式系统之间的协调工作。A 系统发送请求之后可以在 zookeeper 上**对某个节点的值注册个监听器**，一旦 B 系统处理完了就修改 zookeeper 那个节点的值，A 系统立马就可以收到通知，完美解决。

分布式锁

举个栗子。对某一个数据连续发出两个修改操作，两台机器同时收到了请求，但是只能一台机器先执行完另外一个机器再执行。那么此时就可以使用 zookeeper 分布式锁，一个机器接收到了请求之后先获取 zookeeper 上的一把分布式锁，就是可以去创建一个 znode，接着执行操作；然后另外一个机器也**尝试去创建**那个 znode，结果发现自己创建不了，因为被别人创建了，那只能等着，等第一个机器执行完了自己再执行。

元数据/配置信息管理

zookeeper 可以用作很多系统的配置信息的管理，比如 kafka、storm 等等很多分布式系统都会选用 zookeeper 来做一些元数据、配置信息的管理

HA 高可用性

这个应该是很常见的，比如 hadoop、hdfs、yarn 等很多大数据系统，都选择基于 zookeeper 来开发 HA 高可用机制，就是一个**重要进程一般会做主备**两个，主进程挂了立马通过 zookeeper 感知到切换到备用进程。

## 2.zk到底通过什么协议在集群间进行数据一致性同步？

在整个zk的架构和工作原理中，有一个非常关键的环节，就是zk集群的数据同步是用什么协议做的？其实用的是特别设计的ZAB协议，ZooKeeper Atomic Broadcast，就是ZooKeeper原子广播协议。

通过这个协议来进行zk集群间的数据同步，保证数据的强一致性。

## 3.ZAB的核心思想介绍：主从同步机制和崩溃恢复机制

协议的本质而言，划分集群角色，主从架构，Leader和Follower两种角色。

只有Leader可以接受写操作，Leader和Follower都可以读，Leader收到事务请求，转换为事务Proposal（提议）同步给所有的Follower，超过半数的Follower都说收到事务proposal了，Leader再给所有的Follower发一个Commit消息，让所有Follower提交一个事务。

而且如果Leader崩溃了，要重新选举Leader保证继续运行。

## 4.ZAB消息广播流程

每一个消息广播的时候，都是2PC思想走的，先是发起事务Proposal的广播，就是事务提议，仅仅只是个提议而已，各个follower返回ack，过半follower都ack了，就直接发起commit消息到全部follower上去，让大家提交。

发起一个事务proposal之前，leader会分配一个全局唯一递增的事务id，zxid，通过这个可以严格保证顺序。

leader会为每个follower创建一个队列，里面放入要发送给follower的事务proposal，这是保证了一个同步的顺序性。

每个follower收到一个事务proposal之后，就需要立即写入本地磁盘日志中，写入成功之后就可以保证数据不会丢失了，然后返回一个ack给leader，然后过半follower都返回了ack，leader推送commit消息给全部follower。

leader自己也会进行commit操作。

commit之后，就意味这个数据可以被读取到了。

## 5.ZooKeeper到底是强一致性还是最终一致性？

强一致性：只要写入一条数据，立马无论从zk哪台机器上都可以立马读到这条数据，强一致性，你的写入操作卡住，直到leader和全部follower都进行了commit之后，才能让写入操作返回，认为写入成功了。

此时只要写入成功，无论你从哪个zk机器查询，都是能查到的，强一致性。

最终一致性：写入一条数据，方法返回，告诉你写入成功了，此时有可能你立马去其他zk机器上查是查不到的，短暂时间是不一致的，但是过一会儿，最终一定会让其他机器同步这条数据，最终一定是可以查到的。

研究了ZooKeeper的ZAB协议之后，你会发现，其实过半follower对事务proposal返回ack，就会发送commit给所有follower了，只要follower或者leader进行了commit，这个数据就会被客户端读取到了。

那么有没有可能，此时有的follower已经commit了，但是有的follower还没有commit？绝对会的，所以有可能其实某个客户端连接到follower01，可以读取到刚commit的数据，但是有的客户端连接到follower02在这个时间还没法读取到。

所以zk不是强一致的，不是说leader必须保证一条数据被全部follower都commit了才会让你读取到数据，而是过程中可能你会在不同的follower上读取到不一致的数据，但是最终一定会全部commit后一致，让你读到一致的数据的。

zk官方给自己的定义：顺序一致性。

因此zk是最终一致性的，但是其实他比最终一致性更好一点，因为leader一定会保证所有的proposal同步到follower上都是按照顺序来走的，起码顺序不会乱。

但是全部follower的数据一致确实是最终才能实现一致的。

如果要求强一致性，可以手动调用zk的sync()操作。

## 6.ZooKeeper特性

集群模式部署 :一般奇数节点，因为你5台机器可以挂2台，6台机器也是挂2台，不能超过一半的机器挂掉，所以5台和6台效果一致，那奇数节点可以减少机器开销，小集群部署，读多写少。

主从架构：Leader、Follower、Observer

内存数据模型：znode，多种节点类型。

客户端跟zk进行长连接，TCP，心跳，维持session。

zxid，高32位，低32位。

ZAB协议，2PC，过半ack + 磁盘日志写，commit + 写内存数据结构。

支持Watcher机制，监听回调通知。

顺序一致性：消息按顺序同步，但是最终才会一致，不是强一致。

高性能，2PC中的过半写机制，纯内存的数据结构，znode。

高可用，follower宕机没影响，leader宕机有数据不一致问题，新选举的leader会自动处理，正常运行，但是在恢复模式期间，可能有一小段时间是没法写入zk的。

高并发，单机leader写，Observer可以线性扩展读QPS。