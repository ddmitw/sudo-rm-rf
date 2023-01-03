## 1.配置项？

配置示例：

```
spring:
  activiti:
    history-level: full
    db-history-used: true
    check-process-definitions: false
    deployment-mode: never-fail
    database-schema-update: true
```

* spring.activiti.history-level： 历史记录存储等级，可配置的历史级别有none, activity, audit, full。none - 不保存任何的历史数据，因此，在流程执行过程中，这是最高效的；activity - 级别高于none，保存流程实例与流程行为，其他数据不保存；audit - 除activity级别会保存的数据外，还会保存全部的流程任务及其属性，audit为history的默认值；full - 保存历史数据的最高级别，除了会保存audit级别的数据外，还会保存其他全部流程相关的细节数据，包括一些流程参数等。
* spring.activiti.db-history-used： 检测历史表是否存在，activiti7默认不生成历史信息表，不开启此项的话，会少生成8张历史表。
* spring.activiti.check-process-definitions： 校验流程文件，默认校验resources下的processes文件夹里的流程文件，默认true。
* spring.activiti.deployment-mode： 解决Springboot2整合Activiti7解决SpringAutoDeployment自动部署。
* spring.activiti.database-schema-update： 设置流程引擎启动和关闭时，处理数据库模式的策略，可配置项有false、true、create-drop，默认为false，。

参考： 

1. [Activiti User Guide- activiti 6](https://www.activiti.org/userguide/#databaseConfiguration)。
2. [Springboot2整合Activiti7解决SpringAutoDeployment自动部署](https://blog.csdn.net/qq_43719932/article/details/115048588)。
