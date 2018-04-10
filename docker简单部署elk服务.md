---
title: docker简单部署elk服务
date: 2018-02-12T17:32:51.000Z
tags: elk
---

# 本文章记录自己通过docker搭建elk的过程

## docker下载镜像

```sh
  docker pull sebp/elk :latest
```

## docker 至少分配3g内存

elasticsearch 至少需要2g，logstash至少需要1g

## 设置最大运行线程数

```
 最大线程数要超过262144
```

- 使用docker toolbox方式 先进入虚拟机

  > docker-machine ssh default

- 永久生效配置

  > sudo vi /etc/sysctl.conf

  > vm.max_map_count=262144

  > sudo sysctl -p //生效

- 临时生效配置

  > sysctl -w vm.max_map_count=262144

## 启动

启动，并配置端口映射

> docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk

## 查看kibana

打开网址 http:\192.168.99.100:5601 若打开成功说明部署成功

## 配置使用

1. 进入容器

  > docker exec -it elk /bin/bash

2. 配置logstash

  > /opt/logstash/bin/logstash -e 'input { stdin { } } output { elasticsearch { hosts => ["localhost"] } }'

  > **注意**：如果看到这样的报错信息 Logstash could not be started because there is already another instance using the configured data directory. If you wish to run multiple instances, you must change the "path.data" setting. 请执行命令：service logstash stop 然后在执行就可以了。

  > 当命令成功被执行后，看到：Successfully started Logstash API endpoint {:port=>9600} 信息后，输入：test 然后回车，模拟一条日志进行测试。

  > 1. 打开浏览器，输入：<http://192.168.99.100:9200/_search?pretty> ，就会看到我们刚刚输入的日志内容
  > 2. 创建kibana与logstash关联
  > 3. index name or pattern 输入 logstash-*
  > 4. time-field name 输入 @timestamp
  > 5. create
  > 6. 打开kibana首页即可看到刚刚输入的内容
