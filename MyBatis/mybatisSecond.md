## 使用mybatis完成crud

+ 

1. 在Dao接口定义方法
2. 在Dao映射文件配置

可以在接口中给传进的参数起别名，一个参数一个别名

格式是      

```java
public interface UserDao {
    /**
     * 保存用户
     * @param user
     */
    void save(User user);
 void  save (@param("user")User user)//起别名
}
```

+ 

在 UserDao.xml 文件中加入新增配置

```java
    <insert id="save" parameterType="com.huixu.bean.User">
        INSERT INTO t_user(username,sex,birthday,address) VALUES (#{username},#{sex},#{birthday},#{address})
    </insert>

	<!--我们可以发现， 这个 sql 语句中使用#{}字符， #{}代表占位符，我们可以理解是原来 jdbc 部分所学的?，它们都是代表占位符， 具体的值是由 User 类的 username 属性来决定的。
	parameterType 属性：代表参数的类型，因为我们要传入的是一个类的对象，所以类型就写类的
全名称。-->
```

+ 

添加测试类中的测试方法 

```java
    @Test
    public void fun02() throws Exception {
        //1. 读取SqlMapConfig.xml获得输入流
        InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建SqlSessionFactory
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = builder.build(is);
        //3. 获得SqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //4.获得UserDao代理对象
        UserDao userDao = sqlSession.getMapper(UserDao.class);

        //5.调用方法
        User user = new User("赵六","男",new Date(),"广州");
        userDao.save(user);
        sqlSession.commit();

        //6.释放资源
        sqlSession.close();
    }
```

## 返回值类型

### resultType

dao语句需要有返回值

返回值用resultType来接收

可以接收三种类型的返回值

#### 输出简单类型

​	直接写对应的Java类型. eg: 返回int

```xml
<select id="findCount" parameterType="int" resultType="int">
     SELECT  COUNT(*) FROM t_user
</select>
```



#### 输出bean对象

​	直接写当前pojo类的全限定名 eg: 返回User

```xml
<select id="findByUid" parameterType="int" resultType="com.huixu.bean.User">
        select * from t_user where uid = #{uid}
</select>
```



#### 输出pojo列表

​	直接写当前pojo类的全限定名 eg: 返回 List<User> list;

```xml
<select id="findAll" resultType="com.huixu.bean.User">
        select * from t_user
</select>
```



### resultMap

因为数据库中的列名与后端代码中的变量名可能不一样，所以需要在xml配置中指定user_brand=#{userbrand}

此时dao接口文件中的返回值不能是resultType，要指定为resultMap

resultType可以指定pojo将查询结果映射为pojo，但需要pojo的属性名和sql查询的列名一致方可映射成功。

       如果sql查询字段名和pojo的属性名不一致，可以通过resultMap将字段名和属性名作一个对应关系 ，resultMap实质上还需要将查询结果映射到pojo对象中。

## 日志

使用日志导入一下坐标，设置等级就可以

等级如下

配置文件一般的配置

+ 开发阶段:  log4j.rootLogger= debug,std,file	
+ 上线之后:  log4j.rootLogger= error ,file	



## 注解

```java
public interface UserDao {

    //保存  keyProperty:id的属性名;  resultType:id的类型; before:true 之前,false 之后; statement:执行的语句
    @SelectKey(keyProperty = "uid",resultType = int.class,before = false,statement = "SELECT LAST_INSERT_ID()")
    @Insert("INSERT INTO t_user VALUES(null,#{username},#{sex},#{birthday},#{address})")
    void save(User user);


    @Select("select * from t_user where uid = #{uid}")
    User findByUid(int uid);


    @Select("select * from t_user")
    List<User> findAll();

    @Select("select count(*) from t_user")
    int findCount();

    @Update("UPDATE t_user SET username  = #{username} ,sex =#{sex},birthday=#{birthday},address=#{address} WHERE uid = #{uid}")
    void update(User user);

    @Delete("delete from t_user where uid  =#{uid}")
    void delete(int uid);
}
```



## Mybatis 映射文件的 SQL 深入【重点】

### 动态查询

 动态查询语句，因为语句较复杂，所以不用注解来实现

在xml中使用<where><if>标签，if标签里写条件。

下面拼接and uid = #{uid}

要在userdao接口中起了别名才可以=#{uid}.

```xml
  <!--select * from t_user where uid = #{uid} and username = #{username}-->
    <select id="findByCondition1" resultType="user">
        select * from t_user
        <where>
            <if test="uid > 0">
                and uid = #{uid}
            </if>
            <if test="username != null and username != ''">
                and username = #{username}
            </if>
        </where>
    </select>
```

