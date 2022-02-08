## 1.事务

### 1.1 什么是数据库事务？

数据库事务是指作为单个逻辑工作单元的一系列操作，这些操作，要么都执行，要么都不执行，是一个不可分割的工作单位。事务的开始和结束可以由用户显式的定义，也可以由DBMS自动划分事务，事务具有原子性(Atomicity)、一致性(Consistency)、隔离性(Isolation)和持久性(Durabilily)的特点(ACID特性)。

### 1.2 什么是本地事务？

基于单个服务单一数据库资源访问的事务，被称为本地事务(Local Transaction)。紧密依赖于底层资源管理器(例如数据库连接)，事务处理局限在当前事务资源内。此种事务处理方式不存在对应用服务器的依赖，因而部署灵活却无法支持多数据源的分布式事务。在数据库连接中使用本地事务示例如下：

```java
    public void transferAccount() {
        Connection conn = null;
        Statement stmt = null;
        try {
            conn = getDataSource().getConnection();
            // 将自动提交设置为 false，若设置为 true 则数据库将会把每一次数据更新认定为一个事务并自动提交
            conn.setAutoCommit(false);
            stmt = conn.createStatement();
            // 将 A 账户中的金额减少 500
            stmt.execute("update t_account set amount = amount - 500 where account_id = 'A'");
            // 将 B 账户中的金额增加 500
            stmt.execute("update t_account set amount = amount + 500 where account_id = 'B'");
            // 提交事务
            conn.commit();
            // 事务提交：转账的两步操作同时成功
        } catch (SQLException sqle) {
            // 发生异常，回滚在本事务中的操做
            conn.rollback();
            // 事务回滚：转账的两步操作完全撤销
            stmt.close();
            conn.close();
        }
    }
```

### 1.3 什么是分布式事务？

分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上。简单的说，就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

参考：

1. https://juejin.cn/post/6844903647197806605；
2. https://www.cnblogs.com/yuyyg/p/14511641.html；