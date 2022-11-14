## 查询语句

Select 	查询列表	from	表名

+ 查询 表中的单个字段

```mysql
select last_name from employees;
```

+ 查询表中的多个字段

```mysql
select last_name,emali,salary from employess;
```

+ 查询表中的所有字段

```mysql
select * from employees;
```

+ 查询常量值

```mysql
select 100；
```



```mysql
select 'john';
```

+ 查询表达式

```mysql
select 100%98;
```

+ 查询函数

```mysql
select version();
```

+ 起别名 1 使用as
  + 这样子查询处来的“表名“就是结果
  + 便于理解
  + 如果要查询的字段有重名的情况，使用别名可以区分

```mysql
select 100%98 as 结果;
select last_name as 姓,first_name as 名 from employees;
```

+ 起别名 2 使用空格

```mysql
select last_name 姓，frist_name 名 from employees;
```

tips: 当别名是关键字的时候可以给别名加双引号，这样可以避免歧义



+ 去重
  + 加上distinct关键字就可以去重

```mysql
select distinct department_id from employees; 
```

+ mysql中+的作用 只是作为运算符



+  拼接用 concat

```mysql
select concat('a','b','c') as 结果
```

## 插入语句

插入指定列: insert into 表名(列名,列名,...)  values(值,值,...);
插入所有列: insert into 表名 values(值,值,...);



## 更新语句

- 语法:  `update  表名 set 字段名=值,字段名=值,... where 条件; `





## 6.3 删除记录

+ 语法

  + 方式一:  `delete from 表名 where 条件;`
  + 方式二:  `truncate table 表名;`

  
