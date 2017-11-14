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
1、持续的软件版本发布/测试项目。

2、监控外部调用执行的工作。

# 系统环境
CentOS 7、docker

## 安装docker环境
#### 使用仓库安装docker ce
     安装需要的软件包
     $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
     
     设置标准仓库
     $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
     
     安装docker ce
     $ yum install docker-ce -y
     
     列出docker的可选版本
     $ yum list docker-ce --showduplicates | sort -r
     
     如果想让非root用户使用docker，需将用户加入到docker用户组中
     $ sudo usermod -aG docker your-user
     
#### 使用脚本安装docker ce
     下载安装脚本
     $ curl -fsSL get.docker.com -o get-docker.sh
     
     执行安装脚本
     $ sh get-docker.sh



## 时间与更新速度  
鬼才知道我想什么时候更新，毕竟我打字那么慢，我觉得以后可以把这东西当成自己的一个小圈子，于世界分享经验，看法于经历。

# 关于未来的打算
我也想把这个博客一直做下去，因为这东西真的挺好的，毕竟在信息大爆炸的时代，大家热衷于快餐的阅读，并没有时间静下来思考。我也希望以后我能多读点书，成为一个对社会有用的人。

>写在最后：
希望今后能够坚持学习下去~~
