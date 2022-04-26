# MAVEN

##什么是Maven

​	Maven是项目进行模型抽象，充分运用的面向对象的思想，Maven可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。

​	说白了: ==Maven是由Apache开发的一个工具。==用来管理java项目(依赖(jar)管理, 项目构建, 分模块开发 ).

maven 是一种标准的项目构建

将依赖，源码，测试模块分开存放 具有高可重用性。



##Maven的仓库

| 仓库名称 | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| 本地仓库 | 相当于缓存，工程第一次会从远程仓库（互联网）去下载jar 包，将jar包存在本地仓库（在程序员的电脑上）。第二次不需要从远程仓库去下载。先从本地仓库找，如果找不到才会去远程仓库找。 |
| 中央仓库 | 仓库中jar由专业团队（maven团队）统一维护。中央仓库的地址：https://mvnrepository.com/ |
| 远程仓库 | 在公司内部架设一台私服，其它公司架设一台仓库，对外公开。     |

## 如何安装maven

1 下载maven 将maven的路径放进~/.zshrc这个文件夹 并且source  ~/.zshrc

export MAVEN_HOME=/Users/xuhui/Maven
export PATH=$PATH:$MAVEN_HOME/bin

然后输入 mvn -v验证安装是否成功

### 配置本地仓库

在maven的安装目录中conf/ settings.xml文件，在这里配置本地仓库

在第五十多行 将maven的安装路径放进去

  例子：<localRepository>E:/source/04_Maven/repository_pinyougou</localRepository>

​															👆上面这个是本地安装maven的路径

###配置国内镜像

> 默认情况下，maven会去它自己的仓库（中央仓库）里面下载jar包，由于仓库在国外，所以下载会比较慢。一般会配置成国内的镜像，比如：阿里云的镜像
>
> 在setting.xml文件中的<mirrors>标签中追加以下内容即可

  		<mirror>
  	        <id>alimaven</id>
  	        <name>aliyun maven</name>
  	        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
  	        <mirrorOf>central</mirrorOf>
  	    </mirror>

### idea创建maven项目

打开创建新项目时的系统偏好(new project settings)设置搜索maven

在runner这一栏

配置参数(创建工程不需要联网,解决创建慢的问题)  -DarchetypeCatalog=internal



然后设置这两个目录

![7](/Users/xuhui/Desktop/就业班资料/就业班预习PPT/预习3/day19-maven和mybatis/01_笔记/img/7.png)





maven的结构

![image-20191224102316809](/Users/xuhui/Desktop/就业班资料/就业班预习PPT/预习3/day19-maven和mybatis/01_笔记/img/image-20191224102316809.png)

### 导入依赖坐标

无需手动导入jar包就可以引入jar。在pom.xml中使用<dependency>标签引入依赖。

​	做项目/工作里面 都有整套的依赖的, 不需要背诵的. 

​	去Maven官网找, 赋值,粘贴.  `http://mvnrepository.com/`

#### 导入junit的依赖

- 导入junit坐标依赖

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

导入依赖时从官网复制就行 





## mybatis

**3.2.1创建Maven工程(jar)导入坐标**

```xml
  <dependencies>
    <!--MyBatis坐标-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.5</version>
    </dependency>
    <!--mysql驱动-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.6</version>
    </dependency>
  	<!--单元测试-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.10</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

### 创建SqlMapConfig文件






```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

<!--配置连接数据库的环境 default:指定使用哪一个环境-->
<environments default="a">
    <environment id="development">
        <!--配置事务,MyBatis事务用的是jdbc-->
        <transactionManager type="JDBC"/>
        <!--配置连接池, POOLED:使用连接池(mybatis内置的); UNPOOLED:不使用连接池-->
        <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/mybatis_day01?characterEncoding=utf-8"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </dataSource>
    </environment>
    <environment id="a">
        <!--配置事务,MyBatis事务用的是jdbc-->
        <transactionManager type="JDBC"/>
        <!--配置连接池, POOLED:使用连接池(mybatis内置的); UNPOOLED:不使用连接池-->
        <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/mybatis_day02?characterEncoding=utf-8"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </dataSource>
    </environment>
</environments>

<mappers>
    <!--引入映射文件; resource属性: 映射文件的路径-->
    <mapper resource="com/itheima/dao/UserDao.xml"/>
</mappers>
</configuration>
```
#### 3.2.properties

- jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis_day01?characterEncoding=utf-8
jdbc.user=root
jdbc.password=123456
```

+ 引入核心配置文件

```properties
<configuration>
   <properties resource="jdbc.properties">
    </properties>
    <!--数据源配置-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="UNPOOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.user}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    ....
</configuration>
```

