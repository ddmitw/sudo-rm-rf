## 1.服务网关

### 1.1 基本介绍

* Spring Cloud Gateway

`Spring Cloud Gateway`是基于`Spring`生态系统之上构建的`API`网关，包括：`Spring 5.x`，`Spring Boot 2.x`和`Project Reactor`。`Spring Cloud Gateway`旨在提供一种简单而有效的方法来路由到`API`，并为它们提供跨领域的关注点，例如：`安全性`、`监视/指标`、`限流`等。

* 什么是服务网关

`API Gateway(APIGW/API网关)`，顾名思义，是系统对外的唯一入口。`API`网关封装了系统内部架构，为每个客户端提供定制的API。 近几年来移动应用与企业间互联需求的兴起。从以前单一的Web应用，扩展到多种使用场景，且每种使用场景对后台服务的要求都不尽相同。 这不仅增加了后台服务的响应量，还增加了后台服务的复杂性。随着微服务架构概念的提出，API网关成为了微服务架构的一个标配组件。

* 为什么要使用网关

微服务的应用可能部署在不同机房、不同地区、不同域名下。此时客户端（浏览器/手机/软件工具）想要请求对应的服务，都需要知道机器的具体IP或者域名URL，当微服务实例众多时，这是非常难以记忆的，对于客户端来说也太复杂难以维护。此时就有了网关，客户端相关的请求直接发送到网关，由网关根据请求标识解析判断出具体的微服务地址，再把请求转发到微服务实例。这其中的记忆功能就全部交由网关来操作了。

* 核心概念

`路由(Route)`: 路由是网关最基础的部分，路由信息由ID、目标URI、一组断言和一组过滤器组成。如果断言路由为真，则说明请求的URI和配置匹配。
`断言(Predicate)`: Java 8中的断言函数。Spring Cloud Gateway中的断言函数输入类型是Spring 5.0框架中的ServerWebExchange。Spring Cloud Gateway中的断言函数允许开发者去定义匹配来自于Http Request中的任何信息，比如请求头和参数等。
`过滤器(Filter)`: 一个标准的Spring Web Filter。Spring Cloud Gateway中的Filter分为两种类型，分别是Gateway Filter和Global Filter。过滤器将会对请求和响应进行处理。

### 1.2 使用网关

* 添加依赖

```
  <!-- spring cloud gateway 依赖 -->
  <dependency>
  	<groupId>org.springframework.cloud</groupId>
  	<artifactId>spring-cloud-starter-gateway</artifactId>
  </dependency>
```


* 配置文件`resources/application.yml`

```
  server:
    port: 8080
  
  spring: 
    application:
      name: ruoyi-gateway
    cloud:
      gateway:
        routes:
          # 系统模块
          - id: ruoyi-system
            uri: http://localhost:9201/
            predicates:
              - Path=/system/**
            filters:
              - StripPrefix=1
```
  
  
  
* 网关启动类

```
@SpringBootApplication
public class RuoYiGatewayApplication
{
    public static void main(String[] args)
    {
        SpringApplication.run(RuoYiGatewayApplication.class, args);
        System.out.println("(♥◠‿◠)ﾉﾞ  若依网关启动成功   ლ(´ڡ`ლ)ﾞ ");
    }
}
```
  
## 2.Eureka支撑高并发、高性能的原理

* Eureka通过设置适当的请求频率(拉取注册表30秒间隔，发送心跳30秒间隔)，可以保证一个大规模的系统每秒请求Eureka Server的次数在几百次;
* 同时通过纯内存的注册表，保证了所有的请求都可以在内存处理，确保了极高的性能;
* 多级缓存机制(只读缓存、读写缓存、实际注册表)，确保了不会针对内存数据结构发生频繁的读写并发冲突操作，进一步提升性能;

参考：

1. [eureka如何应对高并发？](https://blog.csdn.net/Sunxn1991/article/details/108393582)；
2. [Eureka支撑高并发、高性能的原理](https://whb1990.github.io/posts/2d8525f4.html);

##  3.分布式事务



[分布式事务，这一篇就够了](https://xiaomi-info.github.io/2020/01/02/distributed-transaction/)；

## 4.微服务与分布式的区别

微服务是将原来臃肿的项目拆分为多个模块，是对服务的拆分；分布式是将服务分别部署在不同的机器上；微服务和分布式作用的目标不同。

参考：

1. [深度解析springcloud分布式微服务的实现](http://ifeve.com/%E6%B7%B1%E5%BA%A6%E8%A7%A3%E6%9E%90springcloud%E5%88%86%E5%B8%83%E5%BC%8F%E5%BE%AE%E6%9C%8D%E5%8A%A1%E7%9A%84%E5%AE%9E%E7%8E%B0/) ；

