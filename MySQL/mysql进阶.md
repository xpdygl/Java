# 外键约束

有时一个表中的信息会产生冗余，此时需要将一个表拆分成两个表就可以解决冗余问题。



外键: 从表中的某个字段,该字段的值是引用主表中主键的值
主表： 约束别人的表
副表/从表： 被别人约束的表

外键约束的作用：维护多个表之间的联系。

```sql
1. 新建表时增加外键：
[CONSTRAINT] [外键约束名称] FOREIGN KEY(外键字段名) REFERENCES 主表名(主键字段名)
关键字解释：
CONSTRAINT -- 约束关键字
FOREIGN KEY(外键字段名) –- 某个字段作为外键
REFERENCES -- 主表名(主键字段名) 表示参照主表中的某个字段

2. 已有表增加外键：
ALTER TABLE 从表名 ADD [CONSTRAINT] [外键约束名称] FOREIGN KEY (外键字段名) REFERENCES 主表(主键字段名);
```

constraint  user_orders_id  foreign key (orders_id)  references  user(id);

​						外键约束的名字						被约束的字段名			主键名

__有了外键约束就不能添加非法字段信息进表__



## 外键的级联

在修改和删除主表的主键时，同时更新或删除副表的外键值，称为级联操作
`ON UPDATE CASCADE` -- 级联更新，主表主键发生更新时，外键也会更新
`ON DELETE CASCADE` -- 级联删除，主键主键发生删除时，外键也会删除

具体操作：

* 删除employee表
* 重新创建employee表，添加级联更新和级联删除

```sql
CREATE TABLE employee (
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(30),
	age INT,
	dep_id INT,
	CONSTRAINT employee_dep_fk FOREIGN KEY (dep_id) REFERENCES department(id) ON UPDATE CASCADE ON DELETE CASCADE
);
```

此时更改就能成功。



# 多表间的关系

一对一

直接建立一张表就行。



一对多

班级和学生，部门和员工，客户和订单

一的一方: 班级  部门  客户  

多的一方:学生  员工   订单   

**一对多建表原则**: 在从表(多的一方)创建一个字段,该字段作为外键指向主表(一的一方)的主键





多对多：一个老师可以有多个学生,一个学生也可以有多个老师  多对多的关系

多对多关系建表原则: **需要创建一张中间表，中间表中至少两个字段，这两个字段分别作为外键指向各自一方的主键。**





一对一， 一般不使用 因为在一个表中就可以完成一对一的关系
多对一， 例如班级和学生 ，班级是只有一个班级的，但是可以有很多学生在里面上课。可以用外键将二者连接
__语法：select ... from 表一 where 条件1 in(select ... from 表一 where条件2)；__

多对多， 多对多就像学生和课程。学生可以选很多课 很多课也会有不同的学生。
需要独立构建一个表来连接学生表和课程表，这个表至少有两个字段分别作为外键指向各自一方的主键。
子查询就是嵌套查询

__语法:select ... from (子查询) 别名 ....



# 内连接：

select ... from 表一，表二 where user.id = orders.id;

内连接通过外键相连，来查询交集 得到的就是他们公共的部分



# 左外连接

Select ... from 表一 left  join 表二  on 条件；
通过给定left，来保证左边的表一定全部都会查找出来 ，然后 通过后面的条件把公共的部分查询出来
有时候出现空的情况是因为左边的表有这个字段，但是右边没有相对应的字段
所以用左外连接时，右边有可能会出现是null的情况 但是左边是一定不会出现空的情况的

# 右外连接
Select ... from 表一 right join 表二  on 条件；

通过给定right，来保证左边的表一定全部都会查找出来 ，然后 通过后面的条件把公共的部分查询出来
有时候出现空的情况是因为右边的表有这个字段，但是左边没有相对应的字段
所以用右外连接时，左边有可能会出现是null的情况 但是右边是一定不会出现空的情况的



# 子查询



## 什么是子查询

- **一个查询语句的结果作为另一个查询语句的条件**
- 有查询的嵌套，内部的查询称为子查询
- **子查询要使用括号**

子查询的关系有三种



### 子查询的结果是一个值的时候

子查询结果只要是`单个值`，肯定在`WHERE`后面作为`条件`
`SELECT ... FROM 表 WHERE 字段 [=,>,<,<>,...]（子查询）;`



### 子查询结果是单列多行的时候

子查询结果只要是`单列多行`，肯定在`WHERE`后面作为`条件`
子查询结果是单列多行，结果集类似于一个数组，父查询使用`IN`运算符
`SELECT ... FROM 表 WHERE 字段 IN （子查询）;`



### 子查询的结果是多行多列

子查询结果只要是`多行多列`，肯定在`FROM`后面作为`表`
`SELECT ... FROM （子查询） 表别名 WHERE 条件;`
**子查询作为表需要取别名，否则这张表没用名称无法访问表中的字段**



# 事务

事务的意思就是一组逻辑上的代码一起完成操作，或者一起不完成操作。

例如张三向李四转账

这在数据库中是两个操作

1.更新张三扣钱

2.更新李四加钱

万一出现张三扣完钱后出现异常就会发生张三成功扣钱李四却没加钱的情况。

所以这种情况要开启一个事务

__start transaction;开启事务__

进行操作1

进行操作2

进行操作3

__Commit__

这样就保证能让一种操作全部成功或者全部失败。



#### 回滚点【了解】

##### 3.3.1什么是回滚点

​	在某些成功的操作完成之后，后续的操作有可能成功有可能失败，但是不管成功还是失败，前面操作都已经成功，可以在当前成功的位置设置一个回滚点。可以供后续失败操作返回到该位置，而不是返回所有操作，这个点称之为回滚点。

| Savepoint 名字     | 设置回滚点 |
| ------------------ | ---------- |
| rollback      名字 | 回到回滚点 |

```sql
1)    将数据还原到1000
2)    开启事务
      start transaction;
3)    让张三账号减3次钱 
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
4)    设置回滚点
      savepoint abc;
5)    让张三账号减4次钱
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
6)    回到回滚点
      rollback to abc;
7)    让张三账号减4次钱
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
      update account set money = money - 100 where name = 'zs';
8)    结束事务
      commit;
```

最终结果为400元。







```
必须练习:
	1.外键的添加和删除
    2.多表关系的分析以及多表的建表原则
    3.连接查询(内连接和外连接)   重点
    4.子查询   重点
    5.事务的操作(开启事务,提交事务,回滚事务)   重点
        
- 能够理解外键约束
    作用: 一张主表的主键来约束一张从表外键的值
	语法: constraint 外键名 foreign key(从表字段名) references 主表名(主表主键)  
	添加外键:
	alter table 表名 add constraint 外键名 foreign key(从表字段名) references 主表名(主表主键);
	create table 表名(
			字段名 字段类型 字段约束,
			字段名 字段类型 字段约束,
			...
			constraint 外键名 foreign key(从表字段名) references 主表名(主表主键)
	);
	删除外键:
		alter table 表名 drop foreign key 外键名;
	外键级联操作:
		on update cascade
		on delete cascade
            
- 能够说出多表之间的关系及其建表原则
    一对多: 在多的一方创建一个字段作为外键,指向一的一方的主键
	多对多: 创建一张中间表,至少有2个字段都作为外键,分别指向各自一方的主键
	一对一: 直接创建一张表	
            
- 能够使用内连接进行多表查询
 内连接:
	隐式内连接: select ... from 表1,表2 where 关联条件; (从表外键的字段值等于主表主键的值)
	显式内连接: select ... from 表1 [inner] join 表2 on 关联条件; (从表外键的字段值等于主表主键的值)

- 能够使用左外连接和右外连接进行多表查询
   左外连接: select ... from 左表 left [outer] join 右表 on 关联条件; 
   右外连接: select ... from 左表 right [outer] join 右表 on 关联条件;
    (关联条件:从表外键的字段值等于主表主键的值)  
    
- 能够使用子查询进行多表查询
   子查询的结果是一个值: 作为where条件
			select ... from 表 where 字段 [><=<>...] (子查询);
			
   子查询的结果是单列多行:作为where条件
			select ... from 表 where 字段 in (子查询);
			
  子查询的结果是多列多行:作为一张虚拟表(注意要给子查询的结果(虚拟表)取别名)
      		select ... from (子查询) 别名 where 条件;
			注意: 子查询一定要取别名
                
- 能够理解事务的概念
   作用:事务可以保证一组操作要么全部成功,要么全部失败
	
- 能够在MySQL中使用事务
    使用:
		mysql默认是自动事务: 自动开启事务,自动提交事务
			一条sql语句就是一个事务
			
		手动管理事务:
			开启事务: start transaction;
			提交事务: commit;
			回滚事务: rollback;

	 事务特点: 原子性,一致性,持久性,隔离性
     事务隔离级别: 
			读未提交:可能发生脏读,可能发生不可重复读,可能发生幻读
            读已提交: 不可能发生脏读,可能发生不可重复读,可能发生幻读
            可重复读:不可能发生脏读,不可能发生不可重复读,可能发生幻读
            串行化:不可能发生脏读,不可能发生不可重复读,不可能发生幻读
                
- 能够完成数据的备份和恢复
    使用Navicat操作即可
    使用命令:
		备份:mysqldump -u用户名 -p密码 数据库名 > 文件路径
        还原: source 文件路径     注意:先登录数据库,创建数据库,并使用对应的数据库
- 能够理解三大范式 
   1.列不可分割---1NF
   2.不能产生局部依赖---2NF
   3.不能产生传递依赖--3NF
    记住: 一张表只描述一件事,每个列都不能分割,并且每个列都直接依赖主键
```

| 脏读       | 一个事务中读取到了另一个事务中未提交的数据                   |
| ---------- | ------------------------------------------------------------ |
| 不可重复读 | 一次事务中读取到的数据不一致（这是事务update时引发的问题     |
| 幻读       | 一个事务中两次读取到的数据不一致。这是insert和delete时引发的问题 |

