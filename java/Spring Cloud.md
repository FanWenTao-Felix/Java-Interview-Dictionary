# Spring Cloud

### 1.Spring Cloud

Spring Cloud为企业级分布式Web系统构建提供了一站式的解决方案。为了简化分布式系统的开发流程和降低开发难度，Spring Cloud以组件化的形式提供了配置管理、服务发现、断路器、智能路由、负载均衡和消息总线等模块，应用程序只需要根据需求引入模块，便可方便地实现对应的功能.。

- Spring Cloud Config: Spring Cloud的配置中心，用于将配置存储到服务器中进行中集中化管理，支持本地存储、Git和Subversion 3种存储方式。配置中心除了Spring Cloud Config，还有Apollo配置中心和基于ZooKeeper等方式实现的配置中心。
- Spring Cloud Bus: Spring Cloud的事件消息总线，用于监听和传播集群中事件的状态的变化，例如集群中配置的变化检测和广播等。
- Eureka:Netflix提供的服务注册和发现组件，集群中各个服务以REST的方式将服务注册到注册中心，并与注册中心保持心跳连接，主要用于服务发现和自动故障转移。
- Hystrix:Netflix提供的服务熔断器，主要提供了服务负载过高或服务故障时的容错处理机制，以便集群在出现故障时依然能够对外提供服务，防止服务雪崩。
- Zuul:Netflix Zuul为集群提供通用网关的功能，前端服务访问后端服务均需要通过Zuul的动态路由来实现。同时可以在Zuul上实现服务的弹性扩展、安全监测、统一权限认证等功能。
- Archaius:Netflix提供的配置管理库，用于实现动态化属性配置和验证、线程安全配置操作、轮询框架、回调机制等功能。
- Consul: Consul是基于Golang开发的一个服务注册、发现和配置工具，其功能与Eureka类似。
- SpringCloud for Cloud Foundry: Spring Cloud for Cloud Foundry Pivotal通过OAuth 2.0协议绑定服务到Cloud Foundry, Cloud Foundry是VMware推出的开源PaaS云平台。
- SpringCloud Sleuth: Spring的日志收集工具包，封装了Dapper和Log-based追踪及Zipkin和HTrace操作，为Spring Cloud的应用实现了一种分布式链路追踪解决方案。
- Spring Cloud Data Flow: Spring Cloud Data Flow是一个混合计算模型，结合了流式数据与批量数据的处理方式，为Spring Cloud处理大数据提供了可能。
- Spring Cloud Security：基于Spring Security工具包实现的安全管理组件，主要用于应用程序的安全访问和控制。
- Spring Cloud ZooKeeper: Spring Cloud ZooKeeper封装了操作ZooKeeper的API,用于方便地操作ZooKeeper并实现服务发现和配置管理功能。
- Spring Cloud Stream: Sping的流式数据处理工具包，封装了Redis、RabbitMQ、Kafka等消息的接收和发送功能，用于快速实现流式数据分析功能。
- Spring Cloud CLI：基于Spring Boot CLI, Spring可以让用户使用命令行方式快速建立云组件。
- Ribbon: Netflix Ribbon用于分布式系统API调用的负载均衡，提供随机负载、轮询负载等多种负载均衡策略，常配合服务发现和断路器使用。
- Turbine:Netflix Turbine是实时消息或事件流的聚合工具，常用来监控集群下Hystrix的健康指标数据。
- Feign:Feign是一种声明式、模板化的HTTP访问客户端。
- Spring Cloud Task: Spring Cloud Task是SpringCloud提供的分布式环境下集群任务的统一管理和调度工具。
- SpringCloud Connectors: Spring Cloud Connectors为在云平台上运行的基于JVM的应用程序提供了一个简单的抽象，可以在JVM运行时发现绑定的服务和部署信息，并且支持将发现的服务注册为Spring Bean。
- Spring Cloud Cluster：提供LeaderShip（选举）功能，如ZooKeeper、Redis、Hazeleast、Consul等常见状态模式的抽象和实现。
- Spring Cloud Starters: Spring Boot式的启动项目，为SpringCloud提供开箱即用的依赖管理。

### 2.服务注册发现

服务注册就是维护一个登记簿，它管理系统内所有的服务地址。当新的服务启动后，它会向登记簿交待自己的地址信息。服务的依赖方直接向登记簿要Service Provider 地址就行了。当下用于服务注册的工具非常多ZooKeeper，Consul，Etcd, 还有Netflix 家的eureka 等。服务注册有两种形式：客户端注册和第三方注册。

客户端注册（zookeeper）

客户端注册是服务自身要负责注册与注销的工作。当服务启动后向注册中心注册自身，当服务下线时注销自己。期间还需要和注册中心保持心跳。心跳不一定要客户端来做，也可以由注册中心负责（这个过程叫探活）。这种方式的缺点是注册工作与服务耦合在一起，不同语言都要实现一套注册逻辑。

第三方注册（独立的服务Registrar）

第三方注册由一个独立的服务Registrar 负责注册与注销。当服务启动后以某种方式通知Registrar，然后Registrar 负责向注册中心发起注册工作。同时注册中心要维护与服务之间的心跳，当服务不可用时，向注册中心注销服务。这种方式的缺点是Registrar 必须是一个高可用的系统，否则注册工作没法进展。

客户端发现

客户端发现是指客户端负责查询可用服务地址，以及负载均衡的工作。这种方式最方便直接，而且也方便做负载均衡。再者一旦发现某个服务不可用立即换另外一个，非常直接。缺点也在于多语言时的重复工作，每个语言实现相同的逻辑。

服务端发现

服务端发现需要额外的Router 服务，请求先打到Router，然后Router 负责查询服务与负载均衡。这种方式虽然没有客户端发现的缺点，但是它的缺点是保证Router 的高可用。

## Spring Cloud Config

### 1.Spring Cloud Config

随着项目复杂度的增加和微服务开发组件的细化，散落在服务器各个角落的微服务组件需要一套在线的配置服务；一方面为整个服务提供统一的配置，避免在每个微服务中修改配置带来的不便和易出错的问题；另一方面保证了微服务配置能自动化更新到各个组件中，避免在修改配置后重启时出现服务不稳定的情况。

Spring Cloud Config为分布式系统提供统一的配置管理工具，应用程序在使用过程中可以像使用本地配置一样方便地添加、访问、修改配心的配置。SpringCloud Config将Environment的PropertySource抽象和配置中心的配置进行映射，以便应用程序可以在任何场景下获取和修改配置。

### 2.Spring Cloud Config的原理

Spring Cloud Config支持将配置存储在配置中心的本地服务器、Git仓库或Subversion。在SpringCloud Config的线上环境中，通常将配置文件集中放置在一个Git仓库里，然后通过配置中心服务端（Config Server）来管理所有的配置文件；当某个服务实例需要添加或更新配置时，只要在该服务实例的本地将配置文件进行修改，然后推送到Git仓库，其他服务实例通过配置中心从Git服务端获取最新的配置信息。对于配置中心来说，每个服务实例都相当于客户端（Config Client）。

为了保证系统的稳定，配置中心服务端可以进行多副本集群部署，前端使用负载均衡实现服务之间的请求转发。

### 3.Config Server的定义和使用

Spring Cloud Config服务为外部配置（键值对或YAML）提供了基于HTTP的远程资源访问接口。服务端可以使用@EnableConfigServer注释开启配置中心的功能，声明该应用程序是一个配置中心服务。

创建一个Config Server分为4步，首先在pom.xml中引人spring-cloud-config-server和spring-boot-starter-actuator依赖，然后通过＠EnableConfigServer注解开启配置服务，接着配置appIication. properties配置文件，最后一步是访问和使用。

( 1) pom.xml添加依赖。

( 2 ) @EnableConfigServer添加

@EnableConfigServer开启SpringBoot项目对分布式配置中心的支持功能

```html
@SpringBootApplication
@EnableConfigServer 
public class BootApplcation{ 
    public static void main(String[] args) {        
        SpringApplication.run(BootApplication.class, args); 
    }
}
```

( 3 ) application.properties配置

Spring Cloud Config将分布式配置文件的数据存放在Git仓库中，因此需要配置Git仓库的基本信息，具体配置如下

```html
＃配置中心端口号
server.port=9000 
＃配置中心名称
spring.cloud.config.server.default-application-name=config-server 
＃配置Git仓库地址
spring.cloud.config.server.git.uri＝https://github.com/LOVEGISER/SpringCloud
＃配置仓库的路径
spring.cloud.config.server.git.search-paths=SpringCloudConfig ＃配置仓库的分支
spring.cloud.config.label=master 
＃访问Git仓库的用户名
spring.cloud.config.server.git.username=username
＃访问Git仓库的用户密码如果Git为公开仓库，可以不填写用户名和密码spring.cloud.config.server.git.password=password
```

( 4）访问服务地址

启动应用程序，在浏览器地址栏中输入[http://localhost](http://localhost/): 9000/*/dev，返回配置信息。

从返回信息可以看到，Git仓库的地址为https://github.com/LOVEGISER/SpringCJoud/SpringCloudConfig，仓库中有一个配置文件application.properties，具体的配置信息在Source属性中存储。

### 4.Config Client

配置中心的使用分为3步：首先在pom.xml中引人spring-boot-starter-actuator和spring-cloud-starter-config依赖，然后在bootstrap.properties中设置配置中心的地址，最后是配置中心的使用。

( 1) pom.xml添加依赖。

( 2) bootstrap.properties配置。

bootstrap. properties中的spring.cloud.config.uri参数用于设置应用程序从哪个服务地上获取配置信息，并且可以通过spring.cloud.config.profile指定运行环境，例如，开发环境（dev）、测试环境（test ）、正式运行环境（pro）等。

```html
＃项目名称，一般与Git仓库中的文件名对应spring.application.name=config-client 
＃远程仓库的分支
spring.cloud.config.label=master 
＃运行环境：dev开发环境，test测试环境，pro正式运行环境
spring.cloud.config.profile=dev
＃配置服务中心的地址
spring.cloud.config.uri= http://localhost:9000/ 
＃服务端口号
server.port=9001 
```

( 3 ）配置信息的使用

配置信息的使用简单方便，SpringCloud Config将配置文件和PropetySource做了映射，对于应用程序来说，就像使用本地配置文件样使用ConfigServer上的配置文件，体使用如下

```html
//将Key为spring.datasource.url对应的值映射为springDatasourceURL
//其中spring.datasource.url为Git配置文件中的Key
@Value(”${spring.datasource.url}”) 
String springDatasourceURL;
```

## Spring Cloud **Eureka**

### 1.Eureka

Spring Cloud Eureka 是Spring Cloud Netflix 微服务套件中的一部分，它基于netflix-eureka 做了二次封装，主要负责微服务架构中的服务治理。 服务治理必须实现服务注册和服务发现的功能。服务注册和发现指每个微服务向注册中心汇报自己的服务地址、服务内容、端口和服务状态等信息。当有服务依赖此服务时，从注册中心获取可用的服务列表井发起请求， 服务消费者不用关心服务所处的位置和状态， 具体用哪个实例对外提供服务则由注册中心决定。

### 2.Eureka的原理

Eureka 实现了服务注册和服务发现的功能。Eureka 角色分为注册中心（ Eureka Server）和客户端（ Eureka Client ） 。客户端指注册到注册中心的具体的服务实例，又可抽象分为服务提供者和服务消费者。服务提供者主要用于将自己的服务注册到服务中心供服务消费者使用，服务消费者从注册中心获取相应的服务提供者对应的服务地址并调用该服务。

在Eurek a 中，同一个实例可能既是服务提供者，也是服务消费者。Eureka 服务端也会向另一个服务端实例注册自己的信息，从而实现Eureka Server 集群。其核心概念有服务注册、服务发现、服务同步和服务续约等。

1.服务注册

在服务启动时，服务提供者会将自己的信息注册至Eureka Serve r , Eureka Server 收到信息后，会将数据信息存储在一个双层结构的Map 中，其中，第一层的Key 是服务名，第二层的K ey 是具体服务的实例名。

```html
//注册表数据存储Instance Info
private final ConcurrentHashMap<String , Map<String, Lease<Instance Info>>> registry= new ConcurrentHashMap<String, Map<String , Lease<InstanceInfo>>();
//双层嵌套的HashMap
//第一层存储的Key 为AppName 应用名称（同一微服务节点可能会有多个实例）
//第二层存储的Key 为InstanceName 的实例名称
```

2 . 服务同步

在Eureka Server 集群中，当一个服务提供者向其中一个Eureka Server 注册服务后，该Eureka Server 会向集群中的其他Eureka Server 转发这个服务提供者的注册信息，从而实现Eureka Server 之间的服务同步。

3 服务续约

当服务提供者在注册中心完成注册后，会维护一个续约请求来持续发送信息给该Eureka Server 表示其正常运行，当Eureka Server 长时间收不到续约请求时，会将该服务实例从服务列表中剔除。

4.服务启动

当一个Eureka Server 初始化或重启时，本地注册服务为空。Eureka Server首先调用syncUp(）从别的服务节点获取所有的注册服务，然后执行Register 操作，完成初始化，从而同步所有的服务信息。

5 . 服务下线

当服务实例正常关闭时，服务提供者会给注册中心发送一个服务下线的消息，注册中心收到信息后， 会将该服务实例的状态置为下线，并把该服务下线的信息传播给其他服务实例。

6.服务发现

当一个服务实例依赖另一个服务时，这个服务实例作为服务消费者会发送一个信息给注册中心， 请求获取注册的服务清单，注册中心会维护一份只读的服务清单返回给服务消费者。

7 失效剔除

注册中心为每个服务设定一个服务失效的时间。当服务提供者无法正常提供服务，而注册中心又没有收到服务下线的信息时，注册中心会创建一个定时任务， 将超过一定时间而没有收到服务续约消息的服务实例从服务清单中剔除。失效时间可以通过eureka. instance. leaseExpirationDurationlnSeconds 进行配置，定期扫描时间可以通过eureka.server.evictionlntervalTimerlnMs 进行配置。

### 3.Spring Cloud Eureka 的使用

1 . 注册中心的定义

Eureka Server 是服务的注册中心，服务提供者在启动时会将服务信息注册到注册中心。它维护了集群中的服务列表和状态， 要实现一个注册中心分为4 步： 首先在pom .xml 中引人spring - cloud- starter- eureka - server 依赖， 然后通过＠ EnableEurekaServer 注解开启服务注册、发现功能， 接下来配置application . properties 配置文件，最后一步是服务的访问和使用。 ( 1 ) pom . xml 添加依赖。 ( 2 ) 通过＠EnableEurekaServer 注解开启对注册中心的支持。

```html
@SpringBootApplication
@EnableEurekaServer
public class EurekaserverApplication {
    public static void main（String[] args ) {
        SpringApplication.run(EurekaserverApplication.class,args) ;
    }
}
```

( 3 ) application .properties 配置。 配置Eureka Server 的服务名称、端口和服务地址。在默认情况下，服务注册中心会将自己也作为客户端来尝试注册，因此，应用程序需要禁用其注册行为。具体参数为 eureka.client. register-with- eureka =false 和eureka.client. fetch- registry = false。

( 4 ）访问服务地址。 启动应用程序，在浏览器地址栏中输入http: // localhost:9001访问注册中心。

2.服务提供者的定义

服务提供者（ Service Provider ） 即Eureka Client ， 是服务的定义者。服务提供者在启动时，将自身的服务注册到注册中心，服务消费者从注册中心请求到服务列表后，便会调用具体的服务提供者的服务接口，实现远程过程调用。要实现一个服务提供者分为5 步：首先在pom.xml 中引人spring-cloud - starter- eureka 依赖，然后通过＠ Enable EurekaClient 注解开启服务发现的功能，接着配置application. properties 配置文件，再定义服务接口， 最后一步是服务的访问和使用。 ( 1 ) p om.x ml 添加依赖。 ( 2 ) 通过＠EnableEurekaClient 注解开启服务发现的功能。

```html
@SpringBootApplication
@EnableEurekaClient
public class EurekaclientApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaclientApplication.class,args) ;
    }
}
```

( 3 ) application.properties 配置。 配置Eureka Server 的服务名称、端口和注册中心的地址。

( 4 ) 定义服务。

3.服务消费者的定义

服务消费者（ Service Consumer ） 即Eureka Consumer ， 是服务的具体使用者， 一个服务实例常常既是服务消费者也是服务提供者。要实现一个服务消费者分为5 步： 首先在pom.xm l 中引人spring-cloud-starter-eureka 依赖，然后通过＠ EnableEurekaClient 注解开启服务发现的功能，如果通过Feign 调用， 则需要通过＠ EnableFeignClients 开启对Feign 的支持， 接下来配置application. properties 配置文件， 最后调用服务提供者暴露出来的服务接口。 ( 1) pom.xml 添加依赖。

( 2 ）通过＠ EnableEurekaClient 注解开启服务发现的功能。如果通过F e ign 远程调用，则需要通过＠ EnableFeignClients 开启对Feign 的支持； 如果通过

RestTemplate 调用， 则需要定义RestTemplate 实例。

( 3 ) application.properties 配置。 配置服务名称、端口和注册中心的地址。

( 4 ) 调用RestTemplate 服务。

( 5 ) 调用服务。

( 6 ）定义基于Feign 接口的服务。 基于Feign 接口的服务调用简单方便，上述代码在EurekaconsumerApplication 中已经通过加入＠ EnableFeignClients 开启了对Feign 的支持，这里只需要应用程序定义Feign接口便可。 定义Feign 接口分为2 步：首先通过＠ FeignClient（“ serviceProducerName”）定义需要代理的服务实例名（“ EUREKA-CLIENT ”即服务提供者的实例名称），然后定义一个调用远程服务的方法。方法中＠ RequestMapping 的value （这里为“／serviceProducer" ）需要与服务提供者的地址相对应，这样， Feign 才能知道代理需要具体调用服务提供者的哪个方法。 ( 7 ）调用基于Feign 接口的服务。

### 4.服务注册中心（Eureka Server）

我们在项目中引入 `Eureka Server` 的相关依赖，然后在启动类加上注解 `@EnableEurekaServer` ，就可以将其作为注册中心。

我们继续添加两个模块 `service-provider` ， `service-consumer` ，然后在启动类加上注解 `@EnableEurekaClient` 并指定注册中心地址为我们刚刚启动的 `Eureka Server` ，再次访问可以看到两个服务都已经注册进来了。

可以看到 `Eureka` 的使用非常简单，只需要添加几个注解和配置就实现了服务注册和服务发现，接下来我们看看它是如何实现这些功能的。

服务注册（Register）

注册中心提供了服务注册接口，用于当有新的服务启动后进行调用来实现服务注册，或者心跳检测到服务状态异常时，变更对应服务的状态。服务注册就是发送一个 `POST` 请求带上当前实例信息到类 `ApplicationResource` 的 `addInstance` 方法进行服务注册。

可以看到方法调用了类 `PeerAwareInstanceRegistryImpl` 的 `register` 方法，该方法主要分为两步：

1. 调用父类 `AbstractInstanceRegistry` 的 `register` 方法把当前服务注册到注册中心
2. 调用 `replicateToPeers` 方法使用异步的方式向其它的 `Eureka Server` 节点同步服务注册信息

服务注册信息保存在一个嵌套的 `map` 中，

第一层 `map` 的 `key` 是应用名称（对应 `Demo` 里的 `SERVICE-PROVIDER` ），第二层 `map` 的 `key` 是应用对应的实例名称（对应 `Demo` 里的 `mghio-mbp:service-provider:9999` ），一个应用可以有多个实例。

服务续约（Renew）

服务续约会由服务提供者（比如 `Demo` 中的 `service-provider` ）定期调用，类似于心跳，用来告知注册中心 `Eureka Server` 自己的状态，避免被 `Eureka Server` 认为服务时效将其剔除下线。服务续约就是发送一个 `PUT` 请求带上当前实例信息到类 `InstanceResource` 的 `renewLease` 方法进行服务续约操作。

进入到 `PeerAwareInstanceRegistryImpl` 的 `renew` 方法可以看到，服务续约步骤大体上和服务注册一致，先更新当前 `Eureka Server` 节点的状态，服务续约成功后再用异步的方式同步状态到其它 `Eureka Server` 节上。

服务下线（Cancel）

当服务提供者（比如 `Demo` 中的 `service-provider` ）停止服务时，会发送请求告知注册中心 `Eureka Server` 进行服务剔除下线操作，防止服务消费者从注册中心调用到不存在的服务。服务下线就是发送一个 `DELETE` 请求带上当前实例信息到类 `InstanceResource` 的 `cancelLease` 方法进行服务剔除下线操作。

进入到 `PeerAwareInstanceRegistryImpl` 的 `cancel` 方法可以看到，服务续约步骤大体上和服务注册一致，先在当前 `Eureka Server` 节点剔除下线该服务，服务下线成功后再用异步的方式同步状态到其它 `Eureka Server` 节上。

服务剔除（Eviction）

服务剔除是注册中心 `Eureka Server` 在启动时就启动一个守护线程 `evictionTimer` 来定期（默认为 `60` 秒）执行检测服务的，判断标准就是超过一定时间没有进行 `Renew` 的服务，默认的失效时间是 `90` 秒，也就是说当一个已注册的服务在 `90` 秒内没有向注册中心 `Eureka Server` 进行服务续约（Renew），就会被从注册中心剔除下线。失效时间可以通过配置 `eureka.instance.leaseExpirationDurationInSeconds` 进行修改，定期执行检测服务可以通过配置 `eureka.server.evictionIntervalTimerInMs` 进行修改。

### 5.服务提供者（Service Provider）

对于服务提供方（比如 `Demo` 中的 `service-provider` 服务）来说，主要有三大类操作，分别为 `服务注册（Register）` 、 `服务续约（Renew）` 、 `服务下线（Cancel）` ，接下来看看这三个操作是如何实现的。

服务注册（Register）

一个服务要对外提供服务，首先要在注册中心 `Eureka Server` 进行服务相关信息注册，能进行这一步的前提是你要配置 `eureka.client.register-with-eureka=true` ，这个默认值为 `true` ，注册中心不需要把自己注册到注册中心去，把这个配置设为 `false`。

服务续约（Renew）

服务续约是由服务提供者方定期（默认为 `30` 秒）发起心跳的，主要是用来告知注册中心 `Eureka Server` 自己状态是正常的还活着，可以通过配置 `eureka.instance.lease-renewal-interval-in-seconds` 来修改，当然服务续约的前提是要配置 `eureka.client.register-with-eureka=true` ，将该服务注册到注册中心中去。

服务下线（Cancel）

当服务提供者方服务停止时，要发送 `DELETE` 请求告知注册中心 `Eureka Server` 自己已经下线，好让注册中心将自己剔除下线，防止服务消费方从注册中心获取到不可用的服务。这个过程实现比较简单，在类 `DiscoveryClient` 的 `shutdown` 方法加上注解 `@PreDestroy` ，当服务停止时会自动触发服务剔除下线，执行服务下线逻辑。

### 6.服务消费者（Service Consumer）

这里的服务消费者如果不需要被其它服务调用的话，其实只会涉及到两个操作，分别是从注册中心 `获取服务列表（Fetch）` 和 `更新服务列表（Update）` 。如果同时也需要注册到注册中心对外提供服务的话，那么剩下的过程和上文提到的服务提供者是一致的，这里不再阐述，接下来看看这两个操作是如何实现的。

获取服务列表（Fetch）

服务消费者方启动之后首先肯定是要先从注册中心 `Eureka Server` 获取到可用的服务列表同时本地也会缓存一份。这个获取服务列表的操作是在服务启动后 `DiscoverClient` 类实例化的时候执行的。

能发生这个获取服务列表的操作前提是要保证配置了 `eureka.client.fetch-registry=true` ，该配置的默认值为 `true`。

更新服务列表（Update）

由上面的 `获取服务列表（Fetch）` 操作过程可知，本地也会缓存一份，所以这里需要定期的去到注册中心 `Eureka Server` 获取服务的最新配置，然后比较更新本地缓存，这个更新的间隔时间可以通过配置 `eureka.client.registry-fetch-interval-seconds` 修改，默认为 `30` 秒，能进行这一步更新服务列表的前提是你要配置 `eureka.client.register-with-eureka=true` ，这个默认值为 `true` 。

### 7.**注册表存储结构**

- 这个名字叫做**registry**的**CocurrentHashMap**，就是注册表的核心结构。
- Eureka Server的注册表直接基于**纯内存**，即在内存里维护了一个数据结构。各个服务的注册、服务下线、服务故障，全部会在内存里维护和更新这个注册表。
- 各个服务每隔30秒拉取注册表的时候，Eureka Server就是直接提供内存里存储的有变化的注册表数据给他们就可以了。同样，每隔30秒发起心跳时，也是在这个纯内存的Map数据结构里更新心跳时间。
- 这个ConcurrentHashMap的key就是服务名称,value则代表了一个服务的多个服务实例。
- **Map<String, Lease<InstanceInfo**>>,这个Map的key就是**服务实例的id**,value是一个叫做**Lease**的类，它的泛型是一个叫做**InstanceInfo**的东西,这个InstanceInfo就代表了**服务实例的具体信息**，比如机器的ip地址、hostname以及端口号。而这个Lease，里面则会维护每个服务**最近一次发送心跳的时间**.

### 8.**Eureka Server端优秀的多级缓存机制**

- 在拉取注册表的时候：
  - 首先从**ReadOnlyCacheMap**里查缓存的注册表。
  - 若没有，就找**ReadWriteCacheMap**里缓存的注册表。
  - 如果还没有，就从**内存中获取实际的注册表数据。**
- 在注册表发生变更的时候：
  - 会在内存中更新变更的注册表数据，同时**过期掉ReadWriteCacheMap**。
  - 此过程不会影响ReadOnlyCacheMap提供人家查询注册表。
  - 一段时间内（默认30秒），各服务拉取注册表会直接读ReadOnlyCacheMap
  - 30秒过后，Eureka Server的后台线程发现ReadWriteCacheMap已经清空了，也会清空ReadOnlyCacheMap中的缓存
  - 下次有服务拉取注册表，又会从内存中获取最新的数据了，同时填充各个缓存。

**多级缓存机制的优点是什么？**

- 尽可能保证了内存注册表数据不会出现频繁的读写冲突问题。
- 并且进一步保证对Eureka Server的大量请求，都是快速从纯内存走，性能极高。

## Spring Cloud Consul

### 1.Spring Cloud Consul

Consul是HashiCorp公司推的用于实现分布式系统服务注册、发现和配置的开源工具。和SpringCloud Eureka一样，Consul也是一个一站式的服务注册与发现框架，内置了服务注册与发现、分布式一致性协议实现、健康检查、Key-Vlaue存储和多数据中心方案，因此不需要依赖第三方工具（例如Zookeeper）便可完成服务注册与发现，简单易用。

Consul采用Go编写，支持Linux、Windows和macOS系统，可移植性强。安装包仅是一个可执行文件，方便部署，且与Docker配合方便。

### 2.Spring Cloud Consul的原理

Consul是一个支持多数据中心的高可用的分布式服务注册、发现和配置服务，采用Raft一致性协议算法来保证服务的高可用；使用Gossip协议管理成员状态和广播消息，并且支持ACL访问控制。相对于SpringCloud Eureka或其他服务注册与发现框架，Consul有以下特点。

Consul的特性

- 高效的Raft一致性算法：Consul使用Raft一致性算法来保证集群状态的一致性，在实现上比Paxos一致性算法更简单（ZooKeeper采用Paxos一致性算法实现，Etcd采用Raft一致性算法实现）。
- 支持多数据中心：Consul支持多数据中心。多数据中心可以使集群避免单数据中心的单点故障问题，但在部署的过程中需要考虑网络延迟、数据分片等情况。ZooKeeper 和Etcd均不支持多数据中心。
- 健康检查：Consul支持健康检查，默认每10s做一次健康检查，保证注册中心的服务均可用，Etcd不提供此功能。
- HTTP和DNS支持：Consul支持HTTP和DNS协议接口。ZooKeeper集成了DNS协议，实现复杂，Etcd只支持HTTP。

Consul的角色

Consul按照功能可分为服务端和客户端。

- 服务端：Server，用于保存服务配置信息的高可用集群，在局域网内与本地客户端通信，在广域网内与其他数据中心通信。每个数据中心的Server数量都推荐为3个以上以保证服务高可用。在集群中，Server又分为Server Leader和Server。Server Leader负责同步注册信息和进行各个节点的健康检查，同时负责整个集群的写请求。Server负责把配置信息持久化并接收读请求。Server在一个数据中心( Data Center, DC ）内使用LAN Gossip协议的一致性算法，在跨数据中心内使用WAN Gossip协议的一致性算法。
- 客户端：Client，是无状态的服务，将HTTP和 DNS协议的接口请求转发给局域网内的服务端集群。

Consul的服务端和客户端还支持跨数据中心的访问，提供了跨区域的高可用功能。

Consul的服务注册与发现流程

(1 ）服务注册：Producer在启动时会向Consul服务端发送一个POST注册请求，注册请求中包含服务地址和端口等信息。

(2 ）健康检查：Consul服务端在接收Producer的注册信息后，每10s （默认）会向Producer发送一个健康检查的请求，检验Producer是否健康。

( 3 ）服务发现：当Consumer发起请求（请求格式为“/api/address”）时，首先会从Consul服务端获取一个包含Producer的服务地址和端口的临时表。该表只包含通过了健康检查的Producer的可用服务列表。

( 4 ）服务请求：客户端从临时表中获取一个可用的服务地址，向Producer发送请求。Producer在收到请求后返回请求响应。

### 3.Spring Cloud Consul 的使用

Consul服务提供者的定义

服务提供者在启动的时候会将自身的服务地址状态上报Consul Server，服务消费者在每次发起请求时都要先从Consul Server获取可用的临时服务列表（该列表维护了可用的服务提供者的地址），再选择一个可用的服务提供者的地址进行服务调用。实现一个服务提供者分为5步：首先在pom.xml中加入spring-cloud-starter-consul-discovery依赖，然后通过@EnableDiscoveryClient注解开启服务发现的功能，接着配置application. properties配置文件、定义服务接口，最后一步是访问和使用。

( I ) pom.xml 添加依赖

在pom.xml中加入spring-cloud-starter-consul-discovery依赖。

( 2 通过@EnableDiscoveryClient注解开启对服务发现的支持。

```html
@SpringBootApplication 
@EnableDiscoveryClient 
public class ConsulProducerApplication { 
    public static void main (String [] args) {      
        SpringApplication.run(ConsulProducerApplication.class,args); 
    }
}
```

( 3 ) application.properties配置

配置服务名称、端口、要连接的Consul Server的地址和端口

( 4）定义服务

( 5）调用服务

Consul服务消费者的定义

服务消费者是具体的服务调用方，它每次发起请求都先从Consul Server获取可用的临时服务列表（该列表维护了可用的服务提供者的地址），然后选择一个可用的服务提供者的地址实现服务调用。要实现一个服务消费者分为4步：首先需要在pom.xml中加spring-cloud-spring-cloud-starter-consul-discovery依赖，然后配置apIication. properties配置文件，接着获取临时服务列表，最后一步是访问和使用。

( 1 ) pom.xml添加依赖

在pom.xml中加入spring-cloud-spring-cloud-starer-consul-discovery依赖。

( 2) application.properties配置

配置服务名称、端口、要连接的Consul Server的地址和端口

( 3 ）获取临时服务列表

```html
@Autowired//注入负载均衡器
private LoadBalancerClient loadBalancer; 
//从负载均衡器获取临时服务列表
ServiceInstance serviceList = loadBalancer.choose (”service-producer”); 
```

上述代码通过依赖注入LoadBalancerClient来获取临时服务列表，具体是通过choose（）方法来实现的，choose（）方法的参数是要获取的服务提供者的服务实例名称。

( 4）调用服务

```html
@RestController 
public class ConsumerController { 
    @Autowired //step1: 注入负载均衡器
    private LoadBalancerClient loadBalancer; 
    @RequestMapping(”/call ”) 
    public String call () { 
        //step2 ：从负载均衡器获取临时服务列表
        ServiceInstance serviceList= loadBalancer.choose (”service-producer"); 
        System.out.println（”服务地址：”+serviceList.getUri());   
        System.out.println（”服务名称:"+serviceList.getServiceId());
        //step3 ：通过临时服务列表获取远程服务地址，实现远程过程调用  
        String callServiceResult =new RestTemplate ().getForObject ( serviceList.getUri().toString() + ”/hello”, String.class);
        return callServiceResult;
    }
}   
```

上述代码通过loadBalancer.choose(”serviceName＂）获取一个服务实例，然后使用RestTemplate.getForObject（）实现远程服务调用，其中，第一个参数是服务实例的地址，第二个参数是服务请求的返回值类型。

（ 5 ）验证服务

在浏览器地址栏中输入http://127.0.0.1:8503/call访问客户端服务。

## Spring Cloud Feign

### 1.Spring Cloud Feign

Feign是一种声明式、模板化的HTTP Client。它的目标是编写Java HTTP Client变得更简单。Feign通过使用Jersey和CXF等工具实现一个HTTP Client，用于构建REST或SOAP的服务。Feign还支持用户基于常用的HTTP工具包（OkHTTP、HTTPComponents ）实现自定义的HTTP Client。

Feign基于注解的方式将HTTP请求模板化。Feign将HTTP请求参数写入Template,极大地简化了HTTP请求。尽管Feign目前只支持基于文本的HTTP请求，不适合文件的上传和下载，但它为HTTP请求提供了更多可能性，比如请求回放等功能。同时，Feign使HTTP单元测试变得更加方便。

### 2.Feign的应用

Feign提供了声明式接口编程的方式，其应用一般依赖服务发现组件来实现远程接口调用，在并发要求不高的情况下可以作为RPC方案使用，实现服务之间的解耦。Feign应用分为服务提供者和服务消费者。

要实现一个服务提供者分为5步：首先在pomxm中加入spring-cloud-starter-feign 依赖，然后通过＠EnableFeignClients注解开启对Feign的支持，接下来配置application. properties配置文件，再创建接口和定义需要远程调用的方法，最后一步是服务的访问和使用。

( 1 ) pom.xml添加依赖

在pom.xml中加入spring-cIoud-starter-feign依赖。

( 2)通过@EnableFeignClients注解开启对feign的支持。注意，Feign一般和服务发现配合使用，以下代码使用EnableEurekaClient开启了对服务发现客户端的支持。

```html
@SprngBootApplication
@EnableEurekaClient 
@EnableFeignClients 
public class FeignClientApplication { 
    public static void main(String[] args) {                                    SpringApplication.run(FeignClientApplicaton.class,args); 
```

( 3 ) application.properties配置

配置服务名称、端口和需要连接的服务注册中心的地址。

( 4 ) 创建接口和定义需要远程调用的方法

```html
@FeignClient(”EUREKA-CLIENT") //step1 ：定义服务实例
public interface FeignClientInterface { 
    //step2 ：定义服务接口
    @RequestMapping(value="/serviceProducer”，method= RequestMethod.GET)     
    Map serviceProducer(); 
}
```

上述代码首先定义了一个名为FeignClientInterface的接口，并通过FeignClient（"EUREKA-CLIENT）来实现注解的配置。这里的参数“EUREKA-CLIENT"为远程服务实例的名称；然后定义了serviceProduct（）方法，该方法通过调用服务实例名为“EUREKA-CLIENT”的“／serviceProducer”方法来实现远程调用。

( 5） 调用服务

```html
@RestController 
public class FeignController { 
    @Autowired
    FeignClientInterface feignClentInterface
    @RequestMapping(value =”/consume/feign”,method = RequestMethod.GET)     
    public Map serviceFeign() { 
        return feignClientInterface.serviceProducer(); 
    }
}
```

上述代码通过注入FeignClientInterface依赖，并调用已经定义好的serviceProducer()方法来实现服务调用。

### 3.Feign的常用注解

Feign通过SpringMVC的注解来实现对HTTP参数的封调用，常用注解。

- @RequestLine：Request定义一个HTTP Method和UriTemplate，使用｛｝进行包装，并对＠Param注释参数进行解析
- @Param：定义一个HTTP请求模饭，模板中的参数将根据名称来解析
- @Headers：定义一个HeaderTemplate，在UriTemplate中使用
- @QueryMap：定义一个基于Name-Value的参数列表，也可以是Java实体类，最终以查询字符串的方式传输
- @HeaderMap：定义一个基于Name-Value的参数列表，用于扩展HTTP Headers
- @Body：定义一个类似UriTemplate或者HeaderTemplate的模板，该模板使用＠Param注释参数解析相应的表达式

## Spring Cloud Hystrix

### 1.Spring Cloud Hystrix

Netflix Hystrix为SOA( Service Oriented Architecture，面向服务的架构）和微服务架构提供一整套服务隔离、服务熔断和服务降级的解决方案。它是熔断器的一种实现，主要应用于微服务架构的高可用，防止出现服务雪崩等问题。

微服务架构将传统的单体服务根据业务功能和模块的不同，分解为多个独立的子服务，每个子服务都独立开发、部署和发布，子服务之间通过RPC接口实现服务间的接口调用。每个子服务都可以根据自己的需要进行独立的技术选型，服务之间相互独立，实现敏捷开发和部署。

由于业务之间往往具有复杂的依赖和调用关系，因此，微服务中的各个子服务之间的依赖关系也较为复杂。比如上游某个子服务因网络故障或者操作系统资源不足出现接口调用异常，则将导致下游服务也出现服务异常；此时，如果上游服务没有很好的请求拒绝策略，则会导致请求不断增加，大量的请求积压不但会导致当前服务宕机，还可能导致下游服务宕机，继而引起雪崩效应。

为了避免雪崩效应，提高关键业务的可靠性，可使用熔断器对部分非核心、低服务级别的业务进行服务降级。Netflix Hystrix实现了服务熔断器的功能，具体的做法是通过监控远程接口调用的状态，统计分析远程接口调用的数据，一旦发现某个服务出现宕机或故障过多的情况，则自动进行服务降级，不再调用远程接口服务，而是直接返回错误状态，避免集群雪崩效应，这样既有效保障了集群的安全性，也为恢复服务争取了时间。

资源隔离、限流、熔断、降级、运维监控

资源隔离：让你的系统里，某一块东西，在故障的情况下，不会耗尽系统所有的资源，比如线程资源。

限流：高并发的流量涌入进来，比如说突然间一秒钟100万QPS，废掉了，10万QPS进入系统，其他90万QPS被拒绝了。

熔断：系统后端的一些依赖，出了一些故障，比如说mysql挂掉了，每次请求都是报错的，熔断了，后续的请求过来直接不接收了，拒绝访问，10分钟之后再尝试去看看mysql恢复没有。

降级：mysql挂了，系统发现了，自动降级，从内存里存的少量数据中，去提取一些数据出来。

运维监控：监控+报警+优化，各种异常的情况，有问题就及时报警，优化一些系统的配置和参数，或者代码。

### 2.Hystrix的特性

服务熔断

Hystrix熔断器就像家中的安全阀一样，一旦某个服务不可用，熔断器会直接切断该链路上的请求，避免大量的元效请求影响系统稳定，并且熔断器有自我检测和恢复的功能，在服务状态恢复正常后会自动关闭。

服务降级

Hystrix通过fallback实现服务降级。在需要进行服务降级的类中定义一个fallback 方法，当请求的远程服务出现异常时，可以直接使用fallback方法返回异常信息，而不调用远程服务。fallback方法的返回值一般是系统默认的错误消息或者来向缓存中的数据，用以告知服务消费者当前服务处于不可用状态。Hystrix通过HystrixCommand实现服务降级，熔断器有闭路、开路和半开路3种状态。

( 1）当调用远程服务请求的失败数量超过一定比例（默认为50%)时，熔断器会切换到开路状态，这时所有请求都会直接返回失败信息而不调用远程服务。

( 2 ）熔断器保持开路状态一段时间后（默认为5s），会自动切换到半开路状态。

( 3 ）熔断器判断下一次请求的返回情况，如果请求成功，则熔断器切换回闭路状态，服务进入正常链路调用流程；否则重新切换到开路状态，并保持开路状态。

依赖隔离

Hystrix通过线程池和信号量两种方式实现服务之间的依赖隔离，这样即使其中一个服务出现异常，资源迟迟不能释放，也不会影响其他业务线程的正常运行。

( 1 ）线程池的隔离策略

Hystrix线程池的资源隔离为每个依赖的服务都分配一个线程池，每个线程池都处理特定的服务，多个服务之间的线程资源互不影向，以达到资源隔离的目标。当突然发生流量洪峰、请求增多时，来不及处理的任务将在线程队列中排队等候，这样做的好处是不会丢弃客户端请求，保障所有数据最终都会得到处理。

( 2）信号量的隔离策略

Hystrix信号量的隔离策略是为每个依赖的服务都分配一个信号量（原子数数器），当接收到用户请求时，先判断该请求依赖的服务所在的信号量值是否超过最大线程设置。若超过最大线程设置，则丢弃该类型的请求；若不超过，则在处理请求前执行"信号量+1"的操作，在请求返回后执行“信号量-1”的操作。当流量洪峰来临，收到的请求数量超过设置的最大值时，这种方式会直接将错误状态返回给客户端，不继续去请求依赖的服务。

请求缓存

Hytrix按照请求参数把请求结果缓存起来，当后面有相同的请求时不会再走完整的调用链流程，而是把上次缓存的结果直接返回，以达到服务快速响应和性能优化的目的；同时，缓存可作为服务降级的数据源，当远程服务不可用时，直接返回缓存数据。对于消费者来说，只是可能获取了过期的数据，这样就优雅地处理了系统异常。

请求合并

当微服务需要调用多个远程服务做结果的汇总时，需要使用请求合并。Hystrix采用异步消息订阅的方式进行请求合并。当应用程序需要请求多个接口时，采用异步调用的方式提交请求，然后订阅返回值，这时应用程序的业务可以接着执行其他任务而不用阻塞等待，当所有请求都返回时，应用程序会得到一个通知，取出返回值合并即可。

### 3.Hystrix的设计原则

（1）对依赖服务调用时出现的调用延迟和调用失败进行控制和容错保护。

（2）在复杂的分布式系统中，阻止某一个依赖服务的故障在整个系统中蔓延，服务A->服务B->服务C，服务C故障了，服务B也故障了，服务A故障了，整套分布式系统全部故障，整体宕机。

（3）提供fail-fast（快速失败）和快速恢复的支持。

（4）提供fallback优雅降级的支持。

（5）支持近实时的监控、报警以及运维操作。

详细设计原则

（1）阻止任何一个依赖服务耗尽所有的资源，比如tomcat中的所有线程资源。

（2）避免请求排队和积压，采用限流和fail fast来控制故障。

（3）提供fallback降级机制来应对故障。

（4）使用资源隔离技术，比如bulkhead（舱壁隔离技术），swimlane（泳道技术），circuit breaker（短路技术），来限制任何一个依赖服务的故障的影响。

（5）通过近实时的统计/监控/报警功能，来提高故障发现的速度。

（6）通过近实时的属性和配置热修改功能，来提高故障处理和恢复的速度。

（7）保护依赖服务调用的所有故障情况，而不仅仅只是网络故障情况。

### 4.Hystrix的实现

（1）通过HystrixCommand或者HystrixObservableCommand来封装对外部依赖的访问请求，这个访问请求一般会运行在独立的线程中，资源隔离。

（2）对于超出我们设定阈值的服务调用，直接进行超时，不允许其耗费过长时间阻塞住。这个超时时间默认是99.5%的访问时间，但是一般我们可以自己设置一下。

（3）为每一个依赖服务维护一个独立的线程池，或者是semaphore，当线程池已满时，直接拒绝对这个服务的调用。

（4）对依赖服务的调用的成功次数，失败次数，拒绝次数，超时次数，进行统计。

（5）如果对一个依赖服务的调用失败次数超过了一定的阈值，自动进行熔断，在一定时间内对该服务的调用直接降级，一段时间后再自动尝试恢复。

（6）当一个服务调用出现失败，被拒绝，超时，短路等异常情况时，自动调用fallback降级机制

（7）对属性和配置的修改提供近实时的支持。

### 3.Hystrix的服务降级流程

Hystrix的服务熔断是依赖HystrixCommand指令来实现的，具体流程如下：

( 1 ）当有服务请求时，首先会根据注解创建一个HystrixCommand指令对象，该对象设置了服务调用失败的场景（如服务请求超时等）和调用失败后服务降级的业务逻辑方法。

( 2 ）熔断器判断状态，当熔断器处于开路状态时，直接调用服务降级的业务逻辑方法返回调用失败的反馈信息。

( 3 ）当熔断器处于半开路或者闭路状态时，服务会进行线程池和信号量等资源检查，如果有可用资源，则调用正常业务逻辑。如果调用正常业务逻辑成功，则返回成功后的消息；如果失败，则调用服务降级的业务逻辑，进行服务降级。

( 4 ）当熔断器处于半开路或者闭路状态时，如果当前服务线程池和信号量中没有可用资源，则执行服务降级的业务逻辑，返回失败信息。

( 5 ）当熔断器处于半开路状态并且本次服务执行失败时，熔断器会进入开路状态。

( 6）当正常业务逻辑处理超时或者出现错误时，HystrixCommand会执行服务降级的业务逻辑，返回失败信息.

( 7 ）线程池和信号量的资源检查及正常业务逻辑会将自己的状态和调用结果反馈给监控，监控将服务状态反馈给熔断器，以便熔断器判断熔断状态.

### 4.Hystrix的使用

Hystrix的使用主要分为服务熔断、服务降级和服务监控3个方面

( 1 ) pom.xml添加依赖

在pom.xml中加入Hystrix依赖，其中，spring-cloud-starter-netflix-hystrix和javanica为Hystrix服务熔断所需的依赖，spring-cloud-netflix-hystrix-dash board为Hystrix服务监控所需的依赖。

( 2 ）通过@EnableHystrix注解开启对服务熔断的支持，通过@EnableHysrixDashboard注解开启对服务监控的支持。注意，Hystrix一般和服务发现配合使用，这里使用@EnableEurekaClient开启了对服务发现客户端的支持。

( 3 ) application. properties配置

配置服务名称、端口和需要连接的服务注册中心的地址，这里注册中使用Eureka服务

( 4) 服务熔断和降级

通过@HystrixCommand(fallbackMethod=”exceptionHandler＂）在方法上定义了一个服务降级的命令。当远程方法调用失败时，Hystrix会向动调用fallbackMethod来完成服务熔断和降级，这里会调用exceptionHandler（）方法。

( 5) 服务验证

当关闭EUREKA-CLIENT远程服务时，远程服务将不可用，Hystrix的熔断器打开，程序直接调用fallbackMethod方法实现服务降级。

### 5.异步请求

Hystrix为非阻塞I/O提供了两种实现方式，分别是表示将来式的Future和表示回调式的Callable。

Future

当用Future去请求一个网络IO的任务时，Future会以多线程的形式异步实现服务调用，主线程不必阻塞等待，在结果返回后再通过Future的get（）方法获取Future的返回结果。具体实现如下：

( 1 ）定义HystrixCommand

通过继承HystrixCommand定义一个CommandFuture来实现异步请求，其中，正常业务执行的逻辑在覆写的run（）方体体中被执行，服务降级方法在getFallback()中被执行。需要注意的是，这里使用andCommandKey(HystrixCommandKey.Factory.asKey(”Hello World＂））实现了使用HystrixCommandKey工厂定义依赖名称，每个CommandKey都代表一个依赖抽象，相同的依赖要使用相同的CommandKey名称。依赖隔离的本质就实对相同CommandKey的依赖进行隔离。使用andThreadPoolKey(HystrixThreadPoolKey.Factory.asKey（”HelloWorldPool”））实现了基于HystrixThreadPoolKey工厂定义线程池名称。

当对同一业务的依赖进行资源隔离时，使用CommandGroup进行区分，但是当对同一依赖进行不同远程调用时（例如。一个是Redis服务，一个是HTTP服务），则可以使用HystrixThreadPoolKey进行隔离区分，虽然在业务上都是相同的组，但是当需要在源上进行隔离时，则可以使用HystrixThreadPoolKey进行区分。

(2 ）使用HystrixCommand

通过new CommandFuture（”future”，restTemplate).queue（）定义一个异步服务请求，请求在提交后会以队列的形式在线程池中等待被执行，在执行完成后通过get（）方法获取即可。

Callable

预定义一个回调任务，Callable在发出请求后，主线程继续执行，在请求被执行完成并返回结果后，Callable会自动调用回调任务。具体使用如下。

( 1 ）定义HystrixObservableCommand

定义名为CommandObservable的类，该类继承自HystrixObservableCommand接口，并通过覆写HystrixObservableCommand接口中的construct（）方法实现观察者模。具体实现为通过Observable.create（）创建并返回一个Observable<String＞对象，在创建对象时，通过new Observable.OnSubscribe（）方法实现消息的监听和处理。其中，call方法用于消息的接收和业务的处理，在消息处理完成后通过subscriber.onNext( result）将调用结果传递下去，当所有任务都执行完成时通过subscriber.onCompleted（）将总体执行结果发布出去。

resumeWithFallback方法是服务降级处理逻辑，当服务出现异常时，通过subscriber.onN ext（”hystrix：远程服务异常”）进行服务熔断和异常消息的发布，实现服务降级处理。

( 2 ）使用HystrixObservabeCommand

通过new CommandObservable(”observer" ,restTemplate).observe（）定义了一个实现服务发布的命令。通过调用observable.subscribe（）方法来实现基于观察者模式的请求结果订阅，其中，订阅的数据结果在onNext（）方法中被通知，总体调用结果在onCompleted（）中被通知，服务处理异常结果在onError（）中被通知。

### 6.Hystix Dashboard

Hystrix Dashboard主要用来实时监控Hystrix的各项运行指标。通过Hystrix Dashboard可以查询Hystrix的实时信息，用于快速定位和发现问题。Hystrix Dashboard 的使用简单方便，首先在pom.xml中引人spring-cloud-netflix-hystrix-dashboard服务依赖，然后使用@EnableHystrixDashboard注解开启Dashboard功能即可。

hystrix的dashboard可以支持实时监控metric。

netflix开始用这个dashboard的时候，大幅度优化了工程运维的操作，帮助节约了恢复系统的时间。大多数生产系统的故障持续时间变得很短，而且影响幅度小了很多，主要是因为hystrix dashborad提供了可视化的监控。

截图说明，dashboard上的指标都是什么？

圆圈的颜色和大小代表了健康状况以及流量，折线代表了最近2分钟的请求流量。

集群中的机器数量，请求延时的中位数以及平均值。

最近10秒内的异常请求比例，请求QPS，每台机器的QPS，以及整个集群的QPS。

断路器的状态。

最近一分钟的请求延时百分比，TP90，TP99，TP99.5。

几个有颜色的数字，代表了最近10秒钟的统计，以1秒钟为粒度。

成功的请求数量，绿颜色的数字; 短路的请求数量，蓝色的数字; timeout超时的请求数量，黄色的数字; 线程池reject的请求数量，紫色的数字; 请求失败，抛出异常的请求数量，红色的数字。

### 7.设计原则

- 阻止任何一个依赖服务耗尽所有的资源，比如 tomcat 中的所有线程资源。
- 避免请求排队和积压，采用限流和 `fail fast` 来控制故障。
- 提供 fallback 降级机制来应对故障。
- 使用资源隔离技术，比如 `bulkhead`（舱壁隔离技术）、`swimlane`（泳道技术）、`circuit breaker`（断路技术）来限制任何一个依赖服务的故障的影响。
- 通过近实时的统计/监控/报警功能，来提高故障发现的速度。
- 通过近实时的属性和配置**热修改**功能，来提高故障处理和恢复的速度。
- 保护依赖服务调用的所有故障情况，而不仅仅只是网络故障情况。

### 8.基于 Hystrix 线程池技术实现资源隔离

Hystrix 实现资源隔离，主要有两种技术：

- 线程池
- 信号量

默认情况下，Hystrix 使用线程池模式。

资源隔离，就是说，你如果要把对某一个依赖服务的所有调用请求，全部隔离在同一份资源池内，不会去用其它资源了，这就叫资源隔离。哪怕对这个依赖服务，比如说商品服务，现在同时发起的调用量已经到了 1000，但是分配给商品服务线程池内就 10 个线程，最多就只会用这 10 个线程去执行。不会因为对商品服务调用的延迟，将 Tomcat 内部所有的线程资源全部耗尽。

Hystrix 进行资源隔离，其实是提供了一个抽象，叫做 Command。这也是 Hystrix 最最基本的资源隔离技术。

对某一个依赖服务，所有的调用请求，全部隔离到一个线程池内，每次调用请求都封装在一个command里面，每个command（每次服务调用请求）都是使用线程池内的一个线程去执行的，所以哪怕是对这个依赖服务现在同时发起的调用量已经到了1000了，但是线程池内就10个线程，最多就只会用这10个线程去执行，不会说，对商品服务的请求，因为接口调用延迟，将tomcat内部所有的线程资源全部耗尽，不会出现了。

利用 HystrixCommand 获取单条数据

我们通过将调用商品服务的操作封装在 HystrixCommand 中，限定一个 key，比如下面的 `GetProductInfoCommandGroup`，在这里我们可以简单认为这是一个线程池，每次调用商品服务，就只会用该线程池中的资源，不会再去用其它线程资源了。

```html
 public class GetProductInfoCommand extends HystrixCommand<ProductInfo> {
 
     private Long productId;
 
     public GetProductInfoCommand(Long productId) {
         super(HystrixCommandGroupKey.Factory.asKey("GetProductInfoCommandGroup"));
         this.productId = productId;
     }
 
     @Override
     protected ProductInfo run() {
         String url = "http://localhost:8081/getProductInfo?productId=" + productId;
         // 调用商品服务接口
         String response = HttpClientUtils.sendGetRequest(url);
         return JSONObject.parseObject(response, ProductInfo.class);
     }
 }
```

我们在缓存服务接口中，根据 productId 创建 Command 并执行，获取到商品数据。

```html
 @RequestMapping("/getProductInfo")
 @ResponseBody
 public String getProductInfo(Long productId) {
     HystrixCommand<ProductInfo> getProductInfoCommand = new GetProductInfoCommand(productId);
 
     // 通过command执行，获取最新商品数据
     ProductInfo productInfo = getProductInfoCommand.execute();
     System.out.println(productInfo);
     return "success";
 }
```

上面执行的是 execute() 方法，其实是同步的。也可以对 command 调用 queue() 方法，它仅仅是将 command 放入线程池的一个等待队列，就立即返回，拿到一个 Future 对象，后面可以继续做其它一些事情，然后过一段时间对 Future 调用 get() 方法获取数据。这是异步的。

利用 HystrixObservableCommand 批量获取数据

只要是获取商品数据，全部都绑定到同一个线程池里面去，我们通过 HystrixObservableCommand 的一个线程去执行，而在这个线程里面，批量把多个 productId 的 productInfo 拉回来。

```html
 public class GetProductInfosCommand extends HystrixObservableCommand<ProductInfo> {
 
     private String[] productIds;
 
     public GetProductInfosCommand(String[] productIds) {
         // 还是绑定在同一个线程池
         super(HystrixCommandGroupKey.Factory.asKey("GetProductInfoGroup"));
         this.productIds = productIds;
     }
 
     @Override
     protected Observable<ProductInfo> construct() {
         return Observable.unsafeCreate((Observable.OnSubscribe<ProductInfo>) subscriber -> {
 
             for (String productId : productIds) {
                 // 批量获取商品数据
                 String url = "http://localhost:8081/getProductInfo?productId=" + productId;
                 String response = HttpClientUtils.sendGetRequest(url);
                 ProductInfo productInfo = JSONObject.parseObject(response, ProductInfo.class);
                 subscriber.onNext(productInfo);
             }
             subscriber.onCompleted();
 
         }).subscribeOn(Schedulers.io());
     }
 }
```

在缓存服务接口中，根据传来的 id 列表，比如是以 `,` 分隔的 id 串，通过上面的 HystrixObservableCommand，执行 Hystrix 的一些 API 方法，获取到所有商品数据。

```html
 public String getProductInfos(String productIds) {
     String[] productIdArray = productIds.split(",");
     HystrixObservableCommand<ProductInfo> getProductInfosCommand = new GetProductInfosCommand(productIdArray);
     Observable<ProductInfo> observable = getProductInfosCommand.observe();
 
     observable.subscribe(new Observer<ProductInfo>() {
         @Override
         public void onCompleted() {
             System.out.println("获取完了所有的商品数据");
         }
 
         @Override
         public void onError(Throwable e) {
             e.printStackTrace();
         }
 
         /**
          * 获取完一条数据，就回调一次这个方法
          * @param productInfo
          */
         @Override
         public void onNext(ProductInfo productInfo) {
             System.out.println(productInfo);
         }
     });
     return "success";
 }
```

### 9.基于 Hystrix 信号量机制实现资源隔离

信号量机制

信号量的资源隔离只是起到一个开关的作用，比如，服务 A 的信号量大小为 10，那么就是说它同时只允许有 10 个 tomcat 线程来访问服务 A，其它的请求都会被拒绝，从而达到资源隔离和限流保护的作用。

线程池与信号量区别

线程池隔离技术，并不是说去控制类似 tomcat 这种 web 容器的线程。更加严格的意义上来说，Hystrix 的线程池隔离技术，控制的是 tomcat 线程的执行。Hystrix 线程池满后，会确保说，tomcat 的线程不会因为依赖服务的接口调用延迟或故障而被 hang 住，tomcat 其它的线程不会卡死，可以快速返回，然后支撑其它的事情。

线程池隔离技术，是用 Hystrix 自己的线程去执行调用；而信号量隔离技术，是直接让 tomcat 线程去调用依赖服务。信号量隔离，只是一道关卡，信号量有多少，就允许多少个 tomcat 线程通过它，然后去执行。

**适用场景**：

- **线程池技术**，适合绝大多数场景，比如说我们对依赖服务的网络请求的调用和访问、需要对调用的 timeout 进行控制（捕捉 timeout 超时异常）。
- **信号量技术**，适合说你的访问不是对外部依赖的访问，而是对内部的一些比较复杂的业务逻辑的访问，并且系统内部的代码，其实不涉及任何的网络请求，那么只要做信号量的普通限流就可以了，因为不需要去捕获 timeout 类似的问题。算法+数据结构的效率不是太高，并发量突然太高，因为这里稍微耗时一些，导致很多线程卡在这里的话，不太好，所以进行一个基本的资源隔离和访问，避免内部复杂的低效率的代码，导致大量的线程被hang住。

业务背景里面， 比较适合信号量的是什么场景呢？

比如说，我们一般来说，缓存服务，可能会将部分量特别少，访问又特别频繁的一些数据，放在自己的纯内存中。

一般我们在获取到商品数据之后，都要去获取商品是属于哪个地理位置，省，市，卖家的，可能在自己的纯内存中，比如就一个Map去获取。

对于这种直接访问本地内存的逻辑，比较适合用信号量做一下简单的隔离。

优点在于，不用自己管理线程池拉，不用care timeout超时了，信号量做隔离的话，性能会相对来说高一些。

如何在线程池和信号量之间做选择？

默认的策略就是线程池。

线程池其实最大的好处就是对于网络访问请求，如果有超时的话，可以避免调用线程阻塞住。

而使用信号量的场景，通常是针对超大并发量的场景下，每个服务实例每秒都几百的QPS，那么此时你用线程池的话，线程一般不会太多，可能撑不住那么高的并发，如果要撑住，可能要耗费大量的线程资源，那么就是用信号量，来进行限流保护。

一般用信号量常见于那种基于纯内存的一些业务逻辑服务，而不涉及到任何网络访问请求。

### 10.command的四种调用方式

同步：new CommandHelloWorld("World").execute()，new ObservableCommandHelloWorld("World").toBlocking().toFuture().get()

如果你认为observable command只会返回一条数据，那么可以调用上面的模式，去同步执行，返回一条数据。

异步：new CommandHelloWorld("World").queue()，new ObservableCommandHelloWorld("World").toBlocking().toFuture()

对command调用queue()，仅仅将command放入线程池的一个等待队列，就立即返回，拿到一个Future对象，后面可以做一些其他的事情，然后过一段时间对future调用get()方法获取数据。

// observe()：hot，已经执行过了 // toObservable(): cold，还没执行过

### 10.Hystrix 隔离策略细粒度控制

execution.isolation.strategy

指定了 HystrixCommand.run() 的资源隔离策略：`THREAD` or `SEMAPHORE`，一种基于线程池，一种基于信号量。

```html
 // to use thread isolation
 HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.THREAD)
 
 // to use semaphore isolation
 HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.SEMAPHORE)
```

线程池机制，每个 command 运行在一个线程中，限流是通过线程池的大小来控制的；信号量机制，command 是运行在调用线程中（也就是 Tomcat 的线程池），通过信号量的容量来进行限流。

如何在线程池和信号量之间做选择？

**默认的策略**就是线程池。

**线程池**其实最大的好处就是对于网络访问请求，如果有超时的话，可以避免调用线程阻塞住。

而使用信号量的场景，通常是针对超大并发量的场景下，每个服务实例每秒都几百的 `QPS`，那么此时你用线程池的话，线程一般不会太多，可能撑不住那么高的并发，如果要撑住，可能要耗费大量的线程资源，那么就是用信号量，来进行限流保护。一般用信号量常见于那种基于纯内存的一些业务逻辑服务，而不涉及到任何网络访问请求。

command key & command group

每一个 command，都可以设置一个自己的名称 command key，同时可以设置一个自己的组 command group。

command group 是一个非常重要的概念，默认情况下，就是通过 command group 来定义一个线程池的，而且还会通过 command group 来聚合一些监控和报警信息。同一个 command group 中的请求，都会进入同一个线程池中。

command thread pool

ThreadPoolKey 代表了一个 HystrixThreadPool，用来进行统一监控、统计、缓存。默认的 ThreadPoolKey 就是 command group 的名称。每个 command 都会跟它的 ThreadPoolKey 对应的 ThreadPool 绑定在一起。

如果不想直接用 command group，也可以手动设置 ThreadPool 的名称。

command key & command group & command thread pool

**command key** ，代表了一类 command，一般来说，代表了下游依赖服务的某个接口。

**command group** ，代表了某一个下游依赖服务，这是很合理的，一个依赖服务可能会暴露出来多个接口，每个接口就是一个 command key。command group 在逻辑上对一堆 command key 的调用次数、成功次数、timeout 次数、失败次数等进行统计，可以看到某一个服务整体的一些访问情况。**一般来说，推荐根据一个服务区划分出一个线程池，command key 默认都是属于同一个线程池的。**

比如说有一个服务 A，你估算出来服务 A 每秒所有接口加起来的整体 `QPS` 在 100 左右，你有一个服务 B 去调用服务 A。你的服务 B 部署了 10 个实例，每个实例上，用 command group 去对应下游服务 A。给一个线程池，量大概是 10 就可以了，这样服务 B 对服务 A 整体的访问 QPS 就大概是每秒 100 了。

但是，如果说 command group 对应了一个服务，而这个服务暴露出来的几个接口，访问量很不一样，差异非常之大。你可能就希望在这个服务对应 command group 的内部，包含对应多个接口的 command key，做一些细粒度的资源隔离。**就是说，希望对同一个服务的不同接口，使用不同的线程池。**

```html
 command key -> command group
 
 command key -> 自己的 thread pool key
```

逻辑上来说，多个 command key 属于一个 command group，在做统计的时候，会放在一起统计。每个 command key 有自己的线程池，每个接口有自己的线程池，去做资源隔离和限流。

说白点，就是说如果你的 command key 要用自己的线程池，可以定义自己的 thread pool key，就 ok 了。

coreSize

设置线程池的大小，默认是 10。一般来说，用这个默认的 10 个线程大小就够了。

```html
 HystrixThreadPoolProperties.Setter().withCoreSize(int value);
```

queueSizeRejectionThreshold

如果说线程池中的 10 个线程都在工作中，没有空闲的线程来做其它的事情，此时再有请求过来，会先进入队列积压。如果说队列积压满了，再有请求过来，就直接 reject，拒绝请求，执行 fallback 降级的逻辑，快速返回。

控制 queue 满了之后 reject 的 threshold，因为 maxQueueSize 不允许热修改，因此提供这个参数可以热修改，控制队列的最大大小。

```html
 HystrixThreadPoolProperties.Setter().withQueueSizeRejectionThreshold(int value);
```

execution.isolation.semaphore.maxConcurrentRequests

设置使用 SEMAPHORE 隔离策略的时候允许访问的最大并发量，超过这个最大并发量，请求直接被 reject。

这个并发量的设置，跟线程池大小的设置，应该是类似的，但是基于信号量的话，性能会好很多，而且 Hystrix 框架本身的开销会小很多。

默认值是 10，尽量设置的小一些，因为一旦设置的太大，而且有延时发生，可能瞬间导致 tomcat 本身的线程资源被占满。

```html
HystrixCommandProperties.Setter().withExecutionIsolationSemaphoreMaxConcurrentRequests(int value);
```

### 11.Hystrix 执行时内部原理

Hystrix 最基本的支持高可用的技术：**资源隔离** + **限流**。

- 创建 command；
- 执行这个 command；
- 配置这个 command 对应的 group 和线程池。

步骤一：创建 command

一个 HystrixCommand 或 HystrixObservableCommand 对象，代表了对某个依赖服务发起的一次请求或者调用。创建的时候，可以在构造函数中传入任何需要的参数。

- HystrixCommand 主要用于仅仅会返回一个结果的调用。
- HystrixObservableCommand 主要用于可能会返回多条结果的调用。

```html
// 创建 HystrixCommand
HystrixCommand hystrixCommand = new HystrixCommand(arg1, arg2);

// 创建 HystrixObservableCommand
HystrixObservableCommand hystrixObservableCommand = new HystrixObservableCommand(arg1, arg2);
```

步骤二：调用 command 执行方法

执行 command，就可以发起一次对依赖服务的调用。

要执行 command，可以在 4 个方法中选择其中的一个：execute()、queue()、observe()、toObservable()。

其中 execute() 和 queue() 方法仅仅对 HystrixCommand 适用。

- execute()：调用后直接 block 住，属于同步调用，直到依赖服务返回单条结果，或者抛出异常。
- queue()：返回一个 Future，属于异步调用，后面可以通过 Future 获取单条结果。
- observe()：订阅一个 Observable 对象，Observable 代表的是依赖服务返回的结果，获取到一个那个代表结果的 Observable 对象的拷贝对象。
- toObservable()：返回一个 Observable 对象，如果我们订阅这个对象，就会执行 command 并且获取返回结果。

```html
K             value    = hystrixCommand.execute();
Future<K>     fValue   = hystrixCommand.queue();
Observable<K> oValue   = hystrixObservableCommand.observe();
Observable<K> toOValue = hystrixObservableCommand.toObservable();
```

execute() 实际上会调用 queue().get() 方法，而在 queue() 方法中，会调用 toObservable().toBlocking().toFuture()。

也就是说，先通过 toObservable() 获得 Future 对象，然后调用 Future 的 get() 方法。那么，其实无论是哪种方式执行 command，最终都是依赖于 toObservable() 去执行的。

步骤三：检查是否开启缓存（不太常用）

从这一步开始，就进入到 Hystrix 底层运行原理啦，看一下 Hystrix 一些更高级的功能和特性。

如果这个 command 开启了请求缓存 Request Cache，而且这个调用的结果在缓存中存在，那么直接从缓存中返回结果。否则，继续往后的步骤。

步骤四：检查是否开启了断路器

检查这个 command 对应的依赖服务是否开启了断路器。如果断路器被打开了，那么 Hystrix 就不会执行这个 command，而是直接去执行 fallback 降级机制，返回降级结果。

步骤五：检查线程池/队列/信号量是否已满

如果这个 command 线程池和队列已满，或者 semaphore 信号量已满，那么也不会执行 command，而是直接去调用 fallback 降级机制，同时发送 reject 信息给断路器统计。

步骤六：执行 command

调用 HystrixObservableCommand 对象的 construct() 方法，或者 HystrixCommand 的 run() 方法来实际执行这个 command。

- HystrixCommand.run() 返回单条结果，或者抛出异常。
- HystrixObservableCommand.construct() 返回一个 Observable 对象，可以获取多条结果。

如果是采用线程池方式，并且 HystrixCommand.run() 或者 HystrixObservableCommand.construct() 的执行时间超过了 timeout 时长的话，那么 command 所在的线程会抛出一个 TimeoutException，这时会执行 fallback 降级机制，不会去管 run() 或 construct() 返回的值了。另一种情况，如果 command 执行出错抛出了其它异常，那么也会走 fallback 降级。这两种情况下，Hystrix 都会发送异常事件给断路器统计。

**注意**，我们是不可能终止掉一个调用严重延迟的依赖服务的线程的，只能说给你抛出来一个 TimeoutException。

如果没有 timeout，也正常执行的话，那么调用线程就会拿到一些调用依赖服务获取到的结果，然后 Hystrix 也会做一些 logging 记录和 metric 度量统计。

步骤七：断路健康检查

Hystrix 会把每一个依赖服务的调用成功、失败、Reject、Timeout 等事件发送给 circuit breaker 断路器。断路器就会对这些事件的次数进行统计，根据异常事件发生的比例来决定是否要进行断路（熔断）。如果打开了断路器，那么在接下来一段时间内，会直接断路，返回降级结果。

如果在之后，断路器尝试执行 command，调用没有出错，返回了正常结果，那么 Hystrix 就会把断路器关闭。

步骤八：调用 fallback 降级机制

在以下几种情况中，Hystrix 会调用 fallback 降级机制。

- 断路器处于打开状态；
- 线程池/队列/semaphore 满了；
- command 执行超时；
- run() 或者 construct() 抛出异常。

一般在降级机制中，都建议给出一些默认的返回值，比如静态的一些代码逻辑，或者从内存中的缓存中提取一些数据，在这里尽量不要再进行网络请求了。

在降级中，如果一定要进行网络调用的话，也应该将那个调用放在一个 HystrixCommand 中进行隔离。

- HystrixCommand 中，实现 getFallback() 方法，可以提供降级机制。
- HystrixObservableCommand 中，实现 resumeWithFallback() 方法，返回一个 Observable 对象，可以提供降级结果。

如果没有实现 fallback，或者 fallback 抛出了异常，Hystrix 会返回一个 Observable，但是不会返回任何数据。

不同的 command 执行方式，其 fallback 为空或者异常时的返回结果不同。

- 对于 execute()，直接抛出异常。
- 对于 queue()，返回一个 Future，调用 get() 时抛出异常。
- 对于 observe()，返回一个 Observable 对象，但是调用 subscribe() 方法订阅它时，立即抛出调用者的 onError() 方法。
- 对于 toObservable()，返回一个 Observable 对象，但是调用 subscribe() 方法订阅它时，立即抛出调用者的 onError() 方法。

不同的执行方式

- execute()，获取一个 Future.get()，然后拿到单个结果。
- queue()，返回一个 Future。
- observe()，立即订阅 Observable，然后启动 8 大执行步骤，返回一个拷贝的 Observable，订阅时立即回调给你结果。
- toObservable()，返回一个原始的 Observable，必须手动订阅才会去执行 8 大步骤。

### 12.基于本地缓存的 fallback 降级机制

Hystrix 出现以下四种情况，都会去调用 fallback 降级机制：

- 断路器处于打开的状态。
- 资源池已满（线程池+队列 / 信号量）。
- Hystrix 调用各种接口，或者访问外部依赖，比如 MySQL、Redis、Zookeeper、Kafka 等等，出现了任何异常的情况。
- 访问外部依赖的时候，访问时间过长，报了 TimeoutException 异常。

两种最经典的降级机制

- 纯内存数据 在降级逻辑中，你可以在内存中维护一个 ehcache，作为一个纯内存的基于 LRU 自动清理的缓存，让数据放在缓存内。如果说外部依赖有异常，fallback 这里直接尝试从 ehcache 中获取数据。
- 默认值 fallback 降级逻辑中，也可以直接返回一个默认值。

在 `HystrixCommand`，降级逻辑的书写，是通过实现 getFallback() 接口；而在 `HystrixObservableCommand` 中，则是实现 resumeWithFallback() 方法。

### 13.Hystrix 断路器执行原理

状态机

Hystrix 断路器有三种状态，分别是关闭（Closed）、打开（Open）与半开（Half-Open）。

1. `Closed` 断路器关闭：调用下游的请求正常通过
2. `Open` 断路器打开：阻断对下游服务的调用，直接走 Fallback 逻辑
3. `Half-Open` 断路器处于半开状态：SleepWindowInMilliseconds

1、如果经过短路器的流量超过了一定的阈值，HystrixCommandProperties.circuitBreakerRequestVolumeThreshold()。

举个例子，可能看起来是这样子的，要求在10s内，经过短路器的流量必须达到20个；在10s内，经过短路器的流量才10个，那么根本不会去判断要不要短路。

2、如果断路器统计到的异常调用的占比超过了一定的阈值，HystrixCommandProperties.circuitBreakerErrorThresholdPercentage()。

如果达到了上面的要求，比如说在10s内，经过短路器的流量（你，只要执行一个command，这个请求就一定会经过短路器），达到了30个；同时其中异常的访问数量，占到了一定的比例，比如说60%的请求都是异常（报错，timeout，reject），会开启短路。

3、然后断路器从close状态转换到open状态。

4、断路器打开的时候，所有经过该断路器的请求全部被短路，不调用后端服务，直接走fallback降级。

5、经过了一段时间之后，HystrixCommandProperties.circuitBreakerSleepWindowInMilliseconds()，会half-open，让一条请求经过短路器，看能不能正常调用。如果调用成功了，那么就自动恢复，转到close状态。

短路器，会自动恢复的，half-open，半开状态。

6、circuit breaker短路器的配置

（1）circuitBreaker.enabled

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerEnabled(boolean)
```

控制断路器是否工作，包括跟踪依赖服务调用的健康状况，以及对异常情况过多时是否允许触发断路。默认值 `true`。

（2）circuitBreaker.requestVolumeThreshold

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerRequestVolumeThreshold(int)
```

表示在一次统计的**时间滑动窗口中（这个参数也很重要，下面有说到）**，至少经过多少个请求，才可能触发断路，默认值 20。**经过 Hystrix 断路器的流量只有在超过了一定阈值后，才有可能触发断路。**比如说，要求在 10s 内经过断路器的流量必须达到 20 个，而实际经过断路器的请求有 19 个，即使这 19 个请求全都失败，也不会去判断要不要断路。

（3）circuitBreaker.errorThresholdPercentage

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerErrorThresholdPercentage(int)
```

设置异常请求量的百分比，当异常请求达到这个百分比时，就触发打开短路器，默认是50，也就是50%。

（4）circuitBreaker.sleepWindowInMilliseconds

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerSleepWindowInMilliseconds(int)
```

断路器状态由 Close 转换到 Open，在之后 `SleepWindowInMilliseconds` 时间内，所有经过该断路器的请求会被断路，不调用后端服务，直接走 Fallback 降级机制，默认值 5000(ms)。

而在该参数时间过后，断路器会变为 `Half-Open` 半开闭状态，尝试让一条请求经过断路器，看能不能正常调用。如果调用成功了，那么就自动恢复，断路器转为 Close 状态。

（5）circuitBreaker.forceOpen

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerForceOpen(boolean)
```

如果设置为 true 的话，直接强迫打开断路器，相当于是手动断路了，手动降级，默认值是 `false`。

（6）circuitBreaker.forceClosed

```html
HystrixCommandProperties.Setter()
    .withCircuitBreakerForceClosed(boolean)
```

如果设置为 true，直接强迫关闭断路器，相当于手动停止断路了，手动升级，默认值是 `false`。

### 14.Hystrix 线程池隔离与接口限流

Hystrix 通过判断线程池或者信号量是否已满，超出容量的请求，直接 Reject 走降级，从而达到限流的作用。

限流是限制对后端的服务的访问量，比如说你对 MySQL、Redis、Zookeeper 以及其它各种后端中间件的资源的访问的限制，其实是为了避免过大的流量直接打死后端的服务。

线程池隔离技术的设计

Hystrix 采用了 Bulkhead Partition 舱壁隔离技术，来将外部依赖进行资源隔离，进而避免任何外部依赖的故障导致本服务崩溃。

**舱壁隔离**，是说将船体内部空间区隔划分成若干个隔舱，一旦某几个隔舱发生破损进水，水流不会在其间相互流动，如此一来船舶在受损时，依然能具有足够的浮力和稳定性，进而减低立即沉船的危险。

Hystrix 对每个外部依赖用一个单独的线程池，这样的话，如果对那个外部依赖调用延迟很严重，最多就是耗尽那个依赖自己的线程池而已，不会影响其他的依赖调用。

线程池机制的优点

- 任何一个依赖服务都可以被隔离在自己的线程池内，即使自己的线程池资源填满了，也不会影响任何其他的服务调用。
- 服务可以随时引入一个新的依赖服务，因为即使这个新的依赖服务有问题，也不会影响其他任何服务的调用。
- 当一个故障的依赖服务重新变好的时候，可以通过清理掉线程池，瞬间恢复该服务的调用，而如果是 tomcat 线程池被占满，再恢复就很麻烦。
- 如果一个 client 调用库配置有问题，线程池的健康状况随时会报告，比如成功/失败/拒绝/超时的次数统计，然后可以近实时热修改依赖服务的调用配置，而不用停机。
- 如果一个服务本身发生了修改，需要重新调整配置，此时线程池的健康状况也可以随时发现，比如成功/失败/拒绝/超时的次数统计，然后可以近实时热修改依赖服务的调用配置，而不用停机。
- 基于线程池的异步本质，可以在同步的调用之上，构建一层异步调用层。

简单来说，最大的好处，就是资源隔离，确保说任何一个依赖服务故障，不会拖垮当前的这个服务。

线程池机制的缺点

- 线程池机制最大的缺点就是增加了 CPU 的开销。 除了 tomcat 本身的调用线程之外，还有 Hystrix 自己管理的线程池。
- 每个 command 的执行都依托一个独立的线程，会进行排队，调度，还有上下文切换。
- Hystrix官方自己做了一个多线程异步带来的额外开销，通过对比多线程异步调用+同步调用得出，Netflix API每天通过hystrix执行10亿次调用，每个服务实例有40个以上的线程池，每个线程池有10个左右的线程。
- 最后发现说，用hystrix的额外开销，就是给请求带来了3ms左右的延时，最多延时在10ms以内，相比于可用性和稳定性的提升，这是可以接受的。

我们可以用 Hystrix semaphore 技术来实现对某个依赖服务的并发访问量的限制，而不是通过线程池/队列的大小来限制流量。

semaphore 技术可以用来限流和削峰，但是不能用来对调用延迟的服务进行 timeout 和隔离。

`execution.isolation.strategy` 设置为 `SEMAPHORE`，那么 Hystrix 就会用 semaphore 机制来替代线程池机制，来对依赖服务的访问进行限流。如果通过 semaphore 调用的时候，底层的网络调用延迟很严重，那么是无法 timeout 的，只能一直 block 住。一旦请求数量超过了 semaphore 限定的数量之后，就会立即开启限流。

### 15.基于 timeout 机制为服务接口调用超时提供安全保护

TimeoutMilliseconds

在 Hystrix 中，我们可以手动设置 timeout 时长，如果一个 command 运行时间超过了设定的时长，那么就被认为是 timeout，然后 Hystrix command 标识为 timeout，同时执行 fallback 降级逻辑。

`TimeoutMilliseconds` 默认值是 1000，也就是 1000ms。

```html
HystrixCommandProperties.Setter()
    .withExecutionTimeoutInMilliseconds(int)
```

TimeoutEnabled

这个参数用于控制是否要打开 timeout 机制，默认值是 true。

```html
HystrixCommandProperties.Setter()
    .withExecutionTimeoutEnabled(boolean)
```

### 16.Hystrix 断路器机制

断路器很好理解, 当Hystrix Command 请求后端服务失败数量超过一定比例(默认50%), 断路器会切换到开路状态(Open). 这时所有请求会直接失败而不会发送到后端服务. 断路器保持在开路状态一段时间后(默认5 秒), 自动切换到半开路状态(HALF-OPEN). 这时会判断下一次请求的返回情况,如果请求成功, 断路器切回闭路状态(CLOSED), 否则重新切换到开路状态(OPEN). Hystrix 的断路器就像我们家庭电路中的保险丝, 一旦后端服务不可用, 断路器会直接切断请求链, 避免发送大量无效请求影响系统吞吐量, 并且断路器有自我检测并恢复的能力。

### 17.短路器深入的工作原理

1、如果经过短路器的流量超过了一定的阈值，HystrixCommandProperties.circuitBreakerRequestVolumeThreshold()。

举个例子，可能看起来是这样子的，要求在10s内，经过短路器的流量必须达到20个；在10s内，经过短路器的流量才10个，那么根本不会去判断要不要短路。

2、如果断路器统计到的异常调用的占比超过了一定的阈值，HystrixCommandProperties.circuitBreakerErrorThresholdPercentage()。

如果达到了上面的要求，比如说在10s内，经过短路器的流量（你，只要执行一个command，这个请求就一定会经过短路器），达到了30个；同时其中异常的访问数量，占到了一定的比例，比如说60%的请求都是异常（报错，timeout，reject），会开启短路。

3、然后断路器从close状态转换到open状态。

4、断路器打开的时候，所有经过该断路器的请求全部被短路，不调用后端服务，直接走fallback降级。

5、经过了一段时间之后，HystrixCommandProperties.circuitBreakerSleepWindowInMilliseconds()，会half-open，让一条请求经过短路器，看能不能正常调用。如果调用成功了，那么就自动恢复，转到close状态。

短路器，会自动恢复的，half-open，半开状态。

6、circuit breaker短路器的配置

（1）circuitBreaker.enabled

控制短路器是否允许工作，包括跟踪依赖服务调用的健康状况，以及对异常情况过多时是否允许触发短路，默认是true。

```html
HystrixCommandProperties.Setter().withCircuitBreakerEnabled(boolean value)
```

（2）circuitBreaker.requestVolumeThreshold

设置一个rolling window，滑动窗口中，最少要有多少个请求时，才触发开启短路。

举例来说，如果设置为20（默认值），那么在一个10秒的滑动窗口内，如果只有19个请求，即使这19个请求都是异常的，也是不会触发开启短路器的。

```html
HystrixCommandProperties.Setter() .withCircuitBreakerRequestVolumeThreshold(int value)
```

（3）circuitBreaker.sleepWindowInMilliseconds

设置在短路之后，需要在多长时间内直接reject请求，然后在这段时间之后，再重新导holf-open状态，尝试允许请求通过以及自动恢复，默认值是5000毫秒。

```html
HystrixCommandProperties.Setter().withCircuitBreakerSleepWindowInMilliseconds(int value)
```

（4）circuitBreaker.errorThresholdPercentage

设置异常请求量的百分比，当异常请求达到这个百分比时，就触发打开短路器，默认是50，也就是50%。

```html
HystrixCommandProperties.Setter().withCircuitBreakerErrorThresholdPercentage(int value)
```

（5）circuitBreaker.forceOpen

如果设置为true的话，直接强迫打开短路器，相当于是手动短路了，手动降级，默认false

```html
HystrixCommandProperties.Setter().withCircuitBreakerForceOpen(boolean value)
```

（6）circuitBreaker.forceClosed

如果设置为ture的话，直接强迫关闭短路器，相当于是手动停止短路了，手动升级，默认false

HystrixCommandProperties.Setter() .withCircuitBreakerForceClosed(boolean value)

### 18.基于request cache请求缓存技术

首先，有一个概念，叫做reqeust context，请求上下文，一般来说，在一个web应用中，hystrix。

我们会在一个filter里面，对每一个请求都施加一个请求上下文，就是说，tomcat容器内，每一次请求，就是一次请求上下文。

然后在这次请求上下文中，我们会去执行N多代码，调用N多依赖服务，有的依赖服务可能还会调用好几次。

在一次请求上下文中，如果有多个command，参数都是一样的，调用的接口也是一样的，其实结果可以认为也是一样的。

那么这个时候，我们就可以让第一次command执行，返回的结果，被缓存在内存中，然后这个请求上下文中，后续的其他对这个依赖的调用全部从内存中取用缓存结果就可以了。

不用在一次请求上下文中反复多次的执行一样的command，提升整个请求的性能。

HystrixCommand和HystrixObservableCommand都可以指定一个缓存key，然后hystrix会自动进行缓存，接着在同一个request context内，再次访问的时候，就会直接取用缓存。

用请求缓存，可以避免重复执行网络请求。

多次调用一个command，那么只会执行一次，后面都是直接取缓存。

对于请求缓存（request caching），请求合并（request collapsing），请求日志（request log），等等技术，都必须自己管理HystrixReuqestContext的声明周期。

在一个请求执行之前，都必须先初始化一个request context。

HystrixRequestContext context = HystrixRequestContext.initializeContext();

然后在请求结束之后，需要关闭request context。

context.shutdown();

一般来说，在java web来的应用中，都是通过filter过滤器来实现的。

批量查询商品数据的接口，可以用request cache做一个优化，就是说一次请求，就是一次request context，对相同的商品查询只能执行一次，其余的都走request cache。

### 19.请求合并技术

hystrix，高级的技术，request collapser，请求合并技术，collapser折叠。

优化过一个批量查询的接口了，request cache来做优化，可能有相同的商品就可以直接取用缓存了。

多个商品，需要发送多次网络请求，调用多次接口，才能拿到结果。

可以使用HystrixCollapser将多个HystrixCommand合并到一起，多个command放在一个command里面去执行，发送一次网络请求，就拉取到多条数据。

用请求合并技术，将多个请求合并起来，可以减少高并发访问下需要使用的线程数量以及网络连接数量，这都是hystrix自动进行的。

其实对于高并发的访问来说，是可以提升性能的。

请求合并有很多种级别。

（1）global context，tomcat所有调用线程，对一个依赖服务的任何一个command调用都可以被合并在一起，hystrix就传递一个HystrixRequestContext。

（2）user request context，tomcat内某一个调用线程，将某一个tomcat线程对某个依赖服务的多个command调用合并在一起。

（3）object modeling，基于对象的请求合并，如果有几百个对象，遍历后依次调用每个对象的某个方法，可能导致发起几百次网络请求，基于hystrix可以自动将对多个对象模型的调用合并到一起。

请求合并技术的开销有多大。

使用请求合并技术的开销就是导致延迟大幅度增加，因为需要一定的时间将多个请求合并起来。

发送过来10个请求，每个请求本来大概是2ms可以返回，要把10个请求合并在一个command内，统一一起执行，先后等待一下，5ms。

所以说，要考量一下，使用请求合并技术是否合适，如果一个请求本来耗费的时间就比较长，那么进行请求合并，增加一些延迟影响并不大。

请求合并技术，不是针对那种访问延时特别低的请求的，比如说你的访问延时本身就比较高，20ms，10个请求合并在一起，25ms，这种情况下就还好。

好处在哪里，大幅度削减你的线程池的资源耗费，线程池，10个线程，一秒钟可以执行10个请求，合并在一起，1个线程执行10个请求，10个线程就可以执行100个请求。

增加你的吞吐量。

减少你对后端服务访问时的网络资源的开销，10个请求，10个command，10次网络请求的开销，1次网络请求的开销了。

每个请求就2ms，batch，8 到10ms，延迟增加了4到5倍 。

每个请求本来就30ms-50ms，batch，35ms~55ms，延迟增加不太明显。

将多个command请求合并到一个command中执行。

请求合并时，可以设置一个batch size，以及elapsed time（控制什么时候触发合并后的command执行）。

有两种合并模式，一种是request scope，另一种是global scope，默认是rquest scope，在collapser构造的时候指定scope模式。

request scope的batch收集是建立在一个request context内的，而global scope的batch收集是横跨多个request context的。

所以对于global context来说，必须确保能在一个command内处理多个requeset context的请求。

在netflix，是只用request scope请求合并的，因为默认是用唯一一个request context包含所有的command，所以要做合并，肯定就是request scope。

一般请求合并技术，对于那种访问同一个资源的command，但是参数不同，是很有效的。

批量查询，HystrixObservableCommand，HystrixCommand+request cache，都是每个商品发起一次网络请求。

一个批量的商品过来以后，我们还是多个command的方式去执行，request collapser+request cache，相同的商品还是就查询一次，不同的商品合并到一起通过一个网络请求得到结果。

timeout问题解释：开发机上，特别慢，第一次请求的时候，几百毫秒，默认的timeout时长比较短。

第二次的时候，访问的速度会快很多，就不会超时了。

反应在系统上，第一次启动的时候，会有个别的超时，但是后面就好了，手动将timeout时长设置的大一些。

（1）maxRequestsInBatch

控制一个Batch中最多允许多少个request被合并，然后才会触发一个batch的执行。

默认值是无限大，就是不依靠这个数量来触发执行，而是依靠时间。

```html
HystrixCollapserProperties.Setter()
   .withMaxRequestsInBatch(int value)
```

（2）timerDelayInMilliseconds

控制一个batch创建之后，多长时间以后就自动触发batch的执行，默认是10毫秒。

```html
HystrixCollapserProperties.Setter()
   .withTimerDelayInMilliseconds(int value)

super(Setter.withCollapserKey(HystrixCollapserKey.Factory.asKey("GetProductInfosCollapser"))
				.andCollapserPropertiesDefaults(HystrixCollapserProperties.Setter()
						   .withMaxRequestsInBatch(100)
						   .withTimerDelayInMilliseconds(20))); 
```

### 20.hystirx的fail-fast与fail-silient两种最基础的容错模式

fail-fast，就是不给fallback降级逻辑，HystrixCommand.run()，直接报错，直接会把这个报错抛出来，给你的tomcat调用线程。

fail-silent，给一个fallback降级逻辑，如果HystrixCommand.run()，报错了，会走fallback降级，直接返回一个空值，HystrixCommand，就给一个null。

HystrixObservableCommand，Observable.empty()

很少会用fail-fast模式，比较常用的可能还是fail-silent，特别常用，既然都到了fallback里面，肯定要做点降级的事情。

### 21.stubbed fallback

stubbed fallback，残缺的降级。

用请求中的部分数据拼装成结果，然后再填充一些默认值，返回。

比如说你发起了一个请求，然后请求中可能本身就附带了一些信息，如果主请求失败了，走到降级逻辑。

在降级逻辑里面，可以将这个请求中的数据，以及部分本地缓存有的数据拼装在一起，再给数据填充一些简单的默认值。然后尽可能将自己有的数据返回到请求方。

stubbed，残缺了，比如说应该查询到一个商品信息，里面包含20个字段。

请求参数搂出来一两个字段，从本地的少量缓存中比如说，可以搂出来那么两三个字段，最终的话返回的字段可能就五六个，其他的字段都是填充的默认值

数据有残缺。

在你自己的项目里去用的话，你就必须结合你自己的业务场景，去思考，stubbed fallback，从请求参数里尽可能提取一些数据，请求参数多给一些。

你要考虑到可以将哪些量比较少的数据保存在内存中，提取部分数据。默认的值怎么设置，看起来能稍微靠谱一些。

### 22.多级降级

先降一级，尝试用一个备用方案去执行，如果备用方案失败了，再用最后下一个备用方案去执行。

command嵌套command。

尝试从备用服务器接口去拉取结果。

给大家科普一下，常见的多级降级的做法，有一个操作，要访问MySQL数据库。

mysql数据库访问报错，降级，去redis中获取数据。

如果说redis又挂了，然后就去从本地ehcache缓存中获取数据。

hystrix command fallback语义，很容易就可以实现多级降级的策略。

商品服务接口，多级降级的策略。

command，fallback，又套了一个command，第二个command其实是第一级降级策略。

第二个command的fallback是第二级降级策略。

第一级降级策略，可以是storm，我们之前做storm这块，第一级降级，一般是搞一个storm的备用机房，部署了一套一模一样的拓扑，如果主机房中的storm拓扑挂掉了，备用机房的storm拓扑定顶上。

如果备用机房的storm拓扑也挂了。

第二级降级，可能就降级成用mysql/hbase/redis/es，手工封装的一套，按分钟粒度去统计数据的系统。

第三级降级，离线批处理去做，hdfs+spark，每个小时执行一次数据统计，去降级。

特别复杂，重要的系统，肯定是要搞好几套备用方案的，一个方案死了，立即上第二个方案，而且要尽量做到是自动化的。

商品接口拉取。

主流程，访问的商品服务，是从主机房去访问的，服务，如果主机房的服务出现了故障，机房断电，机房的网络负载过高，机器硬件出了故障。

第一级降级策略，去访问备用机房的服务

第二级降级策略，用stubbed fallback降级策略，比较常用的，返回一些残缺的数据回去。

### 23.手动降级

你写一个command，在这个command它的主流程中，根据一个标识位，判断要执行哪个流程。

可以执行主流程，command，也可以执行一个备用降级的command。

一般来说，都是去执行一个主流程的command，如果说你现在知道有问题了，希望能够手动降级的话，动态给服务发送个请求。

在请求中修改标识位，自动就让command以后都直接过来执行备用command。

3个command，套在最外面的command，是用semaphore信号量做限流和资源隔离的，因为这个command不用去care timeout的问题，嵌套调用的command会自己去管理timeout超时的。

商品服务接口的手动降级的方案。

主流程还是去走GetProductInfoCommand，手动降级的方案，比如说是从某一个数据源，自己去简单的获取一些数据，尝试封装一下返回。

手动降级的策略，就比较low了，调用别人的接口去获取数据的，业务逻辑的封装。

主流程有问题，那么可能你就需要立即自己写一些逻辑发布上去，从mysql数据库的表中获取一些数据去返回，手动调整一下降级标识，做一下手动降级。

### 24.生产环境中的线程池大小以及timeout超时时长优化经验总结

生产环境里面，一个是线程池的大小怎么设置，timeout时长怎么。

不合理的话，问题还是很大的。

在生产环境中部署一个短路器，一开始需要将一些关键配置设置的大一些，比如timeout超时时长，线程池大小，或信号量容量。

然后逐渐优化这些配置，直到在一个生产系统中运作良好。

（1）一开始先不要设置timeout超时时长，默认就是1000ms，也就是1s。 （2）一开始也不要设置线程池大小，默认就是10。 （3）直接部署hystrix到生产环境，如果运行的很良好，那么就让它这样运行好了。 （4）让hystrix应用，24小时运行在生产环境中。 （5）依赖标准的监控和报警机制来捕获到系统的异常运行情况。 （6）在24小时之后，看一下调用延迟的占比，以及流量，来计算出让短路器生效的最小的配置数字。 （7）直接对hystrix配置进行热修改，然后继续在hystrix dashboard上监控。 （8）看看修改配置后的系统表现有没有改善。

下面是根据系统表现优化和调整线程池大小，队列大小，信号量容量，以及timeout超时时间的经验。

假设对一个依赖服务的高峰调用QPS是每秒30次。

一开始如果默认的线程池大小是10。

我们想的是，理想情况下，每秒的高峰访问次数 * 99%的访问延时 + buffer = 30 * 0.2 + 4 = 10线程，10个线程每秒处理30次访问应该足够了，每个线程处理3次访问。

此时，我们合理的timeout设置应该为300ms，也就是99.5%的访问延时，计算方法是，因为判断每次访问延时最多在250ms（TP99如果是200ms的话），再加一次重试时间50ms，就是300ms，感觉也应该足够了。

因为如果timeout设置的太多了，比如400ms，比如如果实际上，在高峰期，还有网络情况较差的时候，可能每次调用要耗费350ms，也就是达到了最长的访问时长。

那么每个线程处理2个请求，就会执行700ms，然后处理第三个请求的时候，就超过1秒钟了，此时会导致线程池全部被占满，都在处理请求。

这个时候下一秒的30个请求再进来了，那么就会导致线程池已满，拒绝请求的情况，就会调用fallback降级机制。

因此对于短路器来说，timeout超时一般应该设置成TP99.5，比如设置成300ms，那么可以确保说，10个线程，每个线程处理3个访问，每个访问最多就允许执行300ms，过时就timeout了。

这样才能保证说每个线程都在1s内执行完，才不会导致线程池被占满，然后后续的请求过来大量的reject。

对于线程池大小来说，一般应该控制在10个左右，20个以内，最少5个，不要太多，也不要太少。

大家可能会想，每秒的高峰访问次数是30次，如果是300次，甚至是3000次，30000次呢？？？

30000 * 0.2 = 6000 + buffer = 6100，一个服务器内一个线程池给6000个线程把。

如果你一个依赖服务占据的线程数量太多的话，会导致其他的依赖服务对应的线程池里没有资源可以用了。

6000 / 20 = 300台虚拟机也是ok的。

虚拟机，4个cpu core，4G内存，虚拟机，300台。

物理机，十几个cpu core，几十个G的内存，5~8个虚拟机，300个虚拟机 = 50台物理机。

你要真的说是，你的公司服务的用户量，或者数据量，或者请求量，真要是到了每秒几万的QPS，3万QPS，60 * 3 = 180万访问量，1800，1亿8千，1亿，10个小时，10亿的访问量，app，系统几十台服务器去支撑，我觉得很正常。

QPS每秒在几千都算多的了。

### 25.生产环境中的线程池自动扩容与缩容的动态资源分配经验

可能会出现一种情况，比如说我们的某个依赖，在高峰期，需要耗费100个线程，但是在那个时间段，刚好其他的依赖的线程池其实就维持一两个就可以了。

但是，如果我们都是设置死的，每个服务就给10个线程，那就很坑，可能就导致有的服务在高峰期需要更多的资源，但是没资源了，导致很多的reject。

但是其他的服务，每秒钟就易一两个请求，结果也占用了10个线程，占着茅坑不拉屎。

做成弹性的线程资源调度的模式。

刚开始的时候，每个依赖服务都是给1个线程，3个线程，但是我们允许说，如果你的某个线程池突然需要大量的线程，最多可以到100个线程。

如果你使用了100个线程，高峰期过去了，自动将空闲的线程给释放掉。

（1）coreSize

设置线程池的大小，默认是10。

```html
HystrixThreadPoolProperties.Setter()
   .withCoreSize(int value)
```

（2）maximumSize

设置线程池的最大大小，只有在设置allowMaximumSizeToDivergeFromCoreSize的时候才能生效。

默认是10

```html
HystrixThreadPoolProperties.Setter()
   .withMaximumSize(int value)
```

（3）keepAliveTimeMinutes

设置保持存活的时间，单位是分钟，默认是1。

如果设置allowMaximumSizeToDivergeFromCoreSize为true，那么coreSize就不等于maxSize，此时线程池大小是可以动态调整的，可以获取新的线程，也可以释放一些线程。

如果coreSize < maxSize，那么这个参数就设置了一个线程多长时间空闲之后，就会被释放掉。

```html
HystrixThreadPoolProperties.Setter()
   .withKeepAliveTimeMinutes(int value)
```

（4）allowMaximumSizeToDivergeFromCoreSize

允许线程池大小自动动态调整，设置为true之后，maxSize就生效了，此时如果一开始是coreSize个线程，随着并发量上来，那么就会自动获取新的线程，但是如果线程在keepAliveTimeMinutes内空闲，就会被自动释放掉。

默认是false。

```html
HystrixThreadPoolProperties.Setter()
   .withAllowMaximumSizeToDivergeFromCoreSize(boolean value)
```

生产环境中，这块怎么玩儿的。

也是根据你的服务的实际的运行的情况切看的，比如说你发现某个服务，平时3个并发QPS就够了，高峰期可能要到30个。

那么你就可以给设置弹性的资源调度。

因为你可能一个服务会有多个线程池，你要计算好，每个线程池的最大的大小加起来不能过大，30个依赖，30个线程池，每个线程池最大给到30,900个线程，很坑的。

### 26.hystrix的metric统计相关的各种高阶配置

1、为什么需要监控与报警？

HystrixCommand执行的时候，会生成一些执行耗时等方面的统计信息。这些信息对于系统的运维来说，是很有帮助的，因为我们通过这些统计信息可以看到整个系统是怎么运行的。hystrix对每个command key都会提供一份metric，而且是秒级统计粒度的。

这些统计信息，无论是单独看，还是聚合起来看，都是很有用的。如果将一个请求中的多个command的统计信息拿出来单独查看，包括耗时的统计，对debug系统是很有帮助的。聚合起来的metric对于系统层面的行为来说，是很有帮助的，很适合做报警或者报表。hystrix dashboard就很适合。

2.hystrix的事件类型

对于hystrix command来说，只会返回一个值，execute只有一个event type，fallback也只有一个event type，那么返回一个SUCCESS就代表着命令执行的结束。

对于hystrix observable command来说，多个值可能被返回，所以emit event代表一个value被返回，success代表成功，failure代表异常。

（1）execute event type

EMIT observable command返回一个value SUCCESS 完成执行，并且没有报错 FAILURE 执行时抛出了一个异常，会触发fallback TIMEOUT 开始执行了，但是在指定时间内没有完成执行，会触发fallback BAD_REQUEST 执行的时候抛出了一个HystrixBadRequestException SHORT_CIRCUITED 短路器打开了，触发fallback THREAD_POOL_REJECTED 线程成的容量满了，被reject，触发fallback SEMAPHORE_REJECTED 信号量的容量满了，被reject，触发fallback

（2）fallback event type

FALLBACK_EMIT observable command，fallback value被返回了 FALLBACK_SUCCESS fallback逻辑执行没有报错 FALLBACK_FAILURE fallback逻辑抛出了异常，会报错 FALLBACK_REJECTION fallback的信号量容量满了，fallback不执行，报错 FALLBACK_MISSING fallback没有实现，会报错

（3）其他的event type

EXCEPTION_THROWN command生命自周期是否抛出了异常 RESPONSE_FROM_CACHE command是否在cache中查找到了结果 COLLAPSED command是否是一个合并batch中的一个

（4）thread pool event type

EXECUTED 线程池有空间，允许command去执行了 REJECTED 线程池没有空间，不允许command执行，reject掉了

（5）collapser event type

BATCH_EXECUTED collapser合并了一个batch，并且执行了其中的command ADDED_TO_BATCH command加入了一个collapser batch RESPONSE_FROM_CACHE 没有加入batch，而是直接取了request cache中的数据

3.metric storage

metric被生成之后，就会按照一段时间来存储，存储了一段时间的数据才会推送到其他系统中，比如hystrix dashboard。

另外一种方式，就是每次生成metric就实时推送metric流到其他地方，但是这样的话，会给系统带来很大的压力。

hystrix的方式是将metric写入一个内存中的数据结构中，在一段时间之后就可以查询到。

hystrix 1.5x之后，采取的是为每个command key都生成一个start event和completion event流，而且可以订阅这个流。每个thread pool key也是一样的，包括每个collapser key也是一样的。

每个command的event是发送给一个线程安全的RxJava中的rx.Subject，因为是线程安全的，所以不需要进行线程同步。

因此每个command级别的，threadpool级别的，每个collapser级别的，event都会发送到对应的RxJava的rx.Subject对象中。这些rx.Subject对象接着就会被暴露出Observable接口，可以被订阅。

4、metric统计相关的配置

（1）metrics.rollingStats.timeInMilliseconds

设置统计的rolling window，单位是毫秒，hystrix只会维持这段时间内的metric供短路器统计使用。

这个属性是不允许热修改的，默认值是10000，就是10秒钟。

```html
HystrixCommandProperties.Setter()
   .withMetricsRollingStatisticalWindowInMilliseconds(int value)
```

（2）metrics.rollingStats.numBuckets

该属性设置每个滑动窗口被拆分成多少个bucket，而且滑动窗口对这个参数必须可以整除，同样不允许热修改。

默认值是10，也就是说，每秒钟是一个bucket。

随着时间的滚动，比如又过了一秒钟，那么最久的一秒钟的bucket就会被丢弃，然后新的一秒的bucket会被创建。

```html
HystrixCommandProperties.Setter()
   .withMetricsRollingStatisticalWindowBuckets(int value)
```

（3）metrics.rollingPercentile.enabled

控制是否追踪请求耗时，以及通过百分比方式来统计，默认是true。

```html
HystrixCommandProperties.Setter()
   .withMetricsRollingPercentileEnabled(boolean value)
```

（4）metrics.rollingPercentile.timeInMilliseconds

设置rolling window被持久化保存的时间，这样才能计算一些请求耗时的百分比，默认是60000，60s，不允许热修改。

相当于是一个大的rolling window，专门用于计算请求执行耗时的百分比。

```html
HystrixCommandProperties.Setter()
   .withMetricsRollingPercentileWindowInMilliseconds(int value)
```

（5）metrics.rollingPercentile.numBuckets

设置rolling percentile window被拆分成的bucket数量，上面那个参数除以这个参数必须能够整除，不允许热修改。

默认值是6，也就是每10s被拆分成一个bucket。

```html
HystrixCommandProperties.Setter()
   .withMetricsRollingPercentileWindowBuckets(int value)
```

（6）metrics.rollingPercentile.bucketSize

设置每个bucket的请求执行次数被保存的最大数量，如果再一个bucket内，执行次数超过了这个值，那么就会重新覆盖从bucket的开始再写。

举例来说，如果bucket size设置为100，而且每个bucket代表一个10秒钟的窗口，但是在这个bucket内发生了500次请求执行，那么这个bucket内仅仅会保留100次执行。

如果调大这个参数，就会提升需要耗费的内存，来存储相关的统计值，不允许热修改。

默认值是100。

```html
HystrixCommandProperties.Setter()
   .withMetricsRollingPercentileBucketSize(int value)
```

（7）metrics.healthSnapshot.intervalInMilliseconds

控制成功和失败的百分比计算，与影响短路器之间的等待时间，默认值是500毫秒。

```html
HystrixCommandProperties.Setter()
   .withMetricsHealthSnapshotIntervalInMilliseconds(int value)
```



## Spring Cloud Zuul

### 1.Zuul的概念和特点

Zuul是Netflix开源的微服务网关，和Eureka、Ribbon、Hystrix等组件配合使用完成服务网关的动态路由、负载均衡等功能。其核心特点如下。

( 1 ）资源审查：对每个请求都进行资源验证审查，拒绝非法请求。

( 2 ）身份认证：到每个请求的用户都进行身份认证，拒绝非法用户，身份认证一般基于HTTP消息头完成。

( 3 ）资源监控：通过对有意义的数据进行追踪和请求统计，为分析生产环境中接口的调用状态和用户的行为提供依据。

( 4 ）动态路由：对外提供统一的网关服务，动态地将不同类型的请求路由到不同的后端集群，实现对外提供统一的网关服务和对内进行有效的服务拆分。

( 5 ）压力测试：通过配置设置不同集群的负载流量，预估集群的性能。

( 6 ）负载均衡：为每一种负载类型都分配对应的容量，针对不同的请求做更细粒度的负载均衡，并弃用超出限定值的请求，进行服务保护。

( 7 ）多区域弹性：跨越区域进行请求路由，旨在实现ELB( Elastic Load Balance，弹性负载均衡）使用的多样化，并保证边缘位置与使用者尽可能接近。

### 2.Zuul的原理

Zuul的核心是通过系列Filter将整个HTTP请求过程连成一系列操作来实现对HTTP请求的控制。Zuul提供了一个对Filter进行动态地加载、编译和运行的框架。Zuul的各个Filter之间不进行直接通信，而是通过一个RequestContext静态类来进行数据的传递，每个Web请求所需要传递的参数都通过ThreadLocal变量来记录。

Zuul Filter有filterType（）、filterOrder（）、shouldFilter（）、run()4种核心方法。

- filterType（）：用以表示路由过程中的阶段（内置包含PRE、ROUTING、POST和ERROR
- filterOrder（）：表示相同Type的Filter的执行顺序
- shouldFilter（）：表示Filter的执行条件
- run() ：表示Filter具体要执行的业务逻辑

Zuul定义了4种Filter Type，这些Filter Type分别对应请求的不同生命周期。

( 1 ) PRE Filter: PRE Filter在请求被路由之前调用。一般用于实现身份验证、资源审查、记录调试信息等。

( 2) ROUTING Filter: ROUTING Filter将请求路由到微服务实例，该Filter用于构建发送给微服务实例的请求，并使用Apache HTTPClient或Netflix Ribbon请求微服务实例。

( 3) POST Filter: POST Filter一般用来为响应添加标准的HTTP Header、收集统计信息和指标，以及将响应从微服务发送给到客户端等，该Filter在将请求路由到微服务实例以后被执行。

( 4) ERROR Filter：在其他阶段发生错误时执行ERROR Filter。

### 3.Zuul的使用

Zuul主要用来构建微服务网关，核心的使用包括定义路由和定义Filter。具体过程如下：

( 1) pom.xml添加依赖

在pom.xml中加入Zuul依赖，其中，spring-cloud-starter-netflix -eureka-client为微服务发现所需的依赖，spring-cloud-starter-netflix-zuul为Zuul网关服务所需的依赖。

( 2) 通过＠EnableZuulProxy注解开启对服务网关的支持，Zuul一般和服务发现配合使用，这里使用@EnableEurekaClient开启对服务发现客户端的支持。

( 3) application.properties配置。

配置服务名称、端口，以及需要连接的服务注册中心的地址，这里注册中心使用Eureka服务。路由配置的核心zuul.routes.routel .path=/proxy／**和zuul.routes.routel.serviceld=EUREKA-CLIENT表示定义了一个名为routel的网关路由，所有以proxy开始的请求都转发到EUREKA-CLIENT服务实例上。

( 4）路由转发验证。

分别启动eurekaserver（注册中心）、eurekaclient（服务生产者）、zuul（网关服务），在浏览器地址栏中输入localhost:9006/proxy/serviceProducer/，可以看到端口9006上的网关服务路由到端口9002的EUREKA-CLIENT服务上了。

( 5 ）忽略指定微服务配置

```html
zuul.ignored-services:/apil/**,xx-service
```

上述配置表示以api开头和xx-service的请求不参与路由转发

（6）指定Path和URL配置

```html
zuul. routes. route-name. url= http://localhost:9002/ 
zuul. routes. route-name. path= /api/＊＊
```

上述配置将[http://ZUULHOST:ZUULPORT/api/开头的请求映射到http](http://zuulhost/api/开头的请求映射到http) ://localhost: 9002/，由于不是用service-id定位服务的，因此无法使用负载均衡功能。

### 4.PreRequestFilter的定义和注入

Zuul的Filter使用包括Filter的定义和注入两步，具体如下：

(1）定义PreRequestFilter。

PreRequestFilter在请求被路由之前调用，一般用于实现身份验证和资源审查等。

通过继承ZuulFilter实现了一个Zuul Filter，该Filter的类型为PRE_Type,表示在路由之前被执行，在run（）方法体中对用户名进行验证，如果用户名合法则通过，如果不合法则抛出异常，程序逻辑转到Error流程。

( 2 ）注入PreRequetFilter。

要想PreRequestAuthFilter生效还需要将Filter注入Zuul，具体实现通过在ZuulApplication中定义Bean实例即可。

```html
@Bean 
public PreRequestAuthFilter preRequestAuthFilter () { 
	return new PreRequestAuthFilter(); 
}
```

### 5.Fallback Provider的服务容错

应用程序在调用第三方的服务时难免会出现错误，但是，应用程序通常不会把错误直接抛给用户，而是根据业务定义不同的错误消息，以便用户分析和排查问题，同时可将其作为全局异常处理方案，具体如下。

（ 1 ）定义DefaultFallbackProvider。

```html
public class DefaultFallbackProvider implements FallbackProvider
```

（2 ）注入DefaultFallbackProvider

```html
@Bean 
public Default FallbackProvider defaultFallbackProvider () { 	return new DefaultFallbackProvider();
}
```

## Spring Cloud链路监控

### 1.Spring Cloud的链路监控

微服务架构是一个按业务划分服务实例的分布式架构，一个分布式系统往往有几十个甚至上百个服务实例。由于服务实例数量众多，随着业务复杂性增加，一个请求可能需要调用很多个服务才能完成业务操作，最终形成很长的调用链。而服务的调用又分内部服务间调用、跨部门服务调用和外部服务调用，一旦线上出了问题，很难快速定位，在微服务架构中，一般通过分布式链路追踪来记录一个请求有哪些服务参与、参与的顺序，以及每个服务的耗时情况，从而达到每个请求的步骤都清晰可见、出现问题都能够及时定位的目的。

目前，比较流行的链路追踪组件有Google的Dapper、Apache的Sleuth、Twitter的Zipkin、NAVER的Pinpoint和阿里巴巴的EagleEye等，它们都是非常优秀的链路追踪开源组件。

### 2.Sleuth+Zipkin

Spring Cloud Sleuth是Spring Cloud的组成部分之一，为Spring Cloud应用提供一种分布式追踪解决方案，同时结合Zipkin 、HTrace和Log采集服务实现分布式链路追踪和展示。

Sleuth的介绍

Sleuth主要依赖Span、Trace和Annotation实现分布式链路追踪。

( 1 ) Span: Span是基本的工作单元，包括一个64位的唯一id、一个64位的Trace码、服务调用的描述信息、时间戳事件、Key-Value注解（Tags）和Span处理者的id（通常为IP）等信息。最初的Span被称为根Span，该Span种的Spanld与TraceId值相同。

( 2 ) Trance: Trance由一系列Span组成，这些Span组成一个树型结构。

( 3 ) Annotation ：用于及时记录存在的事件。常用的Annotation如下。

- cs-Client Sent：客户端发送一个请求到服务端，作为请求的开始，也即Span的开始。
- sr-Server Receive ：服务端接收到客户端的请求并开始处理该请求。sr和cs之间的时间间隔为网络的延迟时间。
- ss-Server Sent：服务端处理完客户端的请求，开始向客户端返回处理结果。ss和sr之间的时间间隔表示服务端处理请求的执行时间。
- er-Client Received ：客户端接收到服务端返回的结果，至此请求结束。er和sr之间的时间间隔表示客户端接收服务端的数据所使用的时间。

Sleuth+Zipkin实现分布式链路追踪

Sleuth为分布式链路追踪提供了数据结构基础，但是在使用过程中往往需要一个可视化的集中式的链路数据的存储和展示系统，方便快速定位问题，Zipkin为应用程序提供了该功能。

1）构建Zipkin Server服务，用于日志的收集

( 1 ) pom.xml添加依赖

在pom.xml中加入Zipkin依赖，其中，spring-cloud-starter-netflix -eureka-cIient为服务发现所需的依赖，zipkin-server为Zipkin服务中心所需的依赖，zipkin-autoconfigure-ui为监控中心WebUI所需的依赖

( 2 ）通过@EnableZipkinServer注解开启对Zipkin服务端的支持。注意，Zipkin一般和服务发现配合使用，这里使用@EnableEurekaClient开启对服务发现客户端的支持。

( 3 ) application. properties配置

配置服务名称、端口，以及需要连接的服务注册中心的地址，这里注册中心使用Eureka服务。

2）构建Sleuth+Zipkin客户端服务，用于日志的上报

( 1) pom.xml添加依赖。

在pom.xml中加入Zipkin依赖，其中，spring-cIoud-starter-netflix -eureka-cIient为服务发现所需的依赖，spring-cloud-starter-zipkin为Zipkin客户端所需的依赖，spring-cloud-starter-sleuth为Sleuth所需的依赖

( 2) Zipkin客户端和Sleuth的实现只需要添加依赖的JAR包即可，在入口服务上不需要注解，但若要实现基于整个分布式微服务的监控，则需要实现服务注册，这里使用@EnableEurekaClient开启对服务发现客户端的支持。

( 3 ) application.properties配置。

配置服务名称、端口，以及要连接的服务注册中心的地址，这里注册中心使用Eureka服务。另外一个核心的配置就是spring.zipkin.base-url，它用于配置将日志汇聚到哪个Zipkin Server上存储和展示，spring.sleuth.sampler. percentage表示监控抽样比例，1表示全部请求都监控，0.1表示只对其中的10%做抽样监控，该参数在线上应用时一般会设置得较小，以尽量减小链路监控对服务器资源的压力。

( 4 ）定义服务接口

( 5 ）基于Zipkin Dashboard进行查询

Zipkin Dashboard的实现只需要在Zipkin Server的pom.xml种加入zipkin-autoconfigure-ui依赖便可。

3）Zipkin的数据持久化

在默认情况下，Zipkin存储记录到内存，如果服务重启，则所有记录都丢失。为了保证持久性，Zipkin支持MySQL、ElasticSearch、Cassandra存储。

( 1 ) pom.xm添加依赖

在zipkin_dashboard项目中加入zipkin-autoconfigure-storage-mysql 依赖，使Zipkin支持MySQL的持久化，同时需要加入MySQL的驱动。

( 2) application.properties配置

加入MySQL的数据库配置，并通过zipkin.storage. type=mysq配置Zipkin的持久化类型为mysql。

( 3 ）数据库验证

重启服务，查看数据库，可以看到Zipkin自动执行数据库建库脚本zipkin.sql，并生成了数据库，数据库建库脚本通过spring.datasource.schema配置。这样就完成了数据库配置，所有Spans信息都存储在数据库中，即使重启Zipkin Server服务，也不会丢失记录。

### 3.Pinpoint

Pinpoint的介绍

Pinpoint是一款无侵入式的全链路监控分析工具，基于字节码增强技术实现了调用链监控、方法执行详情查看和应用状态信息监控等功能。Pinpoint基于Google Dapper实现，与Zipkin（一款开源的全链路分析工具）功能类似，与Zipkin最大的不同是，Pinpoint具有无侵入式的、代码维度的监控特性。Pinpoint功能丰富，以下为常用的几种功能

( 1 ）服务拓扑图：将系统中服务组件的依赖和调用关系进行可视化展示，单击服务节点，可以查看该节点的详细信息（例如当前节点状态、请求数量等）。

( 2 ）实时活跃线程图：监控服务组件中正在运行的线程的活跃情况，常用于线程性能分析。

( 3 ）响应散点图：将单位时间内的请求数和响应时间以时间轴的方式可视化展示，拖动图表可以查看更详细的请求执行情况。

( 4 ）调用栈查看：为每个请求都提供了代码维度的调用栈查看，可以在页面中查看每个请求对应的代码维度的执行详情信息，用于快速定位问题。

( 5 ）服务状态、机器状态检查：该功能用于实时统计和显示服务组件对应的系统资源及进程执行等信息，比如CPU使用率、内存使用率、系统负载、垃圾收集状态、TPS和JVM信息等。

Pinpoint的组件

Pinpoint主要由3个组件和HBase数据库组成，3个组件分别为Agent、Collector和Web UI，它们的功能分别为数据的收集、数据的存储和数据的展示，HBase为数据的持久化数据库。

( 1 ) Agent：收集应用程序的监控数据，Agent无侵入式，只需要在应用程序的启动命令中加入部分参数即可。

( 2 ) Collector：数据收集模块，接收Agent上报的监控数据，并将监控数据存储到HBase。

( 3) Web UI：链路监控展示模块，用于查看系统的拓扑图、实时活跃线程图、调用栈、服务状态和机器状态等，同时支持报警功能。

Pinpoint的数据结构

在Pinpoint种，核心数据结构由Span、Trace、Traceld组成。

( 1) Span: RPC跟踪的基本单元，当一个RPC调用到达时，表明RPC工作已经处理完成，并在返回值中包含了跟踪数据。为了确保代码级别的可见性，Span用带SpanEvent标签的子结构作为数据结构。每个Span都包含一个Traceld。

( 2) Trace：多个 Span的集合，由关联RPC的一系列Span组成，同一个Trace中的Span共享相同的TransactionId。Trace通过SpanId和ParentSpanld将调用栈整理为树结构

( 3 ) Traceld：由Transactionld、SpanId、ParentSpanld组成的Key的集合，Transactionld指明消息的id、SpanId和ParentSpanld表明RPC的子父调用关系。

- Transactionld ( Txld）： 在分布式系统间实现事务唯一性的标识
- SpanId ：当收到RPC消息时处理请求的工作id，在RPC请求到达节点后生成。
- ParentSpanld ( pSpanld）： 发起RPC调用的父Span的Spanld，如果节点是事务的起点，则将没有父Span。对于这种情况，使用-1来表示这个Span是事务的根Span。

Pinpoint的字节码增强技术

Pinpoint基于字节码增强技术（又叫动态探针技术）实现无侵入式的调用链数据采集。

其核心实现基于JVM的JavaAgent机制来实现。应用在启动时通过设置JavaAgent来指定PinpointAgent的加载路径。

在启动后，Pinpoint Agent采用字节码增强技术在加载应用Class文件之前拦截并修改字节码，在Class的方法调用的前后加上链路数据采集逻辑，从而实现链路采集功能。

在Pinpoint中，API拦截部分和数据记录部分是分开的，拦截器被注入应用程序想要追踪的方法中，并调用before（）和after（）方法来处理数据记录。通过字节码增强，Pinpoint Agent能够只从必要的方法中记录数据，这使得分析数据的大小变得紧凑。同时，应用程序不需要修改任何代码就能做到全链路监控，对应用十分友好。

Pinpoint的使用

Pinpoint的使用包括pinpoint-collector安装、pinpoint-web安装、pinpoint-agent的配置和应用。

当应用程序需要通过Pinpoint实现全链路监控时，不需要修改应用程序的任何代码，只需要在启动的时候加上以上三个参数即可。其中，“-javaagent:$AGENT_PATH/pinpoint-bootstrap-$VERSION .jar”表示要通过Pinpoint动态字节码增强技术实现动态探针和全链路监控，agentld表示唯一标识该应用的id，applicationName表示服务名称。

## Spring Cloud Gateway

### 1.API 网关

API Gateway是一个服务器，也可以说是进入系统的唯一节点。这跟面向对象设计模式中的Facade模式很像。API Gateway封装内部系统的架构，并且提供API给各个客户端。它还可能有其他功能，如授权、监控、负载均衡、缓存、请求分片和管理、静态响应处理等。

API Gateway负责请求转发、合成和协议转换。所有来自客户端的请求都要先经过API Gateway，然后路由这些请求到对应的微服务。API Gateway将经常通过调用多个微服务来处理一个请求以及聚合多个服务的结果。它可以在web协议与内部使用的非Web友好型协议间进行转换，如HTTP协议、WebSocket协议。

请求转发

服务转发主要是对客户端的请求安装微服务的负载转发到不同的服务上。

响应合并

把业务上需要调用多个服务接口才能完成的工作合并成一次调用对外统一提供服务。

协议转换

重点是支持SOAP，JMS，Rest间的协议转换。

数据转换

重点是支持XML和Json之间的报文格式转换能力（可选）

安全认证

1.基于Token的客户端访问控制和安全策略。

2.传输数据和报文加密，到服务端解密，需要在客户端有独立的SDK代理包。

3.基于Https的传输加密，客户端和服务端数字证书支持。

4.基于OAuth2.0的服务安全认证(授权码，客户端，密码模式等）

### 2.配置中心

配置中心一般用作系统的参数配置，它需要满足如下几个要求：高效获取、实时感知、分布式访问。

zookeeper 配置中心

采取数据加载到内存方式解决高效获取的问题，借助zookeeper 的节点监听机制来实现实时感知。

## Spring Cloud Sleuth

### 1.服务跟踪（starter-sleuth）

随着微服务数量不断增长，需要跟踪一个请求从一个微服务到下一个微服务的传播过程，Spring Cloud Sleuth 正是解决这个问题，它在日志中引入唯一ID，以保证微服务调用之间的一致性，这样你就能跟踪某个请求是如何从一个微服务传递到下一个。

1.为了实现请求跟踪，当请求发送到分布式系统的入口端点时，只需要服务跟踪框架为该请求创建一个唯一的跟踪标识，同时在分布式系统内部流转的时候，框架始终保持传递该唯一标识，直到返回给请求方为止，这个唯一标识就是前文中提到的Trace ID。通过Trace ID的记录，我们就能将所有请求过程日志关联起来。

2.为了统计各处理单元的时间延迟，当请求达到各个服务组件时，或是处理逻辑到达某个状态时，也通过一个唯一标识来标记它的开始、具体过程以及结束，该标识就是我们前文中提到的Span ID，对于每个Span来说，它必须有开始和结束两个节点，通过记录开始Span和结束Span的时间戳，就能统计出该Span的时间延迟，除了时间戳记录之外，它还可以包含一些其他元数据，比如：事件名称、请求信息等。

3.在快速入门示例中，我们轻松实现了日志级别的跟踪信息接入，这完全归功于spring-cloud�starter-sleuth组件的实现。在Spring Boot应用中，通过在工程中引入spring-cloud-starter-sleuth依赖之后，它会自动的为当前应用构建起各通信通道的跟踪机制，比如：

- 通过诸如RabbitMQ、Kafka（或者其他任何Spring Cloud Stream绑定器实现的消息中间件）传递的请求。
- 通过Zuul代理传递的请求。
- 通过RestTemplate发起的请求。