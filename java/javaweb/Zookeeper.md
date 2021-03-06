# Zookeeper

系统的介绍了分布式的问题以及解决办法

主要参考倪超的 *从paxos到zookeeper* ,并进行了一定的总结和分析

## 问题的提出

### 问题

在书的开头提出了这样几个问题:

1. 数据的一致性

	- 我们需要知道的是:用户对于数据一致性的需求是不一样的.
	- 有些系统可以允许延迟记录到数据库,但展示给修改用户时,必须显示已经修改的内容;而另一部分用户可以展示过去的未修改的数据
	- 有些系统需要给用户保证数据的绝对安全,可以处理的缓慢一些,但是展示的内容必须和实际一致

2. 数据持久化式的并发性

	- 更新的并发性:当多个服务/线程同时修改一个(内存/数据库)变量时,会产生的问题

3. 分布式的其他问题

	- 单点故障
	- 负载均衡
	- ...

### 分布一致性

1. 强一致性

	- 即系统写入什么,读出来就是什么
	- 用户体验好,但是时间/性能要求很高 

2. 弱一致性

	- 在系统写入成功后,不承诺可以立即读到所写入的值,也不承诺什么时候能保持数据一致,当尽可能保证到某个时间级别后,数据能达到一致状态

	在弱一致性中,还可以进行细分
	
	- 会话一致性: 在同一个客户端会话可以得到一致的值,但其它的会话不能保证
	- 用户一致性: 在同一个用户中可以读到一致的值,但其它的用户不保证


3. 最终一致性

	除了以上的两种,还有一种弱一致性的特例,就是**最终一致性**

	- 系统会保证一定时间内,能够达到一个数据一致的状态.
	- 它是弱一致性中,一个非常重要的一致性模型,也是业界大型分布式系统的数据一致性上比较推崇的模型


## 第一章 分布式架构


###  从集中式到分布式

#### 分布式的特点

1. 分布性  
	
	- 多台计算机在空间上随机分布
	- 机器的分布情况会随时时间变动

2. 对等性

	这里有一个```副本 Replica```的概念,指的是分布式系统对数据和服务提供的一种冗余方式.

	- 数据副本: 在不同节点上持久化同一份数据,在某个节点数据丢失时,可以从其他节点获取数据
	- 服务副本: 多个节点提供同样的服务


3. 并发性

	- 多个服务会并发的操作一些共享的资源,如数据库/缓存... ,我们要解决的是 如何高效地协调分布式的并发操作

	
4. 缺乏全局时钟

	这个问题我也没有想过
	
	书中说:在分布式系统中很难定义两个时间谁先谁后,因为分布式系统缺少一个全局的时钟序列控制
	
	
5. 故障发生

	在系统设计时考虑到的可能的异常,在实际运行多了之后一定会发生,所以尽量在开发中解决.
	

#### 分布式环境问题

1. 各个节点的通信问题

	- 遇到网络延迟
	- 数据丢失
	
2. 网络分区

	- 一部分节点网络延迟过大,导致一部分节点需要完成整个分布式系统的任务

3. 三态

	- 指: 成功,失败,超时
	- 在分布式系统中,由于网络是不可靠的,经常会出现超时问题
	
4. 节点故障


### 事务

#### 事务的4个特性 ACID

- 原子性 Atomicity
- 一致性 Consistency
- 隔离性 Isolation

	- Serializable (串行化)
	- Repeatable read (可重复读)
	- Read committed (读已提交)
	- Read uncommitted (读未提交) 
	
- 持久性 Durability

这里仅介绍幻读

> 比如事务A 将 T表 中所有工资不到 10000元 的员工的工资改为10000元，在事务A执行结束尚未提交时，事务B又插入了一条（或删除操作）工资不满10000元的员工记录，然后再提交事务A，事务B。事务A好像发生了幻觉，没有操作成功一样，这是因为“可重复读”锁定的是，已经读取的记录，而不是锁定整张表（或者超出读取范围的数据），可重复读限制了事务本身涉及数据的update行为，但是无法限制事务自身以外的数据。我们可以使用表锁或者范围锁。**参考**:[数据库中隔离性的四种级别详解与例子](https://zhuanlan.zhihu.com/p/25014845)

其他具体含义参考:[数据库事务的四大特性以及事务的隔离级别](https://www.cnblogs.com/fjdingsd/p/5273008.html)

<img src="../gif/isolation.jpg" width="70%" height="70%">

#### 常见数据库默认级别

| 数据库名称 | 默认级别 |
| --------   | -----:|
| Mysql      |  REPEATABLE_READ  |
| PostgreSql | READ_COMMITED    |
| Oracle     | READ_COMMITED    |


**Mysql**

查看隔离等级: ```select @@tx_isolation;```

修改隔离等级的语句格式:

```set [ global | session ] transaction isolation level Read uncommitted | Read committed | Repeatable | Serializable```

- 如果什么都不写，意思是此语句将应用于当前session内的下一个还未开始的事务。

```SET session TRANSACTION ISOLATION LEVEL repeatable read;```

 - 此语句将应用于当前session内之后的所有事务。


```SET global TRANSACTION ISOLATION LEVEL read committed;```

- 此语句将应用于之后的所有session，而当前已经存在的session不受影响。

**PostgreSQL**

查看默认隔离等级:```show default_transaction_isolation;```

查看当前隔离等级:``` show transaction_isolation;```

#### 分布式事务

在分布式事务中,各个子事务的执行是分布式的.

[如何用消息系统避免分布式事务？](http://blog.jobbole.com/89140/)

#### CAP 定理

CAP 定理:

> 一个分布式系统不可能同时满足一致性 consistency ,可用性 availability,分区容错性partition tolerance这三个需求,最多只能同时满足其中的两项.

#### BASE 理论

> basically available 基本可用, soft state 软状态, eventually consistent 最终一致性 

BASE 是对 CAP定理 权衡的结果

## 第二章 一致性协议

### 2PC/3PC  二阶段提交协议/三阶段提交协议

#### 2PC

2PC 即 two-phase commit

问题:

1. 同步阻塞,参与者在等待其他参与者响应时无法操作
2. 一旦协调者出现问题,系统崩溃
3. 数据不一致
4. 太过保守,任何一个节点失败都会导致整个事务的失败


#### 3PC  ---- 2PC 的改进版

分为: canCommit ,PreCommit , Do commit 三个阶段


### Paxos 算法











 






	
	
