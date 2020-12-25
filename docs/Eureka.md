# **Eureka**

## 1.**注册表存储结构**

![Eureka001](/images/Eureka001.webp)

- 这个名字叫做**registry**的**CocurrentHashMap**，就是注册表的核心结构。
- Eureka Server的注册表直接基于**纯内存**，即在内存里维护了一个数据结构。各个服务的注册、服务下线、服务故障，全部会在内存里维护和更新这个注册表。
- 各个服务每隔30秒拉取注册表的时候，Eureka Server就是直接提供内存里存储的有变化的注册表数据给他们就可以了。同样，每隔30秒发起心跳时，也是在这个纯内存的Map数据结构里更新心跳时间。
- 这个ConcurrentHashMap的key就是服务名称,value则代表了一个服务的多个服务实例。
- **Map<String, Lease<InstanceInfo**>>,这个Map的key就是**服务实例的id**,value是一个叫做**Lease**的类，它的泛型是一个叫做**InstanceInfo**的东西,这个InstanceInfo就代表了**服务实例的具体信息**，比如机器的ip地址、hostname以及端口号。而这个Lease，里面则会维护每个服务**最近一次发送心跳的时间**.

## 2.**Eureka Server端优秀的多级缓存机制**

- 在拉取注册表的时候：

- - 首先从**ReadOnlyCacheMap**里查缓存的注册表。

- - 若没有，就找**ReadWriteCacheMap**里缓存的注册表。

- - 如果还没有，就从**内存中获取实际的注册表数据。**

- 在注册表发生变更的时候：

- - 会在内存中更新变更的注册表数据，同时**过期掉ReadWriteCacheMap**。

- - 此过程不会影响ReadOnlyCacheMap提供人家查询注册表。

- - 一段时间内（默认30秒），各服务拉取注册表会直接读ReadOnlyCacheMap

- - 30秒过后，Eureka Server的后台线程发现ReadWriteCacheMap已经清空了，也会清空ReadOnlyCacheMap中的缓存

- - 下次有服务拉取注册表，又会从内存中获取最新的数据了，同时填充各个缓存。

### **多级缓存机制的优点是什么？**

- 尽可能保证了内存注册表数据不会出现频繁的读写冲突问题。

- 并且进一步保证对Eureka Server的大量请求，都是快速从纯内存走，性能极高。

