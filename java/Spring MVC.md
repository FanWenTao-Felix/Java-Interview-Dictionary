# Spring MVC

### 1.Spring MVC的原理

Spring MVC中的MVC即模型-视图-控制器，该框架围绕一个DispatcherServlet设计，DipatcherServlet会把请求分发给各个处理器，并支持可配置的处理器映射和视图渲染等功能，spring mvc的工作流程如下所示：

( 1 ）客户端发起HTTP 请求： 客户端将请求提交到DispatcherServlet 。 ( 2 ）寻找处理器：由DispatcherServlet 控制器查询一个或多个HandlerMapping ，找到处理该请求的Controller 。 ( 3 ）调用处理器： DispatcherServlet 将请求提交到Controller 。 ( 4 ）调用业务处理逻辑并返回结果： Controller 调用业务处理逻辑后，返回ModelAndView 。 ( 5 ）处理视图映射并返回模型： DispatcherServlet 查询一个或多个ViewResoler 视图解析器，找到ModelAndView 指定的视图。 ( 6 ）HTTP 响应： 视图负责将结果在客户端浏览器上渲染和展示。

### 2.Spring MVC注解

@Controller：声明该类为SpringMVC中的控制器

@RequestMapping：用于声明映射Web请求的地址和参数，包括访问路径和参数

@ResponseBody：支持将返问值放在Response Body体中返问，通常用于返回JSON数据到前端

@RequestBody ：允许Request的参数在Request Body体中

@Path Variable ：用于接收基于路径的参数，通常作为RESTful接口的实现

@RestController：组合注解，相当于＠Controller和@ResponseBody 的组合

@ExceptionHandler：用于全局控制器的异常处理

@InitBinder：WebDataBinder用来自动绑定前台请求的参数到模型（Model）