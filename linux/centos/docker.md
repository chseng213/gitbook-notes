---
description: 安装以及加速器安装
---

# docker



> 在镜像的基础上生成容器,镜像只读
>
> 容器是在镜像基础之上创建出的虚拟实例,内容可读可写
>
> 通过镜像可以创建多个容器

**硬件设备->linux->docker->镜像->多个容器**



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

## 命令

### 镜像相关命令

> 与仓库相关
>
> + search
> + push
> + pull
>
> 与导入导出相关
>
> + save/export
> + load/import
>
> 与dockerfile相关
>
> + build
>
> 查看删除
>
> + images
> + inspect
> + rmi



```
# 下载镜像
docker pull  python:3.8

# 查看镜像
docker images

# 查看image信息
docker inspect  [名称/id]

# 导出镜像 
docker save python:3.8  >/root/python.tar

# 删除镜像  rmi   (无以改镜像创建的容器)
docker rmi python:3.8

# tar导入为镜像
docker load < /root/python.tar



```

### 容器相关命令

> + attach/exec/run
> + start/stop/pause/unpause
> + inspect/ps
> + rm



```
# 创建运行并进入容器  退出容器时 容器停止
docker run -it --name=p1 python:3.8 bash 

# 查看全部容器
docker ps -a 

# 运行
docker  start p1

# 暂停
docker pause p1

# 
docker unpause p1
 
# 进入运行容器中  推出容器时  容器依然是执行状态
docker exec -it p1 bash

# 删除容器 (只能删除停止的容器)
docker rm p1

```

