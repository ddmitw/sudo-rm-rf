## 1. 什么是Spring框架?

Spring是一种轻量级开发框架，旨在提高开发人员的开发效率以及系统的可维护性。

Spring官网：[https://spring.io/](https://spring.io/)。

我们一般说Spring框架指的都是Spring Framework，它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发。这些模块是：核心容器(Core Container)、数据访问/集成(Data Access/Integration)、Web、AOP（面向切面编程）、Aspects、工具、消息和测试模块。比如：Core Container中的Core组件是Spring所有组件的核心，Beans组件和Context组件是实现IOC和依赖注入的基础，AOP组件用来实现面向切面编程。

Spring官网列出的Spring的6个特征:

- **核心技术** ：依赖注入(DI)，AOP，事件(events)，资源，i18n，验证，数据绑定，类型转换，SpEL。
- **测试** ：模拟对象，TestContext框架，Spring MVC 测试，WebTestClient。
- **数据访问** ：事务，DAO支持，JDBC，ORM，编组XML。
- **Web支持** : Spring MVC和Spring WebFlux Web框架。
- **集成** ：远程处理，JMS，JCA，JMX，电子邮件，任务，调度，缓存。
- **语言** ：Kotlin，Groovy，动态语言。

## 2. 列举一些重要的Spring模块？

下图对应的是Spring4.x版本。目前最新的5.x版本中Web模块的Portlet组件已经被废弃掉，同时增加了用于异步响应式处理的WebFlux组件。

![Spring主要模块](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-6/Spring主要模块.png)

- **Spring Core：** 基础,可以说Spring其他所有的功能都需要依赖于该类库。主要提供IoC依赖注入功能。
- **Spring Aspects**： 该模块为与AspectJ的集成提供支持。
- **Spring AOP**：提供了面向切面的编程实现。
- **Spring JDBC**: Java数据库连接。
- **Spring JMS**：Java消息服务。
- **Spring ORM**: 用于支持Hibernate等ORM工具。
- **Spring Web**: 为创建Web应用程序提供支持。
- **Spring Test**: 提供了对JUnit和TestNG测试的支持。

## 3. @RestController vs @Controller

**`Controller`返回一个页面**

单独使用`@Controller`不加`@ResponseBody`的话一般使用在要返回一个视图的情况，这种情况属于比较传统的Spring MVC 的应用，对应于前后端不分离的情况。

![SpringMVC 传统工作流程](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-7/SpringMVC传统工作流程.png)

**`@RestController`返回JSON或XML形式数据**

但`@RestController`只返回对象，对象数据直接以JSON或XML形式写入HTTP响应(Response)中，这种情况属于RESTful Web服务，这也是目前日常开发所接触的最常用的情况（前后端分离）。

![SpringMVC+RestController](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-7/SpringMVCRestController.png)

**`@Controller+@ResponseBody`返回JSON或 XML形式数据**

如果你需要在Spring4之前开发RESTful Web服务的话，你需要使用`@Controller`并结合`@ResponseBody`注解，也就是说`@Controller`+`@ResponseBody`= `@RestController`（Spring 4 之后新加的注解）。

> `@ResponseBody`注解的作用是将`Controller`的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到HTTP响应(Response)对象的 body中，通常用来返回JSON或者XML数据，返回JSON数据的情况比较多。

![Spring3.xMVC RESTfulWeb服务工作流程](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-7/Spring3.xMVCRESTfulWeb服务工作流程.png)

Reference:

- https://dzone.com/articles/spring-framework-restcontroller-vs-controller （图片来源）
- https://javarevisited.blogspot.com/2017/08/difference-between-restcontroller-and-controller-annotations-spring-mvc-rest.html?m=1

## 4. Spring bean

### 4.1 Spring中的bean的作用域有哪些？

- singleton: 唯一bean实例，Spring中的bean默认都是单例的。
- prototype: 每次请求都会创建一个新的bean实例。
- request: 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session: 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP session内有效。
- global-session: 全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与servlet不同，每个portlet 都有不同的会话。

### 4.2 Spring中的单例bean的线程安全问题了解吗？

的确是存在安全问题的。因为当多个线程操作同一个对象的时候，对这个对象的成员变量的写操作会存在线程安全问题。

但是一般情况下，我们常用的`Controller`、`Service`、`Dao`这些Bean是无状态的。无状态的Bean不能保存数据，因此是线程安全的。

常见的有2种解决办法：

1. 在类中定义一个`ThreadLocal`成员变量，将需要的可变成员变量保存在`ThreadLocal`中（推荐的一种方式）。
2. 改变Bean的作用域为“prototype”：每次请求都会创建一个新的bean实例，自然不会存在线程安全问题。

### 4.3 @Component和@Bean的区别是什么？

1. 作用对象不同: `@Component`注解作用于类，而`@Bean`注解作用于方法。
2. `@Component`通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中（我们可以使用`@ComponentScan`注解定义要扫描的路径从中找出标识了需要装配的类自动装配到Spring的bean容器中）。`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个bean,`@Bean`告诉了Spring这是某个类的示例，当我需要用它的时候还给我。
3. `@Bean`注解比`Component`注解的自定义性更强，而且很多地方我们只能通过`@Bean` 注解来注册bean。比如当我们引用第三方库中的类需要装配到`Spring`容器时，则只能通过`@Bean`来实现。

`@Bean`注解使用示例：

```java
@Configuration
public class AppConfig {
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }

}
```

上面的代码相当于下面的xml配置

```xml
<beans>
    <bean id="transferService" class="com.acme.TransferServiceImpl"/>
</beans>
```

下面这个例子是通过`@Component`无法实现的。

```java
@Bean
public OneService getService(status) {
    case (status)  {
        when 1:
                return new serviceImpl1();
        when 2:
                return new serviceImpl2();
        when 3:
                return new serviceImpl3();
    }
}
```

### 4.4 将一个类声明为Spring的bean的注解有哪些？

我们一般使用`@Autowired`注解自动装配bean，要想把类标识成可用于`@Autowired`注解自动装配的bean的类，采用以下注解可实现：

- `@Component` ：通用的注解，可标注任意类为`Spring`组件。如果一个Bean不知道属于哪个层，可以使用`@Component`注解标注。
- `@Repository` : 对应持久层即Dao层，主要用于数据库相关操作。
- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到Dao层。
- `@Controller` : 对应Spring MVC控制层，主要用于接受用户请求并调用Service层返回数据给前端页面。

### 4.5 Spring中的bean生命周期？

这部分网上有很多文章都讲到了，下面的内容整理自： ~~https://yemengying.com/2016/07/14/spring-bean-life-cycle/~~ (原作者可能不再维护这个博客，连接无法访问，可通过其 Github 仓库访问 [https://github.com/giraffe0813/giraffe0813.github.io](https://github.com/giraffe0813/giraffe0813.github.io)) ，除了这篇文章，再推荐一篇很不错的文章 ：[https://www.cnblogs.com/zrtqsk/p/3735273.html](https://www.cnblogs.com/zrtqsk/p/3735273.html) 。

- Bean容器找到配置文件中Spring Bean的定义。
- Bean容器利用Java Reflection API创建一个Bean的实例。
- 如果涉及到一些属性值利用`set()`方法设置一些属性值。
- 如果Bean实现了`BeanNameAware`接口，调用`setBeanName()`方法，传入Bean的名字。
- 如果Bean实现了`BeanClassLoaderAware`接口，调用`setBeanClassLoader()`方法，传入`ClassLoader`对象的实例。
- 与上面的类似，如果实现了其他`*.Aware`接口，就调用相应的方法。
- 如果有和加载这个Bean的Spring容器相关的`BeanPostProcessor`对象，执行`postProcessBeforeInitialization()`方法
- 如果Bean实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果Bean在配置文件中的定义包含`init-method`属性，执行指定的方法。
- 如果有和加载这个Bean的Spring容器相关的`BeanPostProcessor`对象，执行`postProcessAfterInitialization()`方法。
- 当要销毁Bean的时候，如果Bean实现了`DisposableBean`接口，执行`destroy()`方法。
- 当要销毁Bean的时候，如果Bean在配置文件中的定义包含`destroy-method`属性，执行指定的方法。

图示：

![Spring Bean 生命周期](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-9-17/48376272.jpg)

与之比较类似的中文版本:

![Spring Bean 生命周期](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-9-17/5496407.jpg)

## 5. Spring AOP

Spring AOP部分的学习大致涉及的内容：

![Spring AOP学习思维导向图](_media/spring/spring-aop-learning-mindmap.png)

### 5.1 什么是Spring AOP及对AOP的理解？

AOP的全称是面向切面编程(Aspect-Oriented Programming)，Aspect是一种新的模块化机制，用来描述分散在对象、类或函数中的横切关注点(crosscutting concern)。从关注点中分离出横切关注点是面向切面的程序设计的核心概念。**分离关注点使解决特定领域问题的代码从业务逻辑中独立出来，业务逻辑的代码中不再含有针对特定领域问题代码的调用，业务逻辑同特定领域问题的关系通过切面来封装、维护**，这样原本分散在整个应用程序中的变动就可以很好地管理起来。

AOP能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，以便**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

**Spring AOP就是基于动态代理的**，如果要代理的对象，实现了某个接口，那么Spring AOP会使用**JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用JDK Proxy去进行代理了，这时候Spring AOP会使用**Cglib**生成一个被代理对象的子类来作为代理，如下图所示：

![SpringAOPProcess](_media/spring/SpringAOPProcess.jpg)

当然你也可以使用AspectJ，Spring AOP已经集成了AspectJ，AspectJ应该算的上是Java生态系统中最完整的AOP框架了。

使用AOP之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样大大简化了代码量。我们需要增加新功能时也方便，这样也提高了系统扩展性。日志功能、事务管理等等场景都用到了AOP。

AOP要实现的是在我们原来写的代码的基础上，进行一定的包装，如在方法执行前、方法返回后、方法抛出异常后等地方进行一定的拦截处理或者叫增强处理。

AOP的实现并不是因为Java提供了什么神奇的钩子，可以把方法的几个生命周期告诉我们，而是我们要实现一个代理，实际运行的实例其实是**生成的代理类的实例**。

### 5.2 Spring AOP相关名词概念解释

在正式进行AOP的使用前，对相关名词先有个大概了解是必要的，如果简单看一遍后对相关名词的理解不是很真切，建议你看过Spring AOP使用方式介绍、使用Demo之后再回来看一遍，相信你那时会都能看得很明白。

### 5.3 Spring AOP使用方式介绍(通过代码片段介绍)

Spring AOP通过日志管理模块来记录操作日志。定义注解`@Log`，并在方法上使用。

```
@Log(title = "通知公告", businessType = BusinessType.INSERT)
@PostMapping
public AjaxResult add(@Validated @RequestBody SysNotice notice) {
    notice.setCreateBy(SecurityUtils.getUsername());
    return toAjax(noticeService.insertNotice(notice));
}
```

定义日志切面：

```
@Aspect
@Component
public class LogAspect
{
    @AfterReturning(pointcut = "@annotation(controllerLog)", returning = "jsonResult")
    public void doAfterReturning(JoinPoint joinPoint, Log controllerLog, Object jsonResult)
    {
        handleLog(joinPoint, controllerLog, null, jsonResult);
    }
}
```

这样，在注解`@Log`出现的位置，该切面就会生效，进行相应记录日志的操作。


### 5.4 JKD动态代理和Cglib

### 5.5 Spring AOP和AspectJ AOP有什么区别？

**Spring AOP属于运行时增强，而AspectJ是编译时增强。** Spring AOP基于代理(Proxying)，而AspectJ基于字节码操作(Bytecode Manipulation)。

Spring AOP已经集成了AspectJ，AspectJ应该算的上是Java生态系统中最完整的AOP框架了。AspectJ相比于Spring AOP功能更加强大，但是 Spring AOP相对来说更简单，

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择AspectJ，它比Spring AOP快很多。

### 5.6 源码分析

https://juejin.cn/post/6844903969349713927

https://juejin.cn/post/6844903974449971214

## 6. Spring IOC

### 6.1 谈谈自己对于Spring IoC和AOP的理解

IoC(Inverse of Control:控制反转)是一种**设计思想**，就是**将原本在程序中手动创建对象的控制权，交由Spring框架来管理。** IoC 在其他语言中也有应用，并非Spring特有。 **IoC容器是Spring用来实现IoC的载体，IoC容器实际上就是个`Map(key，value)`，Map 中存放的是各种对象。**

将对象之间的相互依赖关系交给IoC容器来管理，并由IoC容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。**IoC容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。** 在实际项目中一个Service类可能有几百甚至上千个类作为它的底层，假如我们需要实例化这个Service，你可能要每次都要搞清这个Service所有底层类的构造函数，这可能会把人逼疯。如果利用IoC的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。

Spring时代我们一般通过XML文件来配置Bean，后来开发人员觉得XML文件来配置不太好，于是SpringBoot注解配置就慢慢开始流行起来。

推荐阅读：https://www.zhihu.com/question/23277575/answer/169698662

**Spring IoC的初始化过程：**

![Spring IoC的初始化过程](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-7/SpringIOC初始化过程.png)

IoC源码阅读

- https://javadoop.com/post/spring-ioc

## 7. Spring MVC

### 7.1 说说自己对于Spring MVC了解?

谈到这个问题，我们不得不提提之前Model1和Model2这两个没有Spring MVC的时代。

- **Model1时代**: 很多学Java后端比较晚的朋友可能并没有接触过Model1模式下的JavaWeb应用开发。在Model1模式下，整个Web应用几乎全部用JSP页面组成，只用少量的JavaBean来处理数据库连接、访问等操作。这个模式下JSP既是控制层又是表现层。显而易见，这种模式存在很多问题。比如①将控制逻辑和表现逻辑混杂在一起，导致代码重用率极低；②前端和后端相互依赖，难以进行测试并且开发效率极低；
- **Model2时代**: 学过Servlet并做过相关Demo的朋友应该了解“Java Bean(Model)+ JSP（View,）+Servlet（Controller）  ”这种开发模式,这就是早期的JavaWeb MVC开发模式。Model:系统涉及的数据，也就是dao和bean。View：展示模型中的数据，只是用来展示。Controller：处理用户请求都发送给 ，返回数据给JSP并展示给用户。

Model2模式下还存在很多问题，Model2的抽象和封装程度还远远不够，使用Model2进行开发时不可避免地会重复造轮子，这就大大降低了程序的可维护性和复用性。于是很多JavaWeb开发相关的MVC框架应运而生比如Struts2，但是Struts2比较笨重。随着Spring轻量级开发框架的流行，Spring 生态圈出现了Spring MVC框架，Spring MVC是当前最优秀的MVC框架。相比于Struts2，Spring MVC使用更加简单和方便，开发效率更高，并且 Spring MVC运行速度更快。

MVC是一种设计模式，Spring MVC是一款很优秀的MVC框架。Spring MVC可以帮助我们进行更简洁的Web层的开发，并且它天生与Spring 框架集成。Spring MVC下我们一般把后端项目分为Service层（处理业务）、Dao层（数据库操作）、Entity层（实体类）、Controller层(控制层，返回数据给前台页面)。

**Spring MVC的简单原理图如下：**

![](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-10-11/60679444.jpg)

### 7.2 SpringMVC工作原理了解吗？

**原理如下图所示：**
![SpringMVC运行原理](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-10-11/49790288.jpg)

上图的一个笔误的小问题：Spring MVC的入口函数也就是前端控制器`DispatcherServlet`的作用是接收请求，响应结果。

**流程说明（重要）：**

1. 客户端(浏览器)发送请求，直接请求到`DispatcherServlet`。
2. `DispatcherServlet`根据请求信息调用`HandlerMapping`，解析请求对应的`Handler`。
3. 解析到对应的`Handler`(也就是我们平常说的`Controller`控制器)后，开始由`HandlerAdapter`适配器处理。
4. `HandlerAdapter`会根据`Handler`来调用真正的处理器来处理请求，并处理相应的业务逻辑。
5. 处理器处理完业务后，会返回一个`ModelAndView`对象，`Model`是返回的数据对象，`View`是个逻辑上的`View`。
6. `ViewResolver`会根据逻辑`View`查找实际的`View`。
7. `DispaterServlet`把返回的`Model`传给`View`(视图渲染)。
8. 把`View`返回给请求者(浏览器)。

## 8. Spring框架中用到了哪些设计模式？

关于下面一些设计模式的详细介绍，可以看笔主前段时间的原创文章[《面试官:“谈谈Spring中都用到了那些设计模式?”。》](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485303&idx=1&sn=9e4626a1e3f001f9b0d84a6fa0cff04a&chksm=cea248bcf9d5c1aaf48b67cc52bac74eb29d6037848d6cf213b0e5466f2d1fda970db700ba41&token=255050878&lang=zh_CN#rd) 。

- **工厂设计模式**: Spring使用工厂模式通过`BeanFactory`、`ApplicationContext` 创建bean对象。
- **代理设计模式**: Spring AOP功能的实现。
- **单例设计模式**: Spring中的Bean默认都是单例的。
- **模板方法模式**: Spring中`jdbcTemplate`、`hibernateTemplate`等以Template结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式**: 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:**: Spring事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式**: Spring AOP的增强或通知(Advice)使用到了适配器模式、spring MVC中也是用到了适配器模式适配`Controller`。
- ......

## 9. Spring事务

### 9.1 Spring管理事务的方式有几种？

1. 编程式事务，在代码中硬编码(不推荐使用)。
2. 声明式事务，在配置文件中配置（推荐使用）。

**声明式事务又分为两种：**

1. 基于XML的声明式事务；
2. 基于注解的声明式事务；

### 9.2 Spring事务中的隔离级别有哪几种?

`TransactionDefinition`接口中定义了五个表示隔离级别的常量：

- `TransactionDefinition.ISOLATION_DEFAULT`: 使用后端数据库默认的隔离级别，Mysql默认采用的REPEATABLE_READ隔离级别，Oracle 默认采用的READ_COMMITTED隔离级别。
- `TransactionDefinition.ISOLATION_READ_UNCOMMITTED`: 最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读。
- `TransactionDefinition.ISOLATION_READ_COMMITTED`: 允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生。
- `TransactionDefinition.ISOLATION_REPEATABLE_READ`: 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
- `TransactionDefinition.ISOLATION_SERIALIZABLE`: 最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

### 9.3 Spring事务中哪几种事务传播行为?

**支持当前事务的情况：**

- **TransactionDefinition.PROPAGATION_REQUIRED：** 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
- **TransactionDefinition.PROPAGATION_SUPPORTS：** 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
- **TransactionDefinition.PROPAGATION_MANDATORY：** 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常(mandatory：强制性)。

**不支持当前事务的情况：**

- **TransactionDefinition.PROPAGATION_REQUIRES_NEW：** 创建一个新的事务，如果当前存在事务，则把当前事务挂起。
- **TransactionDefinition.PROPAGATION_NOT_SUPPORTED：** 以非事务方式运行，如果当前存在事务，则把当前事务挂起。
- **TransactionDefinition.PROPAGATION_NEVER：** 以非事务方式运行，如果当前存在事务，则抛出异常。

**其他情况：**

- **TransactionDefinition.PROPAGATION_NESTED：** 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。

### 9.4 `@Transactional(rollbackFor = Exception.class)`注解了解吗？

我们知道：Exception分为运行时异常RuntimeException和非运行时异常。事务管理对于企业应用来说是至关重要的，即使出现异常情况，它也可以保证数据的一致性。

当`@Transactional`注解作用于类上时，该类的所有public方法将都具有该类型的事务属性，同时，我们也可以在方法级别使用该标注来覆盖类级别的定义。如果类或者方法加了这个注解，那么这个类里面的方法抛出异常，就会回滚，数据库里面的数据也会回滚。

在`@Transactional`注解中如果不配置`rollbackFor`属性，那么事务只会在遇到`RuntimeException`的时候才会回滚，加上`rollbackFor=Exception.class`，可以让事务在遇到非运行时异常时也回滚。

关于`@Transactional`注解推荐阅读的文章：

- [透彻的掌握Spring中@transactional的使用](https://www.ibm.com/developerworks/cn/java/j-master-spring-transactional-use/index.html)

## 10. JPA

### 10.1 如何使用JPA在数据库中非持久化一个字段？

假如我们有有下面一个类：

```java
Entity(name="USER")
public class User {
  
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "ID")
    private Long id;
  
    @Column(name="USER_NAME")
    private String userName;
  
    @Column(name="PASSWORD")
    private String password;
  
    private String secrect;
  
}
```

如果我们想让`secrect`这个字段不被持久化，也就是不被数据库存储怎么办？我们可以采用下面几种方法：

```java
static String transient1; // not persistent because of static

final String transient2 = “Satish”; // not persistent because of final

transient String transient3; // not persistent because of transient

@Transient
String transient4; // not persistent because of @Transient
```

一般使用后面两种方式比较多，我个人使用注解的方式比较多。

## 11. 注解

### 11.1 元注解

说简单点，就是定义其他注解的注解。 比如`Override`这个注解，就不是一个元注解，而是通过元注解定义出来的。

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

这里面的`@Target`、`@Retention`就是元注解。

**元注解有六个：**

* `@Target`: 表示该注解可以用于什么地方，用于指定被修饰的注解修饰哪些程序单元，比如类、方法、字段、方法参数。
* `@Retention`: 表示在什么级别保存该注解信息，用于指定被修饰的注解被保留多长时间，分别是`SOURCE`（注解仅存在于源码中，在class字节码文件中不包含）、`CLASS`（默认的保留策略，注解会在class字节码文件中存在，但运行时无法获取）、`RUNTIME`（注解会在class字节码文件中存在，在运行时可以通过反射获取到）三种类型；如果想要在程序运行过程中通过反射来获取注解的信息需要将Retention设置为RUNTIME。
* `@Documented`: 将此注解包含在javadoc中，用于指定被修饰的注解类将被javadoc工具提取成文档。
* `@Inherited`: 允许子类继承父类中的注解，用于指定被修饰的注解类将具有继承性。
* `@Repeatable`: 1.8新增，允许一个注解在一个元素上使用多次。
* `@Native`: 1.8新增，修饰成员变量，表示这个变量可以被本地代码引用，常常被代码生成工具使用。

**ElementType各常量定义的范围：**

- ElementType.TYPE
  - Class, interface(including annotation type), or enum declaration(类、接口、注解、枚举)；
- ElementType.FIELD
  - Field declaration(includes enum constants)(字段、枚举常量)；
- ElementType.METHOD
  - Method declaration(方法)；
- ElementType.PARAMETER
  - Formal parameter declaration(方法参数)；
- ElementType.CONSTRUCTOR
  - Constructor declaration(构造)；
- ElementType.LOCAL_VARIABLE
  - Local variable declaration(局部变量)；
- ElementType.ANNOTATION_TYPE
  - Annotation type declaration(注解)；
- ElementType.PACKAGE
  - Package declaration(包）；
- ElementType.TYPE_PARAMETER
  - Type parameter declaration(泛型参数)；
  - Since: 1.8；
- ElementType.TYPE_USE
  - Use of a type(任意类型，获取class对象和import两种情况除外)；
  - Since: 1.8；
- ElementType.MODULE
  - Module declaration([模块](https://docs.oracle.com/javase/9/whatsnew/toc.htm#JSNEW-GUID-C23AFD78-C777-460B-8ACE-58BE5EA681F6))
  - Since: 9；



### 11.2 自定义注解

除了元注解，都是自定义注解。通过元注解定义出来的注解。 如我们常用的`Override` 、`Autowire`等。 日常开发中也可以自定义一个注解，这些都是自定义注解。

### 11.3 如何自定义一个注解？

在Java中，类使用`class`定义，接口使用`interface`定义，注解和接口的定义差不多，增加了一个`@`符号，即`@interface`，代码如下：

```java
public @interface EnableAuth {

}
```

注解中可以定义成员变量，用于信息的描述，跟接口中方法的定义类似，代码如下：

```java
public @interface EnableAuth {
    String name();
}
```

还可以添加默认值：

```java
public @interface EnableAuth {
    String name() default "优米格";
}
```

上面的介绍只是完成了自定义注解的第一步，开发中日常使用注解大部分是用在类上、方法上、字段上，示列代码如下：

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface EnableAuth {

}
```

自定义注解是通过元注解定义出来的。

### 11.4 切面相关的注解用过那些？

Spring AOP面向切面编程，可以用来配置事务、做日志、权限验证、在用户请求时做一些处理等等。用`@Aspect`做一个切面，就可以直接实现。

**1)切面类定义相关**：

* `@Component`
* `@Aspect`

例：

```java
@Aspect
@Component
public class LogAspect{
    private static final Logger log = LoggerFactory.getLogger(LogAspect.class);
  
    @Autowired
    private AsyncLogService asyncLogService;
}
```

**2)切点定义相关：**

```java
// 配置织入点
@Pointcut("@annotation(com.ddmit.common.log.annotation.Log)")
  public void logPointCut(){
}
```

表达式`@annotation`表示在有对应注解的方法上执行，Pointcut注解表达式相关内容参考：

* [Spring AOP 之二：Pointcut注解表达式](https://my.oschina.net/u/2474629/blog/1083448)；
* [Spring AOP中@Pointcut切入点表达式最全面使用介绍](https://cloud.tencent.com/developer/article/1497814)；

**3)Advice配置相关**

Advice是在方法执行之前或之后采取的实际操作。 这是在Spring AOP框架的程序执行期间调用的实际代码片段。

* `@Before`: 在方法被调用之前执行增强；
* `@After`: 在方法被调用之后执行增强；
* `@AfterReturning`: 在方法成功执行之后执行增强；
* `@AfterThrowing`: 在方法抛出指定异常后执行增强；
* `@Around`：在方法调用的前后执行自定义的增强行为（最灵活的方式），既能做 `@Before` 的事情，也可以做 `@AfterReturning` 的事情；

> 注意@After和@AfterReturning的区别，@After会拦截正常返回和异常的情况。

**参考：**

1. [Springboot（二十一）@Aspect 切面注解使用](https://blog.csdn.net/u012326462/article/details/82529835)；
2. [Spring AOP - 注解方式使用介绍（长文详解）](https://juejin.cn/post/6844903987062243341)；

### 11.5 Spring中的这几个注解有什么区别：@Component、@Repository、@Service、@Controller？

`@Component`用来告知Spring帮我们管理这个对象，在需要时为我们注入此对象。`@Controller`、`@Repository`和`@Service`注解都被`@Component`修饰，用于代码中区分表现层，持久层和业务层的组件，代码中组件不好归类时可以使用`@Component`来标注。

### 11.6 `@Autowired`注解如何实现注入？

Spring自动在代码上下文找到和其匹配(默认是类型匹配)的Bean，并自动注入到相应的位置。

TODO

**参考:**

1. [Spring中 如果该Service有多个实现类，它怎么知道该注入哪个ServiceImpl类？](https://www.cnblogs.com/zoe-java/p/11530888.html)；
2. [@Autowired注解的实现原理](https://juejin.cn/post/6844903957135884295)；

### 11.7 如果Service有多个实现类，Spring怎么知道该注入哪一个实现类？

有三种方式来指定具体注入哪一个实现类。

**1）方法1**

使用`@Autowired`注解，并使用`@Qualifier("beanId")`来指定注入哪一个实现类。

```java
public interface HumanService {
    public String name();
}


@Service
public class TeacherServiceImpl implements HumanService {
    @Override
    public String name() {
        System.out.println("teacher");
        return "teacher";
    }
}

@Service
public class DoctorServiceImpl implements HumanService {
    @Override
    public String name() {
        System.out.println("doctor");
        return "doctor";
    }
}


@RestController
public class HumanController {
    @Autowired
    @Qualifier("teacherServiceImpl")
    private HumanService humanService;

    @RequestMapping("/name")
    public String name(){
        return humanService.name();
    }
}
```

**2）方法2**

使用 `@Resource`注解并指定类名。

```java
public interface HumanService {
    public String name();
}


@Service
public class TeacherServiceImpl implements HumanService {
    @Override
    public String name() {
        System.out.println("teacher");
        return "teacher";
    }
}

@Service
public class DoctorServiceImpl implements HumanService {
    @Override
    public String name() {
        System.out.println("doctor");
        return "doctor";
    }
}


@RestController
public class HumanController {
    @Resource(type = DoctorServiceImpl.class)
    private HumanService humanService;

    @RequestMapping("/name")
    public String name(){
        return humanService.name();
    }
}
```

3）方法3

为实现类指定名称 `@Service("name")`，然后使用时使用 `@Resource(name= "name")`进行注入。

```java

public interface HumanService {
    public String name();
}

@Service("teacherService")
public class TeacherServiceImpl implements HumanService {
    @Override
    public String name() {
        System.out.println("teacher");
        return "teacher";
    }
}

@Service("doctorService")
public class DoctorServiceImpl implements HumanService {
    @Override
    public String name() {
        System.out.println("doctor");
        return "doctor";
    }
}

@RestController
public class HumanController {

    @Resource(name="doctorService")
    private HumanService humanService;

    @RequestMapping("/name")
    public String name(){
        return humanService.name();
    }
}
```

**总结：**`@Service`注解其实做了两件事。

* 声明`TeacherServiceImpl.java`是一个bean。因为`TeacherServiceImpl.java`是一个bean，其他的类才可以使用`@Autowired`将`TeacherServiceImpl`作为一个成员变量自动注入。
* `TeacherServiceImpl.java`在bean中的id是`teacherServiceImpl`，即类名且首字母小写。不能有同名的，不然要报错。

### 11.8 @Autowired和@Resource两个注解的区别？

* `@Autowired`是Spring的注解，`@Resource`是J2EE的注解，这个看一下导入注解的时候这两个注解的包名就一清二楚了。
* `@Autowired`默认按照byType方式进行bean匹配，`@Resource`默认按照byName方式进行bean匹配。
* `@Autowired`默认情况下必须要求依赖对象必须存在，如果要允许null值，可以设置它的required属性为false，如：`@Autowired(required=false`)。

### 11.9 @ConditionalOnProperty注解使用

https://jishu.dev/2022/01/05/spring-conditionalonproperty/


### 11.10 `@RestControllerAdvice`注解使用

在Spring 3.2中，新增了`@ControllerAdvice`、`@RestControllerAdvice`注解。`@ControllerAdvice`和`@RestControllerAdvice`类似于`@Controller`和`@RestController`，一个是返回对象，一个是返回json数据。

此注解用于定义`@ExceptionHandler`、`@InitBinder`、`@ModelAttribute`，并应用到所有`@RequestMapping`中。

`@RestControllerAdvice`与`@ExceptionHandler`配合使用，用于全局统一异常处理。

```
@RestControllerAdvice
public class GlobalExceptionHandler{

    @ExceptionHandler(ServiceException.class)
    public AjaxResult handleServiceException(ServiceException e, HttpServletRequest request){
        log.error(e.getMessage(), e);
        Integer code = e.getCode();
        return StringUtils.isNotNull(code) ? AjaxResult.error(code, e.getMessage()) : AjaxResult.error(e.getMessage());
    }
}
```

其中：ServiceException是一个自定义异常。


参考：

1. [@RestControllerAdvice注解使用](https://www.cnblogs.com/huanshilang/p/10620048.html)；
2. [RestControllerAdvice注解与全局异常处理](https://juejin.cn/post/7025484367539470344)；
3. [SpringMvc @InitBinder 表单多对象精准绑定接收](https://blog.csdn.net/xsf1840/article/details/73556633)；

## 12. Spring对多线程的支持

## 13. Spring对定时任务的支持

**参考：**

- 《Spring 技术内幕》
- [https://www.cnblogs.com/wmyskxz/p/8820371.html](https://www.cnblogs.com/wmyskxz/p/8820371.html)
- [https://www.journaldev.com/2696/spring-interview-questions-and-answers](https://www.journaldev.com/2696/spring-interview-questions-and-answers)
- [https://www.edureka.co/blog/interview-questions/spring-interview-questions/](https://www.edureka.co/blog/interview-questions/spring-interview-questions/)
- https://www.cnblogs.com/clwydjgs/p/9317849.html
- [https://howtodoinjava.com/interview-questions/top-spring-interview-questions-with-answers/](https://howtodoinjava.com/interview-questions/top-spring-interview-questions-with-answers/)
- [http://www.tomaszezula.com/2014/02/09/spring-series-part-5-component-vs-bean/](http://www.tomaszezula.com/2014/02/09/spring-series-part-5-component-vs-bean/)
- [https://stackoverflow.com/questions/34172888/difference-between-bean-and-autowired](https://stackoverflow.com/questions/34172888/difference-between-bean-and-autowired)
- [https://www.interviewbit.com/spring-interview-questions/](https://www.interviewbit.com/spring-interview-questions/)
