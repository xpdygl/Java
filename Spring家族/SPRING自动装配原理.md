### SPRING自动装配原理

pom.xml:

+ Springboot-dependencies:核心依赖在父工程中、

启动器：

+ 各种starter
+ 就是springboot的启动场景，比如我们用了web的starter他就会导入各种依赖
+ 我们要使用什么功能，就只需要找到对应的启动器就行了

 

主程序：

@SpringbootApplication

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
  
}
```



+ ```
  @SpringBootConfiguration:springboot的配置 表明这是一个配置类 而且在工程中是唯一的
  	@configuration ：spring配置类
  	@Component:这也是spring的一个组件  扫描组件用的
  
  @EnableAutoConfiguration：自动配置
  	@AutoConfigurationPackage：自动配置包
  	@Import({AutoConfigurationImportSelector.class})：自动配置‘包注册’->注册完之后自动导入，因为想让自己写的bean，component先被扫描先完成配置，最后配置import自动配置的东西
  
  @componentScant 进行组件扫描的时候排除掉一些包，同时排除所有的自动配置类，因为自动配置类是通过import导入的
  ```

MEAT-INF/spring.properties

 结论：springboot所有自动配置都是启动的时候扫描并加载：spring.factories所有的自动配置类都在这里，但是不一定生效，通过注解@ConditionOnXXX来判断，只有导入了对应的starter才有了启动器，最后才能配置成功 

1.springboot在启动的时候从MEAT-INF/spring.factories获取指定的值

2.将这些自动配置的类导入容器，自动配置就会生效，帮我们自动配置。

3.以前我们需要自动配置的东西，springboot都帮我们做了

4.他会把所有需要导入的组件以类名的方式返回，这些组件都会被我们添加到容器里面。

5.然后会给容器中导入很多自动配置类，给这个容器导入并配置好这些组件。





@configurationProperties:作用是把properties文件中的键值信息 和JAVAbean做一个绑定

