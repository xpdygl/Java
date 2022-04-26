404是路径有问题

500是服务器有问题（代码有问题）

### 重定向

状态码是302

需要设置状态码为302 然后给一个新的地址让页面跳转。

或者使用sendredirect("http://www.baidu.com")

### 编码

请求乱码

```java
request.setCharacterEncoding("utf-8")
```



响应乱码

```java
response.setcontentype("text/html;charset=utf-8");
```

