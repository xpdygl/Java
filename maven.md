## 依赖管理

依赖具有传递性：直接添加直接依赖，资源中又引入的资源被称为简介依赖

+ 路径优先： 依赖资源层级越浅，优先级越高。

+ 特殊优先：同层级下（同一个pom.xml） 后配置的依赖会覆盖掉前面配置的依赖。

+ 声明优先： 当资源在相同层级被依赖时，配置顺序靠前的覆盖掉配置顺序靠后的。（在间接依赖中应用）,在间接依赖中 配置在前的版本生效

  

### 可选依赖

指对外隐藏当前所依赖的资源不透明，将依赖的资源隐藏，不会再进行依赖传递。

```xml
<optional>false</optional>
```

### 排除依赖

调用方不要直接依赖带过来的间接依赖就可以直接排除。

主动断开依赖度的资源，只需指定groupId 和artifactId无需指定版本

```xml
<exclusions>
	<groupId>组织名称</groupId>
  <artifactId>项目名称</artifactId>
  
</exclusions>
```



可选依赖是被调用方做的

排除依赖是调用方做的



## 聚合和继承

## 聚合

将多个模块放到一个工程中管理的过程称为聚合

聚合工程：管理多个子模块的符工程

作用：方便对多个子模块进行同步构建

+ 1创建maven工程，设置打包方式为pom

+ 2设置当前工程，管理哪些子模块。

```xml
<modules>
	<module>../maven02_ssm</module>
	<module>../maven03_pojo</module>
  <module>../maven04_dao</module>
</modules>
//使用../的原因是要去到上一级目录
```

体现：父模块知道自己的子模块，子模块感知不到父模块

编译时先编译底层的依赖 不取决与书写顺序

### 继承

1创建maven工程，设置打包方式为pom

