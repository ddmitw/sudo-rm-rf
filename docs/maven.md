## 1.Maven中dependencyManagement标签使用？

在Maven中`dependencyManagement`的作用其实相当于一个对所依赖jar包进行版本管理的管理器，该标签只是对依赖及版本的声明，不会进行实际依赖的下载。

在maven中，获取依赖的版本有两种途径：

* `<dependencyManagement>`下的`<dependencies>`标签下`<dependency>`定义的版本；
* `<dependencies>`下的`<dependency>` 定义的版本；

如果`<dependencies>`下的`<dependency>`没有声明`version`元素，就会到`<dependencyManagement>`下的`<dependencies>`标签下`<dependency>`找有没有对该`artifactId`和`groupId`进行过版本声明，如果有，就继承它，如果没有就会报错，告诉你必须为`dependency`声明一个`version`；

如果`<dependencies>`下的`<dependency>`声明了`version`元素，那么无论`<dependencyManagement>`下的`<dependencies>`标签下`<dependency>`有没有生命对该`artifactId`和`groupId`的版本声明，都会以`<dependencies>`下的`<dependency>`声明了`version`元素为准。

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

1）[Maven中dependencyManagement 标签使用](https://www.jianshu.com/p/ee15cda51d9d)；
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



## 3.Maven中`<scope>import</scope>` 和 `<type>pom</type>`的用法？

这两个标签用于标记`<dependency>`的作用范围和类型。

**用法1**

在分模块的项目中，父级模块pom中的`dependencyManagement` 标签中需要导入另一个pom中的`dependencyManagement`的时候，必须同时使用`<scope>import</scope> `和 `<type>pom</type>`。

```xml
    <!-- 依赖声明 -->
    <dependencyManagement>
        <dependencies>
            <!-- SpringCloud 微服务 -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

在上面的pom中，父级pom，负责管理依赖的版本，其中父级pom想引入`spring-cloud-dependencies`的pom，从而能把spring-cloud的所以依赖引入进来。`spring-cloud-dependencies`也是一个父级pom，通过`dependencyManagement`管理了一系列依赖。这种情形就必须同时使用`<scope>import</scope> `和 `<type>pom</type>`。

 这个时候，该pom中`dependencyManagement`就会包含导入的`spring-boot-dependencies`中的所有`dependencyManagement`。

这是为了解决pom类型的父工程单继承的问题，通过导入，可以导入各种其他父工程的`dependencyManagement`

*注意*：`dependencyManagement`只在父工程（即pom类型的maven工程）中声明，在子工程中定义无需声明版本从而生效。如果在jar类型的maven工程中添加了`dependencyManagement`，是没有意义的。



**用法2**

在一个子模块中，当需要把一些依赖定义到一个pom工程中，但由于maven单继承机制，该模块又想通过依赖引入该pom工程中的所有依赖，只需要添加`<type>pom</type>`。

```xml
<dependencies>
　　<dependency>
　　　<groupId>org.sonatype.mavenbook</groupId>  　　　  
	  <artifactId>persistence-deps</artifactId>  　　　  
	  <version>1.0</version>
　　　<type>pom</type>
　　</dependency> 
</dependencies>
```

这是为了解决子工程单继承的问题，通过`<type>pom</type>`可以依赖于其他的pom父工程，从而将pom工程中的依赖都传递过来

type默认是jar，依赖jar工程时可以不写type标签，所以如果依赖于一个jar工程，而jar工程中包含大量的依赖，也会一起传递过来，这也就是maven依赖传递的原理。

参考：

1. [Maven中import pom用法](https://www.cnblogs.com/cainiao-Shun666/p/15822262.html)
2. [Introduction to the Dependency Mechanism](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html#Dependency_Scope) ；


## 4.`pom.xml`中的`maven.compiler.source`和`maven.compiler.target`作用？

`pom.xml`中的`maven.compiler.source`和`maven.compiler.target`是用来指定编译源码和打包的JDK版本的。通常它们的版本等于系统JDK版本，在不能控制客户机的jdk版本的情况下，而想让包的适用性更广的话，可以手动降低版本号，比如如从11降到8。

使用：

```
  [...]
  <properties>
   <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  [...]


  <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

参考：

1. [Setting the -source and -target of the Java Compiler](https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html)。


## 5.`pom.xml`中`repositories`及`pluginRepositories`使用？

如果想让某个指定的maven项目，不使用maven的`setting.xml`文件中配置的仓库地址。可以在父级pom.xml文件里通过`repositories`和`pluginRepositories`来指定仓库的远程地址。

```
   <repositories>
        <!-- 阿里云仓库 -->
        <repository>
            <id>public</id>
            <name>aliyun nexus</name>
            <url>https://maven.aliyun.com/repository/public</url>
            <releases>
                <enabled>true</enabled>
            </releases>
        </repository>

         <!-- activiti工作流 -->
        <repository>
            <id>activiti-releases</id>
            <url>https://artifacts.alfresco.com/nexus/content/repositories/activiti-releases</url>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <id>public</id>
            <name>aliyun nexus</name>
            <url>https://maven.aliyun.com/repository/public</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>
```