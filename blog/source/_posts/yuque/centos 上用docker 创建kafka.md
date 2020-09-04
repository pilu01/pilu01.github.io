---
title: centos 上用docker 创建kafka
urlname: hzamta
date: 2019-12-18 13:36:13 +0800
tags: []
categories: []
---

```shell
docker ps # 查看正在运行的容器
docker ps -a # 查看所有容器
docker start 容器id  # 启动一个容器
docker logs 容器id  # 查看一个容器日志

```

[https://my.oschina.net/langwanghuangshifu/blog/2251454](https://my.oschina.net/langwanghuangshifu/blog/2251454)

1，拉取所需要的镜像文件

```shell
docker pull wurstmeister/zookeeper  # 拉取zookeeper 镜像
docker pull wurstmeister/kafka   # 拉取kafka 镜像
```

2， 启动镜像，生成容器

```shell
docker run --name zookeeper -p 2181 -t wurstmeister/zookeeper；

docker run  -d --name kafka -p 9092:9092 -e KAFKA_BROKER_ID=0
-e KAFKA_ZOOKEEPER_CONNECT=49.234.190.114:2181
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://49.234.190.114:9092
-e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -t wurstmeister/kafka

# 启动kafka容器，可能出错的原因：
# 1，端口号没有开放
# 2，内存太大，修改修改bin/kafka-server-start.sh，
#将export KAFKA_HEAP_OPTS="-Xmx1G -Xms1G"
#修改为export KAFKA_HEAP_OPTS="-Xmx256m -Xms256m"即可。
```
