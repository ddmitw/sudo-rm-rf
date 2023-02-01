## 1. 分析下Redis缓存击穿、穿透、雪崩的原因及解决方案?

### 1.1 Redis缓存雪崩

**原因:** 在同一个时间，缓存大批量的失效，然后所有请求都打到DB数据库，导致DB数据库直接扛不住崩了。

**解决方案:** 批量往redis存数据的时候，把每个key的失效时间加上个随机数，比如1-5分钟随机，这样的话就能保证数据不会在同一个时间大面积失效。

### 1.2 Redis缓存击穿

**原因:** 缓存击穿跟缓存雪崩类似，但不同之处是雪崩是大面积缓存失效，导致数据库崩溃；而缓存击穿是一个key是热点，大量并发请求集中对此key进行访问，当此key失效的瞬间，就会导致缓存击穿，从而大量并发请求数据库，这就像一个完好无损的桶上凿开了一个洞。

**解决方案：** 

1）方法1：将这个热点key设置为永久有效；
2）方法2：使用互斥锁(SETNX)；
3）方法3：异步构建缓存；
4）方法4：布隆过滤器；

### 1.3 Redis缓存穿透

**原因：** 指查询一个数据库一定不存在的数据。正常的使用缓存流程大致是，数据查询先进行缓存查询，如果key不存在或者key已经过期，再对数据库进行查询，并把查询到的对象，放进缓存。如果数据库查询对象为空，则不放进缓存。

但是这种方法存在一个问题，比如我传一个用户id为-1，这个用户id 在缓存里面是肯定不存在的，所以会去数据库里面查询，如果有搞事情的人，大批量请求并传用户id为-1，那就和没用redis 一样，导致数据库压力过大而崩溃。

**解决方案：**

1）方法1：接口层增加校验，不合法的参数直接返回；
2）方法2：在缓存查不到，DB中也没有的情况，可以将对应的key的value写为null，或者其他特殊值写入缓存，同时将过期失效时间设置短一点，以免影响正常情况。这样是可以防止反复用同一个ID来暴力攻击；
3）方法3：正常用户是不会这样暴力攻击，只有是恶意者才会这样做，可以在网关NG做一个配置项，为每一个IP设置访问阈值；
4）方法4：高级用户布隆过滤器(Bloom Filter), 这个也能很好地防止缓存穿透。原理就是利用高效的数据结构和算法快速判断出你这个Key是否在 DB中存在，不存在你return就好了，存在你就去查了DB刷新KV再return；

参考：

1. [Redis缓存击穿、穿透、雪崩的原因以及解决方案](https://learnku.com/articles/49688)；
2. [Redis缓存雪崩、击穿、穿透](https://segmentfault.com/a/1190000022029639)；
3. [分布式之缓存击穿](https://www.cnblogs.com/rjzheng/p/8908073.html)；
4. [Redis缓存击穿解决方案之互斥锁](https://juejin.cn/post/7096734273210122253)；

## 2.RedisTemplate和StringRedisTemplate用法区别？

主要区别：

* StringRedisTemplate继承RedisTemplate；
* 两者的数据是不共通，StringRedisTemplate只能管理StringRedisTemplate里面的数据，RedisTemplate只能管理RedisTemplate中的数据；
* 使用的序列化类不同，RedisTemplate使用的是`JdkSerializationRedisSerializer`，存入数据会将数据先序列化成字节数组然后再存入Redis数据库，StringRedisTemplate使用的是StringRedisSerializer；

使用时注意事项：

* 当你的redis数据库里面本来存的是字符串数据或者你要存取的数据就是字符串类型数据的时候，那么你就使用StringRedisTemplate即可；
* 如果你的数据是复杂的对象类型，而取出的时候又不想做任何的数据转换，直接从Redis里面取出一个对象，那么使用RedisTemplate是更好的选择;
* redisTemplate中存取数据都是字节数组，当redis中存入的数据是可读形式而非字节数组时，使用redisTemplate取值的时候会无法获取导出数据，获得的值为null；


参考：

1. [StringRedisTemplate与RedisTemplate区别](https://juejin.cn/post/7111847204251238436)；

## 3.Redis五大类型？

字符串(String)、哈希/散列/字典(Hash)、列表(List)、集合(Set)、有序集合(Sorted Set)。

参考：

1. [redis五大类型用法](https://www.cnblogs.com/yanan7890/p/6617305.html)；
