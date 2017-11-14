---
layout:     post
title:      基于docker搭建jenkins
subtitle:   自己尝试搭建的一个jenkins服务器
date:       2017-11-09
author:     liuliping
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
---

# jenkins介绍

Jenkins是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

# jenkins功能
1.持续的软件版本发布/测试项目。

2.监控外部调用执行的工作。

# 系统环境
* CentOS 7
* docker

## 安装docker环境
#### 使用仓库安装docker ce
```
# 安装需要的软件包
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
     
# 设置标准仓库
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
     
# 安装docker ce
$ yum install docker-ce -y
     
# 列出docker的可选版本
$ yum list docker-ce --showduplicates | sort -r
     
# 如果想让非root用户使用docker，需将用户加入到docker用户组中
$ sudo usermod -aG docker your-user
```

#### 使用脚本安装docker ce
```
# 下载安装脚本
$ curl -fsSL get.docker.com -o get-docker.sh
     
# 执行安装脚本
$ sh get-docker.sh
```

## jenkins安装
#### 使用docker jenkins images
```
# 启动docker服务
$ systemctl start docker
     
# 搜索jenkins镜像
$ docker search jenkins
   
# 下载jenkins镜像
$ docker pull jenkins
```
     
>jenkins打包编译需要使用docker命令，我这里使用docker run的-v参数将宿主机的docker环境挂载到了jenkins容器中

#### 编写jenkins的dockerfile安装依赖和jenkins相应插件
```
dockerfile文件内容
FROM jenkins
 
USER root
RUN apt-get update \
      && apt-get install -y sudo libltdl7 \
      && rm -rf /var/lib/apt/lists/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
 
USER jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/FROM jenkins
 
USER root
RUN apt-get update \
      && apt-get install -y sudo libltdl7 \
      && rm -rf /var/lib/apt/lists/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
 
USER jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt

plugins.txt文件内容(列出jenkins要安装的插件)
structs
ssh-credentials
credentials
junit
script-security
mailer
workflow-scm-step
workflow-step-api
scm-api
git
matrix-project
ssh-slaves
chucknorris
greenballs
ws-cleanup
git-client
durable-task
```

#### 打包生成新的jenkins镜像
```
# 基于dockerfile构建新的jenkins镜像$ docker build -t dockerjenkins:v1.0 .
```

#### 启动jenkins容器
```
# docker run jenkins container
$ docker run -it -d --restart always --name jenkins -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker -p 8080:8080 dockerjenkins:v1.0
```

## 验证

jenkins默认开启的是8080端口，浏览器访问127.0.0.1:8080

>这里因为宿主机是centos7,jenkins容器使用的基础镜像不是centos,直接将宿主机的docker程序挂载到容器中不知道会不会有什么问题

>参考文献
> -<http://container-solutions.com/running-docker-in-jenkins-in-docker/>
