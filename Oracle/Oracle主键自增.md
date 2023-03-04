**Oracle实现主键自增有4种方式：**

1. **Identity Columns新特性自增（Oracle版本≥12c）**
2. **创建自增序列，创建表时，给主键字段默认使用自增序列**
3. **创建自增序列，使用触发器使主键自增**
4. **创建自增序列，插入语句（insert）时，使用自增序列代替值**



本文主要说的是11的自增方式

第一步 先创建一个自增序列，然后让主键的默认字段为这个自增列



```oracle
--设置自增序列，名称为"inc"，名字任意命名
create sequence inc
 increment by 1		--每次+1	
 start with 1		--从1开始
 nomaxvalue			--不限最大值
 nominvalue			--不限最小值
 cache 20;			--设置取值缓存数为20
```



ID为主键

```oracle
--创建userinfo表
CREATE TABLE userinfo (
  id number(11)  DEFAULT inc.nextval, --"inc"为自增序列名称
  name varchar2(20) ,
  age number(3)
);
```

