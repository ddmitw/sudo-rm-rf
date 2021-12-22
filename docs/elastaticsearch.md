## 1.相关名词概念解释

### 1.1 集群(cluster)

拥有同一个集群名称(`cluste.name`)的ES实例(节点)的集合。

### 1.2 索引(index)

索引可以看作是[文档的集合](https://www.elastic.co/guide/en/elasticsearch/reference/current/documents-indices.html)；

### 1.3 映射(mapping)

[映射](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping.html)是定义文档及其包含的字段如何存储和索引的过程。映射数据时，需要创建一个映射定义，其中包含与文档相关的字段列表。

### 1.4 文档(document)

每个文档是字段(fields)的集合，每个字段都有自己的数据类型(data type)。

### 1.5 字段(field)

字段是存储数据的键值对，每个文档都是字段的集合。参考： [Data in: documents and indices](https://www.elastic.co/guide/en/elasticsearch/reference/current/documents-indices.html#documents-indices)。

```
PUT /my-index-000001
{
  "mappings": {
    "properties": {
      "age":    { "type": "integer" },  
      "email":  { "type": "keyword"  }, 
      "name":   { "type": "text"  }     
    }
  }
}
```



### 1.6 主节点(master)

ES集群中会选举一个节点成为**主节点**，主节点的职责是维护全局集群状态，比如：在节点加入或离开集群的时候重新分配分片。如果是主节点宕机了，那么会重新选举一个节点为主节点。

### 1.7 主分片(primary shards)

主分片是两种分片类型的其中之一。分片是一个最小级别的工作单元，索引实际上是指向一个或者多个物理 *分片* 的 *逻辑命名空间* 。文档归属于主分片。

参考： [Scalability and resilience: clusters, nodes, and shards](https://www.elastic.co/guide/en/elasticsearch/reference/current/scalability.html);

### 1.8 副本分片(replica shards)

副本分片是主分片的拷贝。副本提供数据的冗余副本，以防止硬件故障并增强处理读取请求（如搜索或检索文档）的能力。

### 1.9 集群发现

只需要配置相同的`cluster.name`就能将这些节点加入到集群，这取决于ES的集群发现机制。发现机制负责发现集群中的节点，以及选择主节点。每次集群状态发生更改时，集群中的其他节点都会知道状态。主要的发现机制有如下几种：

* [Azure classic discovery](https://www.elastic.co/guide/en/elasticsearch/plugins/6.5/discovery-azure-classic.html) 插件方式，多播；
* [EC2 discovery](https://www.elastic.co/guide/en/elasticsearch/plugins/6.5/discovery-ec2.html) 插件方式，多播；
* [Google Compute Engine (GCE) discovery](https://www.elastic.co/guide/en/elasticsearch/plugins/6.5/discovery-gce.html) 插件方式，多播；
* [Zen discovery](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html) 默认实现，多播/单播；

## 2.elasticsearch 如何实现分布式？

从部署方式和集群内的原理两个层次来说明。

### 2.1 部署方式

Elasticsearch 尽可能地屏蔽了分布式系统的复杂性。Elasticsearch分布式集群的各个节点就是一个个ES实例，节点加入集群是通过配置文件中设置相同的 `cluste.name` 而实现的，所以分布式集群是由一个或者多个拥有相同 `cluster.name` 配置的节点组成， 它们共同承担数据和负载的压力。当有节点加入集群中或者从集群中移除节点时，集群将会重新平均分布所有的数据。

这和数据库的分布式和同源的`solr`实现分布式都是有区别的，数据库分布式(分库分表)需要我们指定路由规则和数据同步策略等，`solr`的分布式也需依赖 `zookeeper`，但是 `Elasticsearch` 完全屏蔽了这些。

### 2.2 集群内的原理

虽然Elasticsearch的分布式集群部署方式简单，但是了解Elasticsearch分布式的内部实现机制可以帮助我们更完整的学习和了解Elasticsearch。

**1)扩容方式**

ElasticSearch 的主旨是随时可用和按需扩容。 而扩容可以通过购买性能更强大（ *垂直扩容* ，或 *纵向扩容* ） 或者数量更多的服务器（ *水平扩容* ，或 *横向扩容* ）来实现。

虽然 Elasticsearch 可以获益于更强大的硬件设备，但是垂直扩容是有极限的。 真正的扩容能力是来自于水平扩容—为集群添加更多的节点，并且将负载压力和稳定性分散到这些节点中。

对于大多数的数据库而言，通常需要对应用程序进行非常大的改动，才能利用上横向扩容的新增资源。 与之相反的是，Elasticsearch天生就是 *分布式的* ，它知道如何通过管理多节点来提高扩容性和可用性。 这也意味着你的应用无需关注这个问题。

**2)主节点选取**

我们启动一定数量的ES实例，并给它们分配相同的`cluster.name`，让它们归属于同一个集群，这一个个实例，就是ES的各个节点。与其他master-slave模式的集群一样，ES集群中也会选举一个节点成为**主节点**，主节点的职责是维护全局集群状态，比如：在节点加入或离开集群的时候重新分配分片。如果是主节点宕机了，那么会重新选举一个节点为主节点。

**3)存储数据**

ES中存储数据的基本单位是索引(index)，如果要将数据添加到ES中，首先就需要创建索引。

ES中索引和分片(shard)的关系是：分片是一个最小级别的工作单元，索引实际上是指向一个或者多个物理 *分片* 的 *逻辑命名空间* 。

一个分片可以是 *主分片*或者 *副本分片*。 索引内任意一个文档都归属于一个主分片，所以主分片的数目决定着索引能够保存的最大数据量。在索引建立的时候就已经确定了主分片数，但是副本分片数可以随时修改。

比如你要在ES里存储订单数据，首先需要在ES中创建一个索引，假设索引的名称是order_idx。订单数据就存储在这个索引里，就类似mysql的一张表。



参考：

1. [集群内的原理](https://www.elastic.co/guide/cn/elasticsearch/guide/current/distributed-cluster.html)；
2. [分布式特性](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_distributed_nature.html);
3. [Elasticsearch 技术分析（四）： 分布式工作原理 ](https://www.cnblogs.com/jajian/p/10176604.html)；
4. [ElasticSearch是如何实现分布式的？](https://zhuanlan.zhihu.com/p/60176461)；
5. [有人问你Elasticsearch分布式架构原理，将这篇文章丢过去](https://cloud.tencent.com/developer/article/1663950)；

## 3.elasticsearch分片内部原理

参考：

1. [分片内部原理](https://www.elastic.co/guide/cn/elasticsearch/guide/current/inside-a-shard.html)；

## 4. 为什么创建索引后主分片数不可以进行改变？

当索引一个文档的时候，文档会被存储在一个主分片中。那么ES是如何知道应该放在哪个分片中呢?

答案是有公式决定的。

```
shard = hash(routing) % number_of_primary_shards
```

其中`routing`是一个可变值，默认是文档的`_id`，也可以设置成一个自定义值。`routing` 通过 `hash` 函数生成一个数字，然后这个数字再除以 `number_of_primary_shards` （主分片的数量）后得到余数。这个分布在 0 到 `number_of_primary_shards-1` 之间的余数，就是我们所寻求的文档所在分片的位置。

这就解释了为什么我们要在创建索引的时候就确定好主分片的数量 并且永远不会改变这个数量：因为如果数量变化了，那么所有之前路由的值都会无效，文档也再也找不到了。

## 5. 什么时候使用多字段？

在对同一字段因不同目的进行多种方式的搜索时。

参考：

1.  [ElasticSearch: when to use multi-field](https://stackoverflow.com/questions/64995345/elasticsearch-when-to-use-multi-field) ；
2.  [`fields`](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-fields.html#multi-fields)；

## 6.分配分片大小及数量的原则？

