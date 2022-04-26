# Redis高级

redis有两种持久化方式

+ RDB
+ AOF

### RDB持久化

RDB全称Redis Database Backup file（Redis数据备份文件），也被叫做Redis数据快照。简单来说就是把内存中的所有数据都记录到磁盘中。当Redis实例故障重启后，从磁盘读取快照文件，恢复数据。快照文件称为RDB文件，默认是保存在当前运行目录。

RDB持久化在四种情况下会执行：

- 执行save命令
- 执行bgsave命令
- Redis停机时
- 触发RDB条件时

**1）save命令**

这个命令由redis主线程来进行，这个命令执行的时候其他的命令都会被阻塞

**2）bgsave命令**

这个命令是reids开启子线程来执行RDB，主进程可以持续处理用户请求，不受影响。

**3）停机时**

Redis停机时会执行一次save命令，实现RDB持久化。

（ctrl+c）的时候会执行

**4）触发RDB条件**

Redis内部有触发RDB的机制，可以在redis.conf文件中找到，格式如下：

```properties
# 900秒内，如果至少有1个key被修改，则执行bgsave ， 如果是save "" 则表示禁用RDB
save 900 1  
save 300 10  
save 60 10000 
```

RDB的其它配置也可以在redis.conf文件中设置：

```properties
# 是否压缩 ,建议不开启，压缩也会消耗cpu，磁盘的话不值钱
rdbcompression yes

# RDB文件名称
dbfilename dump.rdb  

# 文件保存的路径目录
dir ./ 
```

**RDB方式bgsave的基本原理**

+ fork主进程得到一个子进程，共享内存空间。
+ 子进程读取内存数据并写入新的RDB文件
+ 用新的RDB文件替换旧的RDB文件。

**RDB的缺点？**

- RDB执行间隔时间长，两次RDB之间写入数据有丢失的风险
- fork子进程、压缩、写出RDB文件都比较耗时

## AOF持久化

AOF全称为Append Only File（追加文件），Redis处理的每一个写命令都会记录在AOF文件，可以看做是命令日志文件，AOF默认是关闭的，需要配置开启AOF。

### AOF配置

AOF默认是关闭的，需要修改redis.conf配置文件来开启AOF：

```properties
# 是否开启AOF功能，默认是no
appendonly yes
# AOF文件的名称
appendfilename "appendonly.aof"
```

AOF的命令记录的频率也可以通过redis.conf文件来配：

```properties
# 表示每执行一次写命令，立即记录到AOF文件
appendfsync always 
# 写命令执行完先放入AOF缓冲区，然后表示每隔1秒将缓冲区数据写到AOF文件，是默认方案
appendfsync everysec 
# 写命令执行完先放入AOF缓冲区，由操作系统决定何时将缓冲区内容写回磁盘
appendfsync no
```

### **AOF原理**

AOF就是记录命令，每操作一次就将命令记录到AOF文件中

### AOF文件重写

因为是记录命令，**AOF文件会比RDB文件大的多**。

AOF会记录对同一个key的多次写操作，但只有**最后一次写操作**才有意义。

通过执行bgrewriteaof命令，可以让AOF文件执行重写功能，用最少的命令达到相同效果。

## RDB与AOF对比

RDB和AOF各有自己的优缺点，如果对数据安全性要求较高，在实际开发中往往会**结合**两者来使用

|              | RDB                          | AOF                  |
| ------------ | ---------------------------- | -------------------- |
| 持久化方式   | 定时对内存做快照             | 记录每一次执行的命令 |
| 文件大小     | 因为是数据，所以小           | 记录命令，文件大     |
| 宕机恢复速度 | 快                           | 慢                   |
| 系统资源占用 | 高，大量内存和cpu消耗        | 低，主要是磁盘IO资源 |
| 使用场景     | 可以容忍一些数据丢失，速度快 | 对数据安全性要求较高 |

# Redis主从

## 主从数据同步原理

### 2.2.1.全量同步

主从第一次建立连接时，会执行**全量同步**，将master节点的所有数据都拷贝给slave节点，流程：

![image-20210725152222497](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725152222497.png)

这里有一个问题，master如何得知salve是第一次来连接呢？？

有几个概念，可以作为判断依据：

- <u>**Replication Id**：简称replid，是数据集的标记，id一致则说明是同一数据集。每一个master都有唯一的replid，slave则会继承master节点的replid</u>
- <u>**offset**：偏移量，随着记录在repl_baklog中的数据增多而逐渐增大。slave完成同步时也会记录当前同步的offset。如果slave的offset小于master的offset，说明slave数据落后于master，需要更新。</u>

因此slave做数据同步，必须向master声明自己的replication id 和offset，master才可以判断到底需要同步哪些数据。



因为slave原本也是一个master，有自己的replid和offset，当第一次变成slave，与master建立连接时，发送的replid和offset是自己的replid和offset。

master判断发现slave发送来的replid与自己的不一致，说明这是一个全新的slave，就知道要做全量同步了。

master会将自己的replid和offset都发送给这个slave，slave保存这些信息。以后slave的replid就与master一致了。

因此，**master判断一个节点是否是第一次同步的依据，就是看replid是否一致**。

如图：

![image-20210725152700914](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725152700914.png)



简述全量同步的流程：

- slave节点请求增量同步
- master节点判断replid，发现不一致，拒绝增量同步
- master将完整内存数据生成RDB，发送RDB到slave
- slave清空本地数据，加载master的RDB
- master将RDB期间的命令记录在repl_baklog，并持续将log中的命令发送给slave
- slave执行接收到的命令，保持与master之间的同步



### 2.2.2.增量同步

全量同步需要先做RDB，然后将RDB文件通过网络传输个slave，成本太高了。因此除了第一次做全量同步，其它大多数时候slave与master都是做**增量同步**。

什么是增量同步？就是只更新slave与master存在差异的部分数据。如图：

![image-20210725153201086](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725153201086.png)

那么master怎么知道slave与自己的数据差异在哪里呢?

### 2.2.3.repl_backlog原理

master怎么知道slave与自己的数据差异在哪里呢?

这就要说到全量同步时的repl_baklog文件了。

这个文件是一个固定大小的数组，只不过数组是环形，也就是说**角标到达数组末尾后，会再次从0开始读写**，这样数组头部的数据就会被覆盖。

repl_baklog中会记录Redis处理过的命令日志及offset，包括master当前的offset，和slave已经拷贝到的offset：

![image-20210725153359022](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725153359022.png) 

slave与master的offset之间的差异，就是salve需要增量拷贝的数据了。

随着不断有数据写入，master的offset逐渐变大，slave也不断的拷贝，追赶master的offset：

![image-20210725153524190](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725153524190.png) 



直到数组被填满：

![image-20210725153715910](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725153715910.png) 

此时，如果有新的数据写入，就会覆盖数组中的旧数据。不过，旧的数据只要是绿色的，说明是已经被同步到slave的数据，即便被覆盖了也没什么影响。因为未同步的仅仅是红色部分。



但是，如果slave出现网络阻塞，导致master的offset远远超过了slave的offset： 

![image-20210725153937031](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725153937031.png) 

如果master继续写入新数据，其offset就会覆盖旧的数据，直到将slave现在的offset也覆盖：

![image-20210725154155984](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725154155984.png) 



棕色框中的红色部分，就是尚未同步，但是却已经被覆盖的数据。此时如果slave恢复，需要同步，却发现自己的offset都没有了，无法完成增量同步了。只能做全量同步。

![image-20210725154216392](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/image-20210725154216392.png)

### 主从同步优化

- 在master中配置repl-diskless-sync yes启用无磁盘复制，避免全量同步时的磁盘IO。
- Redis单节点上的内存占用不要太大，减少RDB导致的过多磁盘IO
- 适当提高repl_baklog的大小，发现slave宕机时尽快实现故障恢复，尽可能避免全量同步
- 限制一个master上的slave节点数量，如果实在是太多slave，则可以采用**主-从-从链式**结构，减少master压力

## 小结

**简述全量同步和增量同步区别？**

- 全量同步：master将完整内存数据生成RDB，发送RDB到slave。后续命令则记录在repl_baklog，逐个发送给slave。
- 增量同步：slave提交自己的replid  offset到master，master获取repl_baklog中从offset之后的命令给slave

**什么时候执行全量同步？**

- slave节点第一次连接master节点时
- slave节点断开时间太久，repl_baklog中的offset已经被覆盖时

**什么时候执行增量同步？**

- slave节点断开又恢复，并且在repl_baklog中能找到offset时

# Redis哨兵

redis哨兵是自动发现集群故障并且自动修复的技术

哨兵的作用如下：

- **监控**：Sentinel 会不断检查您的master和slave是否按预期工作
- **自动故障恢复**：如果master故障，Sentinel会将一个slave提升为master。当故障实例恢复后也以新的master为主
- **通知**：Sentinel充当Redis客户端的服务发现来源，当集群发生故障转移时，会将最新信息推送给Redis的客户端

### 集群监控原理

Sentinel基于心跳机制监测服务状态，每隔1秒向集群的每个实例发送ping命令：

+ 主观下线：单台sentinel收不到节点的响应就认为这台服务器**主观下线**了。
+ 客观下线：超过一半sentinel都收不到系欸但的响应就认为这台服务器**客观下线**，代表这台服务器真的挂了。

### 集群故障恢复原理

### **如何选举新的master？**

- 1首先会判断slave节点与master节点断开时间长短，如果超过指定值（down-after-milliseconds * 10）则会排除该slave节点
- 2然后判断slave节点的slave-priority值，越小优先级越高，如果是0则永不参与选举
- 3如果slave-prority一样，则判断slave节点的offset值，越大说明数据越新，优先级越高
- 4最后是判断slave节点的运行id大小，越小优先级越高。

**如何实现故障转移？**

- sentinel给备选的slave1节点发送slaveof no one命令，让该节点成为master
- sentinel给所有其它slave发送slaveof 192.168.136.129 7002 命令，让这些slave成为新master的从节点，开始从新的master上同步数据。
- 最后，sentinel将故障节点标记为slave，当故障节点恢复后会自动成为新的master的slave节点

**1**Sentinel给选举出来的slave命令，让他成为大哥；

**2**然后给其他小弟发命令，让这些小弟认刚才这个大哥为大哥；

**3**把刚刚坏掉的大哥变成小弟，如果大哥醒了就让他继续当小弟。

### 小结

**Sentinel的三个作用是什么？**

- 监控
- 故障转移
- 通知

**Sentinel如何判断一个redis实例是否健康？**

- 每隔1秒发送一次ping命令，如果超过一定时间没有相向则认为是主观下线
- 如果大多数sentinel都认为实例主观下线，则判定服务下线

**故障转移步骤有哪些？**

- 首先选定一个slave作为新的master，执行slaveof no one
- 然后让所有节点都执行slaveof 新master
- 修改故障节点配置，添加slaveof 新master

## RedisTemplate【重要】

在Sentinel集群监管下的Redis主从集群，其节点会因为自动故障转移而发生变化，Redis的客户端必须感知这种变化，及时更新连接信息。Spring的RedisTemplate底层利用lettuce实现了节点的感知和自动切换。

下面，我们通过一个测试来实现RedisTemplate集成哨兵机制。

### 3.3.1.导入Demo工程



在项目的pom文件中引入依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### 3.3.3.配置Redis地址

然后在配置文件application.yml中指定redis的sentinel相关信息：

```yaml
spring:
  redis:
    sentinel:
      master: mymaster
      nodes:
        - 192.168.136.129:27001
        - 192.168.136.129:27002
        - 192.168.136.129:27003
```

### 3.3.4.配置读写分离

在项目的启动类中，添加一个新的bean：

```java
@Bean
public LettuceClientConfigurationBuilderCustomizer clientConfigurationBuilderCustomizer(){
    return clientConfigurationBuilder -> clientConfigurationBuilder.readFrom(ReadFrom.REPLICA_PREFERRED);
}
```

这个bean中配置的就是读写策略，包括四种：

- MASTER：从主节点读取
- MASTER_PREFERRED：优先从master节点读取，master不可用才读取replica
- REPLICA：从slave（replica）节点读取
- REPLICA_PREFERRED：优先从slave（replica）节点读取，所有的slave都不可用才读取master



# 4.Redis分片集群

## 4.1.搭建分片集群

主从和哨兵可以解决高可用、高并发读的问题。但是依然有两个问题没有解决：

- 海量数据存储问题

- 高并发**写**的问题

使用分片集群可以解决上述问题，如图:

![image-20210725155747294](C:/Users/1/Desktop/employment/16Redis高级/讲义/md/assets/1648739569538.png) 



**分片集群特征：**

- 集群中有多个master，每个master保存不同数据

- 每个master都可以有多个slave节点

- master之间通过ping监测彼此健康状态

- 客户端请求可以访问集群任意节点，最终都会被转发到正确节点

### 散列插槽（重点）

Redis会把每一个master节点映射到0~16383共**16384**个插槽（hash slot）上，查看集群信息时就能看到：

【0-5460】【5461-10922】【10923-16383】

数据key不是与节点绑定，而是与插槽绑定。redis会根据key的有效部分计算插槽值，分两种情况：

- key中包含"{}"，且“{}”中至少包含1个字符，“{}”中的部分是有效部分
- key中不包含“{}”，整个key都是有效部分

**eg:key是num ,那就根据num  hash计算，如果是{itcast}num，则根据itcast  hash计算。计算方式是利用CRC16算法得到一个hash值，然后对16384取余，得到的结果就是slot值。**

根据插槽(slot)值储存到对应节点