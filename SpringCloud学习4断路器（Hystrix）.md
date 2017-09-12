---
title: SpringCloud学习4断路器（Hystrix）
date: 2017-07-21 14:38:04
tags:
---

在微服务架构中，我们将业务拆分成一个个的服务，服务与服务之间可以相互调用（RPC）。为了保证其高可用，单个服务又必须集群部署。由于网络原因或者自身的原因，服务并不能保证服务的 100% 可用，如果单个服务出现问题，调用这个服务就会出现网络延迟，此时若有大量的网络涌入，会形成任务累计，导致服务瘫痪，甚至导致服务 “雪崩”。
为了解决这个问题，就出现断路器模型。

### 断路器简介
   * Netflix 已经创建了一个名为 Hystrix 的库来实现断路器模式。 在微服务架构中，多层服务调用是非常常见的。
   * 较底层的服务如果出现故障，会导致连锁故障。当对特定的服务的调用达到一个阀值（hystric 是 5 秒 20 次） 断路器将会被打开。
   * 断路打开后，可用避免连锁故障，fallback 方法可以直接返回一个固定值。
   
### 在 ribbon 使用断路器
   * 改造 serice-ribbon 工程的代码：
     
     在 pox.xml 文件中加入：
     
    <dependency>
          <groupId>org.springframework.cloud</groupId>
          <artifactId>spring-cloud-starter-hystrix</artifactId>
    </dependency> 
    
   * 在程序的入口类加 @EnableHystrix：   
   
   * 改造 HelloService 类，加上 @HystrixCommand，并指定 fallbackMethod 方法。
   
### Feign 中使用断路器
   * 如果你使用了 feign，feign 是自带断路器的，并且是已经打开了。如果使用 feign 不想用断路器的话，可以在配置文件中关闭它，配置如下：
     
     feign.hystrix.enabled=false
     
### Circuit Breaker: Hystrix Dashboard (断路器：hystrix 仪表盘)

   * 基于 service-ribbon 改造下：
     
     pom.xml 加入：
     
    <dependency>
                 <groupId>org.springframework.boot</groupId>
                 <artifactId>spring-boot-starter-actuator</artifactId>
             </dependency>
     
             <dependency>
                 <groupId>org.springframework.cloud</groupId>
                 <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
             </dependency>        


### 在主程序入口中加入 @EnableHystrixDashboard 注解，开启 hystrixDashboard：

      该仪表盘可以查看错误率