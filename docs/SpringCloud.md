# Spring Cloud

## 1.核心组件

- **Eureka**：各个服务启动时，Eureka Client都会将服务注册到Eureka Server，并且Eureka Client还可以反过来从Eureka Server拉取注册表，从而知道其他服务在哪里.
  - **Eureka **Client：**负责将这个服务的信息注册到Eureka Server中.
  - **Eureka Server：**注册中心，里面有一个注册表，保存了各个服务所在的机器和端口号.
- **Ribbon**：服务间发起请求的时候，基于Ribbon做负载均衡，从一个服务的多台机器中选择一台.
  - 作用是负载均衡，会帮你在每次请求时选择一台机器，均匀的把请求分发到各个机器上
  - Ribbon的负载均衡默认使用的最经典的Round Robin轮询算法。这是啥？简单来说，就是如果订单服务对库存服务发起10次请求，那就先让你请求第1台机器、然后是第2台机器、第3台机器、第4台机器、第5台机器，接着再来—个循环，第1台机器、第2台机器。。。以此类推。
- **Feign**：基于Feign的动态代理机制，根据注解和选择的机器，拼接请求URL地址，发起请求.
  - 首先，如果你对某个接口定义了@FeignClient注解，Feign就会针对这个接口创建一个动态代理.
  - 接着你要是调用那个接口，本质就是会调用 Feign创建的动态代理，这是核心中的核心.
  - Feign的动态代理会根据你在接口上的@RequestMapping等注解，来动态构造出你要请求的服务的地址.
  - 最后针对这个地址，发起请求、解析响应.
- **Hystrix**：发起请求是通过Hystrix的线程池来走的，不同的服务走不同的线程池，实现了不同服务调用的隔离，避免了服务雪崩的问题.
- **Zuul**：如果前端、移动端要调用后端系统，统一从Zuul网关进入，由Zuul网关转发请求给对应的服务.

**此外，Ribbon是和Feign以及Eureka紧密协作，完成工作的，具体如下：**

- 首先Ribbon会从 Eureka Client里获取到对应的服务注册表，也就知道了所有的服务都部署在了哪些机器上，在监听哪些端口号。
- 然后Ribbon就可以使用默认的Round Robin算法，从中选择一台机器.
- Feign就会针对这台机器，构造并发起请求。

![](D:\workspace\Java-Interview-Dictionary\images\springcloud001.webp)

