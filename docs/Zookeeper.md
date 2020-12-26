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

## 7.ZooKeeper集群的三种角色：Leader、Follower、Observer

![image.png](D:\workspace\Java-Interview-Dictionary\images\zookeeper001.png)

通常来说ZooKeeper集群里有三种角色的机器.

集群启动自动选举一个Leader出来，只有Leader是可以写的，Follower是只能同步数据和提供数据的读取，Leader挂了，Follower可以继续选举出来Leader，Observer也只能读但是Observer不参与选举.

## 8.ZooKeeper的数据模型：znode和节点类型 ![image.png](D:\workspace\Java-Interview-Dictionary\images\zookeeper002.png)

核心数据模型就是znode树，平时我们往zk写数据就是创建树形结构的znode，里面可以写入值，就这数据模型，都在zk内存里存放.

有两种节点，持久节点和临时节点，持久节点就是哪怕客户端断开连接，一直存在.

临时节点，就是只要客户端断开连接，节点就没了.

还有顺序节点，就是创建节点的时候自增加全局递增的序号.加锁的时候，是创建一个临时顺序节点.

zk会自动给你的临时节点加上一个后缀，全局递增的，编号.

如果你客户端断开连接了，就自动销毁这个你加的锁，此时人家会感知到，就会尝试去加锁.

如果你是做元数据存储，肯定是持久节点.

如果你是做一些分布式协调和通知，很多时候是用临时节点，就是说，比如我创建一个临时节点，别人来监听这个节点的变化，如果我断开连接了，临时节点消失，此时人家会感知到，就会来做点别的事情.

顺序节点，在分布式锁里用的比较经典.

每个znode还有一个Stat用来存放数据版本，version（znode的版本），cversion（znode子节点的版本），aversion（znode的ACL权限控制版本）.

## 9.ZooKeeper最核心的一个机制：Watcher监听回调

ZooKeeper最核心的机制，就是你一个客户端可以对znode进行Watcher监听，然后znode改变的时候回调通知你的这个客户端，这个是非常有用的一个功能，在分布式系统的协调中是很有必要的.

支持写和查：只能实现元数据存储，Master选举，部分功能.

分布式系统的协调需求：分布式架构中的系统A监听一个数据的变化，如果分布式架构中的系统B更新了那个数据/节点，zk反过来通知系统A这个数据的变化.

使用zk很简单，内存数据模型（不同节点类型）；写数据，主动读取数据；监听数据变化，更新数据，反向通知数据变化.

 实现分布式集群的集中式的元数据存储、分布式锁、Master选举、分布式协调监听.

## 10.zk到底通过什么协议在集群间进行数据一致性同步？

用的是特别设计的ZAB协议，ZooKeeper Atomic Broadcast，就是ZooKeeper原子广播协议.

通过这个协议来进行zk集群间的数据同步，保证数据的强一致性.

## 11.zk集群启动到数据同步再到崩溃恢复

只有leader可以接受写请求，但是客户端可以随便连接leader或者follower，如果客户端连接到follower，follower会把写请求转发给leader.

leader收到写请求，就把请求同步给所有的follower，过半follower都说收到了，就再发commit给所有的follower，让大家提交这个请求事务.

如果突然leader宕机了，会进入恢复模式，重新选举一个leader，只要过半的机器都承认你是leader，就可以选举出来一个leader，所以zk很重要的一点是主要宕机的机器数量小于一半，他就可以正常工作.

**只要有超过一半的机器，认可你是leader，你就可以被选举为leader**.

新leader重新等待过半follower跟他同步，完了重新进入消息广播模式.

集群启动：恢复模式，leader选举（过半机器选举机制） + 数据同步.

消息写入：消息广播模式，leader采用2PC模式的过半写机制，给follower进行同步.

崩溃恢复：恢复模式，leader/follower宕机，只要剩余机器超过一半，集群宕机不超过一半的机器，就可以选举新的leader，数据同步.

## 12.采用了2PC两阶段提交思想的ZAB消息广播流程

每一个消息广播的时候，都是2PC思想走的，先是发起事务Proposal的广播，就是事务提议，仅仅只是个提议而已，各个follower返回ack，过半follower都ack了，就直接发起commit消息到全部follower上去，让大家提交.

发起一个事务proposal之前，leader会分配一个全局唯一递增的事务id，zxid，通过这个可以严格保证顺序.

leader会为每个follower创建一个队列，里面放入要发送给follower的事务proposal，这是保证了一个同步的顺序性.

每个follower收到一个事务proposal之后，就需要立即写入本地磁盘日志中，写入成功之后就可以保证数据不会丢失了，然后返回一个ack给leader，然后过半follower都返回了ack，leader推送commit消息给全部follower.

leader自己也会进行commit操作.

commit之后，就意味这个数据可以被读取到了.

## 13.ZAB协议下一种可能存在的数据一致性问题

Leader收到了过半的follower的ack，接着leader自己commit了，还没来得及发送commit给所有follower自己就挂了，这个时候相当于leader的数据跟所有follower是不一致的，你得保证全部follower最终都得commit.

另外一个，leader可能会自己收到了一个请求，结果没来得及发送proposal给所有follower之前就宕机了，此时这个Leader上的请求应该是要被丢弃掉的. 

所以在leader崩溃的时候，就会选举一个拥有事务id最大的机器作为leader，他得检查事务日志，如果发现自己磁盘日志里有一个proposal，但是还没提交，说明肯定是之前的leader没来得及发送commit就挂了.

此时他就得作为leader为这个proposal发送commit到其他所有的follower中去，这个就保证了之前老leader提交的事务已经会最终同步提交到所有follower里去.

然后对于第二种情况，如果老leader自己磁盘日志里有一个事务proposal，他启动之后跟新leader进行同步，发现这个事务proposal其实是不应该存在的，就直接丢弃掉就可以了.

## 14.崩溃恢复时选举出来的Leader是如何跟其他Follower进行同步的？

新选举出来一个leader之后，本身人家会挑选已经收到的事务zxid里最大的那个follower作为新的leader。

其他的follower就会跟他进行同步，他给每个follower准备一个队列，然后把所有的proposal都发送给follower，只要过半follower都ack了，就会发送commit给那个follower.

所谓的commit操作，就是把这条数据加入内存中的znode树形数据结构里去，然后就对外可以看到了，也会去通知一些监听这个znode的人.

如果一个follower跟leader完全同步了，就会加入leader的同步follower列表中去，然后过半follower都同步完毕了，就可以对外继续提供服务了.

## 15.对于需要丢弃的消息是如何在ZAB协议中进行处理的？

每一条事务的zxid是64位的，高32位是leader的epoch，就认为是leader的版本吧；低32位才是自增长的zxid.

如果一个leader自己刚把一个proposal写入本地磁盘日志，就宕机了，没来得及发送给全部的follower，此时新leader选举出来，他的epoch会自增长一位.

然后老leader恢复了连接到集群是follower了，此时发现自己比新leader多出来一条proposal，但是自己的epoch比新leader的epoch低了，所以就会丢弃掉这条数据.

启动的时候，过半机器选举leader，数据同步.

对外提供服务的时候，2PC + 过半写机制，顺序一致性（最终的一致性）. 

崩溃恢复，剩余机器过半，重新选举leader，有数据不一致的情况，针对两种情况自行进行处理，保证数据是一致的（磁盘日志文件、zxid的高32位）.

## 16.ZooKeeper的Observer节点是用来干什么的？

Observer节点是不参与leader选举的，他也不参与ZAB协议同步时候的过半follower ack的那个环节，他只是单纯的接收数据，同步数据，可能数据存在一定的不一致的问题，但是是只读的.

leader在进行数据同步的时候，observer是不参与到过半写机制里去.

zk集群无论多少台机器，只能是一个leader进行写，单机写入最多每秒上万QPS，这是没法扩展的，所以zk是适合写少的场景.

但是读呢？follower起码有2个或者4个，读你起码可以有每秒几万QPS，没问题，那如果读请求更多呢？此时你可以引入Observer节点，他就只是同步数据，提供读服务，可以无限的扩展机器.

## 17.ZooKeeper为什么只能是小集群部署？

假设你有1个leader + 20个follower，21台机器，你觉得靠谱吗？不靠谱，因为follower要参与到ZAB的写请求过半ack里去.

如果你有20个follower，一个写请求出去，要起码等待10台以上的Follower返回ack，才能发送commit，才能告诉你写请求成功了，性能是极差的.

所以zk的这个ZAB协议就决定了一般其实就是1个leader + 2个follower的小集群就够了，写请求是无法扩展的，读请求如果量大，可以加observer机器，最终就是适合读多写少的场景.

主要就是用于分布式系统的一些协调工作.

18.