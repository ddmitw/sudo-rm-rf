## 1. `@RabbitListener`注解作用?

该注解可以作用于类或方法，使用`@RabbitListener`注解，可以指定消费消息的方法、指定要监听的消息队列。

①当该注解作用于类时，需要配合使用`@RabbitHandler`注解来指定处理消息的方法。如下：

```
@Component
@RabbitListener(queues = "orderQueue")
public class OrderMqReceiver {

    private static final Logger logger = LoggerFactory.getLogger(OrderMqReceiver.class);

    @RabbitHandler
    public void process(String message) {
        try {
            // 模拟处理过程
            Thread.sleep(5000);
            logger.info("OrderMqReceiver收到消息开始用户下单流程: " + message);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

②当该注解作用于方法时，直接指定要监听的消息队列名称即可。

```
@Component
public class OrderMqReceiver {
    @RabbitListener(queues = "orderQueue")
    public void process(String message) {

    }
}
```


参考：

1. [@RabbitListener起作用的原理](https://blog.csdn.net/zidongxiangxi/article/details/100623548)；
2. [RabbitMQ：@RabbitListener 与 @RabbitHandler 及 消息序列化](https://www.jianshu.com/p/911d987b5f11)；
3. [【MQ系列】RabbitListener消费基本使用姿势介绍](https://spring.hhui.top/spring-blog/2020/03/18/200318-SpringBoot%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B%E4%B9%8BRabbitListener%E6%B6%88%E8%B4%B9%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%E5%A7%BF%E5%8A%BF%E4%BB%8B%E7%BB%8D/)；


## 2.send方法和convertAndSend方法的区别？

区别之处在于，前者不会对消息进行转换，我们传递进去的是什么消息，就会往`RabbitMQ Server` 中发送什么消息；后者则会将我们传递进去的消息进行转换，具体如何转换的需要我们观察源码之后才会清楚，并将转换过后的消息发送到`RabbitMQ Server`中。

## 3.AmqpTemplate和RabbitTemplate区别？

AmqpTemplate是一个接口，而RabbitTemplate是AmqpTemplate的实现类。两个用哪个都可以，都对编码来说的最佳实践是使用接口，这样就不会对实现类硬性依赖。

