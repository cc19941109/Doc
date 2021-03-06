# Mysql 一条查询是如何完成的

## 查询过程

- server 层
  - 连接器
  - 缓存
  - 分析器
  - 优化器
  - 执行器
- 存储引擎

### server 层

涵盖mysql的大多数核心服务功能, 所有跨存储引擎的功能都在这层实现

1. 所有内置函数
2. 存储过程, 触发器, 视图

#### 连接器

连接器负责跟客户端建立连接、获取权限、维持和管理连接

1. 为了加快查询客户端, 减少建立连接时的耗时, 往往使用长连接的方式, 并用连接池管理.
2. 当使用长连接时, mysql 执行过程中临时使用的内存是管理在连接对象中的, 在连接断开时才会释放, 所以当长连接积累下来可能导致内存占用过大, 被系统强行杀掉, 解决方案如下
  - 定期断开长连接, 或执行过一个占用内存的大请求后, 断开连接
  - `mysql5.7` 以上可以用 `mysql_reset_connection`来重新初始化连接资源

#### 查询缓存

mysql 拿到一个查询请求后, 会先到查询缓存找时候执行过这条语句

如果语句不在查询缓存中, 才继续执行后面的步骤

**但不建议使用查询缓存**, 原因如下

查询缓存的失效非常频繁，只要有对一个表的更新，这个表上所有的查询缓存都会被清空

另外, 我查看了 `mysql 8`, 最新版本的mysql 已经停用对于查询缓存的支持, 其中原因有很多

具体详见 [MySQL 8.0：停用对查询缓存的支持](https://mysqlserverteam.com/mysql-8-0-retiring-support-for-the-query-cache/) , [查询缓存的运行方式](https://dev.mysql.com/doc/refman/5.7/en/query-cache-operation.html) 下面我列出其中一些原因: 

 	1.  查询必须逐字节匹配（查询缓存避免解析）
 	2.  查询缓存的适用范围有限, 包括用户自定义的函数/变量, 分区表,临时表等都不能使用缓存
 	3.  当缓存靠近客户端时, 能获得更好的性能

#### 分析器

词法解析

1. 检查语法, 表名/列名是否正确 Oracle 中也是这么做的

#### 优化器

优化器在表中有多个索引时, 决定使用哪个索引, 或者在多表join时, 决定各个表的连接顺序

#### 执行器

1. 权限验证
2. 取下一行

数据库中的慢查询日志中, 能看到一个 `rows_examined`, 表示在这个语句执行过程中扫描了多少行

关于为什么要在执行器中权限认证, 而不是在分析器中做 ?

有些时候, sql语句要操作的表并不在 sql 的字段上, 如触发器. 需要在执行器阶段中才能确定要操作的表.


### 存储引擎

复杂数据的存储和提取, 默认为 `innoDB`











