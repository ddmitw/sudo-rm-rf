## 1.Spring Boot出现背景

解决Spring一直以来为人诟病的复杂配置问题。

借助强大的Groovy动态语言，如强大的MetaObject协议、可插拔的AST转换器及内置的依赖解决方案引擎等。在其核心的编译模型中，Spring Boot使用Groovy来构建工程文件，所以它可以轻松的利用导入模板及方法模板对类所生成的字节码进行改造，从而让开发者仅用很简洁的代码就可以完成很复杂的操作。

从Spring Boot的项目名称中的Boot可以看出，Spring Boot的作用在于创建和启动新的基于Spring框架的项目，其目的是帮助开发人员快速构建出基于Spring的应用。

## 2.Spring Boot特点

Spring Boot包含如下特性：

* 为开发者提供Spring快速入门体验；
* 内嵌Tomcat和Jetty容器，不需要部署WAR文件到Web容器就可以独立运行应用；
* 提供需要基于Maven的pom配置模板来简化工程配置；
* 提供实现自动化配置的基础设施；
* 提供可以直接在生产环境中使用的功能，如性能指标、应用信息和应用健康检查；
* 开箱即用，没有代码生成，也无需XML配置文件，支持修改默认值来满足特定需求；

## 3.Spring启动器

Spring Boot是由一系列启动器组成的，这些启动器构成一个强大的、灵活的开发助手。开发人员根据项目需要，选择并组合相应的启动器，就可以快速搭建一个适合项目需要的基础运行框架。Spring提供的启动器：

|启动器名称|启动器说明|
|:------:|:------:|
|spring-boot-starter|核心模块，包含自动配置支持、日志库和对YAML配置文件的支持|
|spring-boot-starter-amqp|支持AMQP，包含Spring-rabbit|
|spring-boot-starter-aop|支持面向切面编程(AOP)，包含spring-aop和AspectJ|
|spring-boot-starter-artemis|通过Apache Artemis支持JMS的API(Java Message Service API)|
|spring-boot-starter-batch|支持Spring Batch，包含HSQLDB|
|spring-boot-starter-cache|支持Spring的Cache抽象|
|spring-boot-starter-cloud-connectors|支持Spring Cloud Connectors，简化了在像Cloud Foundry或Heroku这样的云平台上连接服务|
|spnng-boot-starter-data-gemfire|支持GemFire分布式数据存储，包含spring-data-gemfire|
|spring-boot-starter-data-jpa|支持JPA，包含spring-data-jpa、spring-orm和Hibernate|
|spring-boot-starter-data-elasticsearch|支持ElasticSearch搜索和分析引擎，包含spring-data-elasticsearch|
|spring-boot-starter-data-solr|支持Apache Solr搜索平台，包含spring-data-solr|
|spring-boot-starter-data-mongodb|支持MongoDB，包含spring-data-mongodb|
|spring-boot-starter-data-rest|支持以REST方式暴露Spring Data仓库，包含spring-data-rest-webmvc|
|spring-boot-starter-redis|支持Redis键值存储数据库，包含spring-redis|

……
