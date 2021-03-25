# Spring Boot

## 1.Spring Boot

Spring Boot是由Pivotal团队开发的全新的Spring开发框架，其设计的初衷是简化Spring应用复杂的搭建及开发过程。该框架提供了一套简单的Spring模块依赖和管理工具，从而避免了开发人员处理复杂的模块依赖和版本冲突问题，同时提供打包即可用的Web服务，成为快速应用开发领域（Rapid Application Development ）的领导者

Spring Boot的特点如下。

( l ）快速创建独立的Spring应用程序

( 2 ）嵌入Tomcat和Undertow等Web容器，实现快速部署。

( 3 ）自动配置JAR包依赖和版本控制，简化Maven配置

( 4 ）自动装配Spring实例，不需要XML配置

( 5 ）提供诸如性能指标、健康检查、外部配置等线上监控和配置功能

## 2.Spring Boot的使用

Spring Boot把传统的Spring项目从繁杂的XML配置中解放出来，应用只需要用注解自动扫描即可，同时SprinBoot为应用提供了统一的JAR管理和维护，不需要应用程序管理复杂的JAR依赖和处理多版本冲突问题，只需要在pom.xml文件中加入对应模块的Starter即可。对内部的JAR依赖的管理，SpringBoot会自动维护。具体使用过程如下。

( 1) Spring Boot的引入。

Spring Boot项目定义简单，使用方便，第一步需要在pom.xml文件中引入org.springframework.boot及相关依赖。

```html
<!-- 继承spring boot的父工程 -->
<!-- spring boot父工程直接约束了常用依赖的版本 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>
<!-- 添加一个支持web应用的spring boot依赖 -->
<!-- spring boot会根据我们引入的依赖，判断出来我们要开发一个web工程 -->
<!-- 接着会对web工程需要的东西，比如tomcat服务器，自动进行配置 -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
<!-- 引入一个spring boot插件，可以支持我们打包程序 -->
<!-- 打包时是需要将所有依赖的第三方jar包都打进来的，spring boot这个插件可以支持 -->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>

```

( 2）配置文件设置.

Spring Boot的配置分为application.properties和application.yml两种，两种配置有语法差别，但其实现的功能相同。下面的配置文件通过server.port=9090设置了服务端口为9090，如果不设置，则默认端口为Tomcat的8080，通过server.name=hello设置了服务名称为hello

( 3 ）定义启动类。

启动类是SpringBoot项目的入口，应用程序通过在类上设置一个@SpringBootApplication注解，声明该类是一个SpringBoot启动类，SpringBoot会扫描启动类所在的包及其子包中的所有类的注解，并将其加载到SpringBoot的容器中进行管理。只需要在main（）函数中执行SpringApplication.run(SpringbootApplication.class, args), 便完成了启动的定义。代码如下

```html
@SprigBootApplication
public class SpringbootApplication{ 
    public static void main ( String[] args) {      
        SpringApplication.run(SpringbootApplication.class, args); 
    }
}
```

在定义启动类后，单击右键执行run，便可启动该SpringBoot项目。

( 4）定义控制器.

在SpringbootApplication的根目录定义一个控制器，用于Web接口的访问。控制器的定义方式和Spring项目中控制器的常规定义方式一样，具体代码如下。

```html
@RestController 
public class BaseController { 
    @RequestMapping(”/hello ”) 
    public String home() { 
        return "Hello World !”
    }
}
```

( 5 ）项目启动和访问。

在SpringbootApplication上单击右键执行run，便可启动该SpringBoot服务，在浏览器地址栏中输入127.0.0.1:9090/hello，便能访问定义好的REST服务。

( 6 ) 将spring boot应用打包成可以直接执行的jar包

因为我们之前使用了spring-boot-maven-plugin插件，所以可以直接打包，就会包含依赖的所有第三方jar包，成为一个可以执行的jar包

mvn pakcage

然后执行这个jar包：java -jar target\springboot-demo-1.0.0.jar

## 3.spring-boot-starter-parent的作用

（1）默认的编译级别是JDK 1.6

（2）指定了UTF-8为source coding

（3）有一个<dependencyManagement>声明，对spring boot可以集成的大部分流行第三方依赖，都指定了对应的可以互相兼容的版本

（4）开启了资源过滤器，支持对应资源文件中占位符的替换

（5）指定了多个插件：exec、surefire、git commit id、shade，用来支持运行、测试、版本控制、打包

### 不直接继承spring-boot-starter-parent

如果要继承自己的parent工程，而不是直接继承spring-boot-starter-parent，那么可以考虑使用import方式，将spring-boot-start-parent中的dependency management引入进来。但是这样的话，就没法用到plugin management了

```html
<dependencyManagement>
     <dependencies>
        <dependency>
            <!-- Import dependency management from Spring Boot -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>1.5.9.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 自己指定JDK版本

```html
<properties>
    <java.version>1.8</java.version>
</properties>
```

### 使用spring boot的打包插件

spring-boot-starter-parent指定了pluginManagement，配置好了一系列的插件，其中一个就是用于打成可执行jar包的shade插件。如果我们要使用的话，需要自己手动声明这个插件，然后通过mvn pakcage命令就可以将工程打包成可以执行的jar包了。

```html
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### spring-boot-starter-*

spring-boot提供了一些列的starter类依赖，跟流行的常用项目进行整合，比如mybatis、redis、mongodb、elasticsearch，等等。我们只要声明对应的spring-boot-starter-*，就可以直接使用对应版本的依赖，多个依赖的版本都是兼容的。可能就是通过application.properties，注解，少量的配置，就可以快速整合进来一个技术开始使用。

## 4.Configuration类

spring boot的一个核心思想，就是尽量消除掉各种复杂的xml配置文件，所以一般建议如果要进行一些配置，可以采用Configuration类，在Java类中做一些配置。

而且通常比较好的实践是，就将用来放main方法的Application类，作为Configuration类，所以一般会给Application类加@Configuration注解。

如果不想将所有的配置都放在Application类中，也可以使用@Import注解将其他的Configuration类引入进来，或者是依靠@ComponentScan注解自动扫描其他的Configuration类。

即使一定要使用一个xml配置文件，建议是用@ImportResource来导入一个xml配置文件。

## 5.Auto Configuration

（1）什么是Auto Configuration

Auto Configuration是spring boot中非常核心的一个功能，因为spring boot的一个核心思想，就是尽可能减少我们要配置的东西，尽量才有约定规则自动完成一些配置。这个Auto Configuration功能，就会根据我们引入的依赖，来推测我们做什么事情，然后自动给我们完成需要的一些配置。举个例子，如果依赖了HSQLDB，那么spring boot会基于HSQLDB自动配置和创建一个内存数据库。

（2）如何启用Auto Configuration

我们给@Configuration类，增加了@EnableAutoConfiguration注解之后，就会开启Auto Configuration功能，而且一般推荐使用使用@EnableAutoConfiguration开启Auto Configuration功能。

## 6.与Spring核心框架进行整合使用

spring boot可以无缝与spring框架进行整合使用，一般就是在Application类上加@ComponentScan注解，启用自动扫描所有的spring bean，同时搭配好@Autowired注解来进行自动装配。只要按照上面的那个约定，将Application类放在最顶层的包结构中，那么使用了@ComponentScan之后，就可以自动扫描和搜索到所有的spring bean，包括了：@Component、@Contorller、@Service、@Repository。

## 7.@SpringBootApplication

大多数的spring boot项目，都会在main类上标注：@Configuration、@EnableAutoConfiguration、@ComponantScan，因为这个实在是太过于常见了，所以spring boot提供了一个@SpringBootApplication注解，这个注解就相当于是上面3个注解的综合。所以实际上在开发的时候，一般就会在main类上加一个@SpringBootApplication注解

## 8.devtools工具

### devtools工具介绍

spring boot为了贯彻简化开发过程的原则，提供了一个devtools工具，基于这个工具，可以在开发的各个环节，尽可能地简化工作

```html
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

如果我们启动一个打好包的spring boot应用程序，这个应用程序会被认为是生产应用，那么此时是不会让devtools生效的。而且一般推荐将optional设置为true，这样如果我们的项目被别人依赖，devtools是不会传递过去的。

### 让系统在代码变化的时候自动重启（常见于本地开发和调试）

如果我们的应用程序使用了devtools，此时如果在IDE中直接启动，则会认为是开发调试过程。则此时如果classpath中的任何一个文件变化了，就会自动重启应用程序。在IDE中启动程序的时候，这个功能时很有用的，因为我们启动了一个web程序之后，会随时调试和修改代码，代码修改之后，不需要手动重启，web程序自己就重启了 。

但是上面介绍中也说了，打成一个jar包，java -jar启动一个jar包，对于执行jar包的情况下，spring boot会认为这是生产环境，则不会启用devtools的任何功能，包括这个自动重启功能。

devtools使用了两个classloader来加载类，一个是base classloader来加载第三方依赖的类；一个是restart classloader来加载我们自己编写的文件。如果我们自己编写的文件改动了，则此时会重新创建一个restart classloader来重新加载我们的类，这个速度是很快的，因为base classloader是始终保持的，不需要重新加载第三方依赖的类。

如果要排除掉某些资源文件的变动不要自动重启，那么可以进行如下的配置，application.properties

```html
spring.devtools.restart.exclude=static/**,public/**
```

如果要彻底禁用自动重启功能，需要做如下配置，application.properties

```html
spring.devtools.restart.enabled=false
```

## 9.启动失败的处理

如果启动失败了，那么失败的异常会交给spring boot预先注册好的某个FailureAnalyzer来处理，这个FailureAnalyzer就会打印出完整的失败原因以及解决的办法。

spring boot内置了多个FailureAnalyzer，分别用于处理不同的启动失败问题

此外，我们还可以在启动应用程序的时候，打开debug开关，这样，即使没有一个FailureAnalyzer可以处理启动失败，也会打印出完整的auto configuration信息，供我们参考，比如下面这样

```html
$ java -jar myproject-0.0.1-SNAPSHOT.jar --debug
```

## 10.ApplicationRunner / CommandLineRunner

如果要在SpringApplication.run()开始运行，但是完成应用启动之前，同时并行运行某些代码，比如一些系统初始化的工作，那么可以用ApplicationRunner / CommandLineRunner，两者唯一的区别，就是ApplicationRunner会传递进来ApplicationArguments，CommandLineRunner会传递进来数组。

```html
import org.springframework.boot.*
import org.springframework.stereotype.*
@Component
public class MyBean implements CommandLineRunner {
    public void run(String... args) {
        // Do something...
    }
}
```

## 11.框架整合

### 1、Spring Boot + Spring Core

Spring Boot默认原生就是支持Spring Core的，只要在main类上加了@ComponantScan，就会自动去扫描各种spring bean，同时基于@Autowired可以完成自动装配。

### 2、Spring Boot + Spring MVC + Spring Core

如果要在Spring Boot中使用Spring MVC，就意味着你肯定是要开发web应用了，那么就在pom.xml中声明对spring-boot-starter-web的依赖，可以让spring boot自动配置好内置tomcat容器，同时支持spring mvc接收http请求。

接着可以在包目录中创建你的Controller，对Controller而言，一般会使用@RestController注解，因为现在大多数都是前后端彻底分离的架构，很少再有Controller接收请求之后再去渲染视图的了，只要提供RESTful接口即可。

Controller可以基于@Autowired来装配后面的@Service业务逻辑组件。

### 3、Spring Boot + Spring MVC + Spring Core + MyBatis

spring boot和mybatis整合是基于mybatis提供的mybatis-spring-boot-starter，基于近乎0配置的方式，完成spring boot和mybatis的整合。

mybatis-spring-boot-starter的工作原理如下。

（1）自动发现一个注册好的DataSource：applicationContext.xml手动配置，不用了 -> @Configuration作用替代掉以前的xml配置文件。

（2）自动创建一个SqlSessionFactory，并且将DataSource传入SqlSessionFactory中。

（3）自动基于SqlSessionFactory创建一个SqlSessionTemplate。

（4）扫描所有的Mapper，将SqlSessionTemplate注入其中，然后将Mapper注册到Spring容器上下文中。

### 4、Spring Boot + Spring MVC + Spring Core + MyBatis + Druid

1.在pom.xml中引入需要的依赖

2.在application.properties配置文件中配置druid连接池

3.编写Druid连接池配置管理类

4.编写Domain类

5.编写Mapper类

6.编写Service类

7.编写controller类

（1）系统启动的时候，首先会去扫描DruidDBConfig类，这就可以将外部的druid连接池配置加载进来，同时初始化出来一个druid连接池的DataSource bean

（2）mybatis-spring-boot-starter接着开始干活儿，发现了一个DataSource bean，就会将其注入SqlSessionFactory，再创建SqlSessionTemplate，接着扫描Mapper接口，将SqlSessionTemplate注入每个Mapper，然后将Mapper放入spring容器中来管理

（3）spring boot的@ComponantScan注解开始干活儿，自动扫描所有的bean，依次初始化实例以及注入依赖，EmployeeServiceImpl（将EmployeeMapper注入其中），EmployeeCongtroller（将EmployeeServiceImpl注入其中）

（4）此时浏览器发送请求，会先由controlller处理，接着调用service，再调用mapper。mapper底层会基于druid连接池访问到数据库，进行数据操作

## 11.spring-boot-starter-web介绍

spring boot崇尚的就是尽可能减少手动配置，尽量基于约定规则来完成所有的配置，同时尽量基于注解来完成配置。只要在依赖中引入spring-boot-starter-web，其实spring boot就会基于自己的Auto Configuration功能，完成web服务器和spring mvc的自动配置，然后我们上手就可以开发web程序，直接运行main类就可以启动内置的web服务器了。这能让我们快速启动web程序的开发、调试和验证。

## 12.RESTful风格

### RESTful中的HTTP method

POST：代表的是新增

PUT：代表的是修改

GET：代表的是查询

PATCH：代表的是更新部分属性

DELETE：代表的是删除

HEAD：获取头信息，要查找的信息在响应头中

OPTIONS：获取URI支持的方法，在响应头中会有Allow，包含了支持的方法

### RESTful中的HTTP status

200，成功

400，错误的请求，比如请求参数匹配错误

404，没找到这个资源

405，使用的HTTP method不被允许

500，服务器内部错误

### spring boot支持RESTful接口

只要依赖了spring-boot-starter-web，就自动支持RESTful接口。

### 系统中调用RESTful接口

spring boot提供了一个RestTemplate来辅助发起REST请求，底层通过HttpURLConnection来发送HTTP消息，使用Jackson来进行JSON序列化

```html
@Controller
@RequestMapping("/test")
public class TestController {
    
    @Autowired
    RestTemplateBuilder restTemplateBuilder;
    
    @GetMapping("/user/{id}")
    public User testGetUserById(@PathVariable("id") Long id) {
        RestTemplate client = restTemplateBuilder.build();
        String uri = "http://localhost:8080/api/v1.0/user/{id}";  
        return client.getForObject(uri, User.class, id);
    }    
}
```

### @RestController注解

RESTful风格的Controller中，直接用这个注解即可，对每个方法的返回值默认采用@ResponseBody的方式来处理。

不会去找某个视图资源文件来渲染后发回浏览器，因为@RestController就代表了这是一个负责提供RESTful接口的Controller。

### @RequestMapping注解

（1）一般在Controller类级别加一个@RequestMapping注解，代表对一类资源的映射

（2）在Controller中每个方法级别加一个@RequestMapping，支持某一类请求

### @PathVariable注解

```html
// 这个功能在RESTful接口设计中很常见
// 就是在请求中附带一个id或者别的什么参数，代表了对一个资源的标识和定位
// 然后在方法入参中使用@PathVariable注解配置请求中参数和方法入参的映射关系
@RequestMapping(value = "/{userId}", method = HttpMethod.GET)
public User getUserById(@PathVariable("userId") Long userId) {
}
```

### @XMapping系列注解

如果是做REStful风格的Controller，其实一般就更加简化，使用@GetMapping，@PutMapping，@PostMapping，@DeleteMapping。来代表了增删改查等一些操作和请求即可。

### 将请求参数映射到方法入参

使用@RequestParam可以将某个请求参数，映射到方法入参上

```html
// 假设浏览器发送过来了一个user_status=1的请求参数
// 可以将其映射到方法入参userStatus上去
@GetMapping("/{userId}")
public User getUserById(@PathVariable("userId") Long userId, 
                        @RequestParam("user_status") Integer userStatus) {
    
}
```

### @ResponseBody

就是将方法的返回值直接用json等格式返回到浏览器，一般我们现在都直接用@RestController了，自带这个功能

## 13.Spring Boot对请求校验的支持

Spring Boot支持JSR-303验证框架，默认实现是Hibernate Validator，只要在Java Bean上放一些校验注解，就可以实现校验支持。

常用的校验注解包括下面这些：

（1）空检查：@Null，@NotNull，@NotBlank，@NotEmpty

（2）长度检查：@Size(min=20, max=50)，@Length

（3）数值检查：@Min，@Max，@Range(min=1, max=99)

（4）其他检查：@Email，@Pattern

而且这个校验是支持group的概念的，对于不同的group生效的校验不一样。这个很有用，因为对于增删改查等不同的操作，需要执行的校验本来就是不一样的。

## 14.自定义校验注解

```html
public class AgeValidator implements ConstraintValidator<Age, Integer> {
    private Age age;
    private Integer max;
    
    public void initialize(Age age) {
        this.age = age;
        this.max = age.max();
    }
    
    public boolean isValid(Integer value, ConstraintValidatorContext context) {
        return value < max;
    }    
}
@Constraint(validatedBy = {AgeValidator.class})
@Documented
@Target({ElementType.ANNOTATION_TYPE, ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolicty.RUNTIME)
public @interface Age {
    
    String message() default "年龄是非法的，不能超过{max}岁";
    int max() default 100;
    Class<?>[] groups default {};
    Class<? extends Payload>[] payload() default {};    
}
```

## 15.spring boot+mvc中常用的一些全局配置

### WebMvcConfigurer

```html
@Configuration
public class MvcConfigurer implements WebMvcConfigurer {
    
    // 拦截器
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new UserLoginInterceptor()).addPathPatterns("/user/**");
    }
    
    // 跨域访问配置
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/user/**")
            .allowedOrigins("http://www.zhss.com")
            .allowedMethods("POST", "GET")
    }
    
    // 格式化
    public void addFormatters(FormatterRegistry registry) {
        registry.addFormatter(new DateFormatter("yyyy-MM-dd HH:mm:ss"));
    }
    
    // URI到视图的映射
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/addUserUI").setViewName("/addUser.jsp");
    }    
} 
public class UserLoginInterceptor implements HandlerInterceptor {
    
    public boolean preHandle(HttpServletRequest request, 
                             HttpServletResponse response, Object handler) 
                             throws Exception {        
    }
    
    public void postHandle(HttpServletRequest request, 
                           HttpServletResponse response, 
                           Object handler, 
                           ModelAndView moodelAndView)
                           throws Exception {
    
    }
    
    public void afterCompletion(HttpServletRequest request, 
                                HttpServletResponse response, 
                                Object handler, 
                                Exception ex)
                                throws Exception {
    
    }    
}
```

### spring监听器的使用

```html
public class MyApplicationStartedEventListener implements ApplicationListener<ApplicationStartedEvent> {
    @Override
    public void onApplicationEvent(ApplicationStartedEvent event) {
    }
}  
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class); 
        app.addListeners(new MyApplicationStartedEventListener());
        app.run(args);
    }
}
```

### 全局异常处理

Controller中抛出的异常全部交给/error来处理，Spring Boot是有一个默认实现的，但是也可以我们自己覆盖掉

```html
@ControllerAdvice
class GlobalErrorHandler {
    @ExceptionHandler(value = Exception.class)
    @ResponseBody
    public ErrorInfo<String> defaultErrorHandler(HttpServletRequest req, Exception e) throws Exception {
        ErrorInfo<String> r = new ErrorInfo<String>();  
        r.setMessage(e.getMessage());
        r.setCode(ErrorInfo.ERROR);
        r.setData("自定义的全局异常处理");
        r.setUrl(req.getRequestURL().toString());
        return r;
    }
}
public class ErrorInfo<T> {
    public static final Integer OK = 0;
    public static final Integer ERROR = 100;
    private Integer code;
    private String message;
    private String url;
    private T data;    
}        
```

## 16.spring boot对事务的支持

```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency> 
@Transactional
public void updateUser(User user) {
}
```

## 17.系统相关配置

### Spring Boot的基础配置

在application.properties中即可完成spring boot的配置。

1.1 监听端口配置

默认的监听端口是8080，但是可以用如下三种方式来修改监听的端口

在application.properties中：server.port=9090

启动系统的时候：java -jar target\springboot-demo-1.0.0.jar --server.port=9090

启动系统的时候：java -Dserver.port=9090 -jar target\springboot-demo-1.0.0.jar

1.2 web上下文配置

默认的web上下文是：/，可以通过属性来修改web上下文

server.context-path=/springboot-demo

1.3 使用其他web服务器

默认的web服务器是用的内嵌的tomcat，可以使用jetty或者是undertow

比如使用jetty作为web服务器

```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>     
```

但是一般来说，都是用tomcat作为web容器即可，较为重要的配置参数如下

```html
# 打开tomcat访问日志
server.tomcat.accesslog.enabled=true
# 访问日志所在的目录
server.tomcat.accesslog.directory=logs
# 允许HTTP请求缓存到请求队列的最大个数，默认是不限制的
server.tomcat.accept-count=
# 最大连接数，默认是不限制的，如果连接数达到了上限，那么剩下的连接就会保存到请求缓存队列里，也就是上面参数指定的个数
server.tomcat.max-connections=
# 最大工作线程数
server.tomcat.max-threads=
# HTTP POST内容最大长度，默认不限制
server.tomcat.max-http-post-size=
```

### spring组件加载外部配置文件

在实际开发中，将核心配置放在外部配置文件，是很重要的。因为通常我们都是对不同的环境使用一套完全不同的配置，但是保证我们的代码必须是相同的。

比如我们有一个类，其中要从外部配置文件中注入一个name属性

```html
import org.springframework.stereotype.*
import org.springframework.beans.factory.annotation.*
// 无论用什么spring组件都可以被注入外部配置
@Component
public class MyBean {
    // 用@Value注解就可以将外部配置注入到类中来
    // 对配置项的名称用${}形式来表达
    @Value("${name}")
    private String name;
    // ...
} 
```

然后通常，我们会在src/main/resources中，放置一个application.properties，其中可以给name属性一个值。也可以在命令行中传递这个参数值来替代：java -jar app.jar --name="Spring"。

而且spring boot还提供了一个随机属性值生成器，供我们使用，可以对某个属性是用的随机生成的一个值：

```html
my.secret=${random.value}
my.number=${random.int}
my.bignumber=${random.long}
my.uuid=${random.uuid}
my.number.less.than.ten=${random.int(10)}
my.number.in.range=${random.int[1024,65536]} 
```

当然，也可以用application.yml来替代application.properties，用yaml格式来编写配置文件，但是这两种方式都可以，一般没必要搞的那么复杂，用application.properties也就可以了

### spring boot多环境支持

spring boot支持使用@Profile注解来标志，在哪个环境profile下，可以激活使用某个@Configuration类。

```html
@Configuration
@Profile("production")
public class ProductionConfiguration {
    // ...
} 
```

接着可以在启动的时候，命令行中，使用如下参数来指定某个环境profile：--spring.profiles.active=dev,hsqldb

但是实际上如果基于spring boot的profile支持来做，一般是没法完全满足我们的期望的，所以多环境profile支持，通常还是基于彻底的maven profile来使用，不同的profile直接对应不同的文件夹，然后mvn pakcage打包的时候，指定对应的profile来打包，将对应环境的配置文件，全部放到src/main/resources下面去。

这种方式是最彻底的，而且足够灵活。

### 系统读取应用的配置

（1）Environment类

spring boot提供了一个Environment类，可以读取applicaiton.properties、命令行参数、系统参数、操作系统环境变量，等等，各种参数和配置。可以通过spring bean注入的方式获取到这个Environment，很多时候，可能你并不想直接使用@Value将某些参数读取到系统里面来，但是你可以注入一个Environment，从这个里面去获取需要的一些参数

```html
@Configuration
public class EnvironmentConfig {
    @Autowired
    private Environment env;
    
    public int getServerPort() {
        return env.getProperty("server.port", Integer.class);
    }    
} 
```

（2）@Value

也可以通过@Value直接将外部的配置参数注入到配置管理类中

这块可以参考我们之前写的那个DruidDbConfig类，就是基于@Value将外部的druid配置参数注入进来，然后基于配置参数初始化了一个Druid连接池bean

（3）@ConfigurationProperties

如果有一组相关联的配置属性，比如说下面这样的：

```html
server.port=9090
server.context-path=/springboot-demo 
```

都是server打头的，那么可以一次性将一组配置属性读取到一个配置管理类中

```html
@ConfigurationProperties("server")
@Configuration
public class ServerConfig {
    
    private int port;
    private String contextPath;
    
    // port和contextPath的getter和setter方法    
} 
```

可以使用这个@ConfigurationProperties来重构dDruidDbConfig类

### spring boot的自动装配

@Configuration和@Bean

spring boot的核心思想，就是用@Configuration来标注一个类，从外部配置文件中，基于@Value或者是@ConfigurationProperties来加载各种配置信息进来，然后基于这些配置信息，实例化对应的bean

## 18.spring-boot-starter-test

spring boot对单元测试提供了很好的支持，只要依赖spring-boot-starter-test即可，这个依赖会自动导入单元测试需要的所有依赖，包括了JUnit、AssertJ、Hamcrest以及其他的一些包，而且这个spring-boot-starter-test一般是设置为test scope即可

具体来说，spring-boot-starter-test导入之后，会包含下面这些东西：

（1）JUnit：最经典的单元测试框架

（2）Spring Test、Spring Boot Test：是spring和spring boot环境下，对测试的一个支持

（3）AssertJ：是用来进行断言的

（4）Hamcrest：是用来进行复杂断言，复杂的表达式

（5）Mockito：测试替身的模拟

（6）JSONassert：都是对json数据进行操作的

（7）JsonPath

spring boot单元测试脚手架

```html
// @RunWith的意思，是不要使用默认方式进行单元测试，而是使用指定的类来提供单元测试
// 所有的spring测试都是找SpringRunner.class
@RunWith(SpringRunner.class)
// 这个是spring boot提供的，会一直找到一个Application类，只要包含了@SpringBootApplication的就算，然后会先启动这个类，来给单元测试提供环境
@SpringBootTest
public class EmployeeServiceTest {
    
    // 这里就可以从启动的spring上下文中，将EmployeeService注入到这里来，供我们进行测试
    @Autowired
    private EmployeeService employeeService;
    
    @Test
    public void testFindById() {
        
    }
}    
```

### 测试service组件

要考虑的地方有3点：

（1）测试之前自动构造好数据，测试结束之后自动回滚数据构造

（2）将service依赖的service进行模拟打桩进来

（3）可能需要在数据库中构造好数据

spring boot会默认在单元测试结束之后，进行数据库事务回滚；spring boot会集成mockito框架来模拟依赖service进行打桩；spring boot有一个@Sql注解，可以在测试开始前执行SQL语句初始化数据

```html
import static org.mockito.BDDMockito.given;
import static org.mockito.Mockito.*;
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
@Rollback(true)
public class EmployeeServiceTest {
    
    @Autowired
    private EmployeeService employeeService;
    
    // 这里是基于Mockito框架模拟出来了一个EmployeeMapper
    // 可能我们到这里为止，只想要测试EmployeeService，而EmployeeService依赖了EmployeeMapper，那么我们就需要自己模拟一个EmployeeMapper出来
    // 这里定义的@MockBean，会模拟生成一个EmployeeMapper，放入spring容器中去，然后被EmployeeService给引用到
    @MockBean
    private EmployeeMapper employeeMapper;
        
    @Test
    public void testService() {
        int employeeId = 1;
        
        Employee employee = new Employee();
        employee.setId(1);
        employee.setName("张三");
        employee.setAge(20);
        
        // 这行代码是对employeeMapper的findById方法进行模拟
        // 无论给这个方法传入什么参数
        // 都会返回一个Employee对象，这个是我们预先定义好的employee对象
        given(this.employeeMapper.findById(anyLong()))
                .willReturn(employee);
        
        Employee resultEmployee = employeeService.findById(employeeId);
        
        assertEquals(employee, resultEmployee);
    }    
}
```

### 测试Controller

```html
@RunWith(SpringRunner.class)
// 这个代表是Mvc测试，可以传入要测试的Controller类
@WebMvcTest(EmployeeController.class)
public class EmployeeControllerTest {
    
    // 这个是专门用来测试mvc的类，可以模拟发起http请求
    @Autowired
    private MockMvc mvc;
    // 这里是模拟以一个service组件
    @MockBean
    private EmployeeService employeeService;
    
    @Test
    public void testMvc() throws Exception {
        int employeeId = 1;
        
        // 这里是模拟service组件的行为
        Employee employee = new Employee();
        employee.setId(1);
        employee.setName("张三");
        employee.setAge(20);
        
        given(this.employeeService.findById(employeeId)).willReturn(employee);
        
        // 这里是模拟发起一个http请求
        mvc.perform(get("/employee/{id}", employeeId))
                .andExpect(model().attribute("id", 1))
                .andExpect(model().attribute("name", "张三"))
                .andExpect(model().attribute("age", 20));
    }
    
}
```

### DAO层单元测试

@Sql

可以使用@Sql先构造好需要的数据

```html
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
@Rollback(true)
public class EmployeeMapperTest {
    
    @Autowired
    private EmployeeMapper employeeMapper;
    
    @Test
    // 这个@Sql注解，就会在我们执行测试之前，先执行sql语句，初始化数据
    @Sql({"employee.sql"})
    public void testAddEmployee() {
        Employee employee = new Employee();
        employee.setName("李四");
        employee.setAge(30);
        
        Long employeeId = employeeService.add(employee);
        employee.setId(employeeId);
        assertTrue(employeeId > 0);
        
        // 接着需要从数据库中查询数据来比较
        Employee resultEmployee = employeeMapper.findById(1);
        assertEquals(employee, resultEmployee);
    }
    
}
```

## 19.日志

### 日志打印

默认情况下，spring boot使用apache commons logging作为日志接口，但是使用logback作为日志框架实现，所以打印日志的代码一般是下面这样的：

```html
public class HelloworldController {
    
    private Log log = LogFactory.getLog(HelloworldController.class);
}    
```

### 日志格式

（1）日期和时间

（2）日志级别：ERROR，WARN，INFO，DEBUG，TRACE

（3）进程ID

（4）---，这是个分隔符

（5）线程名称

（6）日志内容

### 日志级别

默认是INFO以上的日志才会打印，可以在application.properties中配置一下

```html
# 配置打印日志的默认最小级别
logging.level.root=info
# 配置org包下打印日志的最小级别是WARN
# org包下的一般都是第三方的组件，不需要打印info级别的日志
logging.level.org=warn
# 自己包下的日志，从debug级别就开始打印
logging.level.com.zhss=debug
```

### 日志文件

同时默认仅仅在控制台打印日志，如果要打印日志到文件，需要进行配置

```html
# 这是相对路径，放在应用的目录下
logging.file=my.log
# 当然一般其实是将日志文件放在指定的某个日志目录中的
# 这就会在/var/logs/com.zhss.springboot.demo目录下生成一个spring.log日志文件
logging.path=/var/logs/com.zhss.springboot.demo 
```

当日志达到10MB的时候，就会重新生成一个日志文件，避免单个日志文件过大

### 自定义日志格式

可以对控制台和文件两个地方的日志分别定义格式

```html
logging.pattern.console=%level %date{HH:mm:ss} %logger{20}.%M %L :%m%n
logging.pattern.file=%level %date{ISO8601} [%thread] %logger{2000}.%M %L :%m%n 
```

- %level，日志级别
- %date，日期时间，HH:mm:ss是输出时分秒，ISO8601是标准日期输出，yyyy-MM-dd HH:mm:ss.SSS
- %logger，输出Logger的名字，包名+类名，{n}限制了输出的长度，如果太长的话，会尽可能压缩包名和类名，但是建议一般保持的长一些
- %thread，线程名称
- %M，日志打印的方法名
- %L，打印日志所在的行，性能消耗较大
- %m，日志消息
- %n，换行

spring boot虽然支持多种日志框架，比如log4j2，java util logging等，但是一般建议就使用默认的logback即可，可以在src/main/resources目录下放一个logback-spring.xml来对logback进行更加细致的配置。

## 20.将系统部署到线上服务器

### 以jar方式来部署

如果要以jar方式来部署spring boot应用，只需要加入下面的插件即可

```html
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <executable>true</executable>
            </configuration>
        </plugin>
    </plugins>
</build> 
```

接着只要执行：mvn package，即可完成打包，这是一个fatjar包，包含了所有依赖，可以执行的一个jar包

接着可以将jar包通过scp命令上传到服务器上

执行jar包启动系统：java -jar target\springboot-demo-1.0.0.jar

### 以war包方式来部署

一般来说，实际去部署的时候，都不会是以jar包方式部署的，因为那是用的spring boot内嵌的tomcat服务器来部署的，一般还是以war包方式，部署到线上已有的tomcat容器中的

此时要做的第一件事情，就是将spring boot应用的pom.xml中的packaging从jar修改为war

```html
<groupId>com.zhss.springboot</groupId>
<artifactId>springboot-demo</artifactId>
<packaging>war</packaging> 
```

接着需要将spring boot内嵌的tomcat修改为provided范围

```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```

接着要再次修改Application类

```html
// 这里必须继承SpringBootServletInitializer基类
@SpringBootApplication
public class Application extends SpringBootServletInitializer {
    
    // 这里必须实现一个configure方法
    @Override
    protected SpringAppliciationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }
    
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
    
}
```

然后再次使用mvn package打包，就是一个war包

然后将war包放到tomcat的webapps目录中，启动tomcat，即可完成系统的部署

### 多环境部署

可以在src/main/resources目录下，放多个application-{profile}.properties文件

然后在启动的时候，使用java -jar -Dspring.profiles.active=prod target\springboot-demo-1.0.0.jar来指定要激活哪个环境的profile

在实际的项目中，你永远无法保证说，你多环境下，就是一个properties就可以了，我告诉大家一点，很多大公司里，自己研发的设施，是要求你不同的环境必须放不同的一个文件，让他们去读取的，跟spring boot是不兼容的。META-INF/conf.properties

我们用的是maven profile，我们在不同profile的路径下，放一个application.properites，以及也许需要放置的其他的一些配置文件，为了以防万一

也可以在tomcat的catalina.sh中，编辑系统属性来激活对应的profile，比如JAVA_OPTS="-Dspring.profiles.active=prod"

还可以使用@Profile来标准@Configuration，来决定不同的环境下，可以让不同的类生效

## 21.线上应用系统监控

在pom.xml中引入依赖：

```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency> 
```

此时打包工程，通过java -jar方式启动工程

访问下面的地址，可以看到应用的健康情况：

```html
// 查看系统的基本健康信息：系统是否正常，磁盘空间，数据库
http://localhost:8080/springboot-demo/health
// 查看最近的一些http请求
http://localhost:8080/springboot-demo/trace
// 在线查看日志
http://localhost:8080/springboot-demo/logfile
// 在线查看线程栈信息
http://localhost:8080/springboot-demo/dump
// 在线查看内存快照
http://localhost:8080/springboot-demo/heapdump
// 查看系统的性能指标：内存占用、垃圾回收次数
http://localhost:8080/springboot-demo/metrics
// 查看系统的一些额外信息
http://localhost:8080/springboot-demo/info
// 在线查看日志配置
http://localhost:8080/springboot-demo/loggers
// 查看URL映射
http://localhost:8080/springboot-demo/mappings
// 查看所有的spring bean
http://localhost:8080/springboot-demo/beans
// 查看环境变量
http://localhost:8080/springboot-demo/env
// 查看configpropss环境变量
http://localhost:8080/springboot-demo/configprops
// 查看auto config的一些信息
http://localhost:8080/springboot-demo/autoconfig
```



## 22.Spring Boot的常用组件及其使用

Spring Boot的核心特点是通过Starter能快速将各个组件集成到应用中，并提供良好的操作接口。

### Spring Boot （使用MySQL）

Spring Boot基于Starter能够快速将不同的服务组件集成到应用程序中。SpringBoot 服务组件的集成过程分为引入Starter、设置application.properties和使用服务组件（组件会根据配置文件自动装配）3步。MySQL具体使用如下。

( 1 ）引入Starter

```html
<dependency> 
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jdbc</artifactId> 
</dependency> 
```

( 2 ）设置application.properties

```html
spring.datasource.url=jdbc:mysql//localhost/test＃数据库地址
spring.datasource.username=dbuser＃数据库用户名
spring.datasource.password=dbpass＃数据库密码
spring.datasource.driver-class-name=com.mysql.jdbc.Driver＃数据库驱动
```

( 3 ）使用服务组件

```html
@Component 
public class MyBean { 
	private final JdbcTemplate jdbcTemplate; 
	@Autowired 
	public MyBean(JdbcTemplate jdbcTemplate){ 
		this.jdbcTemplate =jdbcTemplate;
	}
}
```

### Spring Boot 使用Redis

( 1 ）引入Starter。

```html
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId>spring-boot-starter-data-redis</artifactId> 
</dependency> 
```

( 2）设置appication.properties

```html
#Redis数据库名称（默认为0)
spring.redis.database=O
#Redis数据库地址
spring.redis.host=l72.31.19.222 
#Redis数据库端口
spring.redis.port=6379
#Redis数据库密码（默认为空〉
sping.redis.password=
#Redis连接池的最大连接数（使用负值表示没有限制）
spring.redis.pool.max-active=8
#Redis连接池的最大阻塞等待时间（使用负值表示没有限制）
spring.redis.pool.max-wait=-1
#Redis连接池中的最大空闲连接
spring.redis.pool.max-idle=8
#Redis连接池中的最小空闲连接
spring.redis.pool.min-idle=O
```

( 3 ）使用服务组件

```html
@Component 
public class MyBean { 
	private StringRedisTemplate template; 
	@Autowired 
	public MyBean(StringRedisTemplate template) { 
		this.template= template; 
	}
}
```

### Spring Boot 使用MongoDB

( 1 ）引入Starter

```html
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId> spring-boot-starter-data-mongodb</artifactId> </dependency> 
```

( 2）设置application.properties

```html
spring.data.mongodb.uri＝mongodb//user:secret@mongol.example.com:l2345,mongo2.example.com:23456/test 
＃数据库的连接地址
```

( 3 ）使用服务组件

```html
@Component 
public class MyBean { 
	private MongoTemplate template; 
	@Autowired 
	public MyBean(MongoTemplate template) { 
		this.template = template; 
	}
}
```

### SpingBoot 使用Neo4j

( 1 ）引入Starter

```html
<dependency> 
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-neo4j</artifactId> 
</dependency> 
```

( 2）设置application.properties

```html
spring.data.neo4j.uri＝bolt://my-server:7687#Neo4j图数据库地址spring.data.neo4j.username=neo4j #Neo4j图数据库用户名spring.data.neo4j.password=secret #Neo4j图数据库密码
```

( 3 ）使用服务组件

```html
@Component
public class MyBean { 
	private final Session session; 
	@Autowired 
	public MyBean(Session session) { 
		this.session = session;
	}
}
```

### Spring Boot 使用Solr

( 1 ）引入Starter

```html
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId>spring-boot-starter-data-solr</artifactId> 
</dependency> 
```

( 2）设置application.properties

```html
#Solr数据库地址
spring.data.solr.host: http://127.0.0.1:8080/solr/criri_core 
```

( 3 ）使用服务组件

```html
@Component 
public class MyBean { 
	private SolrClient solr; 
	@Autowired 
	public MyBean(SolrClient solr) { 
		this.solr= solr; 
	}
}
```

### Spring Boot 使用ElasticSearch

( 1 )引入Starter

```html
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId> spring-boot-starter-data-elasticsearch</artifactId> </dependency> 
```

( 2）设置application.properties

```html
#ElasticSearch数据库地址
spring.data.elasticsearch.cluster-nodes=localhost:9300 
```

( 3 ）使用服务组件

```html
@Component 
public class MyBean { 
	private final ElasticsearchTemplate template; 
	public MyBean(ElasticsearchTemplate template) { 
		this.template = template; 
	}
}
```

### Spring Boot 使用Cassandra

( 1 ）引入starter

```html
<dependency> 
	<groupId>org.springframework.boot</groupId> 
	<artifactId> spring-boot-starter-data-cassandra</artifactId> </dependency> 
```

( 2）设置application.properties

```html
#Cassandra的命名空间
spring.data.cassandra.keyspace-name=mykeyspace 
#Cassandra数据库地址
spring.data.cassandra.contact-points=cassandrahostl,cassandrahost2 
```

( 3 ）使用服务组件

```html
@Component
public class MyBean { 
	private CassandraTemplate template; 
	@Autowired 
	public MyBean(CassandraTemplate template) { 
		this.template = template;
	}
}
```

### SpingBoot 使用RabbitMQ

( 1 ) 引入Starter

```html
<dependency> 
	<groupId>org.springframework.boot</groupId>
	<artifactId> spring-boot-starter-rabbitmq</artifactId> 
</dependency>
```

( 2）设置application.properties

```html
spring.rabbitmq.host=localhost #RabbitMQ服务地址
spring.rabbitmq.port=5672 #RabbitMQ端口号
spring.rabbitmq.username=admin #RabbitMQ用户名
spring.rabbitmq.password=secret #RabbitMQ密码
```

( 3 ）定义服务组件

```html
@Component 
public class MyBean { 
	private final AmqpAdmin amqpAdmin; 
	private final AmqpTemplate amqpTemplate; 
	@Autowired 
	public MyBean(AmqpAdmin amqpAdmin, AmqpTemplate amqpTemplate) { 	
		this.amqpAdmin = amqpAdmin;
		this.amqpTemplate = amqpTemplate;
	}
}
```

（4 ）定义队列

```html
@Configuration
public class QueueConf { 
	//1：定义Queue实例对象，队列名称为someQueue
	@Bean(name=”message”) 
	public Queue queueMessage() { 
		return new Queue (”someQueue”);
	}
}
```

( 5 ）发送消息

```html
@Component 
public class MyBeanSender { 
	@Autowired 
	private AmqpTemplate template; 
	public void send() { 
		//向队列someQueue发送一条消息hello,rabbit 
		template.convertAndSend(”someQueue”,”hello, rabbit”); 
	}
}
```

( 5）接收消息

```html
@Component 
public class MyBean { 
	//监听和接收队列someQueue上的消息
	@RabbitListener(queues=”someQueue”) 
	public void processMessage(String cotent){ 
	}
}
```

### Spring Boot 使用Kafka

( 1 ）引入Starter

```html
<dependency> 
    <groupId>org.springframework.boot</groupId> 
    <artifactId>spring-boot-starter-kafka </artifactId> 
</dependency>
```

( 2 ）设置application.propeties

```html
#Kafka服务地址
spring.kafka.bootstrap-servers=localhost:9092 
#Kafka消费组
spring.kafka.consumer.group-id=myGroup 
```

( 3 ）发送消息

```html
@Component 
public class MyBean｛
    private final KafkaTemplate kafkaTemplate; 
    @Autowired
    public MyBean(KafkaTemplate kafkaTemplate) { 
        this.kafkaTemplate = kafkaTemplate; 
    }
    public Response sendKafka() { 
        //向Kafka的someTopic发送一条消息
        kafkaTemplate.send (”someTopic”,”key”,message) ; 
    }
}
```

( 4）接收消息

```html
@Component 
public class MyBean { 
    //监听并接收someTopic上的消息
    @KafkaListener(topics =”someTopic”) 
    public void processMessage(String content) { 
        System.out.println(”message:”+ content) ;
    }
}
```