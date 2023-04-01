建表语句

```mysql
create table test.course
(
    课程号  varchar(255) not null
        primary key,
    课程名称 varchar(255) not null,
    教师号  varchar(255) not null
);

create table test.score
(
    学号  varchar(255) not null,
    课程号 varchar(255) not null,
    成绩  float(3, 0)  not null,
    primary key (学号, 课程号)
);

create table test.student
(
    学号   varchar(255) not null
        primary key,
    姓名   varchar(255) not null,
    出生日期 date         not null,
    性别   varchar(255) not null
);

create table test.teacher
(
    教师号  varchar(255) not null
        primary key,
    教师姓名 varchar(255) null
);

-- 学生表
insert into student(学号,姓名,出生日期,性别) 
values('0001' , '猴子' , '1989-01-01' , '男');

insert into student(学号,姓名,出生日期,性别) 
values('0002' , '猴子' , '1990-12-21' , '女');

insert into student(学号,姓名,出生日期,性别) 
values('0003' , '马云' , '1991-12-21' , '男');

insert into student(学号,姓名,出生日期,性别) 
values('0004' , '王思聪' , '1990-05-20' , '男');

--成绩表

insert into score(学号,课程号,成绩) 
values('0001' , '0001' , 80);

insert into score(学号,课程号,成绩) 
values('0001' , '0002' , 90);

insert into score(学号,课程号,成绩) 
values('0001' , '0003' , 99);

insert into score(学号,课程号,成绩) 
values('0002' , '0002' , 60);

insert into score(学号,课程号,成绩) 
values('0002' , '0003' , 80);

insert into score(学号,课程号,成绩) 
values('0003' , '0001' , 80);

insert into score(学号,课程号,成绩) 
values('0003' , '0002' , 80);

insert into score(学号,课程号,成绩) 
values('0003' , '0003' , 80);

-- 课程表

insert into course(课程号,课程名称,教师号)
values('0001' , '语文' , '0002');

insert into course(课程号,课程名称,教师号)
values('0002' , '数学' , '0001');

insert into course(课程号,课程名称,教师号)
values('0003' , '英语' , '0003');

-- 教师表：添加数据
insert into teacher(教师号,教师姓名) 
values('0001' , '孟扎扎');

insert into teacher(教师号,教师姓名) 
values('0002' , '马化腾');

-- 这里的教师姓名是空值（null）
insert into teacher(教师号,教师姓名) 
values('0003' , null);

-- 这里的教师姓名是空字符串（''）
insert into teacher(教师号,教师姓名) 
values('0004' , '');

```



+ sql练习

```mysql
select *
from test.score;
USE test;

SELECT COUNT(test.teacher.教师姓名)
from teacher
where 教师姓名 like '孟%';

#查询课程编号0002的总成绩
select sum(test.score.成绩)
from score
where 课程号 = '0002';

#查询选了课程的学生人数
select count(distinct test.score.学号)
from score;

#查询最高分最低分
select max(test.score.成绩)
from score
group by score.学号;

select min(test.score.成绩)
from score
group by score.学号;

# 查询每门课程被选修的学生人数
select count(distinct 课程号, test.score.学号)
from score;

select 课程号, count(学号)
from score
group by 课程号;
# 查询男生、女生人数
select 性别, count(性别)
from student
group by test.student.性别;
# 查询平均成绩大于60分学生的学号和平均成绩
select score.学号, avg(score.成绩)
from score
where 成绩 > (select avg(score.成绩) from score);


select student.学号, 姓名
from student
where student.学号 in
      (select score.学号
       from score
       where score.成绩 > 60);
#
/*
查询所有课程成绩小于60分学生的学号、姓名

1.翻译成大白话
1）查询结果：学生学号，姓名
2）查询条件：所有课程成绩 < 60 的学生，需要从成绩表里查找，用到子查询

第1步，写子查询（所有课程成绩 < 60 的学生）
select 查询结果[学号]
from 从哪张表中查找数据[成绩表：score]
where 查询条件[成绩 < 60]
group by 分组[没有]
having 对分组结果指定条件[没有]
order by 对查询结果排序[没有]
limit 从查询结果中取出指定行[没有];

select 学号
from student
where score < 60;

第2步，查询结果：学生学号，姓名，条件是前面1步查到的学号

select 查询结果[学号,姓名]
from 从哪张表中查找数据[学生表:student]
where 查询条件[用到运算符in]
group by 分组[没有]
having 对分组结果指定条件[没有]
order by 对查询结果排序[没有]
limit 从查询结果中取出指定行[没有];
*/
select 学号, 姓名
from student
where 学号 in (
    select 学号
    from score
    where 成绩 > 60
);

/*
查询没有学全所有课的学生的学号、姓名
查找出学号，条件：没有学全所有课，也就是该学生选修的课程数 < 总的课程数
【考察知识点】in，子查询
*/

select student.学号, student.姓名
from student
where student.学号 in (
    select score.学号
    from score
    group by score.学号
    having count(课程号) < (select count(课程号) from course)
);

select 学号, 姓名
from student
where 学号 in (
    select 学号
    from score
    group by 学号
    having count(课程号) < (select count(课程号) from course)
);

/*
查找1990年出生的学生名单
学生表中出生日期列的类型是datetime
*/
select student.学号, student.姓名
from student
where year(student.出生日期) = 1990;

# 查询所有学生的学号、姓名、选课数、总成绩
select student.学号, student.姓名, sum(score.成绩), count(score.课程号)
from student
         left join score
                   on student.学号 = score.学号
group by student.学号;

select a.学号, a.姓名, count(b.课程号) as 选课数, sum(b.成绩) as 总成绩
from student as a
         right join score as b
                    on a.学号 = b.学号
group by a.学号;

# 查询平均成绩大于85的所有学生的学号、姓名和平均成绩
select avg(score.成绩), score.学号, s.姓名
from score
         left join student s on score.学号 = s.学号

group by score.学号
having avg(score.成绩) > 85;

# 查询出每门课程的及格人数和不及格人数
select score.课程号,
       sum(case
               when score.成绩 >= 60 then 1
               else 0 end) as 及格人数,
       sum(case
               when score.成绩 < 60 then 1
               else 0 end) as 不及格人数

from score
group by score.课程号;

select 课程号,
       sum(case
               when 成绩 >= 60 then 1
               else 0
           end) as 及格人数,
       sum(case
               when 成绩 < 60 then 1
               else 0
           end) as 不及格人数
from score
group by 课程号;

# 查询课程编号为0003且课程成绩在80分以上的学生的学号和姓名|
select score.课程号, score.成绩, s.姓名
from score
         join student s on score.学号 = s.学号
where score.课程号 = '0003'
  and score.成绩 > 80;

# 检索"0001"课程分数小于60，按分数降序排列的学生信息
select score.课程号, score.成绩, s.*
from score
         join student s on score.学号 = s.学号
where 课程号 = '0001'
  and score.成绩 > 60
order by score.成绩 DESC;

# 查询不同老师所教不同课程平均分从高到低显示
select t.教师号, t.教师姓名, avg(s.成绩)
from teacher t
         join course c on t.教师号 = c.教师号
         join score s on s.课程号 = c.课程号
group by t.教师姓名, t.教师号
order by avg(s.成绩) DESc;


# -查询课程名称为"数学"，且分数低于60的学生姓名和分数
select s.姓名, course.课程名称, score.成绩
from course
         inner join score on course.课程号 = score.课程号
         inner join student s on score.学号 = s.学号
where 课程名称 = '数学'
  and score.成绩 > 60;

select a.姓名, b.成绩
from student as a
         inner join score as b
                    on a.学号 = b.学号
         inner join course c on b.课程号 = c.课程号
where b.成绩 > 60
  and c.课程名称 = '数学';

# -查询任何一门课程成绩在70分以上的姓名、课程名称和分数（与上题类似）
select student.姓名, course.课程名称, score.成绩
from score
         join student on score.学号 = student.学号
         join course on score.课程号 = course.课程号
where 成绩 > 70;

# -查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩
select student.姓名, avg(score.成绩), score.学号
from score
         join student on score.学号 = student.学号
where score.成绩 < 60
group by score.学号
having count(score.学号) > 2;

# -查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
+ 原来表还可以自己连接自己，然后再查询
select distinct a.课程号, a.成绩
from score a
         join score b on a.学号 = b.学号

where a.课程号 != b.课程号
  and a.成绩 = b.成绩;


# -查询课程编号为“0001”的课程比“0002”的课程成绩高的所有学生的学号
+ 子查询还可以用在from后面，将查询出的结果当做一个新的表，然后用这个表去join别的表进行查询。
select a.学号
from (select score.学号, score.成绩 from score where 课程号 = '0001') a
         join
         (select score.学号, score.成绩 from score where 课程号 = '0002') b on
         a.学号 = b.学号
where a.成绩 < b.成绩;

# -查询学过“孟扎扎”老师所教的所有课的同学的学号、姓名
```

