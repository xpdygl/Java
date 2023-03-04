MySQL和Oracle的区别之一就是MySQL有limit关键字

但是Oracle没有。

### 如何分页

根据rowid来分



```oracle
eg、
 5 = (currentPage-1) * pageSize + pageSize   每页显示几条
 0 = (currentPage-1) * pageSize              当前页数
SELECT *
  FROM EMP
 WHERE ROWID IN
       (SELECT RID
          FROM (SELECT ROWNUM RN, RID
                  FROM (SELECT ROWID RID, EMPNO FROM EMP ORDER BY EMPNO DESC)
                 WHERE ROWNUM <= ( (1-1) * 5 + 5 )) --每页显示几条
         WHERE RN > ((1-1) * 5) ) --当前页数
 ORDER BY EMPNO DESC;
```



分析：

```
1、SELECT * FROM emp;
2、显示rownum，由oracle分配的
SELECT e.*, ROWNUM rn FROM (SELECT * FROM emp) e; --rn相当于Oracle分配的行的ID号 
3、先查出1-10条记录
正确的: SELECT e.*, ROWNUM rn FROM (SELECT * FROM emp) e WHERE ROWNUM<=10;
错误的：SELECT e.*, ROWNUM rn FROM (SELECT * FROM emp) e WHERE rn<=10;
4、然后查出6-10条记录
SELECT * FROM (SELECT e.*, ROWNUM rn FROM (SELECT * FROM emp) e WHERE ROWNUM<=10) WHERE rn>=6;
```