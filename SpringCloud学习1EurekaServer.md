---
title: SpringCloud学习1EurekaServer
date: 2017-07-20 18:23:14
tags:
---

          
## 创建EurekaServer

### Idea初始化项目
   * 选择cloud discovery->eureka server 。


### 创建启动类
   * @EnableEurekaServer
   
### 配置参数
    server:
      port: 8761
    
    eureka:
      instance:
        hostname: localhost
      client:
        registerWithEureka: false
        fetchRegistry: false
        serviceUrl:
          defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
          
          
          
## 创建EurekaClient
### 初始化项目
   * 通server
   
### 创阿金启动类
   * @EnableEurekaClient
   
### 配置参数
     eureka:
       client:
         serviceUrl:
           defaultZone: http://localhost:8761/eureka/
     server:
       port: 8762
     spring:
       application:
         name: service-hi    

