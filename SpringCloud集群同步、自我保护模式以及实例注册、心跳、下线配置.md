---
title: SpringCloud集群同步、自我保护模式以及实例注册、心跳、下线配置
date: 2017-12-26 00:28:39
tags:
---

# 1. 概述

本文在上文 Spring cloud系列四 Eureka 之概述和服务注册中心集群的基础上，继续介绍Eureka新的内容:

    集群重要类：PeerAwareInstanceRegistryImpl
    新的Eureka Server节点加入集群后的影响
    新服务注册(Register)注册时的影响
    服务心跳(renew)
    服务下线和剔除
    自我保护模式

# 2. Eureka Server的集群同步操作
## 2.1. Eureka官网的架构图

下方的操作需要结合下图理解：
这里写图片描述
##2.2. PeerAwareInstanceRegistryImpl

集群相关重要的类com.netflix.eureka.registry.PeerAwareInstanceRegistryImpl: 为了保证集群里所有Eureka Server节点的状态同步，所有以下操作都会同步到集群的所有服务上：服务注册（Registers）、服务更新（Renewals）、服务取消（Cancels）,服务超时（Expirations）和服务状态变更（Status Changes）。以下是一些部分方法：

    syncUp：在Eureka Server重启或新的Eureka Server节点加进来的，会执行初始化，从集群其他节点中获取所有的实例注册信息，从而能够正常提供服务。当Eureka Server启动时，它会从其它节点获取所有的注册信息，如果获取同步失败，它在一定时间（此值由决定）内拒绝服务。
    replicateToPeers： 同步以下操作到所有的集群节点：服务注册（Registers）、服务更新（Renewals）、服务取消（Cancels）,服务超时（Expirations）和服务状态变更（Status Changes）
    register: 注册实例，并且复印此实例的信息到所有的eureka server的节点。如果其它Eureka Server调用此节点，只在本节点更新实例信息，避免通知其他节点执行更新
    renew：心跳，同步集群
    cancel
    其他

Eureka Server集群之间的状态是采用异步方式同步的，所以不保证节点间的状态一定是一致的，不过基本能保证最终状态是一致的。
## 2.3. 新的Eureka Server节点加入集群后的影响

当有新的节点加入到集群中，会对现在Eureka Server和Eureka Client有什么影响以及他们如何发现新增的Eureka Server节点：

    新增的Eureka Server：在Eureka Server重启或新的Eureka Server节点加进来的，它会从集群里其它节点获取所有的实例注册信息。如果获取同步失败，它会在一定时间（此值由决定eureka.server.peer-eureka-nodes-update-interval-ms决定）内拒绝服务。

    已有Eureka Server和Service Consumer如何发现新的Eureka Server
        已有的Eureka Server：在运行过程中，Eureka Server之间会定时同步实例的注册信息。这样即使新的Application Service只向集群中一台注册服务，则经过一段时间会集群中所有的Eureka Server都会有这个实例的信息。那么Eureka Server节点之间如何相互发现，各个节点之间定时（时间由eureka.server.peer-eureka-nodes-update-interval-ms决定）更新节点信息，进行相互发现。
        Service Consumer：Service Consumer刚启动时，它会从配置文件读取Eureka Server的地址信息。当集群中新增一个Eureka Server中时，那么Service Provider如何发现这个Eureka Server？Service Consumer会定时（此值由eureka.client.eureka-service-url-poll-interval-seconds决定）调用Eureka Server集群接口，获取所有的Eureka Server信息的并更新本地配置。

## 2.4. 新服务注册(Register)注册时的影响

Service Provider要对外提供服务，把自己注册到Eureka Server上。如果配置参数eureka.client.registerWithEureka=true（默认值true）时，会向Eureka Server注册进行注册，Eureka Server会保存注册信息到内存中。

Service Consumer为了避免每次调用服务请求都需要向Eureka Server获取服务实例的注册信息，此时需要设置eureka.client.fetchRegistry=true，它会在本地缓存所有实例注册信息。为了保证缓存数据的有效性，它会定时（值由eureka.client.registry-fetch-interval-seconds定义，默认值为30s）向注册中心更新实例。
## 2.5. 服务心跳(renew)

服务实例会通过心跳(eureka.instance.lease-renewal-interval-in-seconds定义心跳的频率，默认值为30s)续约的方式向Eureka Server定时更新自己的状态。Eureka Server收到心跳后，会通知集群里的其它Eureka Server更新此实例的状态。Service Provider/Service Consumer也会定时更新缓存的实例信息。
## 2.6. 服务下线和剔除

服务的下线有两种情况：

    在Service Provider服务shutdown的时候，主动通知Eureka Server把自己剔除，从而避免客户端调用已经下线的服务。
    Eureka Server会定时（间隔值是eureka.server.eviction-interval-timer-in-ms，默认值为0，默认情况不删除实例）进行检查，如果发现实例在在一定时间（此值由eureka.instance.lease-expiration-duration-in-seconds定义，默认值为90s）内没有收到心跳，则会注销此实例。

这种情况下，Eureka Client的最多需要[eureka.instance.lease-renewal-interval-in-seconds + eureka.client.registry-fetch-interval-seconds]时间才发现服务已经下线。同理，一个新的服务上线后，Eureka Client的服务消费方最多需要相同的时间才发现服务已经上线

服务下线，同时会更新到Eureka Server其他节点和Eureka client的缓存，流程类似同以上的register过程
## 2.7. 自我保护模式

如果Eureka Server最近1分钟收到renew的次数小于阈值（即预期的最小值），则会触发自我保护模式，此时Eureka Server此时会认为这是网络问题，它不会注销任何过期的实例。等到最近收到renew的次数大于阈值后，则Eureka Server退出自我保护模式。

自我保护模式阈值计算：

    每个instance的预期心跳数目 = 60/每个instance的心跳间隔秒数
    阈值 = 所有注册到服务的instance的数量的预期心跳之和 *自我保护系数

以上的参数都可配置的：

    instance的心跳间隔秒数：eureka.instance.lease-renewal-interval-in-seconds
    自我保护系数：eureka.server.renewal-percent-threshold

如果我们的实例比较少且是内部网络时，推荐关掉此选项。我们也可以通过eureka.server.enable-self-preservation = false来禁用自我保护系数
## 2.8 配置demo

以下配置的demo如下：
``` yml
server:
  port: 10761
spring:
  application:
    name: cloud-registration-center
## eureka ： 主要配置属性在EurekaInstanceConfigBean和EurekaClientConfigBean中
eureka:
  instance:
    # hostname: 127.0.0.1
    # 使用IP注册
    preferIpAddress: true
    # 心跳间隔
    lease-renewal-interval-in-seconds: 3
    # 服务失效时间： 如果多久没有收到请求，则可以删除服务
    lease-expiration-duration-in-seconds: 7
  client:
    # 关闭eureka client
    # enabled: false
    # 注册自身到eureka服务器
    registerWithEureka: true
    # 表示是否从eureka服务器获取注册信息
    fetchRegistry: false
    # 客户端从Eureka Server集群里更新Eureka Server信息的频率
    eureka-service-url-poll-interval-seconds: 60
    # 定义从注册中心获取注册服务的信息
    registry-fetch-interval-seconds: 5
    # 设置eureka服务器所在的地址，查询服务和注册服务都需要依赖这个地址
    serviceUrl:
      defaultZone: http://127.0.0.1:10761/eureka/
       # 设置eureka服务器所在的地址，可以同时向多个服务注册服务
       # defaultZone: http://192.168.21.3:10761/eureka/,http://192.168.21.4:10761/eureka/
  server:
     # renewal-percent-threshold: 0.1
     # 关闭自我保护模式
     enable-self-preservation: false
     # Eureka Server 自我保护系数，当enable-self-preservation=true时，启作用
     # renewal-percent-threshold:
     # 设置清理间隔,单位为毫秒,默认为0
     eviction-interval-timer-in-ms: 3000
     # 设置如果Eureka Server启动时无法从临近Eureka Server节点获取注册信息，它多久不对外提供注册服务
     wait-time-in-ms-when-sync-empty: 6000000
     # 集群之间相互更新节点信息的时间频率
     peer-eureka-nodes-update-interval-ms: 60000

```
