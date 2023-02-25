# junit

先导包,然后加注解

@TEST

必须是无参 无返回值的公共方法。

@Before

在test之前执行，每一个test执行前都会执行一次@before

有两个test就分别在运行两个test之前运行所有的@before

@After

在test之后执行，每一个test执行后都会执行一次@After

有两个test就分别在运行两个test之后运行所有的@after

@ClassBefore

必须修饰静态方法

@ClassAfter 

### 注意事项

必须在pubilc修饰的方法上才能使用@Test @Before @ After

- 如何查看junit测试结果
  * 绿色：表示测试通过
  * 红色：表示测试失败，有问题

# JDBC

是一种规范

里面有大量的接口 少量的类



+ 导入驱动包，添加依赖
+ 注册驱动
+ 获得连接
+ 创建执行sql语句对象,给定需要的参数
+ 调用方法执行sql语句
+ 释放资源

通过jdbc连接数据库

```java
Class.forName("com.mysql.jdbc.Driver");
```

首先给一个数据库地址

然后将地址和账号密码都传进去，用getconnection获得连接

```java
String url = "jdbc:mysql://localhost:3306/day16_1";
Connection connection = DriverManager.getConnection(url, "root", "root");

```



3.预编译sql语句,得到预编译对象

```java
String sql = "insert into user values(null,?,?,?)";
PreparedStatement ps = connection.prepareStatement(sql);
```

// 4.为sql语句设置参数

```java
ps.setString(1, "zl");
ps.setString(2, "123456");
ps.setString(3, "老赵");
```

// 5.执行sql语句,得到结果

```java
int rows = ps.executeUpdate();
System.out.println("受影响的行数:" + rows);
```





### PreparedStatement完成CRUD

```java
package com.huixu.demo5_PreparedStatement完成CRUD;

import com.huixu.bean.User;
import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;

/**
 * @author xuhui
 * @version 1.0
 */
public class TestDemo {
    @Test
    public void insert() throws Exception {
        // 1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");


        // 2.获得连接
        String url = "jdbc:mysql://localhost:3306/day16_1";
        Connection connection = DriverManager.getConnection(url, "root", "root");

        // 3.预编译sql语句,得到预编译对象
        String sql = "insert into user values(null,?,?,?)";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 4.为sql语句设置参数
        ps.setString(1, "zl");
        ps.setString(2, "123456");
        ps.setString(3, "老赵");

        // 5.执行sql语句,得到结果
        int rows = ps.executeUpdate();
        System.out.println("受影响的行数:" + rows);

        // 6.释放资源
        ps.close();
        connection.close();

    }

    @Test
    public void update() throws Exception {
        // 1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");

        // 2.获得连接
        String url = "jdbc:mysql://localhost:3306/day16_1";
        Connection connection = DriverManager.getConnection(url, "root", "root");

        // 3.预编译sql语句,得到预编译对象
        String sql = "update user set username = ?,password = ? where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 4.为sql语句设置参数
       ps.setString(1,"ww");
       ps.setString(2,"123456");
       ps.setInt(3,3);

        // 5.执行sql语句,得到结果
        int rows = ps.executeUpdate();
        System.out.println("受影响的行数:" + rows);

        // 6.释放资源
        ps.close();
        connection.close();

    }

    @Test
    public void delete() throws Exception {
        // 1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");

        // 2.获得连接
        String url = "jdbc:mysql://localhost:3306/day16_1";
        Connection connection = DriverManager.getConnection(url, "root", "root");

        // 3.预编译sql语句,得到预编译对象
        String sql = "delete from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 4.为sql语句设置参数
        ps.setInt(1,2);

        // 5.执行sql语句,得到结果
        int rows = ps.executeUpdate();
        System.out.println("受影响的行数:" + rows);

        // 6.释放资源
        ps.close();
        connection.close();

    }

    // 查询结果是一条记录的
    @Test
    public void select1() throws Exception {
        // 1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");

        // 2.获得连接
        String url = "jdbc:mysql://localhost:3306/day16_1";
        Connection connection = DriverManager.getConnection(url, "root", "root");

        // 3.预编译sql语句,得到预编译对象
        String sql = "select * from user where id = ?";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 4.为sql语句设置参数
        ps.setInt(1,1);

        // 5.执行sql语句,得到结果
        ResultSet resultSet = ps.executeQuery();
        // 定义一个user变量,用来存储查询到的记录
        User user = null;
        // 循环取数据
        while (resultSet.next()){
            // 取数据
            int id = resultSet.getInt("id");
            String username = resultSet.getString("username");
            String password = resultSet.getString("password");
            String nickname = resultSet.getString("nickname");
            // 封装数据
            user = new User(id,username,password,nickname);
        }
        System.out.println("user:"+user);
        // 6.释放资源
        resultSet.close();
        ps.close();
        connection.close();

    }

    // 查询结果是多条记录的
    @Test
    public void select2() throws Exception {
        // 1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");

        // 2.获得连接
        String url = "jdbc:mysql://localhost:3306/day16_1";
        Connection connection = DriverManager.getConnection(url, "root", "root");

        // 3.预编译sql语句,得到预编译对象
        String sql = "select * from user";
        PreparedStatement ps = connection.prepareStatement(sql);

        // 4.为sql语句设置参数
        // 5.执行sql语句,得到结果
        ResultSet resultSet = ps.executeQuery();
        // 定义一个集合,用来存储查询到的记录
        ArrayList<User> list = new ArrayList<>();
        // 循环取数据
        while (resultSet.next()){
            // 取数据
            int id = resultSet.getInt("id");
            String username = resultSet.getString("username");
            String password = resultSet.getString("password");
            String nickname = resultSet.getString("nickname");
            // 封装数据
            User user = new User(id,username,password,nickname);
            list.add(user);
        }
        for (User user : list) {
            System.out.println(user);
        }
        // 6.释放资源
        resultSet.close();
        ps.close();
        connection.close();
    }
}
```



# 连接池

druid连接池

##### 通过配置文件方式【重点】

- 思路:

  - 拷贝配置文件到src路径下
    - druid.properties配置文件的名字可以修改
    - 配置文件建议放在 src路径下
    - **配置文件中的键名,必须为设置连接参数的set方法去掉set然后首字母变小写**
  - 创建Properties对象,加载配置文件
  - 创建DRUID的连接池对象,传入Properties对象
  - 获得连接
  - 书写sql语句
  - 预编译sql语句,得到预编译对象
  - 设置sql语句参数
  - 执行sql语句,处理结果
  - 释放资源

- 实现:

- Mysql8中的driver是com.mysql.cj.jdbc.Driver

  ```java
  // 配置文件
  # 数据库连接参数
  driverClassName=com.mysql.cj.jdbc.Driver
  url=jdbc:mysql://localhost:3306/day19_1
  username=root
  password=root
  # 连接池的参数
  initialSize=10
  maxActive=20
  maxWait=2000
  ```

  
