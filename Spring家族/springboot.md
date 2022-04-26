

## 配置文件

springboot使用yml配置文件。



```xml
server:
  port: 80
#  通过缩进表示层级关系
#  层级关系用多行描述，每行结尾用冒号结束
#  属性名与属性值之间用冒号+空格作为分隔
```

## 读取配置文件



## 多环境切换

不同的生产环境用---分割

```yml
指定生产环境
spring:
	profiles:
		active: pro 
---
spring:
	profiles: pro
server:
	port: 80
---
spring:
	profiles: dev
server:
	port: 82
---
spring:
	profiles: test
server:
	port: 84
```

