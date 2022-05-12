### springcloud高级

#### 雪崩

微服务中每一个服务之间彼此依赖，当其中一个服务发生了故障之后其他的微服务还在继续调用这个服务，导致系统资源不断被占用。依赖服务I的业务请求被阻塞，用户不会得到响应，则tomcat的这个线程不会释放，于是越来越多的用户请求到来，越来越多的线程会阻塞。最终几乎所有服务都挂掉，这就是雪崩。

#### 如何处理雪崩

一般有四种处理方法

+ 超时等待  设定超时时间，请求超过一定时间没有响应就返回错误信息，不会无休止等待
+ 仓壁模式（线程池隔离）
+ 断路器 
+ 限流 限制业务访问的QPS

如何避免因瞬间高并发流量而导致服务故障？

- **限流**（**限流控制**）是对服务的保护，避免因瞬间高并发流量而导致服务故障，进而避免雪崩。是一种**预防**措施。

如何避免因服务故障引起的雪崩问题？

- **超时处理、线程隔离、降级熔断(补救措施)**是在部分服务故障时，将故障控制在一定范围，避免雪崩。是一种**补救**措施。



### Sentinel 

•**丰富的应用场景**：Sentinel 承接了阿里巴巴近 10 年的双十一大促流量的核心场景，例如秒杀（即突发流量控制在系统容量可以承受的范围）、消息削峰填谷、集群流量控制、实时熔断下游不可用应用等。

•**完备的实时监控**：Sentinel 同时提供实时的监控功能。您可以在控制台中看到接入应用的单台机器秒级数据，甚至 500 台以下规模的集群的汇总运行情况。

•**广泛的开源生态**：Sentinel 提供开箱即用的与其它开源框架/库的整合模块，例如与 Spring Cloud、Dubbo、gRPC 的整合。您只需要引入相应的依赖并进行简单的配置即可快速地接入 Sentinel。

•**完善的** **SPI** **扩展点**：Sentinel 提供简单易用、完善的 SPI 扩展接口。您可以通过实现扩展接口来快速地定制逻辑。例如定制规则管理、适配动态数据源等。

将zip包解压到没有中文的目录下，然后打开命令行输入

```jiue key l
java -Dserver.port=8090 -jar sentinel-dashboard-1.8.1.jar
```

意思是用8090这个端口启动服务。访问local host:8090就可以了



### 流量控制

#### 簇点链路

当请求进入微服务时，首先会访问DispatcherServlet，然后进入Controller、Service、Mapper，这样的一个调用链就叫做**簇点链路**。簇点链路中被监控的每一个接口就是一个**资源**。

默认情况下sentinel会监控SpringMVC的每一个端点（Endpoint，也就是controller中的方法），因此SpringMVC的每一个端点（Endpoint）就是调用链路中的一个资源。



- 流控：流量控制
- 降级：降级熔断
- 热点：热点参数限流，是限流的一种
- 授权：请求的权限控制

流控就是限流，给这个资源名限制最大QPS，QPS超过这个数量就访问不成功

#### 关联模式

将当前资源和另一个资源一起统计，当达到促发条件时就对其限流

eg:比如用户支付时需要修改订单状态，同时用户要查询订单。查询和修改操作会争抢数据库锁，产生竞争。业务需求是优先支付和更新订单的业务，因此当修改订单业务触发阈值时，需要对查询订单业务限流。

#### 链路模式

针对指定链路的请求做统计，判断是否超过阈值。

对指定的路径限流。



**eg:白俄罗斯进入俄罗斯不做限制，乌克兰进入俄罗斯进行限制**



链路模式中，是对不同来源的两个链路做监控。但是sentinel默认会给进入SpringMVC的所有请求设置同一个root资源，会导致链路模式失效。

我们需要**关闭这种对SpringMVC的资源聚合**，修改huixu-order服务的application.yml文件：

```yaml
spring:
  cloud:
    sentinel:
      web-context-unify: false # 关闭context整合
```

### 流控效果

流控的高级选项中还有一个流控效果选项。

流控效果是指请求达到流控阈值时应该采取的措施，包括三种：

#### 快速失败：

达到阈值后，新的请求会被立即拒绝并抛出FlowException异常。是默认的处理方式。我的理解是达到阈值后就直接失败不做多的操作。



#### warm up：

预热模式，对超出阈值的请求同样是拒绝并抛出异常。但这种模式阈值会动态变化，从一个较小值逐渐增加到最大阈值。服务器刚开始启动，性能还没有拉满，如果直接将请求跑到最大值就会宕机，所以有了这个预热模式，设置最大预热时长，时长过后就将请求放到最大值

#### 排队等待：

让所有的请求按照先后次序排队执行，两个请求的间隔不能小于指定时长。我的理解是就像削峰填谷一样

- 请求会进入队列，按照阈值允许的时间间隔依次执行请求；如果请求预期等待时长大于超时时间，直接拒绝
- 

工作原理

例如：QPS = 5，意味着每200ms处理一个队列中的请求；timeout = 2000，意味着**预期等待时长**超过2000ms的请求会被拒绝并抛出异常。

QPS非常平滑，一致保持在5，但是超出的请求没有被拒绝，而是放入队列。因此**响应时间**（等待时间）会越来越长。

当队列满了以后，才会有部分请求失败.

### 线程隔离

**用法说明**：

在添加限流规则时，可以选择两种阈值类型：

![image-20210716123411217](C:/Users/1/Desktop/employment/15springcloud高级/讲义/md/assets/1637243320525.png)  

- QPS：就是每秒的请求数，在快速入门中已经演示过

- 线程数：是该资源能使用用的tomcat线程数的最大值。也就是通过限制线程数量，实现**线程隔离**（舱壁模式）

### 熔断降级(sentinel)

熔断降级是解决雪崩问题的重要手段。其思路是由**断路器**统计服务调用的异常比例、慢请求比例，如果超出阈值则会**熔断**该服务。即拦截访问该服务的一切请求；而当服务恢复时，断路器会放行访问该服务的请求。

断路器控制熔断和放行是通过状态机来完成的：

![image-20210716130958518](C:/Users/1/Desktop/employment/15springcloud高级/讲义/md/assets/image-20210716130958518.png)

状态机包括三个状态：

- closed：关闭状态，断路器放行所有请求，并开始统计异常比例、慢请求比例。超过阈值则切换到open状态
- open：打开状态，服务调用被**熔断**，访问被熔断服务的请求会被拒绝，快速失败，直接走降级逻辑。Open状态5秒后会进入half-open状态
- half-open：半开状态，放行一次请求，根据执行结果来判断接下来的操作。
  - 请求成功：则切换到closed状态
  - 请求失败：则切换到open状态



断路器熔断策略有三种：慢调用、异常比例、异常数

**慢调用**：业务的响应时长（RT）大于指定时长的请求认定为慢调用请求。在指定时间内，如果请求数量超过设定的最小数量，慢调用比例大于设定的阈值，则触发熔断。

例如：

![image-20210716145934347](C:/Users/1/Desktop/employment/15springcloud高级/讲义/md/assets/image-20210716145934347.png) 

解读：RT超过500ms的调用是慢调用，统计最近10000ms内的请求，如果请求量超过10次，并且慢调用比例不低于0.5，则触发熔断，熔断时长为5秒。然后进入half-open状态，放行一次请求做测试。





**慢调用**：业务的响应时长（RT）大于指定时长的请求认定为慢调用请求。在指定时间内，如果请求数量超过设定的最小数量，慢调用比例大于设定的阈值，则触发熔断。

例如：

![image-20210716145934347](C:/Users/1/Desktop/employment/15springcloud高级/讲义/md/assets/image-20210716145934347.png) 

解读：RT超过500ms的调用是慢调用，统计最近10000ms内的请求，如果请求量超过10次，并且慢调用比例不低于0.5，则触发熔断，熔断时长为5秒。然后进入half-open状态，放行一次请求做测试。



### 授权规则

授权规则可以对调用方的来源做控制，有白名单和黑名单两种方式。

- 白名单：来源（origin）在白名单内的调用者允许访问

- 黑名单：来源（origin）在黑名单内的调用者不允许访问

在sentinel中点击左侧菜单的授权，可以看到授权规则

- 资源名：就是受保护的资源，例如/order/{id}

- 流控应用：是来源者的名单，
  - 如果是勾选白名单，则名单中的来源被许可访问。
  - 如果是勾选黑名单，则名单中的来源被禁止访问。

我们允许请求从gateway到order-service，不允许浏览器访问order-service，那么白名单中就要填写**网关的来源名称（origin）**

默认情况下，sentinel不管请求者从哪里来，返回值永远是default，也就是说一切请求的来源都被认为是一样的值default。

因此，我们需要自定义这个接口的实现，让**不同的请求，返回不同的origin**。

```java'
package com.huixu.sentinel;

import com.alibaba.csp.sentinel.adapter.spring.webmvc.callback.RequestOriginParser;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;

import javax.servlet.http.HttpServletRequest;

/**
 * 自定义接口的实现，让不同的请求，返回不同的origin
 */
@Component
public class HeaderOriginParser implements RequestOriginParser {

    /**
     * 获取请求头值 为空返回blank
     * @param httpServletRequest
     * @return
     */
    @Override
    public String parseOrigin(HttpServletRequest httpServletRequest) {
        // 1.获取请求头
        String origin = httpServletRequest.getHeader("origin");
        // 2.非空判断
        if(StringUtils.isEmpty(origin)){
            origin = "blank";
        }
        return origin;
    }
}
```

#### 给网关添加请求头

既然获取请求origin的方式是从reques-header中获取origin值，我们必须让**所有从gateway路由到微服务的请求都带上origin头**。

这个需要利用之前学习的一个GatewayFilter来实现，AddRequestHeaderGatewayFilter。

1）修改gateway服务中的application.yml，添加一个defaultFilter：

```yaml
spring:
  application:
    name: gateway
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: SZ #集群设置名称
        namespace: devnamespace #环境隔离 命名空间ID
    gateway:
      default-filters: #全局过滤器配置 针对所有的微服务
        - AddRequestHeader=huixu,shenzhen119 nb!!!
        - AddRequestHeader=origin,gateway
      routes:
       # ...略
```

从gateway路由的所有请求都会带上origin头，值为gateway。而从其它地方到达微服务的请求则没有这个头。

**注意：删除eureka配置**

2）添加nacos起步依赖

```xml
<!--nacos起步依赖-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

**注意：删除eureka依赖**

### 配置授权规则

接下来，我们添加一个授权规则，放行origin值为gateway的请求。

点击授权

配置资源名

编辑网关服务名

设置这是黑名单还是白名单