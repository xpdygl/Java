## spring创建对象

在spring中 通过配置文件让spring工厂创建对象，我们向工厂拿对象

用拿来的对象调用方法使用。



## spring依赖

添加依赖时使用稳定的版本

```xml
 <!--Spring-context Spring核心依赖坐标-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.1.2.RELEASE</version>
        </dependency>
```

## 配置核心配置文件

基本属性

* `id`：唯一标识
* `class`：bean的全限定类名

class是写我们要创建的对象的路径

```xml
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>
```

##### .创建Spring核心配置文件，并配置`UserDaoImpl`

> 这个步骤的作用就是告诉spring，要创建哪个类的对象，并且给这个类起一个别名，方便以后我们问spring要对象。它的作用等于我们前面写的`beans.properties`

* 配置文件名称，通常叫`applicationContext.xml`

  ```
  <!--注入简单类型数据|注入不是IOC容器中管理的数据  使用value属性-->
  <!--
      property：为当前对象的属性赋值 使用的是set方法注入  对应的属性一定要提供set方法
          name：属性名称
          value：属性值  简单类型数据|注入不是IOC容器中管理的数据
          ref：注入引用类型属性值  在IOC容器中管理的bean对象  注入到当前对象中
          注意：value和ref不能同时使用
  -->
  ```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        在这里告诉spring要创建哪个类的对象，并且给这个对象起一个别名
                id: 唯一标识，不能出现重复！
                class: 托管类的全路径
     -->
    <bean id="ud"  class="com.itheima.dao.impl.UserDaoImpl" />
</beans>
```

### 给对象传入数据 用set方法

在对象中先建好set方法 然后通过xml配置的方式用property的方式传入对象

property中的name 是属性名称

value 是传进去的值

ref只能是对象 

同时value和ref不能同时用。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--创建UserDaoImpl对象-->
    <bean id="ud" class="com.itheima.dao.impl.UserDaoImpl"/>

    <!--创建UserServiceImpl对象-->
    <bean id="us1" class="com.itheima.service.impl.UserServiceImpl">
        <!--注入简单类型数据|注入不是IOC容器中管理的数据  使用value属性-->
        <!--
            property：为当前对象的属性赋值 使用的是set方法注入  对应的属性一定要提供set方法
                name：属性名称
                value：属性值  简单类型数据|注入不是IOC容器中管理的数据
                ref：注入引用类型属性值  在IOC容器中管理的bean对象  注入到当前对象中
                注意：value和ref不能同时使用
        -->
        <property name="name" value="李四"/>
        <property name="userDao" ref="ud"/>
    </bean>

</beans>
```







### 引入外部文件配置

+ class指定谁就创建谁，每次在test里面运行时如果出错就去看看核心配置指定的路径有没有出错。

+ 核心配置文件可以import其他的文件

  可以现在applicationContext07-dao.xml中创建一个userdao对象

  \<bean id="ud" class="com.itheima.dao.impl.UserDaoImpl"/>

  

  然后在applicationContext07-service.xml文件中

  \<import resource="classpath:applicationContext07-dao.xml"/>

  就可以在下面导入property name=userDao 

  

  \<bean id="us7" class="com.itheima.service.impl.UserServiceImpl07">
          \<property name="userDao" ref="ud"/>    ps：name为dao中的userDao ref 为导入进来的ud
      \</bean>

  

