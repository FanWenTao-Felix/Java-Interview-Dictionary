# Redis

### 1.Redis

Redis是一个开源（BSD许可）的内存中的数据结构存储系统，可以用作数据库、缓存和消息中间件，支持多种类型的数据结构，例如String（字符串）、Hash（散列）、List（列表）、Set（集合）、ZSet（有序集合）、Bitmap（位图）、HyperLogLog（超级日志）和Geospatial（地理空间）。Redis内置了复制、Lua脚本、LRU驱动事件、事务和不同级别的磁盘持久化，并通过Redis哨兵（Sentinel）模式和集群模式（Cluster）提供高可用性（High Availability）。

Redis不但支持丰富的数据类型，还支持分布式事务、数据分片、数据持久化等功能，是分布式系统中不可或缺的内存数据库服务。

### 2.Redis的数据类型

Redis 支持String 、Hash 、List 、Set 、ZSet 、Bitmap 、HyperLogLog和Geospatial这 8种数据类型。

（1）String

String是Redis基本的数据类型，一个key对应一个value。String类型的值最大能存储 512MB数据。Redis的String数据类型支持丰富的操作命令，常用的String操作命令如下所示。

- Setnx：只有在key不存在时才设置key的值
- Getrange：返回key中字符串值的子字符
- Mset：同时设置一个或多个key-value对
- Setex：将值value关联到key，并将key的过期时间设为seconds（以秒为单位）
- Set：设置指定Key的值
- Get：获取指定key的值
- Getbit：获取key所对应的字符串值指定偏移量上的位（bit）
- Setbit：设置或清除key所对应的字符串值指定偏移量上的位（bit）
- Decr：将key中存储的数字值减1
- Decrby：将key所对应的值减去给定的减量值（Decrement）
- Strlen：返回key所存储的字符串值的长度
- Msetnx：同时设置一个或多个key-value对，在且仅在所有给定的key都不存在时
- Incrby：将key所存储的值加上给定的增量值
- Incrbyfloat：将key所存储的值加上给定的浮点增量值
- Setrange：用value参数覆写给定key所存储的字符串值，从偏移量offset开始
- Psetex：和Setex相似，但它以毫秒为单位设置key的生存时间，而不是像Setex那样以秒为单位
- Append：如果key已经存在并且是一个字符串，则Append将value追加到该字符串的末尾
- Getset：将给定key的值设为value，并返回key的旧值（old value）
- Mget：获取（一个或多个）给定的key的值
- Incr：将在key中存储的数字值加1

（ 2 ） Hash

Redis Hash 是一个键值（ key->value ） 对集合。

- Hmset：同时将多个field-value（域-值）对设置到散列表key中
- Hmget：获取所有给定字段的值
- Hset：将散列表key中field字段的值设为value
- Hgetall：获取散列表中指定key的所有字段和值
- Hget：获取存储在散列表中指定字段的值
- Hexists：查看散列表key中指定的字段是否存在
- Hincrby：为散列表key中指定字段的整数值加上增量increment
- Hlen：获取散列表中字段的数量
- Hdel：删除一个或多个散列表字段
- Hvals：获取散列表中的所有值
- Hincrbyfloat：为散列表key中指定字段的浮点数值加上增量increment
- Hkeys：获取所有散列表中的字段
- Hsetnx：只有在字段field不存在时，才设置散列表字段的值

（3）List

Redis List是简单的字符串列表，按照插入顺序排序。我们可以添加一个元素到列表的头部（左边）或者尾部（右边）。列表最多可存储 231-1（4 294 967 295≈4亿多）个元素。

- Lindex：通过索引获取列表中的元素
- Rpush：在列表中添加一个或多个值
- Lrange：获取列表指定范围内的值元素
- Rpoplpush：移除列表的最后一个元素，将该元素添加到另一个列表中并返回
- Blpop：移除并获取列表的第1个元素，如果列表没有元素，则会阻塞列表直到等待超时或发现可移除的元素
- Brpop：移除并获取列表的最后1个元素，如果列表没有元素，则会阻塞列表直到等待超时或发现可移除的元素
- Brpoplpush：从列表中弹出一个值，将弹出的元素插入另一个列表中并返回它，如果列表没有元素，则会阻塞列表直到等待超时或发现可移除的元素
- Lrem：移除列表元素
- Llen：获取列表长度
- Ltrim：对一个列表进行修剪，让列表只保留指定区间内的元素，不在指定区间内的元素都将被删除
- Lpop：移除并获取列表的第一个元素
- Lpushx：将一个或多个值插入已存在的列表头部
- Linsert：在列表的元素前或者后插入元素
- Rpop：移除并获取列表的最后一个元素
- Lset：通过索引设置列表元素的值
- Lpush：将一个或多个值插入列表头部
- Rpushx：为已存在的列表添加值

（4）Set

Set是String类型的无序集合。集合是通过散列表实现的，所以添加、删除、查找的复杂度都是O(1)。

- Sunion：返回所有给定集合的并集
- Scard：获取集合的成员数
- Srandmember：返回集合中的一个或多个随机数
- Smembers：返回集合中的所有成员
- Sinter：返回给定所有集合的交集
- Srem：移除集合中的一个或多个成员
- Smove：将member元素从source集合移动到destination集合
- Sadd：向集合中添加一个或多个成员
- Sismember：判断member元素是否是集合key的成员
- Sdiffstore：返回给定集合的差集并将其存储在destination中
- Sdiff：返回给定集合的差集
- Sscan：迭代集合中的元素
- Sinterstore：返回给定集合的交集并将其存储在destination中
- Sunionstore：将所有给定集合的并集都存储在destination中
- Spop：移除并返回集合中的一个随机元素

（5）ZSet

Redis ZSet和Set一样也是String类型元素的集合，且不允许有重复的成员，不同的是，每个元素都会关联一个double类型的分数。Redis正是通过分数来为集合中的成员进行从小到大的排序的。

- Zrevrank：返回有序集合中指定成员的排名，有序集合中的成员按分数值递减（从大到小）排序
- Zlexcount：在有序集合中计算指定字典区间内的成员数量
- Zunionstore：计算给定的一个或多个有序集的并集，并将其存储在新的key中
- Zremrangebyrank：移除有序集合中给定的排名区间的所有成员
- Zcard：获取有序集合的成员数
- Zrem：移除有序集合中的一个或多个成员
- Zinterstore：计算给定的一个或多个有序集合的交集并将结果集存储在新的有序集合key中
- Zrank：返回有序集合中指定成员的索引
- Zincrby：在有序集合中对指定成员的分数加上增量increment
- Zrangebyscore：通过分数返回有序集合指定区间内的成员
- Zrangebylex：通过字典区间返回有序集合的成员
- Zscore：返回有序集合中成员的分数值
- Zremrangebyscore：移除有序集合中给定分数区间内的所有成员
- Zscan：迭代有序集合中的元素（包括元素成员和元素分值）
- Zrevrangebyscore：返回有序集合中指定分数区间内的成员，分数从高到低排序
- Zremrangebylex：移除有序集合中给定字典区间内的所有成员
- Zrevrange：返回有序集合中指定区间内的成员，通过索引按分数从高到低排序
- Zrange：通过索引区间返回有序集合成指定区间内的成员
- Zcount：计算有序集合中指定分数区间内的成员数量
- Zadd：向有序集合添加一个或多个成员，或者更新已存在成员的分数

（6）Bitmap

通过操作二进制位记录数据。

- setbit：设置Bitmap值
- getbit：获取Bitmap值
- bitcount：获取指定范围内值为1的个数
- destkey：对Bitmap进行操作，可以是add（交集）、or（并集）、not（非集）或xor（异或）

（7）HyperLogLog

被用于估计一个Set中元素数量的概率性的数据结构。

- PFADD：添加指定的元素到HyperLogLog中
- PFCOUNT：返回给定HyperLogLog的基数估算值
- PFMERGE：将多个HyperLogLog合并为一个HyperLogLog

（8）Geospatial

用于地理空间关系计算。

- GEOHASH：返回一个或多个位置元素的Geohash表示
- GEOPOS：从key里返回所有给定位置的元素的位置（经度和纬度）
- GEODIST：返回两个给定位置之间的距离
- GEORADIUS：以给定的经纬度为中心，找出某一半径内的元素
- GEOADD：将指定的地理空间位置（纬度、经度、名称）添加到指定的key中
- GEORADIUSBYMEMBER：找出位于指定范围内的元素，中心点由给定的位置元素决定

### 3.Redis管道

Redis是基于请求/响应协议的TCP服务。在客户端向服务器发送一个查询请求后，需要监听Socket的返回，该监听过程一直阻塞，直到服务器有结果返回。由于Redis集群是部署在多个服务器上的，所以Redis的请求/响应模型在每次请求时都要跨网络在不同的服务器之间传输数据，这样每次查询都存在一定的网络延迟（服务器之间的网络延迟一般在 20ms左右）。由于服务器一般采用多线程处理业务，并且内存操作效率很高，所以如果一次请求延迟 20ms，则多次请求的网络 延迟会不断累加。也就是说，在分布式环境下，Redis的性能瓶颈主要体现在网络延迟上。 Redis的管道技术指在服务端未响应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应。管道技术能减少客户端和服务器交互的次数，将客户端的请求批量发送给服务器，服务器针对批量数据分别查询并统一回复，能显著提高Redis的性能。

### 4.Redis的事务

Redis支持分布式环境下的事务操作，其事务可以一次执行多个命令，事务中的所有命令都会序列化地顺序执行。事务在执行过程中，不会被其他客户端发送来的命令请求打断。服务器在执行完事务中的所有命令之后，才会继续处理其他客户端的其他命令。Redis的事务操作分为开启事务、命令入队列、执行事务三个阶段。Redis的事务执行流程如下。 （1）事务开启：客户端执行Multi命令开启事务。 （2）提交请求：客户端提交命令到事务。 （3）任务入队列：Redis将客户端请求放入事务队列中等待执行。 （4）入队状态反馈：服务器返回QURUD，表示命令已被放入事务队列。 （5）执行命令：客户端通过Exec执行事务。 （6）事务执行错误：在Redis事务中如果某条命令执行错误，则其他命令会继续执行，不会回滚。可以通过Watch监控事务执行的状态并处理命令执行错误的异常情况。 （7）执行结果反馈：服务器向客户端返回事务执行的结果。

Redis 事务的相关命令有Multi 、Exec 、Discard 、Watch 和Unwatch。

- Multi ：标记一个事务块的开始
- Exec ：执行所有事务块内的命令
- Discard： 取消事务，放弃执行事务块内的所有命令
- Watch ：监视一个（或多个）key，如果在事务执行之前这个（或这些）key被其他命令改动，那么事务将被打断
- Unwatch：取消Watch 命令对所有key的监视

```html
//开启事务权限
redisTemplate.setRnableTransactionSupport(true);
//开启事务
redisTemplate.muiti();
执行事务命令
redisTemplate.opsForValue().set(key,value);
//成功就提交
redisTemplate.exec();
//失败就回滚
redisTemplate.discard();
```

### 5.Redis发布、订阅

Redis发布、订阅是一种消息通信模式：发送者（Pub）向频道（Channel）发送消息，订阅者（Sub）接收频道上的消息。Redis客户端可以订阅任意数量的频道，发送者也可以向任意频道发送数据。如果3个订阅者sub1、sub2、sub3都订阅了频道channel0，在发送者pub1向频道channel0发送一条消息后，这条消息就会被发送给订阅它的三个客户端。 Redis常用的消息订阅与发布命令如下所示。

- PSUBSCRIBE：订阅一个或多个符合给定模式的频道
- PUBSUB：查看订阅与发布系统的状态
- PUBLISH：将信息发送到指定的频道
- SUBSCRIBE：订阅给定的一个或多个频道的信息
- UNSUBSCRIBE：退订指定的频道

### 6.Redis集群数据复制的原理

Redis提供了复制功能，可以实现在主数据库（Master）中的数据更新后，自动将更新的数据同步到从数据库（Slave）。一个主数据库可以拥有多个从数据库，而一个从数据库只能拥有一个主数据库。Redis的主从数据复制原理如下。 （1）一个从数据库在启动后，会向主数据库发送SYNC命令。 （2）主数据库在接收到SYNC命令后会开始在后台保存快照（即RDB持久化的过程），并将保存快照期间接收到的命令缓存起来。在该持久化过程中会生成一个.rdb快照文件。 （3）在主数据库快照执行完成后，Redis会将快照文件和所有缓存的命令以.rdb快照文件的形式发送给从数据库。 （4）从数据库收到主数据库的.rdb快照文件后，载入该快照文件到本地。 （5）从数据库执行载入后的.rdb快照文件，将数据写入内存中。以上过程被称为复制初始化。 （6）在复制初始化结束后，主数据库在每次收到写命令时都会将命令同步给从数据库，从而保证主从数据库的数据一致。 在Redis中开启复制功能时需要在从数据库配置文件中加入如下配 置，对主数据库无须进行任何配置：

```html
#slave of master_address master_port
slave of 127.0.0.1 9000
#如果master有密码，则需要设置masterauth
masterauth=123
```

在上述配置中，slaveof后面的配置分别为主数据库的IP地址和端口，在主数据库开启了密码认证后需要将masterauth设置为主数据库的密码，在配置完成后重启Redis，主数据库上的数据就会同步到从数据库上。

### 7.Redis的集群模式及工作原理

Redis有三种集群模式：主从模式、哨兵模式和集群模式。

主从模式

所有的写请求都被发送到主数据库上，再由主数据库将数据同步到从数据库上。主数据库主要用于执行写操作和数据同步，从数据库主要用于执行读操作缓解系统的读压力。 Redis的一个主库可以拥有多个从库，从库还可以作为其他数据库的主库。

哨兵模式

在主从模式上添加了一个哨兵的角色来监控集群的运行状态。哨兵通过发送命令让Redis服务器返回其运行状态。哨兵是一个独立运行的进程，在监测到Master宕机时会自动将Slave切换成Master，然后通过发布与订阅模式通知其他从服务器修改配置文件，完成主备热切。

集群模式

Redis集群实现了在多个Redis节点之间进行数据分片和数据复制。基于Redis集群的数据自动分片能力，我们能够方便地对Redis集群进行横向扩展，以提高Redis集群的吞吐量。基于Redis集群的数据复制能力，在集群中的一部分节点失效或者无法进行通信时，Redis仍然可以基于副本数据对外提供服务，这提高了集群的可用性。

Redis集群遵循如下原则

- 所有Redis节点彼此都通过PING-PONG 机制互联，内部使用二进制协议优化传输速度和带宽。
- 在集群中超过半数的节点检测到某个节点Fail后将该节点设置为Fail状态。
- 客户端与Redis节点直连，客户端连接集群中任何一个可用节点即可对集群进行操作。
- Redis-Cluster 把所有的物理节点都映射到0 ～ 16383 的slot（槽）上，Cluster负责维护每个节点上数据槽的分配。Redis的具体数据分配策略为：在Redis集群中内置了16384个散列槽；在需要在Redis集群中放置一个Key-Value时，Redis会先对Key使用CRC16 算法算出一个结果，然后把结果对 16384 求余数，这样每个Key都会对应一个编号为0～16383的散列槽；Redis会根据节点的数量大致均等地将散列槽映射到不同的节点。

## 持久化

### 1.Redis的持久化

Redis支持RDB和AOF两种持久化方式。

RDB（Redis DataBase）

RDB在指定的时间间隔内对数据进行快照存储。RDB的特点在于：文件格式紧凑，方便进行数据传输和数据恢复；在保存.rdb快照文件时父进程会fork出一个子进程，由子进程完成具体的持久化工作，所以可以最大化Redis的性能；同时，与AOF相比，在恢复大的数据集时会更快一些。

RDB持久化机制，对redis中的数据执行周期性的持久化。

AOF（Append Of Flie）

AOF记录对服务器的每次写操作，在Redis重启时会重放这些命令来恢复原数据。AOF命令以Redis协议追加和保存每次写操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。AOF的特点有：可以使用不同的fsync策略（无fsync、每秒fsync、每次写的时候fsync），只有某些操作追加命令到文件中，操作效率高；同时，AOF文件是日志的格式，更容易被操作。

AOF机制对每条写入命令作为日志，以append-only的模式写入一个日志文件中，在redis重启的时候，可以通过回放AOF日志中的写入指令来重新构建整个数据集。

如果我们想要redis仅仅作为纯内存的缓存来用，那么可以禁止RDB和AOF所有的持久化机制。

通过RDB或AOF，都可以将redis内存中的数据给持久化到磁盘上面来，然后可以将这些数据备份到别的地方去，比如说阿里云，云服务。

如果redis挂了，服务器上的内存和磁盘上的数据都丢了，可以从云服务上拷贝回来之前的数据，放到指定的目录中，然后重新启动redis，redis就会自动根据持久化数据文件中的数据，去恢复内存中的数据，继续对外提供服务。

如果同时使用RDB和AOF两种持久化机制，那么在redis重启的时候，会使用AOF来重新构建数据，因为AOF中的数据更加完整。

现代操作系统中，写文件并不是直接写磁盘，会先写os cache，然后到一定时间再从os cache到desk file。

AOF每隔一秒调用一次操作系统fsync操作，强制将os cache中的数据，刷入磁盘文件中。只有fsync到磁盘中了，才是安全的，要不然光是在os cache中，机器只要重启，就什么都没了。而RDB是每隔几分钟，几小时，几天，生成redis内存中的数据的一份完整的快照。

### 2.RDB和AOF选择

RDB持久化机制的优点

（1）RDB会生成多个数据文件，每个数据文件都代表了某一个时刻中redis的数据，这种多个数据文件的方式，非常适合做冷备，可以将这种完整的数据文件发送到一些远程的安全存储上去，比如说Amazon的S3云服务上去，在国内可以是阿里云的ODPS分布式存储上，以预定好的备份策略来定期备份redis中的数据。

RDB也可以做冷备，生成多个文件，每个文件都代表了某一个时刻的完整的数据快照。 AOF也可以做冷备，只有一个文件，但是你可以，每隔一定时间，去copy一份这个文件出来。

RDB做冷备，优势在哪儿呢？由redis去控制固定时长生成快照文件的事情，比较方便; AOF，还需要自己写一些脚本去做这个事情，各种定时。 RDB数据做冷备，在最坏的情况下，提供数据恢复的时候，速度比AOF快。

（2）RDB对redis对外提供的读写服务，影响非常小，可以让redis保持高性能，因为redis主进程只需要fork一个子进程，让子进程执行磁盘IO操作来进行RDB持久化即可。

RDB，每次写，都是直接写redis内存，只是在一定的时候，才会将数据写入磁盘中。 AOF，每次都是要写文件的，虽然可以快速写入os cache中，但是还是有一定的时间开销的,速度肯定比RDB略慢一些。

（3）相对于AOF持久化机制来说，直接基于RDB数据文件来重启和恢复redis进程，更加快速。

AOF，存放的指令日志，做数据恢复的时候，其实是要回放和执行所有的指令日志，来恢复出来内存中的所有数据的。 RDB，就是一份数据文件，恢复的时候，直接加载到内存中即可。

结合上述优点，RDB特别适合做冷备份，冷备。

RDB持久化机制的缺点

（1）如果想要在redis故障时，尽可能少的丢失数据，那么RDB没有AOF好。一般来说，RDB数据快照文件，都是每隔5分钟，或者更长时间生成一次，这个时候就得接受一旦redis进程宕机，那么会丢失最近5分钟的数据。

这个问题，也是rdb最大的缺点，就是不适合做第一优先的恢复方案，如果你依赖RDB做第一优先恢复方案，会导致数据丢失的比较多。

（2）RDB每次在fork子进程来执行RDB快照数据文件生成的时候，如果数据文件特别大，可能会导致对客户端提供的服务暂停数毫秒，或者甚至数秒。

一般不要让RDB的间隔太长，否则每次生成的RDB文件太大了，对redis本身的性能可能会有影响的。

AOF持久化机制的优点

（1）AOF可以更好的保护数据不丢失，一般AOF会每隔1秒，通过一个后台线程执行一次fsync操作，最多丢失1秒钟的数据。

每隔1秒，就执行一次fsync操作，保证os cache中的数据写入磁盘中。

redis进程挂了，最多丢掉1秒钟的数据。

（2）AOF日志文件以append-only模式写入，所以没有任何磁盘寻址的开销，写入性能非常高，而且文件不容易破损，即使文件尾部破损，也很容易修复。

（3）AOF日志文件即使过大的时候，出现后台重写操作，也不会影响客户端的读写。因为在rewrite log的时候，会对其中的指导进行压缩，创建出一份需要恢复数据的最小日志出来。再创建新日志文件的时候，老的日志文件还是照常写入。当新的merge后的日志文件ready的时候，再交换新老日志文件即可。

（4）AOF日志文件的命令通过非常可读的方式进行记录，这个特性非常适合做灾难性的误删除的紧急恢复。比如某人不小心用flushall命令清空了所有数据，只要这个时候后台rewrite还没有发生，那么就可以立即拷贝AOF文件，将最后一条flushall命令给删了，然后再将该AOF文件放回去，就可以通过恢复机制，自动恢复所有数据。

AOF持久化机制的缺点

（1）对于同一份数据来说，AOF日志文件通常比RDB数据快照文件更大。

（2）AOF开启后，支持的写QPS会比RDB支持的写QPS低，因为AOF一般会配置成每秒fsync一次日志文件，当然，每秒一次fsync，性能也还是很高的。

如果你要保证一条数据都不丢，也是可以的，AOF的fsync设置成每写入一条数据，fsync一次，那就完蛋了，redis的QPS大降。

（3）以前AOF发生过bug，就是通过AOF记录的日志，进行数据恢复的时候，没有恢复一模一样的数据出来。所以说，类似AOF这种较为复杂的基于命令日志/merge/回放的方式，比基于RDB每次持久化一份完整的数据快照文件的方式，更加脆弱一些，容易有bug。不过AOF就是为了避免rewrite过程导致的bug，因此每次rewrite并不是基于旧的指令日志进行merge的，而是基于当时内存中的数据进行指令的重新构建，这样健壮性会好很多。

唯一的比较大的缺点，其实就是做数据恢复的时候，会比较慢，还有做冷备，定期的备份，不太方便，可能要自己手写复杂的脚本去做，做冷备不太合适。

RDB和AOF到底该如何选择

（1）不要仅仅使用RDB，因为那样会导致你丢失很多数据。

（2）也不要仅仅使用AOF，因为那样有两个问题，第一，你通过AOF做冷备，没有RDB做冷备，来的恢复速度更快; 第二，RDB每次简单粗暴生成数据快照，更加健壮，可以避免AOF这种复杂的备份和恢复机制的bug。

（3）综合使用AOF和RDB两种持久化机制，用AOF来保证数据不丢失，作为数据恢复的第一选择; 用RDB来做不同程度的冷备，在AOF文件都丢失或损坏不可用的时候，还可以使用RDB来进行快速的数据恢复。

### 3.AOF rewrite

redis中的数据其实有限的，很多数据可能会自动过期，可能会被用户删除，可能会被redis用缓存清除的算法清理掉。

redis中的数据会不断淘汰掉旧的，就一部分常用的数据会被自动保留在redis内存中。

所以可能很多之前的已经被清理掉的数据，对应的写日志还停留在AOF中，AOF日志文件就一个，会不断的膨胀，到很大很大。

所以AOF会自动在后台每隔一定时间做rewrite操作，比如日志里已经存放了针对100w数据的写日志了; redis内存只剩下10万; 基于内存中当前的10万数据构建一套最新的日志，到AOF中; 覆盖之前的老日志; 确保AOF日志文件不会过大，保持跟redis内存数据量一致。

redis 2.4之前，还需要手动，开发一些脚本，crontab，通过BGREWRITEAOF命令去执行AOF rewrite，但是redis 2.4之后，会自动进行rewrite操作。

在redis.conf中，可以配置rewrite策略

```html
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
```

比如说上一次AOF rewrite之后，是128mb。

然后就会接着128mb继续写AOF的日志，如果发现增长的比例，超过了之前的100%，256mb，就可能会去触发一次rewrite。

但是此时还要去跟min-size，64mb去比较，256mb > 64mb，才会去触发rewrite。

（1）redis fork一个子进程。 （2）子进程基于当前内存中的数据，构建日志，开始往一个新的临时的AOF文件中写入日志。 （3）redis主进程，接收到client新的写操作之后，在内存中写入日志，同时新的日志也继续写入旧的AOF文件。 （4）子进程写完新的日志文件之后，redis主进程将内存中的新日志再次追加到新的AOF文件中。 （5）用新的日志文件替换掉旧的日志文件。

### 4.如何配置RDB持久化机制

redis.conf文件，也就是/etc/redis/6379.conf，去配置持久化。

```html
save 60 1000
```

每隔60s，如果有超过1000个key发生了变更，那么就生成一个新的dump.rdb文件，就是当前redis内存中完整的数据快照，这个操作也被称之为snapshotting，快照。

也可以手动调用save或者bgsave命令，同步或异步执行rdb快照生成。

save可以设置多个，就是多个snapshotting检查点，每到一个检查点，就会去check一下，是否有指定的key数量发生了变更，如果有，就生成一个新的dump.rdb文件。

### 5.RDB持久化机制的工作流程

（1）redis根据配置自己尝试去生成rdb快照文件。 （2）fork一个子进程出来。 （3）子进程尝试将数据dump到临时的rdb快照文件中。 （4）完成rdb快照文件的生成之后，就替换之前的旧的快照文件。

dump.rdb，每次生成一个新的快照，都会覆盖之前的老快照。

通过redis-cli SHUTDOWN这种方式去停掉redis，其实是一种安全退出的模式，redis在退出的时候会将内存中的数据立即生成一份完整的rdb快照。

用kill -9粗暴杀死redis进程，模拟redis故障异常退出，会导致内存数据丢失。

### 6.AOF持久化的配置

AOF持久化，默认是关闭的，默认是打开RDB持久化。

appendonly yes，可以打开AOF持久化机制，在生产环境里面，一般来说AOF都是要打开的，除非你说随便丢个几分钟的数据也无所谓。

打开AOF持久化机制之后，redis每次接收到一条写命令，就会写入日志文件中，当然是先写入os cache的，然后每隔一定时间再fsync一下。

而且即使AOF和RDB都开启了，redis重启的时候，也是优先通过AOF进行数据恢复的，因为aof数据比较完整。

可以配置AOF的fsync策略，有三种策略可以选择，一种是每次写入一条数据就执行一次fsync; 一种是每隔一秒执行一次fsync; 一种是不主动执行fsync。

always: 每次写入一条数据，立即将这个数据对应的写日志fsync到磁盘上去，性能非常非常差，吞吐量很低; 确保说redis里的数据一条都不丢，那就只能这样了。

mysql -> 内存策略，大量磁盘，QPS到多少，一两k。QPS，每秒钟的请求数量。 redis -> 内存，磁盘持久化，QPS到多少，单机，一般来说，上万QPS没问题。

everysec: 每秒将os cache中的数据fsync到磁盘，这个最常用的，生产环境一般都这么配置，性能很高，QPS还是可以上万的。

no: 仅仅redis负责将数据写入os cache就撒手不管了，然后后面os自己会时不时有自己的策略将数据刷入磁盘，不可控了。

### 7.AOF破损文件的修复

如果redis在append数据到AOF文件时，机器宕机了，可能会导致AOF文件破损。

用redis-check-aof --fix命令来修复破损的AOF文件。

### 8.AOF和RDB同时工作

（1）如果RDB在执行snapshotting操作，那么redis不会执行AOF rewrite; 如果redis在执行AOF rewrite，那么就不会执行RDB snapshotting。 （2）如果RDB在执行snapshotting，此时用户执行BGREWRITEAOF命令，那么等RDB快照生成之后，才会去执行AOF rewrite。 （3）同时有RDB snapshot文件和AOF日志文件，那么redis重启的时候，会优先使用AOF进行数据恢复，因为其中的日志更完整。

### 9.企业级的数据备份方案

RDB非常适合做冷备，每次生成之后，就不会再有修改了。

数据备份方案

（1）写crontab定时调度脚本去做数据备份。 （2）每小时都copy一份rdb的备份，到一个目录中去，仅仅保留最近48小时的备份。 （3）每天都保留一份当日的rdb的备份，到一个目录中去，仅仅保留最近1个月的备份。 （4）每次copy备份的时候，都把太旧的备份给删了。 （5）每天晚上将当前服务器上所有的数据备份，发送一份到远程的云服务上去。

/usr/local/redis

每小时copy一次备份，删除48小时前的数据。

```html
crontab -e
0 * * * * sh /usr/local/redis/copy/redis_rdb_copy_hourly.sh
redis_rdb_copy_hourly.sh
#!/bin/sh 
cur_date=`date +%Y%m%d%k`
rm -rf /usr/local/redis/snapshotting/$cur_date
mkdir /usr/local/redis/snapshotting/$cur_date
cp /var/redis/6379/dump.rdb /usr/local/redis/snapshotting/$cur_date
del_date=`date -d -48hour +%Y%m%d%k`
rm -rf /usr/local/redis/snapshotting/$del_date
```

每天copy一次备份。

```html
crontab -e
0 0 * * * sh /usr/local/redis/copy/redis_rdb_copy_daily.sh
redis_rdb_copy_daily.sh
#!/bin/sh 
cur_date=`date +%Y%m%d`
rm -rf /usr/local/redis/snapshotting/$cur_date
mkdir /usr/local/redis/snapshotting/$cur_date
cp /var/redis/6379/dump.rdb /usr/local/redis/snapshotting/$cur_date
del_date=`date -d -1month +%Y%m%d`
rm -rf /usr/local/redis/snapshotting/$del_date
```

每天一次将所有数据上传一次到远程的云服务器上去。

### 10.数据恢复方案

（1）如果是redis进程挂掉，那么重启redis进程即可，直接基于AOF日志文件恢复数据。

最多就丢一秒的数据。

（2）如果是redis进程所在机器挂掉，那么重启机器后，尝试重启redis进程，尝试直接基于AOF日志文件进行数据恢复

AOF没有破损，也是可以直接基于AOF恢复的。

AOF append-only，顺序写入，如果AOF文件破损，那么用redis-check-aof fix。

（3）如果redis当前最新的AOF和RDB文件出现了丢失/损坏，那么可以尝试基于该机器上当前的某个最新的RDB数据副本进行数据恢复。

当前最新的AOF和RDB文件都出现了丢失/损坏到无法恢复，一般不是机器的故障，人为。

大数据系统，hadoop，有人不小心就把hadoop中存储的大量的数据文件对应的目录，rm -rf一下，我朋友的一个小公司，运维不太靠谱，权限也弄的不太好

/var/redis/6379下的文件给删除了。

找到RDB最新的一份备份，小时级的备份可以了，小时级的肯定是最新的，copy到redis里面去，就可以恢复到某一个小时的数据。

appendonly.aof + dump.rdb，优先用appendonly.aof去恢复数据，但是我们发现redis自动生成的appendonly.aof是没有数据的。

然后我们自己的dump.rdb是有数据的，但是明显没用我们的数据。

redis启动的时候，自动重新基于内存的数据，生成了一份最新的rdb快照，直接用空的数据，覆盖掉了我们有数据的，拷贝过去的那份dump.rdb。

你停止redis之后，其实应该先删除appendonly.aof，然后将我们的dump.rdb拷贝过去，然后再重启redis。

很简单，就是虽然你删除了appendonly.aof，但是因为打开了aof持久化，redis就一定会优先基于aof去恢复，即使文件不在，那就创建一个新的空的aof文件。

停止redis，暂时在配置中关闭aof，然后拷贝一份rdb过来，再重启redis，数据能不能恢复过来，可以恢复过来。

脑子一热，再关掉redis，手动修改配置文件，打开aof，再重启redis，数据又没了，空的aof文件，所有数据又没了。

在数据安全丢失的情况下，基于rdb冷备，如何完美的恢复数据，同时还保持aof和rdb的双开。

停止redis，关闭aof，拷贝rdb备份，重启redis，确认数据恢复，直接在命令行热修改redis配置，打开aof，这个redis就会将内存中的数据对应的日志，写入aof文件中。

此时aof和rdb两份数据文件的数据就同步了。

redis config set热修改配置参数，可能配置文件中的实际的参数没有被持久化的修改，再次停止redis，手动修改配置文件，打开aof的命令，再次重启redis。

（4）如果当前机器上的所有RDB文件全部损坏，那么从远程的云服务上拉取最新的RDB快照回来恢复数据。

（5）如果是发现有重大的数据错误，比如某个小时上线的程序一下子将数据全部污染了，数据全错了，那么可以选择某个更早的时间点，对数据进行恢复。

举个例子，12点上线了代码，发现代码有bug，导致代码生成的所有的缓存数据，写入redis，全部错了。

找到一份11点的rdb的冷备，然后按照上面的步骤，去恢复到11点的数据，不就可以了吗。

## 复制（主从架构）

### 1.如果redis要支撑超过10万+的并发，那应该怎么做？

单机的redis几乎不太可能说QPS超过10万+，除非一些特殊情况，比如你的机器性能特别好，配置特别高，物理机，维护做的特别好，而且你的整体的操作不是太复杂。

单机在几万。

读写分离，一般来说，对缓存，一般都是用来支撑读高并发的，写的请求是比较少的，可能写请求也就一秒钟几千，一两千。

大量的请求都是读，一秒钟二十万次读。

读写分离。

主从架构 -> 读写分离 -> 支撑10万+读QPS的架构。

架构做成主从架构，一主多从，主负责写，并且将数据同步复制到其他slave节点，从节点负责读。所有的读请求全部走从节点。

如果你的读QPS再增加，也很简单，水平扩容，继续增加redis slave就可以了。

### 2.edis replication的核心机制

（1）redis采用异步方式复制数据到slave节点，不过redis 2.8开始，slave node会周期性地确认自己每次复制的数据量。 （2）一个master node是可以配置多个slave node的。 （3）slave node也可以连接其他的slave node。 （4）slave node做复制的时候，是不会block master node的正常工作的。 （5）slave node在做复制的时候，也不会block对自己的查询操作，它会用旧的数据集来提供服务; 但是复制完成的时候，需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了。 （6）slave node主要用来进行横向扩容，做读写分离，扩容的slave node可以提高读的吞吐量。

slave，高可用性，有很大的关系。

### 3.master持久化对于主从架构的安全保障的意义

如果采用了主从架构，那么建议必须开启master node的持久化！

不建议用slave node作为master node的数据热备，因为那样的话，如果你关掉master的持久化，可能在master宕机重启的时候数据是空的，然后可能一经过复制，salve node数据也丢了。

master -> RDB和AOF都关闭了 -> 全部在内存中。

master宕机，重启，是没有本地数据可以恢复的，然后就会直接认为自己IDE数据是空的。

master就会将空的数据集同步到slave上去，所有slave的数据全部清空。

100%的数据丢失。

master节点，必须要使用持久化机制。

第二个，master的各种备份方案，要不要做，万一说本地的所有文件丢失了; 从备份中挑选一份rdb去恢复master; 这样才能确保master启动的时候，是有数据的。

即使slave node可以自动接管master node，但是也可能sentinal还没有检测到master failure，master node就自动重启了，还是可能导致上面的所有slave node数据清空故障。

### 4.主从架构的核心原理

当启动一个slave node的时候，它会发送一个PSYNC命令给master node。

如果这是slave node重新连接master node，那么master node仅仅会复制给slave部分缺少的数据; 否则如果是slave node第一次连接master node，那么会触发一次full resynchronization。

开始full resynchronization的时候，master会启动一个后台线程，开始生成一份RDB快照文件，同时还会将从客户端收到的所有写命令缓存在内存中。RDB文件生成完毕之后，master会将这个RDB发送给slave，slave会先写入本地磁盘，然后再从本地磁盘加载到内存中。然后master会将内存中缓存的写命令发送给slave，slave也会同步这些数据。

slave node如果跟master node有网络故障，断开了连接，会自动重连。master如果发现有多个slave node都来重新连接，仅仅会启动一个rdb save操作，用一份数据服务所有slave node。

### 5.主从复制的断点续传

从redis 2.8开始，就支持主从复制的断点续传，如果主从复制过程中，网络连接断掉了，那么可以接着上次复制的地方，继续复制下去，而不是从头开始复制一份。

master node会在内存中常见一个backlog，master和slave都会保存一个replica offset还有一个master id，offset就是保存在backlog中的。如果master和slave网络连接断掉了，slave会让master从上次的replica offset开始继续复制。

但是如果没有找到对应的offset，那么就会执行一次resynchronization。

### 6.无磁盘化复制

master在内存中直接创建rdb，然后发送给slave，不会在自己本地落地磁盘了。

repl-diskless-sync repl-diskless-sync-delay，等待一定时长再开始复制，因为要等更多slave重新连接过来。

### 7.过期key处理

slave不会过期key，只会等待master过期key。如果master过期了一个key，或者通过LRU淘汰了一个key，那么会模拟一条del命令发送给slave。

### 8.复制的完整流程

（1）slave node启动，仅仅保存master node的信息，包括master node的host和ip，但是复制流程没开始。

master host和ip是从哪儿来的，redis.conf里面的slaveof配置的。

（2）slave node内部有个定时任务，每秒检查是否有新的master node要连接和复制，如果发现，就跟master node建立socket网络连接。 （3）slave node发送ping命令给master node。 （4）口令认证，如果master设置了requirepass，那么salve node必须发送masterauth的口令过去进行认证。 （5）master node第一次执行全量复制，将所有数据发给slave node。 （6）master node后续持续将写命令，异步复制给slave node。

### 9.数据同步相关的核心机制

指的就是第一次slave连接msater的时候，执行的全量复制，那个过程里面你的一些细节的机制。

（1）master和slave都会维护一个offset

master会在自身不断累加offset，slave也会在自身不断累加offset。 slave每秒都会上报自己的offset给master，同时master也会保存每个slave的offset。

这个倒不是说特定就用在全量复制的，主要是master和slave都要知道各自的数据的offset，才能知道互相之间的数据不一致的情况。

（2）backlog

master node有一个backlog，默认是1MB大小。 master node给slave node复制数据时，也会将数据在backlog中同步写一份。 backlog主要是用来做全量复制中断候的增量复制的。

（3）master run id

info server，可以看到master run id。 如果根据host+ip定位master node，是不靠谱的，如果master node重启或者数据出现了变化，那么slave node应该根据不同的run id区分，run id不同就做全量复制。 如果需要不更改run id重启redis，可以使用redis-cli debug reload命令。

（4）psync

从节点使用psync从master node进行复制，psync runid offset。 master node会根据自身的情况返回响应信息，可能是FULLRESYNC runid offset触发全量复制，可能是CONTINUE触发增量复制。

### 10.全量复制

（1）master执行bgsave，在本地生成一份rdb快照文件。 （2）master node将rdb快照文件发送给salve node，如果rdb复制时间超过60秒（repl-timeout），那么slave node就会认为复制失败，可以适当调节大这个参数。 （3）对于千兆网卡的机器，一般每秒传输100MB，6G文件，很可能超过60s。 （4）master node在生成rdb时，会将所有新的写命令缓存在内存中，在salve node保存了rdb之后，再将新的写命令复制给salve node。 （5）client-output-buffer-limit slave 256MB 64MB 60，如果在复制期间，内存缓冲区持续消耗超过64MB，或者一次性超过256MB，那么停止复制，复制失败。 （6）slave node接收到rdb之后，清空自己的旧数据，然后重新加载rdb到自己的内存中，同时基于旧的数据版本对外提供服务。 （7）如果slave node开启了AOF，那么会立即执行BGREWRITEAOF，重写AOF。

rdb生成、rdb通过网络拷贝、slave旧数据的清理、slave aof rewrite，很耗费时间。

如果复制的数据量在4G~6G之间，那么很可能全量复制时间消耗到1分半到2分钟。

### 11.增量复制

（1）如果全量复制过程中，master-slave网络连接断掉，那么salve重新连接master时，会触发增量复制。 （2）master直接从自己的backlog中获取部分丢失的数据，发送给slave node，默认backlog就是1MB。 （3）msater就是根据slave发送的psync中的offset来从backlog中获取数据的。

### 12.heartbeat

主从节点互相都会发送heartbeat信息。

master默认每隔10秒发送一次heartbeat，salve node每隔1秒发送一个heartbeat。

### 13.异步复制

master每次接收到写命令之后，先在内部写入数据，然后异步发送给slave node。

### 14.强制读写分离

基于主从复制架构，实现读写分离。

redis slave node只读，默认开启，slave-read-only。

开启了只读的redis slave node，会拒绝所有的写操作，这样可以强制搭建成读写分离的架构。

## 哨兵（高可用，主备切换）

### 1.什么是99.99%高可用？

架构上，高可用性，99.99%的高可用性。

讲的学术，99.99%，公式，系统可用的时间 / 系统故障的时间，365天，在365天 * 99.99%的时间内，你的系统都是可以哗哗对外提供服务的，那就是高可用性，99.99%。

系统可用的时间 / 总的时间 = 高可用性。

### 2.redis不可用是什么？单实例不可用？主从架构不可用？不可用的后果是什么？

一个slave挂掉了，是不会影响可用性的，还有其他的slave在提供相同数据下的相同的对外的查询服务。

如果master node死掉了，会怎么样？会没法写数据了，写缓存的时候全部失效了，slave节点还有什么用呢，没有master给他们复制数据了，系统相当于就是不可用了。

缓存不可用了，高并发情况下大量的流量，超过mysql最大承受能力的大并发，大流量，会涌入mysql中，mysql宕机，整个系统不可用。

### 3.主备切换

redis高可用架构，叫做故障转移，failover，也可以叫做主备切换。

这个在master node故障时，自动检测，并且将某个slave node自动切换为master node的过程，叫做主备切换。这个过程，实现了redis的主从架构下的高可用性。

一旦master故障，在很短的时间内，就会切换到另外一个master上去，可能也就几分钟，几秒钟，redis是不可用的。

### 4.哨兵的介绍

sentinal，中文名是哨兵。

哨兵是redis集群架构中非常重要的一个组件，主要功能如下：

（1）集群监控，负责监控redis master和slave进程是否正常工作。 （2）消息通知，如果某个redis实例有故障，那么哨兵负责发送消息作为报警通知给管理员。 （3）故障转移，如果master node挂掉了，会自动转移到slave node上。 （4）配置中心，如果故障转移发生了，通知client客户端新的master地址。

哨兵本身也是分布式的，作为一个哨兵集群去运行，互相协同工作。

（1）故障转移时，判断一个master node是宕机了，需要大部分的哨兵都同意才行，涉及到了分布式选举的问题。 （2）即使部分哨兵节点挂掉了，哨兵集群还是能正常工作的，因为如果一个作为高可用机制重要组成部分的故障转移系统本身是单点的，那就很坑爹了。

目前采用的是sentinal 2版本，sentinal 2相对于sentinal 1来说，重写了很多代码，主要是让故障转移的机制和算法变得更加健壮和简单。

### 5.哨兵的核心知识

（1）哨兵至少需要3个实例，来保证自己的健壮性。 （2）哨兵 + redis主从的部署架构，是不会保证数据零丢失的，只能保证redis集群的高可用性。 （3）对于哨兵 + redis主从这种复杂的部署架构，尽量在测试环境和生产环境，都进行充足的测试和演练。

### 6.为什么redis哨兵集群只有2个节点无法正常工作？

哨兵集群必须部署2个以上节点。

如果哨兵集群仅仅部署了个2个哨兵实例，quorum=1。

+----+ +----+ | M1 |---------| R1 | | S1 | | S2 | +----+ +----+

Configuration: quorum = 1

master宕机，s1和s2中只要有1个哨兵认为master宕机就可以还行切换，同时s1和s2中会选举出一个哨兵来执行故障转移。

同时这个时候，需要majority，也就是大多数哨兵都是运行的，2个哨兵的majority就是2（2的majority=2，3的majority=2，5的majority=3，4的majority=2），2个哨兵都运行着，就可以允许执行故障转移。

但是如果整个M1和S1运行的机器宕机了，那么哨兵只有1个了，此时就没有majority来允许执行故障转移，虽然另外一台机器还有一个R1，但是故障转移不会执行。

### 7.经典的3节点哨兵集群

```html
   +----+
   | M1 |
   | S1 |
   +----+
      |
+----+    |    +----+
| R2 |----+----| R3 |
| S2 |         | S3 |
+----+         +----+      
```

Configuration: quorum = 2，majority

如果M1所在机器宕机了，那么三个哨兵还剩下2个，S2和S3可以一致认为master宕机，然后选举出一个来执行故障转移。

同时3个哨兵的majority是2，所以还剩下的2个哨兵运行着，就可以允许执行故障转移。

### 8.两种数据丢失的情况

主备切换的过程，可能会导致数据丢失。

（1）异步复制导致的数据丢失

因为master -> slave的复制是异步的，所以可能有部分数据还没复制到slave，master就宕机了，此时这些部分数据就丢失了。

（2）脑裂导致的数据丢失

脑裂，也就是说，某个master所在机器突然脱离了正常的网络，跟其他slave机器不能连接，但是实际上master还运行着。

此时哨兵可能就会认为master宕机了，然后开启选举，将其他slave切换成了master。

这个时候，集群里就会有两个master，也就是所谓的脑裂。

此时虽然某个slave被切换成了master，但是可能client还没来得及切换到新的master，还继续写向旧master的数据可能也丢失了。

因此旧master再次恢复的时候，会被作为一个slave挂到新的master上去，自己的数据会清空，重新从新的master复制数据。

### 9.解决异步复制和脑裂导致的数据丢失

min-slaves-to-write 1 min-slaves-max-lag 10

要求至少有1个slave，数据复制和同步的延迟不能超过10秒。

如果说一旦所有的slave，数据复制和同步的延迟都超过了10秒钟，那么这个时候，master就不会再接收任何请求了。

上面两个配置可以减少异步复制和脑裂导致的数据丢失。

（1）减少异步复制的数据丢失

有了min-slaves-max-lag这个配置，就可以确保说，一旦slave复制数据和ack延时太长，就认为可能master宕机后损失的数据太多了，那么就拒绝写请求，这样可以把master宕机时由于部分数据未同步到slave导致的数据丢失降低的可控范围内。

一般来说，会在client做降级，写到本地磁盘里面，在client对外接收请求，再做降级，做限流，减慢请求涌入的速度。或者client可能会采取将数据临时灌入一个kafka消息队列，每隔十分钟去队列里取一次，尝试重新发回master。

（2）减少脑裂的数据丢失

如果一个master出现了脑裂，跟其他slave丢了连接，那么上面两个配置可以确保说，如果不能继续给指定数量的slave发送数据，而且slave超过10秒没有给自己ack消息，那么就直接拒绝客户端的写请求。

这样脑裂后的旧master就不会接受client的新数据，也就避免了数据丢失。

上面的配置就确保了，如果跟任何一个slave丢了连接，在10秒后发现没有slave给自己ack，那么就拒绝新的写请求。

因此在脑裂场景下，最多就丢失10秒的数据。

### 10.sdown和odown转换机制

sdown和odown两种失败状态。

sdown是主观宕机，就一个哨兵如果自己觉得一个master宕机了，那么就是主观宕机。

odown是客观宕机，如果quorum数量的哨兵都觉得一个master宕机了，那么就是客观宕机。

sdown达成的条件很简单，如果一个哨兵ping一个master，超过了is-master-down-after-milliseconds指定的毫秒数之后，就主观认为master宕机。

sdown到odown转换的条件很简单，如果一个哨兵在指定时间内，收到了quorum指定数量的其他哨兵也认为那个master是sdown了，那么就认为是odown了，客观认为master宕机。

### 11.哨兵集群的自动发现机制

哨兵互相之间的发现，是通过redis的pub/sub系统实现的，每个哨兵都会往**sentinel**:hello这个channel里发送一个消息，这时候所有其他哨兵都可以消费到这个消息，并感知到其他的哨兵的存在。

每隔两秒钟，每个哨兵都会往自己监控的某个master+slaves对应的**sentinel**:hello channel里发送一个消息，内容是自己的host、ip和runid还有对这个master的监控配置。

每个哨兵也会去监听自己监控的每个master+slaves对应的**sentinel**:hello channel，然后去感知到同样在监听这个master+slaves的其他哨兵的存在。

每个哨兵还会跟其他哨兵交换对master的监控配置，互相进行监控配置的同步。

### 12.slave配置的自动纠正

哨兵会负责自动纠正slave的一些配置，比如slave如果要成为潜在的master候选人，哨兵会确保slave在复制现有master的数据; 如果slave连接到了一个错误的master上，比如故障转移之后，那么哨兵会确保它们连接到正确的master上。

### 13.slave->master选举算法

如果一个master被认为odown了，而且majority哨兵都允许了主备切换，那么某个哨兵就会执行主备切换操作，此时首先要选举一个slave来。

会考虑slave的一些信息。

（1）跟master断开连接的时长。 （2）slave优先级。 （3）复制offset。 （4）run id。

如果一个slave跟master断开连接已经超过了down-after-milliseconds的10倍，外加master宕机的时长，那么slave就被认为不适合选举为master。

(down-after-milliseconds * 10) + milliseconds_since_master_is_in_SDOWN_state

接下来会对slave进行排序。

（1）按照slave优先级进行排序，slave priority越低，优先级就越高。 （2）如果slave priority相同，那么看replica offset，哪个slave复制了越多的数据，offset越靠后，优先级就越高。 （3）如果上面两个条件都相同，那么选择一个run id比较小的那个slave。

### 14.quorum和majority

每次一个哨兵要做主备切换，首先需要quorum数量的哨兵认为odown，然后选举出一个哨兵来做切换，这个哨兵还得得到majority哨兵的授权，才能正式执行切换。

如果quorum < majority，比如5个哨兵，majority就是3，quorum设置为2，那么就3个哨兵授权就可以执行切换。

但是如果quorum >= majority，那么必须quorum数量的哨兵都授权，比如5个哨兵，quorum是5，那么必须5个哨兵都同意授权，才能执行切换。

### 15.configuration epoch

哨兵会对一套redis master+slave进行监控，有相应的监控的配置。

执行切换的那个哨兵，会从要切换到的新master（salve->master）那里得到一个configuration epoch，这就是一个version号，每次切换的version号都必须是唯一的。

如果第一个选举出的哨兵切换失败了，那么其他哨兵，会等待failover-timeout时间，然后接替继续执行切换，此时会重新获取一个新的configuration epoch，作为新的version号。

### 16.configuraiton传播

哨兵完成切换之后，会在自己本地更新生成最新的master配置，然后同步给其他的哨兵，就是通过之前说的pub/sub消息机制。

这里之前的version号就很重要了，因为各种消息都是通过一个channel去发布和监听的，所以一个哨兵完成一次新的切换之后，新的master配置是跟着新的version号的。

其他的哨兵都是根据版本号的大小来更新自己的master配置的。

### 17.哨兵的配置文件

sentinel.conf

最小的配置

每一个哨兵都可以去监控多个maser-slaves的主从架构

因为可能你的公司里，为不同的项目，部署了多个master-slaves的redis主从集群

相同的一套哨兵集群，就可以去监控不同的多个redis主从集群

你自己给每个redis主从集群分配一个逻辑的名称

```html
sentinel monitor mymaster 127.0.0.1 6379 2
sentinel down-after-milliseconds mymaster 60000
sentinel failover-timeout mymaster 180000
sentinel parallel-syncs mymaster 1

sentinel monitor resque 192.168.1.3 6380 4
sentinel down-after-milliseconds resque 10000
sentinel failover-timeout resque 180000
sentinel parallel-syncs resque 5

sentinel monitor mymaster 127.0.0.1 6379 
```

类似这种配置，来指定对一个master的监控，给监控的master指定的一个名称，因为后面分布式集群架构里会讲解，可以配置多个master做数据拆分

```html
sentinel down-after-milliseconds mymaster 60000
sentinel failover-timeout mymaster 180000
sentinel parallel-syncs mymaster 1
```

上面的三个配置，都是针对某个监控的master配置的，给其指定上面分配的名称即可。

上面这段配置，就监控了两个master node。

这是最小的哨兵配置，如果发生了master-slave故障转移，或者新的哨兵进程加入哨兵集群，那么哨兵会自动更新自己的配置文件。

sentinel monitor master-group-name hostname port quorum

quorum的解释如下：

（1）至少多少个哨兵要一致同意，master进程挂掉了，或者slave进程挂掉了，或者要启动一个故障转移操作。 （2）quorum是用来识别故障的，真正执行故障转移的时候，还是要在哨兵集群执行选举，选举一个哨兵进程出来执行故障转移操作。 （3）假设有5个哨兵，quorum设置了2，那么如果5个哨兵中的2个都认为master挂掉了; 2个哨兵中的一个就会做一个选举，选举一个哨兵出来，执行故障转移; 如果5个哨兵中有3个哨兵都是运行的，那么故障转移就会被允许执行。

down-after-milliseconds，超过多少毫秒跟一个redis实例断了连接，哨兵就可能认为这个redis实例挂了。

parallel-syncs，新的master别切换之后，同时有多少个slave被切换到去连接新master，重新做同步，数字越低，花费的时间越多。

假设你的redis是1个master，4个slave。

然后master宕机了，4个slave中有1个切换成了master，剩下3个slave就要挂到新的master上面去。

这个时候，如果parallel-syncs是1，那么3个slave，一个一个地挂接到新的master上面去，1个挂接完，而且从新的master sync完数据之后，再挂接下一个

如果parallel-syncs是3，那么一次性就会把所有slave挂接到新的master上去。

failover-timeout，执行故障转移的timeout超时时长。

### 18.正式的配置

哨兵默认用26379端口，默认不能跟其他机器在指定端口连通，只能在本地访问

```html
mkdir /etc/sentinal
mkdir -p /var/sentinal/5000

/etc/sentinel/5000.conf

port 5000
bind 192.168.31.187
dir /var/sentinal/5000
sentinel monitor mymaster 192.168.31.187 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel failover-timeout mymaster 60000
sentinel parallel-syncs mymaster 1

port 5000
bind 192.168.31.19
dir /var/sentinal/5000
sentinel monitor mymaster 192.168.31.187 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel failover-timeout mymaster 60000
sentinel parallel-syncs mymaster 1

port 5000
bind 192.168.31.227
dir /var/sentinal/5000
sentinel monitor mymaster 192.168.31.187 6379 2
sentinel down-after-milliseconds mymaster 30000
sentinel failover-timeout mymaster 60000
sentinel parallel-syncs mymaster 1
```

### 19.哨兵节点的增加和删除

增加sentinal，会自动发现。

删除sentinal的步骤。

（1）停止sentinal进程。 （2）SENTINEL RESET *，在所有sentinal上执行，清理所有的master状态。 （3）SENTINEL MASTER mastername，在所有sentinal上执行，查看所有sentinal对数量是否达成了一致。

### 20.slave的永久下线

让master摘除某个已经下线的slave：SENTINEL RESET mastername，在所有的哨兵上面执行。

### 21.slave切换为Master的优先级

slave->master选举优先级：slave-priority，值越小优先级越高。

### 22.基于哨兵集群架构下的安全认证

每个slave都有可能切换成master，所以每个实例都要配置两个指令

master上启用安全认证，requirepass master连接口令，masterauth

sentinal，sentinel auth-pass <master-group-name> <pass>

### 23.容灾演练

通过哨兵看一下当前的master：SENTINEL get-master-addr-by-name mymaster。

把master节点kill -9掉，pid文件也删除掉。

查看sentinal的日志，是否出现+sdown字样，识别出了master的宕机问题; 然后出现+odown字样，就是指定的quorum哨兵数量，都认为master宕机了。

（1）三个哨兵进程都认为master是sdown了。 （2）超过quorum指定的哨兵进程都认为sdown之后，就变为odown。 （3）哨兵1是被选举为要执行后续的主备切换的那个哨兵。 （4）哨兵1去新的master（slave）获取了一个新的config version。 （5）尝试执行failover。 （6）投票选举出一个slave区切换成master，每隔哨兵都会执行一次投票。 （7）让salve，slaveof noone，不让它去做任何节点的slave了; 把slave提拔成master; 旧的master认为不再是master了。 （8）哨兵就自动认为之前的187:6379变成了slave了，19:6379变成了master了。 （9）哨兵去探查了一下187:6379这个salve的状态，认为它sdown了。

所有哨兵选举出了一个，来执行主备切换操作。

如果哨兵的majority都存活着，那么就会执行主备切换操作。

再通过哨兵看一下master：SENTINEL get-master-addr-by-name mymaster。

尝试连接一下新的master。

故障恢复，再将旧的master重新启动，查看是否被哨兵自动切换成slave节点。

（1）手动杀掉master。 （2）哨兵能否执行主备切换，将slave切换为master。 （3）哨兵完成主备切换后，新的master能否使用。 （4）故障恢复，将旧的master重新启动。 （5）哨兵能否自动将旧的master变为slave，挂接到新的master上面去，而且也是可以使用的。

## redis cluster（海量数据+横向扩容+高可用/主备切换）

### 1.单master架构的容量瓶颈问题

master节点的数据和slave节点的数据是一模一样的，master最大能容纳多大的数据流，那么slave也就只能容纳多大的数据量。

缓存清理的算法，将旧的很少使用的数据，给清除出内存，然后保证内存中，就只有固定大小的内存，不可能超过master内存的物理上限的。

### 2.横向扩容

如果想要支撑更大的数据量的缓存，那就横向扩容更多的master节点，每个master节点存放一部分数据，比如单台服务器是32GB,30台左右，那么就可以存放下1T的数据了。

### 3.redis的集群架构

redis cluster

支撑N个redis master node，每个master node都可以挂载多个slave node。

读写分离的架构，对于每个master来说，写就写到master，然后读就从mater对应的slave去读。

高可用，因为每个master都有salve节点，那么如果mater挂掉，redis cluster这套机制，就会自动将某个slave切换成master。

redis cluster（多master + 读写分离 + 高可用）

我们只要基于redis cluster去搭建redis集群即可，不需要手工去搭建replication复制+主从架构+读写分离+哨兵集群+高可用。

### 4、redis cluster vs. replication + sentinal

如果你的数据量很少，主要是承载高并发高性能的场景，比如你的缓存一般就几个G，单机足够了。

replication，一个mater，多个slave，要几个slave跟你的要求的读吞吐量有关系，然后自己搭建一个sentinal集群，去保证redis主从架构的高可用性，就可以了。

redis cluster，主要是针对海量数据+高并发+高可用的场景，海量数据，如果你的数据量很大，那么建议就用redis cluster。

正式环境下，建议都是说在6台机器上去搭建，至少3台机器。

保证，每个master都跟自己的slave不在同一台机器上，如果是6台自然更好，一个master+一个slave就死了。

### 5.redis cluster介绍

redis cluster

（1）自动将数据进行分片，每个master上放一部分数据。 （2）提供内置的高可用支持，部分master不可用时，还是可以继续工作的。

在redis cluster架构下，每个redis要放开两个端口号，比如一个是6379，另外一个就是加10000的端口号，比如16379。

16379端口号是用来进行节点间通信的，也就是cluster bus的东西，集群总线。cluster bus的通信，用来进行故障检测，配置更新，故障转移授权。

cluster bus用了另外一种二进制的协议，主要用于节点间进行高效的数据交换，占用更少的网络带宽和处理时间。

redis cluster集群，要求至少3个master，去组成一个高可用，健壮的分布式的集群，每个master都建议至少给一个slave，3个master，3个slave，最少的要求，每个master挂一两个slave，主要是做数据的热备，还有master故障时的主备切换，实现高可用的。

### 6.redis cluster的hash slot算法

redis cluster有固定的16384个hash slot，对每个key计算CRC16值，然后对16384取模，可以获取key对应的hash slot。

redis cluster中每个master都会持有部分slot，比如有3个master，那么可能每个master持有5000多个hash slot。

hash slot让node的增加和移除很简单，增加一个master，就将其他master的hash slot移动部分过去，减少一个master，就将它的hash slot移动到其他master上去。

移动hash slot的成本是非常低的。

客户端的api，可以对指定的数据，让他们走同一个hash slot，通过hash tag来实现。

### 7.一致性hash算法的优缺点

原始hash算法弊端

比如有3个master，某一个master宕机了，就导致1/3的数据全部失效，全部要重新对剩下的2台master取模，再分布到其他的节点上去，大量的数据要重新计算写入缓存，风险很大。导致几乎大部分的请求，全部无法拿到有效的缓存，大量的数据涌入数据库。

key过来以后，同样也是计算hash值，然后会用hash值在圆环对应的各个点上（每个点都有一个hash值）去对比，看hash值应该落在这个圆环的哪个部位。

key落在圆环上以后，就会顺时针旋转去寻找距离自己最近的一个节点。

一致性hash算法的缺点，可能集中在某个hash区间内的值特别多，那么会导致大量的数据都涌入同一个master内，造成master的热点问题，性能出现瓶颈。

一致性hash算法，保证，任何一个master宕机，只有之前那个master上的数据，会受到影响，因为照着顺时针走，全部在之前的master

上找不到了，master宕机了，会顺时针走到下一个master去，也找不到，流量会瞬间都涌入数据库中，重新查询一次，mysql扛不住可能导致不可用。

### 8.一致性hash算法的虚拟节点实现负载均衡

给每个master都做均匀分布的虚拟节点，这样的话，在每个区间内，大量的数据，都会均匀的分不到不同的节点内，而不是按照顺时针的顺序去走，全部涌入一个master内。

### 9.redis cluster的重要配置

cluster-enabled <yes/no>

cluster-config-file <filename>：这是指定一个文件，供cluster模式下的redis实例将集群状态保存在那里，包括集群中其他机器的信息，比如节点的上线和下限，故障转移，不是我们去维护的，给它指定一个文件，让redis自己去维护的

cluster-node-timeout <milliseconds>：节点存活超时时长，超过一定时长，认为节点宕机，master宕机的话就会触发主备切换，slave宕机就不会提供服务

### 10.多master写入 -> 海量数据的分布式存储

你在redis cluster写入数据的时候，其实是你可以将请求发送到任意一个master上去执行。

但是，每个master都会计算这个key对应的CRC16值，然后对16384个hashslot取模，找到key对应的hashslot，找到hashslot对应的master。

如果对应的master就在自己本地的话，set mykey1 v1，mykey1这个key对应的hashslot就在自己本地，那么自己就处理掉了。

但是如果计算出来的hashslot在其他master上，那么就会给客户端返回一个moved error，告诉你，你得到哪个master上去执行这条写入的命令。

什么叫做多master的写入，就是每条数据只能存在于一个master上，不同的master负责存储不同的数据，分布式的数据存储。

100w条数据，5个master，每个master就负责存储20w条数据，分布式数据存储。

大型的java系统架构，还专注在大数据系统架构，分布式，分布式存储，hadoop hdfs，分布式资源调度，hadoop yarn，分布式计算，hadoop mapreduce/hive

分布式的nosql数据库，hbase，分布式的协调，zookeeper，分布式通用计算引擎，spark，分布式的实时计算引擎，storm。

如果你要处理海量数据，就涉及到了一个名词，叫做大数据，只要涉及到大数据，那么其实就会涉及到分布式。

redis AOF，fsync

elasticsearch建立索引的时候，先写内存缓存，每秒钟把数据刷入os cache，接下来再每隔一定时间fsync到磁盘上去。

redis cluster，写可以到任意master，任意master计算key的hashslot以后，告诉client，重定向，路由到其他mater去执行，分布式存储的一个经典的做法。

elasticsearch，建立索引的时候，也会根据doc id/routing value，做路由，路由到某个其他节点，重定向到其他节点去执行。

jedis cluster api，就可以自动针对多个master进行写入和读取。

### 11、redis是怎么扩容的

1.加入新master

```html
mkdir -p /var/redis/7007

port 7007
cluster-enabled yes
cluster-config-file /etc/redis-cluster/node-7007.conf
cluster-node-timeout 15000
daemonize	yes							
pidfile		/var/run/redis_7007.pid 						
dir 		/var/redis/7007		
logfile /var/log/redis/7007.log
bind 192.168.31.227		
appendonly yes
```

搞一个7007.conf，再搞一个redis_7007启动脚本。

手动启动一个新的redis实例，在7007端口上。

redis-trib.rb add-node 192.168.31.227:7007 192.168.31.187:7001

redis-trib.rb check 192.168.31.187:7001

连接到新的redis实例上，cluster nodes，确认自己是否加入了集群，作为了一个新的master。

2.reshard一些数据过去

resharding的意思就是把一部分hash slot从一些node上迁移到另外一些node上。

redis-trib.rb reshard 192.168.31.187:7001

要把之前3个master上，总共4096个hashslot迁移到新的第四个master上去。

How many slots do you want to move (from 1 to 16384)?

1000

3.添加node作为slave

```html
eshop-cache03

mkdir -p /var/redis/7008

port 7008
cluster-enabled yes
cluster-config-file /etc/redis-cluster/node-7008.conf
cluster-node-timeout 15000
daemonize	yes							
pidfile		/var/run/redis_7008.pid 						
dir 		/var/redis/7008		
logfile /var/log/redis/7008.log
bind 192.168.31.227		
appendonly yes

redis-trib.rb add-node --slave --master-id 28927912ea0d59f6b790a50cf606602a5ee48108 192.168.31.227:7008 192.168.31.187:7001
```

4.删除node

先用resharding将数据都移除到其他节点，确保node为空之后，才能执行remove操作。

redis-trib.rb del-node 192.168.31.187:7001 bd5a40a6ddccbd46a0f4a2208eb25d2453c2a8db

2个是1365，1个是1366。

当你清空了一个master的hashslot时，redis cluster就会自动将其slave挂载到其他master上去。

这个时候就只要删除掉master就可以了。

### 12.slave的自动迁移

比如现在有10个master，每个有1个slave，然后新增了3个slave作为冗余，有的master就有2个slave了，有的master出现了salve冗余。

如果某个master的slave挂了，那么redis cluster会自动迁移一个冗余的slave给那个master。

只要多加一些冗余的slave就可以了。

为了避免的场景，就是说，如果你每个master只有一个slave，万一说一个slave死了，然后很快，master也死了，那可用性还是降低了。

但是如果你给整个集群挂载了一些冗余slave，那么某个master的slave死了，冗余的slave会被自动迁移过去，作为master的新slave，此时即使那个master也死了

还是有一个slave会切换成master的。

### 13.节点间的内部通信机制

1、基础通信原理

（1）redis cluster节点间采取gossip协议进行通信

跟集中式不同，不是将集群元数据（节点信息，故障，等等）集中存储在某个节点上，而是互相之间不断通信，保持整个集群所有节点的数据是完整的。

维护集群的元数据用得，集中式，一种叫做gossip。

集中式：好处在于，元数据的更新和读取，时效性非常好，一旦元数据出现了变更，立即就更新到集中式的存储中，其他节点读取的时候立即就可以感知到; 不好在于，所有的元数据的跟新压力全部集中在一个地方，可能会导致元数据的存储有压力。

gossip：好处在于，元数据的更新比较分散，不是集中在一个地方，更新请求会陆陆续续，打到所有节点上去更新，有一定的延时，降低了压力; 缺点，元数据更新有延时，可能导致集群的一些操作会有一些滞后。

我们刚才做reshard，去做另外一个操作，会发现说，configuration error，达成一致。

（2）10000端口

每个节点都有一个专门用于节点间通信的端口，就是自己提供服务的端口号+10000，比如7001，那么用于节点间通信的就是17001端口。

每隔节点每隔一段时间都会往另外几个节点发送ping消息，同时其他几点接收到ping之后返回pong。

（3）交换的信息

故障信息，节点的增加和移除，hash slot信息，等等。

2、gossip协议

gossip协议包含多种消息，包括ping，pong，meet，fail，等等。

meet: 某个节点发送meet给新加入的节点，让新节点加入集群中，然后新节点就会开始与其他节点进行通信。

redis-trib.rb add-node

其实内部就是发送了一个gossip meet消息，给新加入的节点，通知那个节点去加入我们的集群。

ping: 每个节点都会频繁给其他节点发送ping，其中包含自己的状态还有自己维护的集群元数据，互相通过ping交换元数据。

每个节点每秒都会频繁发送ping给其他的集群，ping，频繁的互相之间交换数据，互相进行元数据的更新。

pong: 返回ping和meet，包含自己的状态和其他信息，也可以用于信息广播和更新。

fail: 某个节点判断另一个节点fail之后，就发送fail给其他节点，通知其他节点，指定的节点宕机了。

3、ping消息深入

ping很频繁，而且要携带一些元数据，所以可能会加重网络负担。

每个节点每秒会执行10次ping，每次会选择5个最久没有通信的其他节点。

当然如果发现某个节点通信延时达到了cluster_node_timeout / 2，那么立即发送ping，避免数据交换延时过长，落后的时间太长了。

比如说，两个节点之间都10分钟没有交换数据了，那么整个集群处于严重的元数据不一致的情况，就会有问题。

所以cluster_node_timeout可以调节，如果调节比较大，那么会降低发送的频率。

每次ping，一个是带上自己节点的信息，还有就是带上1/10其他节点的信息，发送出去，进行数据交换。

至少包含3个其他节点的信息，最多包含总节点-2个其他节点的信息。

### 14.面向集群的jedis内部实现原理

开发，jedis，redis的java client客户端，redis cluster，jedis cluster api。

jedis cluster api与redis cluster集群交互的一些基本原理。

1、基于重定向的客户端

redis-cli -c，自动重定向。

（1）请求重定向

客户端可能会挑选任意一个redis实例去发送命令，每个redis实例接收到命令，都会计算key对应的hash slot。

如果在本地就在本地处理，否则返回moved给客户端，让客户端进行重定向。

cluster keyslot mykey，可以查看一个key对应的hash slot是什么。

用redis-cli的时候，可以加入-c参数，支持自动的请求重定向，redis-cli接收到moved之后，会自动重定向到对应的节点执行命令。

（2）计算hash slot

计算hash slot的算法，就是根据key计算CRC16值，然后对16384取模，拿到对应的hash slot。

用hash tag可以手动指定key对应的slot，同一个hash tag下的key，都会在一个hash slot中，比如set mykey1:{100}和set mykey2:{100}。

（3）hash slot查找

节点间通过gossip协议进行数据交换，就知道每个hash slot在哪个节点上。

2、smart jedis

（1）什么是smart jedis

基于重定向的客户端，很消耗网络IO，因为大部分情况下，可能都会出现一次请求重定向，才能找到正确的节点。

所以大部分的客户端，比如java redis客户端，就是jedis，都是smart的。

本地维护一份hashslot -> node的映射表，缓存，大部分情况下，直接走本地缓存就可以找到hashslot -> node，不需要通过节点进行moved重定向。

（2）JedisCluster的工作原理

在JedisCluster初始化的时候，就会随机选择一个node，初始化hashslot -> node映射表，同时为每个节点创建一个JedisPool连接池。

每次基于JedisCluster执行操作，首先JedisCluster都会在本地计算key的hashslot，然后在本地映射表找到对应的节点。

如果那个node正好还是持有那个hashslot，那么就ok; 如果说进行了reshard这样的操作，可能hashslot已经不在那个node上了，就会返回moved。

如果JedisCluter API发现对应的节点返回moved，那么利用该节点的元数据，更新本地的hashslot -> node映射表缓存。

重复上面几个步骤，直到找到对应的节点，如果重试超过5次，那么就报错，JedisClusterMaxRedirectionException。

jedis老版本，可能会出现在集群某个节点故障还没完成自动切换恢复时，频繁更新hash slot，频繁ping节点检查活跃，导致大量网络IO开销。

jedis最新版本，对于这些过度的hash slot更新和ping，都进行了优化，避免了类似问题。

（3）hashslot迁移和ask重定向

如果hash slot正在迁移，那么会返回ask重定向给jedis。

jedis接收到ask重定向之后，会重新定位到目标节点去执行，但是因为ask发生在hash slot迁移过程中，所以JedisCluster API收到ask是不会更新hashslot本地缓存。

已经可以确定说，hashslot已经迁移完了，moved是会更新本地hashslot->node映射表缓存的。

### 15.高可用性与主备切换原理

redis cluster的高可用的原理，几乎跟哨兵是类似的。

1、判断节点宕机

如果一个节点认为另外一个节点宕机，那么就是pfail，主观宕机。

如果多个节点都认为另外一个节点宕机了，那么就是fail，客观宕机，跟哨兵的原理几乎一样，sdown，odown。

在cluster-node-timeout内，某个节点一直没有返回pong，那么就被认为pfail。

如果一个节点认为某个节点pfail了，那么会在gossip ping消息中，ping给其他节点，如果超过半数的节点都认为pfail了，那么就会变成fail。

2、从节点过滤

对宕机的master node，从其所有的slave node中，选择一个切换成master node。

检查每个slave node与master node断开连接的时间，如果超过了cluster-node-timeout * cluster-slave-validity-factor，那么就没有资格切换成master。

这个也是跟哨兵是一样的，从节点超时过滤的步骤。

3、从节点选举

哨兵：对所有从节点进行排序，slave priority，offset，run id。

每个从节点，都根据自己对master复制数据的offset，来设置一个选举时间，offset越大（复制数据越多）的从节点，选举时间越靠前，优先进行选举。

所有的master node开始slave选举投票，给要进行选举的slave进行投票，如果大部分master node（N/2 + 1）都投票给了某个从节点，那么选举通过，那个从节点可以切换成master。

从节点执行主备切换，从节点切换为主节点。

4、与哨兵比较

整个流程跟哨兵相比，非常类似，所以说，redis cluster功能强大，直接集成了replication和sentinal的功能。

## 多级缓存架构

### 1.三级缓存

三级缓存：nginx本地缓存+redis分布式缓存+tomcat堆缓存的多级缓存架构。

一般来说，显示的库存，都是时效性要求会相对高一些，因为随着商品的不断的交易，库存会不断的变化。

当然，我们就希望当库存变化的时候，尽可能更快将库存显示到页面上去，而不是说等了很长时间，库存才反应到页面上去。

时效性要求不高的数据：商品的基本信息（名称、颜色、版本、规格参数，等等）。

时效性要求不高的数据，就还好，比如说你现在改变了商品的名称，稍微晚个几分钟反应到商品页面上，也还能接受。

商品价格/库存等时效性要求高的数据，而且种类较少，采取相关的服务系统每次发生了变更的时候，直接采取数据库和redis缓存双写的方案，这样缓存的时效性最高。

商品基本信息等时效性不高的数据，而且种类繁多，来自多种不同的系统，采取MQ异步通知的方式，写一个数据生产服务，监听MQ消息，然后异步拉取服务的数据，更新tomcat jvm缓存+redis缓存。

nginx+lua脚本做页面动态生成的工作，每次请求过来，优先从nginx本地缓存中提取各种数据，结合页面模板，生成需要的页面。

如果nginx本地缓存过期了，那么就从nginx到redis中去拉取数据，更新到nginx本地。

如果redis中也被LRU算法清理掉了，那么就从nginx走http接口到后端的服务中拉取数据，数据生产服务中，现在本地tomcat里的jvm堆缓存中找，ehcache，如果也被LRU清理掉了，那么就重新发送请求到源头的服务中去拉取数据，然后再次更新tomcat堆内存缓存+redis缓存，并返回数据给nginx，nginx缓存到本地。

### 2.多级缓存架构中每一层的意义

nginx本地缓存

抗的是热数据的高并发访问，一般来说，商品的购买总是有热点的，比如每天购买iphone、nike、海尔等知名品牌的东西的人，总是比较多的。

这些热数据，利用nginx本地缓存，由于经常被访问，所以可以被锁定在nginx的本地缓存内。

大量的热数据的访问，就是经常会访问的那些数据，就会被保留在nginx本地缓存内，那么对这些热数据的大量访问，就直接走nginx就可以了。

那么大量的访问，直接就可以走到nginx就行了，不需要走后续的各种网络开销了。

可以用ehcache来做本地的堆缓存。

redis分布式大规模缓存

抗的是很高的离散访问，支撑海量的数据，高并发的访问，高可用的服务。

redis缓存最大量的数据，最完整的数据和缓存，1T+数据; 支撑高并发的访问，QPS最高到几十万; 可用性，非常好，提供非常稳定的服务。

nginx本地内存有限，也就能cache住部分热数据，除了各种iphone、nike等热数据，其他相对不那么热的数据，可能流量会经常走到redis那里。

利用redis cluster的多master写入，横向扩容，1T+以上海量数据支持，几十万的读写QPS，99.99%高可用性，那么就可以抗住大量的离散访问请求。

tomcat jvm堆内存缓存

主要是抗redis大规模灾难的，如果redis出现了大规模的宕机，导致nginx大量流量直接涌入数据生产服务，那么最后的tomcat堆内存缓存至少可以再抗一下，不至于让数据库直接裸奔。

同时tomcat jvm堆内存缓存，也可以抗住redis没有cache住的最后那少量的部分缓存。

### 3.缓存数据生产服务的工作流程分析

（1）监听多个kafka topic，每个kafka topic对应一个服务（简化一下，监听一个kafka topic） （2）如果一个服务发生了数据变更，那么就发送一个消息到kafka topic中 （3）缓存数据生产服务监听到了消息以后，就发送请求到对应的服务中调用接口以及拉取数据，此时是从mysql中查询的 （4）缓存数据生产服务拉取到了数据之后，会将数据在本地缓存中写入一份，就是ehcache中 （5）同时会将数据在redis中写入一份

基于kafka+ehcache+redis完成缓存数据生产服务。

### 4.缓存命中率低

缓存数据生产服务那一层已经搞定了，相当于三层缓存架构中的本地堆缓存+redis分布式缓存都搞定了。

就要来做三级缓存中的nginx那一层的缓存了。

如果一般来说，你默认会部署多个nginx，在里面都会放一些缓存，就默认情况下，此时缓存命中率是比较低的。

如何提升缓存命中率

分发层+应用层，双层nginx。

分发层nginx，负责流量分发的逻辑和策略，这个里面它可以根据你自己定义的一些规则，比如根据productId去进行hash，然后对后端的nginx数量取模。

将某一个商品的访问的请求，就固定路由到一个nginx后端服务器上去，保证说只会从redis中获取一次缓存数据，后面全都是走nginx本地缓存了。

后端的nginx服务器，就称之为应用服务器; 最前端的nginx服务器，被称之为分发服务器。

看似很简单，其实很有用，在实际的生产环境中，可以大幅度提升你的nginx本地缓存这一层的命中率，大幅度减少redis后端的压力，提升性能。

### 5.流量分发策略

在分发层nginx中，编写lua脚本，完成流量分发策略。比如：

1、获取请求参数，比如productId。 2、对productId进行hash。 3、hash值对应用服务器数量取模，获取到一个应用服务器。 4、利用http发送请求到应用层nginx。 5、获取响应后返回。

这个就是基于商品id的定向流量分发的策略，lua脚本来编写和实现。

### 6.nginx+lua实现多级缓存

1、应用nginx的lua脚本接收到请求。

2、获取请求参数中的商品id，以及商品店铺id。

3、根据商品id和商品店铺id，在nginx本地缓存中尝试获取数据。

4、如果在nginx本地缓存中没有获取到数据，那么就到redis分布式缓存中获取数据，如果获取到了数据，还要设置到nginx本地缓存中。

但是这里有个问题，建议不要用nginx+lua直接去获取redis数据。

因为openresty没有太好的redis cluster的支持包，所以建议是发送http请求到缓存数据生产服务，由该服务提供一个http接口。

缓存数生产服务可以基于redis cluster api从redis中直接获取数据，并返回给nginx。

5、如果缓存数据生产服务没有在redis分布式缓存中没有获取到数据，那么就在自己本地ehcache中获取数据，返回数据给nginx，也要设置到nginx本地缓存中。

6、如果ehcache本地缓存都没有数据，那么就需要去原始的服务中拉去数据，该服务会从mysql中查询，拉去到数据之后，返回给nginx，并重新设置到ehcache和redis中。

7、nginx最终利用获取到的数据，动态渲染网页模板。

8、将渲染后的网页模板作为http响应，返回给分发层nginx。

### 7.分布式重建缓存的并发冲突问题

如果缓存服务在本地的ehcache中都读取不到数据，那就意味着，需要重新到源头的服务中去拉去数据，拉取到数据之后，赶紧先给nginx的请求返回，同时将数据写入ehcache和redis中。

分布式重建缓存的并发冲突问题。

重建缓存：比如我们这里，数据在所有的缓存中都不存在了（LRU算法弄掉了），就需要重新查询数据写入缓存，重建缓存。

分布式的重建缓存，在不同的机器上，不同的服务实例中，去做上面的事情，就会出现多个机器分布式重建去读取相同的数据，然后写入缓存中。

分布式重建缓存的并发冲突问题。。。。。。

1、流量均匀分布到所有缓存服务实例上

应用层nginx，是将请求流量均匀地打到各个缓存服务实例中的，可能咱们的eshop-cache那个服务，可能会部署多实例在不同的机器上。

2、应用层nginx的hash，固定商品id，走固定的缓存服务实例

分发层的nginx的lua脚本，是怎么写的，怎么玩儿的，搞一堆应用层nginx的地址列表，对每个商品id做一个hash，然后对应用nginx数量取模。

将每个商品的请求固定分发到同一个应用层nginx上面去。

在应用层nginx里，发现自己本地lua shared dict缓存中没有数据的时候，就采取一样的方式，对product id取模，然后将请求固定分发到同一个缓存服务实例中去

这样的话，就不会出现说多个缓存服务实例分布式的去更新那个缓存了。

3、源信息服务发送的变更消息，需要按照商品id去分区，固定的商品变更走固定的kafka分区，也就是固定的一个缓存服务实例获取到

缓存服务，是监听kafka topic的，一个缓存服务实例，作为一个kafka consumer，就消费topic中的一个partition。

所以你有多个缓存服务实例的话，每个缓存服务实例就消费一个kafka partition。

所以这里，一般来说，你的源头信息服务，在发送消息到kafka topic的时候，都需要按照product id去分区。

也就时说，同一个product id变更的消息一定是到同一个kafka partition中去的，也就是说同一个product id的变更消息，一定是同一个缓存服务实例消费到的。

我们也不去做了，其实很简单，kafka producer api，里面send message的时候，多加一个参数就可以了，product id传递进去，就可以了。

4、问题是，自己写的简易的hash分发，与kafka的分区，可能并不一致！！！

我们自己写的简易的hash分发策略，是按照crc32去取hash值，然后再取模的。

关键你又不知道你的kafka producer的hash策略是什么，很可能说跟我们的策略是不一样的。

那就可能导致说，数据变更的消息所到的缓存服务实例，跟我们的应用层nginx分发到的那个缓存服务实例也许就不在一台机器上了。

这样的话，在高并发，极端的情况下，可能就会出现冲突。

5、分布式的缓存重建并发冲突问题发生了。。。

6、基于zookeeper分布式锁的解决方案

分布式锁，如果你有多个机器在访问同一个共享资源，那么这个时候，如果你需要加个锁，让多个分布式的机器在访问共享资源的时候串行起来。

那么这个时候，那个锁，多个不同机器上的服务共享的锁，就是分布式锁。

分布式锁当然有很多种不同的实现方案，redis分布式锁，zookeeper分布式锁。

zk分布式锁的解决并发冲突的方案

（1）变更缓存重建以及空缓存请求重建，更新redis之前，都需要先获取对应商品id的分布式锁。 （2）拿到分布式锁之后，需要根据时间版本去比较一下，如果自己的版本新于redis中的版本，那么就更新，否则就不更新。 （3）如果拿不到分布式锁，那么就等待，不断轮询等待，直到自己获取到分布式的锁。

### 8.zk分布式锁的原理

我们通过去创建zk的一个临时node，来模拟给某一个商品id加锁。

zk会给你保证说，只会创建一个临时node，其他请求过来如果再要创建临时node，就会报错，NodeExistsException。

那么所以说，我们的所谓上锁，其实就是去创建某个product id对应的一个临时node。

如果临时node创建成功了，那么说明我们成功加锁了，此时就可以去执行对redis里面数据的操作。

如果临时node创建失败了，说明有人已经在拿到锁了，在操作reids中的数据，那么就不断的等待，直到自己可以获取到锁为止。

释放一个分布式锁，去删除掉那个临时node就可以了，就代表释放了一个锁，那么此时其他的机器就可以成功创建临时node，获取到锁。

### 9.zk分布式锁解决方案

1、主动更新

监听kafka消息队列，获取到一个商品变更的消息之后，去哪个源服务中调用接口拉取数据，更新到ehcache和redis中。

先获取分布式锁，然后才能更新redis，同时更新时要比较时间版本。

2、被动重建

直接读取源头数据，直接返回给nginx，同时推送一条消息到一个队列，后台线程异步消费。

后台线程负责先获取分布式锁，然后才能更新redis，同时要比较时间版本。

## 缓存双写一致

### 1.cache aside pattern

最经典的缓存+数据库读写的模式，cache aside pattern。

（1）读的时候，先读缓存，缓存没有的话，那么就读数据库，然后取出数据后放入缓存，同时返回响应。

（2）更新的时候，先删除缓存，然后再更新数据库。

### 2.为什么是删除缓存，而不是更新缓存呢？

原因很简单，很多时候，复杂点的缓存的场景，因为缓存有的时候，不简单是数据库中直接取出来的值。

商品详情页的系统，修改库存，只是修改了某个表的某些字段，但是要真正把这个影响的最终的库存计算出来，可能还需要从其他表查询一些数据，然后进行一些复杂的运算，才能最终计算出。

现在最新的库存是多少，然后才能将库存更新到缓存中去。

比如可能更新了某个表的一个字段，然后其对应的缓存，是需要查询另外两个表的数据，并进行运算，才能计算出缓存最新的值的。

更新缓存的代价是很高的。

是不是说，每次修改数据库的时候，都一定要将其对应的缓存去跟新一份？也许有的场景是这样的，但是对于比较复杂的缓存数据计算的场景，就不是这样了。

如果你频繁修改一个缓存涉及的多个表，那么这个缓存会被频繁的更新，频繁的更新缓存。

但是问题在于，这个缓存到底会不会被频繁访问到？？？

举个例子，一个缓存涉及的表的字段，在1分钟内就修改了20次，或者是100次，那么缓存跟新20次，100次; 但是这个缓存在1分钟内就被读取了1次，有大量的冷数据。

28法则，黄金法则，20%的数据，占用了80%的访问量。

实际上，如果你只是删除缓存的话，那么1分钟内，这个缓存不过就重新计算一次而已，开销大幅度降低。

每次数据过来，就只是删除缓存，然后修改数据库，如果这个缓存，在1分钟内只是被访问了1次，那么只有那1次，缓存是要被重新计算的，用缓存才去算缓存。

其实删除缓存，而不是更新缓存，就是一个lazy计算的思想，不要每次都重新做复杂的计算，不管它会不会用到，而是让它到需要被使用的时候再重新计算。

mybatis，hibernate，懒加载，思想。

查询一个部门，部门带了一个员工的list，没有必要说每次查询部门，都里面的1000个员工的数据也同时查出来啊。

80%的情况，查这个部门，就只是要访问这个部门的信息就可以了。

先查部门，同时要访问里面的员工，那么这个时候只有在你要访问里面的员工的时候，才会去数据库里面查询1000个员工。

### 3.最初级的缓存不一致问题以及解决方案

问题：先修改数据库，再删除缓存，如果删除缓存失败了，那么会导致数据库中是新数据，缓存中是旧数据，数据出现不一致。

解决思路

先删除缓存，再修改数据库，如果删除缓存成功了，如果修改数据库失败了，那么数据库中是旧数据，缓存中是空的，那么数据不会不一致。

因为读的时候缓存没有，则读数据库中旧数据，然后更新到缓存中。

### 4.比较复杂的数据不一致问题分析

数据发生了变更，先删除了缓存，然后要去修改数据库，此时还没修改。

一个请求过来，去读缓存，发现缓存空了，去查询数据库，查到了修改前的旧数据，放到了缓存中。

数据变更的程序完成了数据库的修改。

完了，数据库和缓存中的数据不一样了。。。。

### 5.数据库与缓存更新与读取操作进行异步串行化

更新数据的时候，根据数据的唯一标识，将操作路由之后，发送到一个jvm内部的队列中。

读取数据的时候，如果发现数据不在缓存中，那么将重新读取数据+更新缓存的操作，根据唯一标识路由之后，也发送同一个jvm内部的队列中。

一个队列对应一个工作线程。

每个工作线程串行拿到对应的操作，然后一条一条的执行。

这样的话，一个数据变更的操作，先执行，删除缓存，然后再去更新数据库，但是还没完成更新。

此时如果一个读请求过来，读到了空的缓存，那么可以先将缓存更新的请求发送到队列中，此时会在队列中积压，然后同步等待缓存更新完成

这里有一个优化点，一个队列中，其实多个更新缓存请求串在一起是没意义的，因此可以做过滤，如果发现队列中已经有一个更新缓存的请求了，那么就不用再放个更新请求操作进去了，直接等待前面的更新操作请求完成即可。

待那个队列对应的工作线程完成了上一个操作的数据库的修改之后，才会去执行下一个操作，也就是缓存更新的操作，此时会从数据库中读取最新的值，然后写入缓存中。

如果请求还在等待时间范围内，不断轮询发现可以取到值了，那么就直接返回; 如果请求等待的时间超过一定时长，那么这一次直接从数据库中读取当前的旧值。

```html
int h;
return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);

(queueNum - 1) & hash
```

高并发的场景下，该解决方案要注意的问题

（1）读请求长时阻塞

由于读请求进行了非常轻度的异步化，所以一定要注意读超时的问题，每个读请求必须在超时时间范围内返回。

该解决方案，最大的风险点在于说，可能数据更新很频繁，导致队列中积压了大量更新操作在里面，然后读请求会发生大量的超时，最后导致大量的请求直接走数据库。

务必通过一些模拟真实的测试，看看更新数据的频繁是怎样的。

另外一点，因为一个队列中，可能会积压针对多个数据项的更新操作，因此需要根据自己的业务情况进行测试，可能需要部署多个服务，每个服务分摊一些数据的更新操作。

如果一个内存队列里居然会挤压100个商品的库存修改操作，每隔库存修改操作要耗费10ms区完成，那么最后一个商品的读请求，可能等待10 * 100 = 1000ms = 1s后，才能得到数据。

这个时候就导致读请求的长时阻塞。

一定要做根据实际业务系统的运行情况，去进行一些压力测试，和模拟线上环境，去看看最繁忙的时候，内存队列可能会挤压多少更新操作，可能会导致最后一个更新操作对应的读请求，会hang多少时间，如果读请求在200ms返回，如果你计算过后，哪怕是最繁忙的时候，积压10个更新操作，最多等待200ms，那还可以的。

如果一个内存队列可能积压的更新操作特别多，那么你就要加机器，让每个机器上部署的服务实例处理更少的数据，那么每个内存队列中积压的更新操作就会越少。

其实根据之前的项目经验，一般来说数据的写频率是很低的，因此实际上正常来说，在队列中积压的更新操作应该是很少的。

针对读高并发，读缓存架构的项目，一般写请求相对读来说，是非常非常少的，每秒的QPS能到几百就不错了。

一秒，500的写操作，5份，每200ms，就100个写操作。

单机器，20个内存队列，每个内存队列，可能就积压5个写操作，每个写操作性能测试后，一般在20ms左右就完成。

那么针对每个内存队列中的数据的读请求，也就最多hang一会儿，200ms以内肯定能返回了。

写QPS扩大10倍，但是经过刚才的测算，就知道，单机支撑写QPS几百没问题，那么就扩容机器，扩容10倍的机器，10台机器，每个机器20个队列，200个队列

大部分的情况下，应该是这样的，大量的读请求过来，都是直接走缓存取到数据的。

少量情况下，可能遇到读跟数据更新冲突的情况，如上所述，那么此时更新操作如果先入队列，之后可能会瞬间来了对这个数据大量的读请求，但是因为做了去重的优化，所以也就一个更新缓存的操作跟在它后面。

等数据更新完了，读请求触发的缓存更新操作也完成，然后临时等待的读请求全部可以读到缓存中的数据。

（2）读请求并发量过高

这里还必须做好压力测试，确保恰巧碰上上述情况的时候，还有一个风险，就是突然间大量读请求会在几十毫秒的延时hang在服务上，看服务能不能抗的住，需要多少机器才能抗住最大的极限情况的峰值。

但是因为并不是所有的数据都在同一时间更新，缓存也不会同一时间失效，所以每次可能也就是少数数据的缓存失效了，然后那些数据对应的读请求过来，并发量应该也不会特别大

按1:99的比例计算读和写的请求，每秒5万的读QPS，可能只有500次更新操作。

如果一秒有500的写QPS，那么要测算好，可能写操作影响的数据有500条，这500条数据在缓存中失效后，可能导致多少读请求，发送读请求到库存服务来，要求更新缓存。

一般来说，1:1，1:2，1:3，每秒钟有1000个读请求，会hang在库存服务上，每个读请求最多hang多少时间，200ms就会返回。

在同一时间最多hang住的可能也就是单机200个读请求，同时hang住。

单机hang200个读请求，还是ok的。

1:20，每秒更新500条数据，这500秒数据对应的读请求，会有20 * 500 = 1万。

1万个读请求全部hang在库存服务上，就死定了。

（3）多服务实例部署的请求路由

可能这个服务部署了多个实例，那么必须保证说，执行数据更新操作，以及执行缓存更新操作的请求，都通过nginx服务器路由到相同的服务实例上。

（4）热点商品的路由问题，导致请求的倾斜

万一某个商品的读写请求特别高，全部打到相同的机器的相同的队列里面去了，可能造成某台机器的压力过大。

就是说，因为只有在商品数据更新的时候才会清空缓存，然后才会导致读写并发，所以更新频率不是太高的话，这个问题的影响并不是特别大。

但是的确可能某些机器的负载会高一些。

### 6.缓存纬度化

做缓存更新的操作，每次都全量更新缓存，那么就会耗费更多的网络资源，对redis的压力也大，因为要频繁对redis存储大量数据。

缓存纬度化之后，就是说，按纬度去拆分数据，按维度去存储缓存，按纬度去更新缓存。每次对redis读写的数据也就变少了，redis压力就变小了。

## 缓存清理

### 1.LRU算法概述

redis默认情况下就是使用LRU策略的，因为内存是有限的，但是如果你不断地往redis里面写入数据，那肯定是没法存放下所有的数据在内存的。

所以redis默认情况下，当内存中写入的数据很满之后，就会使用LRU算法清理掉部分内存中的数据，腾出一些空间来，然后让新的数据写入redis缓存中。

LRU：Least Recently Used，最近最少使用算法。

将最近一段时间内，最少使用的一些数据，给干掉。

### 2.缓存清理设置

redis.conf

maxmemory，设置redis用来存放数据的最大的内存大小，一旦超出这个内存大小之后，就会立即使用LRU算法清理掉部分数据。

如果用LRU，那么就是将最近最少使用的数据从缓存中清除出去。

对于64 bit的机器，如果maxmemory设置为0，那么就默认不限制内存的使用，直到耗尽机器中所有的内存为止; 但是对于32 bit的机器，有一个隐式的闲置就是3GB。

maxmemory-policy，可以设置内存达到最大闲置后，采取什么策略来处理。

（1）noeviction: 如果内存使用达到了maxmemory，client还要继续写入数据，那么就直接报错给客户端。 （2）allkeys-lru: 就是我们常说的LRU算法，移除掉最近最少使用的那些keys对应的数据。 （3）volatile-lru: 也是采取LRU算法，但是仅仅针对那些设置了指定存活时间（TTL）的key才会清理掉。 （4）allkeys-random: 随机选择一些key来删除掉。 （5）volatile-random: 随机选择一些设置了TTL的key来删除掉。 （6）volatile-ttl: 移除掉部分keys，选择那些TTL时间比较短的keys。

在redis里面，写入key-value对的时候，是可以设置TTL，存活时间，比如你设置了60s。那么一个key-value对，在60s之后就会自动被删除。

redis，给了这么多种乱七八糟的缓存清理的算法，其实真正常用的可能也就那么一两种，allkeys-lru是最常用的

### 3.缓存清理的流程

（1）客户端执行数据写入操作。 （2）redis server接收到写入操作之后，检查maxmemory的限制，如果超过了限制，那么就根据对应的policy清理掉部分数据。 （3）写入操作完成执行。

### 4、redis的LRU近似算法

redis采取的是LRU近似算法，也就是对keys进行采样，然后在采样结果中进行数据清理。

redis 3.0开始，在LRU近似算法中引入了pool机制，表现可以跟真正的LRU算法相当，但是还是有所差距的，不过这样可以减少内存的消耗。

redis LRU算法，是采样之后再做LRU清理的，跟真正的、传统、全量的LRU算法是不太一样的。

maxmemory-samples，比如5，可以设置采样的大小，如果设置为10，那么效果会更好，不过也会耗费更多的CPU资源。

## 缓存预热

### 1.缓存预热

缓存冷启动，redis启动后，一点数据都没有，直接就对外提供服务了，mysql就裸奔。

（1）提前给redis中灌入部分数据，再提供服务。 （2）肯定不可能将所有数据都写入redis，因为数据量太大了，第一耗费的时间太长了，第二根本redis容纳不下所有的数据。 （3）需要根据当天的具体访问情况，实时统计出访问频率较高的热数据。 （4）然后将访问频率较高的热数据写入redis中，肯定是热数据也比较多，我们也得多个服务并行读取数据去写，并行的分布式的缓存预热。 （5）然后将灌入了热数据的redis对外提供服务，这样就不至于冷启动，直接让数据库裸奔了。

1、nginx+lua将访问流量上报到kafka中。

要统计出来当前最新的实时的热数据是哪些，我们就得将商品详情页访问的请求对应的流浪，日志，实时上报到kafka中。

2、storm从kafka中消费数据，实时统计出每个商品的访问次数，访问次数基于LRU内存数据结构的存储方案。

优先用内存中的一个LRUMap去存放，性能高，而且没有外部依赖。

其实我们只要统计出最近一段时间访问最频繁的商品，然后对它们进行访问计数，同时维护出一个前N个访问最多的商品list即可。

热数据，最近一段时间，可以拿到最近一段，比如最近1个小时，最近5分钟，1万个商品请求，统计出最近这段时间内每个商品的访问次数，排序，做出一个排名前N的list。

计算好每个task大致要存放的商品访问次数的数量，计算出大小。

然后构建一个LRUMap，apache commons collections有开源的实现，设定好map的最大大小，就会自动根据LRU算法去剔除多余的数据，保证内存使用限制。

即使有部分数据被干掉了，然后下次来重新开始计数，也没关系，因为如果它被LRU算法干掉，那么它就不是热数据，说明最近一段时间都很少访问了。

3、每个storm task启动的时候，基于zk分布式锁，将自己的id写入zk同一个节点中。

4、每个storm task负责完成自己这里的热数据的统计，每隔一段时间，就遍历一下这个map，然后维护一个前3个商品的list，更新这个list。

5、写一个后台线程，每隔一段时间，比如1分钟，都将排名前3的热数据list，同步到zk中去，存储到这个storm task对应的一个znode中去。

6、我们需要一个服务，比如说，代码可以跟缓存数据生产服务放一起，但是也可以放单独的服务。

服务可能部署了很多个实例。

每次服务启动的时候，就会去拿到一个storm task的列表，然后根据taskid，一个一个的去尝试获取taskid对应的znode的zk分布式锁。

如果能获取到分布式锁的话，那么就将那个storm task对应的热数据的list取出来。

然后将数据从mysql中查询出来，写入缓存中，进行缓存的预热，多个服务实例，分布式的并行的去做，基于zk分布式锁做了协调了，分布式并行缓存的预热。

### 2.缓存预热方案

1.基于nginx+lua完成商品详情页访问流量实时上报kafka

在nginx这一层，接收到访问请求的时候，就把请求的流量上报发送给kafka。

这样的话，storm才能去消费kafka中的实时的访问日志，然后去进行缓存热数据的统计。

从lua脚本直接创建一个kafka producer，发送数据到kafka。

2.基于storm+kafka完成商品访问次数实时统计拓扑

1、kafka consumer spout

单独的线程消费，写入队列。

nextTuple，每次都是判断队列有没有数据，有的话再去获取并发射出去，不能阻塞。

2、日志解析bolt

3、商品访问次数统计bolt

基于LRUMap完成统计。

3.基于storm完成LRUMap中topn热门商品列表的算法

1、storm task启动的时候，基于分布式锁将自己的taskid累加到一个znode中。

2、开启一个单独的后台线程，每隔1分钟算出top3热门商品list。

3、每个storm task将自己统计出的热数据list写入自己对应的znode中。

4.基于storm+zookeeper完成热门商品列表的分段存储

1、task初始化。

2、热门商品list保存。

5.基于双重zookeeper分布式锁完成分布式并行缓存预热

1、服务启动的时候，进行缓存预热。

2、从zk中读取taskid列表。

3、依次遍历每个taskid，尝试获取分布式锁，如果获取不到，快速报错，不要等待，因为说明已经有其他服务实例在预热了。

4、直接尝试获取下一个taskid的分布式锁。

5、即使获取到了分布式锁，也要检查一下这个taskid的预热状态，如果已经被预热过了，就不再预热了。

6、执行预热操作，遍历productid列表，查询数据，然后写ehcache和redis。

7、预热完成后，设置taskid对应的预热状态。

### 3.瞬间的缓存热点

某一个缓存数据，突然间，变成了热点，比如搞活动的时候，特别是秒杀，导致一秒之间，某些商品访问量特别巨大。同一个商品，hash后，访问全部都是到后端同一台nginx服务器。

### 4.基于nginx+lua+storm的热点缓存的流量分发策略自动降级解决方案

1、在storm中，实时的计算出瞬间出现的热点。

某个storm task，上面算出了1万个商品的访问次数，LRUMap。

频率高一些，每隔5秒，去遍历一次LRUMap，将其中的访问次数进行排序，统计出往后排的95%的商品访问次数的平均值

比如说，95%的商品，访问次数的平均值是100。

然后，从最前面开始，往后遍历，去找有没有瞬间出现的热点数据。

1000，95%的平均值（100）的10倍，这个时候要设定一个阈值，比如说超出95%平均值得n倍，5倍。

我们就认为是瞬间出现的热点数据，判断其可能在短时间内继续扩大的访问量，甚至达到平均值几十倍，或者几百倍。

当遍历，发现说第一个商品的访问次数，小于平均值的5倍，就安全了，就break掉这个循环。

热点数据，热数据，不是一个概念。

有100个商品，前10个商品比较热，都访问量在500左右，其他的普通商品，访问量都在200左右，就说前10个商品是热数据。

统计出来。

预热的时候，将这些热数据放在缓存中去预热就可以了。

热点，前面某个商品的访问量，瞬间超出了普通商品的10倍，或者100倍，1000倍，热点。

2、storm这里，会直接发送http请求到nginx上，nginx上用lua脚本去处理这个请求。

storm会将热点本身对应的productId，发送到流量分发的nginx上面去，放在本地缓存中。

storm会将热点对应的完整的缓存数据，发送到所有的应用nginx服务器上去，直接放在本地缓存中。

3、流量分发nginx的分发策略降级。

流量分发nginx，加一个逻辑，就是每次访问一个商品详情页的时候，如果发现它是个热点，那么立即做流量分发策略的降级。

hash策略，同一个productId的访问都同一台应用nginx服务器上。

降级成对这个热点商品，流量分发采取随机负载均衡发送到所有的后端应用nginx服务器上去。

瞬间将热点缓存数据的访问，从hash分发，全部到一台nginx，变成了，负载均衡发送到多台nginx上去。

避免说大量的流量全部集中到一台机器，50万的访问量到一台nginx，5台应用nginx，每台就可以承载10万的访问量。

4、storm还需要保存下来上次识别出来的热点list。

下次去识别的时候，这次的热点list跟上次的热点list做一下diff，看看可能有的商品已经不是热点了。

热点的取消的逻辑，发送http请求到流量分发的nginx上去，取消掉对应的热点数据，从nginx本地缓存中，删除。

## 缓存服务

### 1.缓存服务

缓存服务，订阅一个MQ的消息变更，如果有消息变更的话，那么就会发送一个网络请求，调用一个底层的对应的源数据服务的接口，去获取变更后的数据。

将获取到的变更后的数据填充到分布式的redis缓存中去。nginx会去从redis分布式缓存中获取最新的缓存数据缓存到自己本地，然后返回填充了新数据的html页面。虽然没有直接返回html页面那么快，但是因为数据是在nginx自己本地缓存，所以也很快，耗费的也就是动态渲染一个html页面的性能。html模板变更了，不需要将所有的页面全部重新静态化，每次请求过来，直接就会用最新的html页面模板去响应请求。

高可用这一块儿，最可能出现说可用性不高的情况，是什么呢？就是说，在接收到消息之后，可能在调用各种底层依赖服务的接口时，会遇到各种不稳定的情况。

比如底层服务的接口调用超时，200ms，2s都没有返回; 底层服务的接口调用失败，比如说卡了500ms之后，返回一个报错。

在分布式系统中，对于这种大量的底层依赖服务的调用，就可能会出现各种可用性的问题，一旦没有处理好的话。可能就会导致缓存服务自己本身会挂掉，或者故障掉，就会导致什么呢？不可以对外提供服务，严重情况下，甚至会导致说整个商品详情页显示不出来。

## 缓存雪崩

### 1.缓存雪崩

1、redis集群彻底崩溃。

2、缓存服务大量对redis的请求hang住，占用资源。

3、缓存服务大量的请求打到源头服务去查询mysql，直接打死mysql。

4、源头服务因为mysql被打死也崩溃，对源服务的请求也hang住，占用资源。

5、缓存服务大量的资源全部耗费在访问redis和源服务无果，最后自己被拖死，无法提供服务。

6、nginx无法访问缓存服务，redis和源服务，只能基于本地缓存提供服务，但是缓存过期后，没有数据提供。

7、网站崩溃。

### 2.缓存雪崩的基于事前+事中+事后三个层次的完美解决方案

1、事前解决方案

发生缓存雪崩之前，事情之前，怎么去避免redis彻底挂掉。

redis本身的高可用性，复制，主从架构，操作主节点，读写，数据同步到从节点，一旦主节点挂掉，从节点跟上。

双机房部署，一套redis cluster，部分机器在一个机房，另一部分机器在另外一个机房。

还有一种部署方式，两套redis cluster，两套redis cluster之间做一个数据的同步，redis集群是可以搭建成树状的结构的。

一旦说单个机房出了故障，至少说另外一个机房还能有些redis实例提供服务。

2、事中解决方案

redis cluster已经彻底崩溃了，已经开始大量的访问无法访问到redis了。

（1）ehcache本地缓存

所做的多级缓存架构的作用上了，ehcache的缓存，应对零散的redis中数据被清除掉的现象，另外一个主要是预防redis彻底崩溃。

多台机器上部署的缓存服务实例的内存中，还有一套ehcache的缓存。

ehcache的缓存还能支撑一阵。

（2）对redis访问的资源隔离

（3）对源服务访问的限流以及资源隔离

3、事后解决方案

（1）redis数据可以恢复，做了备份，redis数据备份和恢复，redis重新启动起来。

（2）redis数据彻底丢失了，或者数据过旧，快速缓存预热，redis重新启动起来。

redis对外提供服务。

缓存服务里，熔断策略，自动可以恢复，half-open，发现redis可以访问了，自动恢复了，自动就继续去访问redis了。

缓存架构应对高并发下的缓存雪崩的解决方案，基于hystrix去做缓存服务的保护

### 总结

1、事前，redis高可用性，redis cluster，sentinal，复制，主从，从->主，双机房部署。

2、事中，ehcache可以抗一抗，redis挂掉之后的资源隔离、超时控制、熔断，商品服务的访问限流、多级降级，缓存服务在雪崩场景下存活下来，基于ehcache和存活的商品服务提供数据。

3、事后，快速恢复Redis，备份+恢复，快速的缓存预热的方案。

### 3.基于hystrix完成对redis访问的资源隔离以避免缓存服务被拖垮

用hystrix的command进行封装，做资源隔离，确保说，redis的访问只能在固定的线程池内的资源来进行访问。

哪怕是redis访问的很慢，有等待和超时，也不要紧，只有少量额线程资源用来访问，缓存服务不会被拖垮。

为redis集群崩溃时的访问失败增加fail silent容错机制

资源隔离，避免说redis访问频繁失败，或者频繁超时的时候，耗尽大量的tomcat容器的资源去hang在redis的访问上。

限定只有一部分线程资源可以用来访问redis。

你是不是说，如果redis集群彻底崩溃了，这个时候，可能command对redis的访问大量的报错和timeout超时，熔断（短路）。

降级机制，fallback。

fail silent模式，fallback里面直接返回一个空值，比如一个null，最简单了。

在外面调用redis的代码（CacheService类），是感知不到redis的访问异常的，只要你把timeout、熔断、熔断恢复、降级，都做好了。

可能会出现的情况是，当redis集群崩溃的时候，CacheService获取到的是大量的null空值。

根据这个null空值，我们还可以去做多级缓存的降级访问，nginx本地缓存，redis分布式集群缓存，ehcache本地缓存，CacheController。

### 4.为redis集群崩溃时的场景部署定制化的熔断策略

redis集群崩溃的时候，会怎么样？

（1）首先大量的等待，超时，报错。 （2）如果是短时间内报错，会直接走fallback降级，直接返回null。 （3）超时控制，你应该判断说redis访问超过了多长时间，就直接给timeout掉了。

不推荐说用默认的值，一般不太精准，redis的访问你首先自己先统计一下访问时长的百分比，hystrix dashboard，TP90 TP95 TP99。

一般来说，redis访问，假设说TP99在100ms，那么此时，你的timeout稍微多给一些，100ms

1、timeout超时控制

```html
HystrixCommandProperties.Setter()
   .withExecutionTimeoutInMilliseconds(int value)
```

意义在于哪里，一旦说redis出现了大面积的故障，此时肯定是访问的时候大量的超过100ms，大量的在等待和超时。

就可以确保说，大量的请求不会hang住过长的时间，比如说hang住个1s，500ms，100ms直接就报timeout，走fallback降级了。

2、熔断策略

（1）circuitBreaker.requestVolumeThreshold

设置一个rolling window，滑动窗口中，最少要有多少个请求时，才触发开启短路。

举例来说，如果设置为20（默认值），那么在一个10秒的滑动窗口内，如果只有19个请求，即使这19个请求都是异常的，也是不会触发开启短路器的。

```html
HystrixCommandProperties.Setter()
   .withCircuitBreakerRequestVolumeThreshold(int value)
```

我们应该根据我们自己的平时的访问流量去设置，而不是用默认值，比如说，我们认为平时一般的时候，流量也可以在每秒在QPS 100，10秒的滑动窗口就是1000。

一般来说，你可以设置这样的一个值，根据你自己的系统的流量去设置。

假如说，你设置的太少了，或者太多了，都不太合适。

举个例子，你设置一个20，结果在晚上最低峰的时候，刚好是30，可能晚上的时候因为访问不频繁，大量的找不到缓存，可能超时频繁了一些，结果直接就给短路了。

（2）circuitBreaker.errorThresholdPercentage

设置异常请求量的百分比，当异常请求达到这个百分比时，就触发打开短路器，默认是50，也就是50%。

```html
HystrixCommandProperties.Setter()
   .withCircuitBreakerErrorThresholdPercentage(int value)
```

我们最好还是自己定制，自己设置，你说如果是要50%的时候才短路的话，会有什么情况呢，10%短路，也不太靠谱，90%异常，才短路。

我觉得这个值可以稍微高一些，redis集群彻底崩溃，那么基本上就是所有的请求，100%都会异常，60%，70%。

也有可能偶然出现网络的抖动，导致比如说就这10秒钟，访问延时高了一些，其实可能并不需要立即就短路，可能下10秒马上就恢复了。

金融支付类的接口，可能这个比例就会设置的很低，因为对异常系统必须要很敏感，可能就是10%异常了，就直接短路了，不让继续访问了。

比如金融支付类的接口，正常来说，是很重要的，而且必须是很稳定，我们不能容忍任何的延迟或者是报错。

一旦支付类的接口，有10%的异常的话，我们基本就可以认为这个接口已经出问题了，再继续访问的话，也许访问的就是有问题的接口，可能造成资金的错乱，等给公司造成损失。

熔断，不让访问了，走降级策略。

就是对整个系统，是一个安全性的保障。

（3）circuitBreaker.sleepWindowInMilliseconds

设置在短路之后，需要在多长时间内直接reject请求，然后在这段时间之后，再重新导holf-open状态，尝试允许请求通过以及自动恢复，默认值是5000毫秒。

```html
HystrixCommandProperties.Setter()
   .withCircuitBreakerSleepWindowInMilliseconds(int value)
```

如果redis集群崩溃了，会在5s内就直接恢复。

### 5.基于hystrix限流完成源服务的过载保护以避免流量洪峰打死MySQL

redis集群彻底崩溃的时候，一个是对redis本身做资源隔离、超时控制、熔断策略。

大量的请求，高并发会去访问源服务，商品服务（提供商品数据），QPS 10000去访问商品服务，基于mysql去查询。

QPS 10000去访问mysql，会怎么样，mysql打死，商品服务也会死掉。

就是要对商品服务这种源服务的访问施加限流的措施。

限流怎么限，hystrix本身就是提供了两种机制，线程池（内部做了异步化处理，可以处理超时），semaphore（信号量，让tomcat线程执行运行逻辑，没有内部的异步化处理，一旦超时，会导致tomcat线程就hang住了）。

一般推荐的是，线程池用来做有网络访问的这种资源隔离，因为涉及到网络，就很容易超时；sempahore是用来做对服务纯内存的一些复杂业务逻辑的操作，进行限流，因为不涉及网络访问，就是纯粹为了避免说对内存内的复杂业务逻辑进行太高并发的访问，造成系统本身的故障。

semaphore是很合适的，比如一些推荐、搜索，有部分算法，复杂的算法，是放在服务内部纯内存去运行的，一个服务暴露出来的就是某个算法的执行。

这个时候，就很适合用semaphore。

访问外部的商品服务，所以还是用线程池做限流了。。。

算一下，要限多少，怎么限。

假设说，每次商品服务的访问性能在200ms，1个线程一秒可以执行5次访问，假设说我们一个缓存服务实例对这个商品服务的访问每秒在150次。

所以这个时候，我们就需要30个线程，每个线程每秒可以访问5次，总共每秒30个线程可以访问150次。

这个时候呢，我们限流，要做得事情是这样子的，我们算的这个每秒150次访问时正常情况下，如果是非正常情况下，每秒1000次，甚至1w次，此时就可以自然限流。

因为我们的线程池就30个。。。，还要设置等待队列。

非正常情况下，直接线程池+等待队列全满，此时就会会出现大量的reject操作，然后就会去调用降级逻辑。

接下来，我们要做限流，设置的就是线程池的大小，还有等待队列的大小，30个线程可以每秒处理150个请求，但是偶尔会多一些出来，同时30个线程处理150个请求会快一些，不用花费1秒钟，等待队列给一些buffer，不要偶尔1秒钟来了200条请求，50条直接给reject掉，等待队列，150个，30个线程直接500ms处理完了，等待队列中的50个请求可以继续处理。

### 6.stubbed fallback降级机制

限流过后，就会导致什么呢，比如redis集群崩溃了，雪崩，大量的请求涌入到商品服务调用的command中，是线程池不够。

reject，被reject掉的请求就会去执行fallback降级逻辑。

理清楚一些前提，首先一个请求都发送到这里来了，那么nginx本地缓存肯定就没了，redis已经崩溃了，ehcache中找不到这条数据对应的缓存。

只能从源头的商品服务里面去查询，但是被限流了，这个请求只能走降级方案。

都是用之前讲解的一些技术，stubbed fallback降级机制，残缺的降级。

一般这种情况下，就是说，用请求参数中少量的数据，加上纯内存中缓存的少量的数据来提供残缺的数据服务。

就给大家举个例子，我们之前讲解的stubbed fallback，是从内存中加载了部分品牌数据，加载了部分城市地理位置的数据啦。。。

方案，可以做，冷热分离。

冷数据，也就是说你可以这么认为，将一些过时的数据，比如一个商品信息一周前的版本，放入大数据的在线存储中，比如比较合适做冷数据存放的是hbase。

hadoop，离线批处理，hdfs分布式存储，yarn分布式资源调度（跟hbase没关系），mapreduce分布式计算。

hbase，基于hdfs分布式存储基础之上，封装了一个系统，叫做hbase，分布式在线存储，分布式NoSQL数据库，里面可以放大量的冷数据。

hbase，可以做商品服务热数据是放mysql，可以将一周前，一个月前的数据快照，做一份冷备放到hbase来备用。

你本来正常情况下是直接去访问商品服务，去拉取热数据。

发送请求去访问hbase，去加载冷数据，hbase本身是分布式的，所以也是可以承载高并发的访问的（分布式的特性比mysql），即使这个时候大量并发到了hbase，如果你集群运维够好的话，也开始以撑住的，加载到一条冷数据的话，那么此时就是过期的数据，商品一周前或者一个月前的一个快照版本。

但是至少有数据，还可以显示一下。

多级降级机制，先走hbase冷备，然后再走stubbed fallback。

## 缓存穿透

### 1.缓存穿透

大量数据库中没有的数据请求访问，相当于说缓冲根本没起作用，这样的话会导致每次请求都会直接穿透所有层的缓存，然后大量的高并发请求达到MYSQL上去，执行各种SQL语句，但是呢查询出来的数据每次都是空。

### 2.缓存穿透的保护性机制

每次如果从源服务（商品服务）查询到的数据是空，就说明这个数据根本就不存在。

那么如果这个数据不存在的话，我们不要不往redis和ehcache等缓存中写入数据，我们呢，给写入一个空的数据，比如说空的productInfo的json串。

给nginx也是，返回一个空的productInfo的json串咯。

因为我们有一个异步监听数据变更的机制在里面，也就是说，如果数据变更的话，某个数据本来是没有的，可能会导致缓存穿透，所以我们给了个空数据。

但是现在这个数据有了，我们接收到这个变更的消息过后，就可以将数据再次从源服务中查询出来。

然后设置到各级缓存中去了。

## 缓存失效

### 1.缓存失效

我们在nginx中设置本地的缓存的时候，会给一个过期的时间，比如说10分钟。

10分钟以后自动过期，过期了以后，就会重新从redis中去获取数据。

这个10分钟到期自动过期的事情，就叫做缓存的失效。

如果缓存失效以后，那么实际上此时，就会有大量的请求回到redis中去查询。

缓存失效的问题。。。。

如果说同一时间来了1000个请求，都将缓存cache在了nginx自己的本地，缓存失效的时间都设置了10分钟。

那么是不是可能导致10分钟过后，这些数据，就自动全部在同一时间失效了。

如果同一时间全部失效，会不会导致说同一时间大量的请求过来，在nginx里找不到缓存数据，全部高并发走到redis上去了。

加重大量的网络请求，网络负载也会加重。

### 2.缓存失效的保护机制

在nginx lua脚本中开发缓存失效的保护性机制。基于随机过期时间的缓存失效解决方案。

```html
math.randomseed(tostring(os.time()):reverse():sub(1, 7))
local expireTime = math.random(600, 1200)  
```