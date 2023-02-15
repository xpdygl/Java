yml里面的配置的写法是怎么来的：

首先要说到自动配置，spring会扫描MEAT-INF/spring.properties下面的所有内容，

这些内容都对应着实体类中的默认配置。

只有实体类有的属性我们才可以对其修改并配置进spring中。

如果我们要配置一个地址

```yml
server:
  address: 
```

那么一定有一个serverProperties类在里面。我们写进yml里面之后

通过这个注解@ConfigurationProperties (prefix = "server"）

让yml里面的属性对应到实体类上，然后被spring自动装配。

最后成功修改spring的配置。