## 1.Maven 中 dependencyManagement 标签使用？

在Maven中`dependencyManagement`的作用其实相当于一个对所依赖jar包进行版本管理的管理器，该标签只是对依赖及版本的声明，不会进行实际依赖的下载。

在maven中，获取依赖的版本有两种途径：

* `<dependencyManagement>`下的`<dependencies>` 标签下`<dependency>` 定义的版本；
* `<dependencies>`下的`<dependency>` 定义的版本；

如果`<dependencies>`下的`<dependency>`没有声明`version`元素，就会到`<dependencyManagement>`下的`<dependencies>` 标签下`<dependency>` 找有没有对该`artifactId`和`groupId`进行过版本声明，如果有，就继承它，如果没有就会报错，告诉你必须为`dependency`声明一个`version`；

如果`<dependencies>`下的`<dependency>`声明了`version`元素，那么无论`<dependencyManagement>`下的`<dependencies>` 标签下`<dependency>`有没有生命对该`artifactId`和`groupId`的版本声明，都会以`<dependencies>`下的`<dependency>`声明了`version`元素为准。

使用示例：

```
pom.xml  
//只是对版本进行管理，不会实际引入jar  
<dependencyManagement>  
      <dependencies>  
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-core</artifactId>  
                <version>3.2.7</version>  
            </dependency>  
    </dependencies>  
</dependencyManagement>  
  
//会实际下载jar包  
<dependencies>  
       <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-core</artifactId>  
       </dependency>  
</dependencies>
```



参考：

1）[Maven 中 dependencyManagement 标签使用](https://www.jianshu.com/p/ee15cda51d9d)；

2）[Maven中的dependencyManagement 意义](https://www.cnblogs.com/mr-wuxiansheng/p/6189438.html)；



## 2.`<packaging>`标签含义？

使用形式如下：

```
#pom.xml
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ddmit</groupId>
    <artifactId>dashcloud</artifactId>
    <packaging>pom</packaging>
    <version>0.0.1-SNAPSHOT</version>

    <modules>
        <module>dash-discovery</module>
        <module>dash-gateway</module>
        <module>dash-config</module>
        <module>dash-auth</module>
        <module>dash-common</module>
        <module>dash-modules</module>
    </modules>
```

如上面的`<packaging>pom</packaging>`用法所示，该配置的意思是使用maven分模块管理，用在父级工程或聚合工程中，用来做项目的子模块的整合、jar包的版本控制。

除了`<packaging>pom</packaging>`，还有其他两种类型：`jar`和`war`。区别如下：

- `pom` ：父类型都为pom类型；
- `jar `： 内部调用或者是作服务使用；
- `war `： 需要部署的项目；
