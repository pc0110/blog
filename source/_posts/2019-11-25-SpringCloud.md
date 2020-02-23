---
title: 2019-11-25-SpringCloud
date: 2019-11-25 18:52:43
tags:
- SpringCloud
- Spring
---

## 基本流程

1. 引入相关依赖
2. 覆盖默认的配置
3. 在引导类上添加注解，开启相关的组件

# eureka 注册中心，服务的注册与发现

### 心跳过期 hhh-service-provider

```yaml
eureka:
  instance:
    lease-expiration-duration-in-seconds: 15 #过期时间
    lease-renewal-interval-in-seconds: 5 # 心跳时间
```

### 拉取服务的间隔时间 hhh-service-consumer

```yaml
eureka:
  client:
    registry-fetch-interval-seconds: 5 
```

### 关闭自我保护，定期清除无效链接

```yaml
eureka:
  server:
    eviction-interval-timer-in-ms: 5000 #单位是毫秒
    enable-self-preservation: false #关闭自我保护
```

## testTemplate

#  ribbon:负载均衡组件



1. eureka集成了
2. @LoadBalanced: 开启负载均衡
3. this.restTemplate.getForObject("http://service-provider/user/" + id,User.class);



#### BaseLoadBalancer负载均衡策略

默认springcloud使用的是自己提供的轮询策略，也可以使用自己的策略

# hystrix：容错组件



降级:检查每次请求，是否请求超时，或者连接池已满

1. 引入hystrix启动器
2.  hystrix.command.execution.isolation.thead.timeoutInMilliseconds: 6000 # 设置hystrix的超时时间为6000ms
3. 在引导类上添加一个注解：@EnableCircuitBreaker
4. 定义熔断方法，局部(要和被熔断的方法返回值和参数列表一致)  全局(返回值类型要被熔断的方法一致，参数列表必须为空)
5. @HystrixCommand(failbackMethod="局部熔断的方法名"):声明被熔断的方法
6. @DefaultProperties(defaultFailback=“全部熔断方法名”)

熔断：不在发送请求

1. close:闭合状态，所有请求正常方法
2. open: 打开状态，所有请求都无法访问，如果一定时间内容，失败的比例大于50%或者次数不少于20次
3. half open: 半开状态，打开状态默认5s休眠期，在休眠期所有请求都无法正常访问，过了休眠期会进入半开状态，放部分请求通过

# Feign：远程调用组件



1. 引入openFegin启动器
2.  fegin.hystrix.enable=true  ，开启Fegin的熔断功能
3. 在引导类上 @EnableFeignClients
4. 创建一个接口  @FeginClient(value="服务名"，fallback=实现类.class)
5. 在接口中定义一些方法，这些方法的书写方式跟之前controller类似
6. 创建一个熔断类，实现feigin接口，实现对应的方法，这些方法都是熔断方法

# Zuul 网关组件



1. 引入zuul的启动器

2. 配置：zuul.routers.<路由名称>.path=/service-procider/**

   ​		   zuul.routers.<路由名称>.url=http://localhost:8002

   ​		   zuul.routers.<路由名称>.path=/service-provider

   ​		   zuul.routers.<路由名称>.serviceId=service-provider

   ​		   zuul.routers.服务名=/servuce-provider/**

   ​			不用配置，默认就是服务id开头路径

3. @EnableZuulProxy

过滤器：

​	创建一个类继承ZuulFilter基类

​	重写四个方法

			-  filterType: pre route post  error
			-  filterOrder:返回值越小，优先级越高
			-  shouldFilter:是否执行run方法，true执行
			-  run:具体的拦截逻辑

​			

