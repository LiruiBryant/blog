---
title: 谷歌jib工具试用
date: 2018-07-18 21:13:21
tags:
---


# Jib
好久没写blog了，最近看到google新开源的工具心痒痒，试着玩下~
首先什么是Jib，：
> Jib 是 Google 开发的可以直接构建 Java 应用的 Docker 和 OCI 镜像的类库，以 Maven 和 Gradle 插件形式提供。
> 通过 Jib，Java 开发者可以使用他们熟悉的 Java 工具来构建容器。Jib 是一个快速而简单的容器镜像构建工具，它负责处理将应用程序打包到容器镜像中所需的所有步骤。它不需要你编写 Dockerfile 或安装 Docker，而且可以直接集成到 Maven 和 Gradle中 —— 只需要将插件添加到构建中，就可以立即将 Java 应用程序容器化。

## 构建流程
* Docker构建的复杂流程
![](https://static.oschina.net/uploads/space/2018/0710/155927_vHQt_2720166.png)

* Jib构建流程
![](https://static.oschina.net/uploads/space/2018/0710/155918_xYRX_2720166.png)

从此可以告别繁琐的Dockerfile啦~~

## 上手
1. 首先配置jib插件
    maven:
    ```xml
    <plugin>
      <groupId>com.google.cloud.tools</groupId>
      <artifactId>jib-maven-plugin</artifactId>
      <version>0.9.6</version>
      <configuration>
        <from>
              <image>
                            registry.hub.docker.com/adoptopenjdk/openjdk8
              </image>
        </from>
        <to>
          <image>registry.hub.docker.com/earyant/earyant</image>
        </to>
      </configuration>
    </plugin>
    ```

    gradle
    ```gradle
    plugins {
      id 'com.google.cloud.tools.jib' version '0.9.6'
    }
    jib.to.image = 'registry.hub.docker.com/earyant/earyant'
    jib.from.image = 'registry.hub.docker.com/adoptopenjdk/openjdk8'
    ```
    *****
需要注意的是image不加域名的话，默认是gcr.io,google Cloud下的镜像，**需要梯子**，所以没有梯子的话，请使用 registry.hub.docker.com,阿里云加速同理

如果不加from标签，也是默认访问grc.io去下载jdk镜像，emmmm，同样需要梯子，报错如下：
```shell
Build to Docker daemon failed: Connect to gcr.io/108.177.97.82:443 timed out
```
所以记得加上from镜像标签哦~~

如果报错信息如下，请先登录docker login -name=earyant registry.hub.docker.com  ，如果没有注册过，请到docker官网注册~
Retrieving registry credentials for registry.hub.docker.com


2. Build
* build到远程仓库
  * gradle jib
  * mvn compile jib:build

* 本地运行（确保本地docker已运行 ）
  * mvn compile jib:dockerBuild
  * gradle jibDockerBuild

3. 运行
本地镜像查看如下：
docker images
```
earyant/earyant       latest              e34b4cad637b        48 years ago        367MB
```

docker run -p 8880:8880  -it --rm --name earyant registry.hub.docker.com/earyant/earyant

访问 http://localhost:8880 即可纵享丝滑，开心~~~
