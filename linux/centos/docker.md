---
description: 安装以及加速器安装
---

# docker



## 安装docker

```
# 添加 -y 避免安装时候的确认操作
yum install docker -y

service   start docker
```



## 安装docker加速器

```
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io

# 修改/etc/docker/deamon.json文件,将mirrors列表中的逗号去掉(部分系统不需要去)

service restart docker
```

