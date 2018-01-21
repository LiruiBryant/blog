---
title: springcloud配置问题
date: 2018-01-14 21:41:03
tags:
---
* 注册中心不自动上线下线
* 异步返回值
* bean参数传值
*


### eureka 不自动下线
Eureka Server在运行期间，会统计心跳失败的比例在15分钟之内是否低于85%，如果出现低于的情况（在单机调试的时候很容易满足，实际在生产环境上通常是由于网络不稳定导致），Eureka Server会将当前的实例注册信息保护起来，同时提示这个警告。保护模式主要用于一组客户端和Eureka Server之间存在网络分区场景下的保护。一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据（也就是不会注销任何微服务）。

详见：https://github.com/Netflix/eureka/wiki/Understanding-Eureka-Peer-to-Peer-Communication

解决方法：设置enableSelfPreservation:false

配置心跳检测时长，下线leaseRenewalIntervalInSeconds: 2




### [负载均衡在服务下掉后的重试策略：]( http://blog.didispace.com/spring-cloud-ribbon-failed-retry/ )
默认没有开启：
```java
spring.cloud.loadbalancer.retry.enabled=true

hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=10000

hello-service.ribbon.ConnectTimeout=250
hello-service.ribbon.ReadTimeout=1000
hello-service.ribbon.OkToRetryOnAllOperations=true
hello-service.ribbon.MaxAutoRetriesNextServer=2
hello-service.ribbon.MaxAutoRetries=1

```

### [断路器配置重试:](http://blog.didispace.com/spring-cloud-ribbon-failed-retry/)
如何处理服务挂掉后或者手动关闭服务后，Ribbon负载均衡还是一直调用这个服务，然后调用@HystrixCommand断路器注解的方法：利用Hystrix，在error callback方法中可以shutdown指定的server
```java
ZoneAwareLoadBalancer<Server> lb = (ZoneAwareLoadBalancer<Server>) springClientFactory.getLoadBalancer("CLOUD-SERVICE");
Server server = lb.chooseServer();
System.out.println("error->" + server.getHostPort());
lb.markServerDown(server);

```
