---
title: SpringCloud学习3服务消费者feign
date: 2017-07-21 14:20:56
tags:
---


### 使用feign

spring-cloud-starter-eureka
spring-cloud-starter-feign
spring-boot-starter-web
spring-boot-starter-test


### 配置文件
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
server:
  port: 8765
spring:
  application:
    name: service-feign
    
### 开启feign  
  * 在程序的入口类，需要通过注解 @EnableFeignClients 来开启 feign:
### 调用服务
  * 定义一个 feign 接口类, 通过 @ FeignClient（“服务名”），来指定调用哪个服务：