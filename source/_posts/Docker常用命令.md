---
title: Docker常用命令
cover: 'https://img.showydream.com/img/ZhBcmx-iStock-1144628524.jpeg'
description: Docker常用命令
keywords: docker
tags: Docker
categories:
  - Docker
abbrlink: 29dc6fe8
date: 2021-06-11 00:00:00
---



### 安装和常用CLI

添加阿里云镜像：sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

安装命令：sudo yum install -y  docker-ce docker-ce-cli containerd.io

启动命令：sudo systemctl start docker

添加当前用户到docker用户组：sudo usermod -aG docker $USER （需注销），newgrp docker （立即生效）

Helloworld：docker run hello-world  （本地没有镜像的话会自动从远端仓库pull）

pull nginx 镜像：docker pull nginx（等效于nginx:latest）

运行：docker run -【d】（后台运行不阻塞shell） 【-p 80:80】（指定容器端口映射，内部：外部） nginx

查看正在运行：docker ps

删除容器：docker rm -f container id(不用打全，前缀区分)

进入bash：docker exec -it container id(不用打全，前缀区分) bash

commit镜像：docker commit container id(不用打全，前缀区分)  name

查看镜像列表：docker images （刚才commit的镜像）

使用运行刚才commit的镜像：docker run -d name

使用Dockerfile构建镜像：docker build -t name 存放Dockerfile的文件夹

删除镜像：docker rmi name

保存为tar：docker save name  tar name

从tar加载：docker load  tar name

一些启动参数：

后台运行容器：-d

容器内外端口映射：-p 内部端口号:外部端口号

目录映射：-v dir name : dir

指定映像版本：name:ver
