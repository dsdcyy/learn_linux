# Docker基本概念

 1、解决的问题 

 1、统一标准 

●应用构建 

○Java、C++、JavaScript

○打成软件包

○.exe

○docker build ....   镜像

●应用分享

○所有软件的镜像放到一个指定地方  docker hub

○安卓，应用市场

●应用运行

○统一标准的 镜像

○docker run

●.......



容器化







 2、资源隔离 

●cpu、memory资源隔离与限制

●访问设备隔离与限制

●网络隔离与限制

●用户、用户组隔离限制

●......



 2、架构 

![img](https://cdn.nlark.com/yuque/0/2021/svg/1613913/1624937894925-f437bd98-94e2-4334-9657-afa69bb52179.svg)







●Docker_Host：

○安装Docker的主机

●Docker Daemon：

○运行在Docker主机上的Docker后台进程

●Client：

○操作Docker主机的客户端（命令行、UI等）

●Registry：

○镜像仓库

○Docker Hub

●Images：

○镜像，带环境打包好的程序，可以直接启动运行

●Containers：

○容器，由镜像启动起来正在运行中的程序



交互逻辑

装好Docker，然后去 软件市场 寻找镜像，下载并运行，查看容器状态日志等排错



 3、安装 

 1、centos下安装docker 

其他系统参照如下文档

https://docs.docker.com/engine/install/centos/



 1、移除以前docker相关包 







 2、配置yum源 





Bash

复制代码

1

2

3

4

5

sudo yum install -y yum-utils

sudo yum-config-manager \

--add-repo \

http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo



 3、安装docker 





Bash

复制代码

1

2

3

4

5

sudo yum install -y docker-ce docker-ce-cli containerd.io

\#以下是在安装k8s的时候使用

yum install -y docker-ce-20.10.7 docker-ce-cli-20.10.7  containerd.io-1.4.6



 4、启动 





Bash

复制代码

1

systemctl enable docker --now



 5、配置加速 

这里额外添加了docker的生产环境核心配置cgroup





Bash

复制代码

1

2

3

4

5

6

7

8

9

10

11

12

13

14

sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'

{

  "registry-mirrors": ["https://82m9ar63.mirror.aliyuncs.com"],

  "exec-opts": ["native.cgroupdriver=systemd"],

  "log-driver": "json-file",

  "log-opts": {

​    "max-size": "100m"

  },

  "storage-driver": "overlay2"

}

EOF

sudo systemctl daemon-reload

sudo systemctl restart docker