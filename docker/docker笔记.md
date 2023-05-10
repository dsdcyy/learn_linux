# **什么是容器**

容器一词的英文是container，其实container还有集装箱的意思，集装箱绝对是商业史上了不起的一项发明，大大降低了海洋贸易运输成本。让我们来看看集装箱的好处：

- 集装箱之间相互隔离
- 长期反复使用
- 快速装载和卸载
- 规格标准，在港口和船上都可以摆放

![img](https://pic3.zhimg.com/v2-f698870a2becd150a5376942be7368de_b.jpg)

回到软件中的容器，其实容器和集装箱在概念上是很相似的。

现代软件开发的一大目的就是隔离，应用程序在运行时相互独立互不干扰，这种隔离实现起来是很不容易的，其中一种解决方案就是上面提到的虚拟机技术，通过将应用程序部署在不同的虚拟机中从而实现隔离。

![img](https://pic1.zhimg.com/v2-0f6ede7f0b920b5d0d5571c937a04838_b.jpg)

但是虚拟机技术有上述提到的各种缺点，那么容器技术又怎么样呢？

与虚拟机通过操作系统实现隔离不同，容器技术**只隔离应用程序的运行时环境但容器之间可以共享同一个操作系统**，这里的运行时环境指的是程序运行依赖的各种库以及配置。

![img](https://pic2.zhimg.com/v2-907214eadd65987e84a0751c08143f91_b.jpg)

从图中我们可以看到容器更加的**轻量级且占用的资源更少**，与操作系统动辄几G的内存占用相比，容器技术只需数M空间，因此我们可以在同样规格的硬件上**大量部署容器**，这是虚拟机所不能比拟的，而且不同于操作系统数分钟的启动时间容器几乎瞬时启动，容器技术为**打包服务栈**提供了一种更加高效的方式，So cool。

那么我们该怎么使用容器呢？这就要讲到docker了。

注意，容器是一种通用技术，docker只是其中的一种实现。

# **什么是docker**

docker是一个用Go语言实现的开源项目，可以让我们方便的创建和使用容器，docker将程序以及程序所有的依赖都打包到docker container，这样你的程序可以在任何环境都会有一致的表现，这里程序运行的依赖也就是容器就好比集装箱，容器所处的操作系统环境就好比货船或港口，**程序的表现只和集装箱有关系(容器)，和集装箱放在哪个货船或者哪个港口(操作系统)没有关系**。

因此我们可以看到docker可以屏蔽环境差异，也就是说，只要你的程序打包到了docker中，那么无论运行在什么环境下程序的行为都是一致的，程序员再也无法施展表演才华了，**不会再有“在我的环境上可以运行”**，真正实现“build once, run everywhere”。

此外docker的另一个好处就是**快速部署**，这是当前互联网公司最常见的一个应用场景，一个原因在于容器启动速度非常快，另一个原因在于只要确保一个容器中的程序正确运行，那么你就能确信无论在生产环境部署多少都能正确运行。

# docker最核⼼的组件

image镜像，构建容器（我们讲应⽤程序运⾏所需的环境，打包为镜像⽂件）

Container，容器（你的应⽤程序，就跑在容器中）

镜像仓库(dockerhub)（保存镜像⽂件，提供上传，下载镜像）作⽤好⽐github

Dockerfile，将你部署项⽬的操作，写成⼀个部署脚本，这就是dockerfile，且该脚本还

能够构建出镜像⽂件

创建容器的过程

获取镜像，如 docker pull centos ，从镜像仓库拉取

使⽤镜像创建容器

分配⽂件系统，挂载⼀个读写层，在读写层加载镜像

分配⽹络/⽹桥接⼝，创建⼀个⽹络接⼝，让容器和宿主机通信

容器获取IP地址

执⾏容器命令，如/bin/bash

反馈容器启动结果。

Version:0.9 StartHTML:0000000105 EndHTML:0000000530 StartFragment:0000000141 EndFragment:0000000490

# **安装**docker 

提前准备好⼀个宿主机（vmware去创建⼀个linux机器，然后安装docker去使⽤）

## 基础环境配置

```bash
wget -O /etc/yum.repos.d/CentOS-Base.repo
http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo
http://mirrors.aliyun.com/repo/epel-7.repo
yum clean all
yum makecache
yum install -y bash-completion vim lrzsz wget expect net-tools nc nmap tree dos2unix htop iftop iotop unzip telnet sl
psmisc nethogs glances bc ntpdate openldap-devel

```

安装docker

## 开启linux内核的流量转发

```bash
cat <<EOF > /etc/sysctl.d/docker.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.all.rp_filter = 0
net.ipv4.ip_forward=1
EOF
89 # 加载修改内核的参数，配置⽂件
10 # 按照如下命令，执⾏顺序
modprobe br_netfilter
sysctl -p /etc/sysctl.d/docker.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.all.rp_filter = 0
net.ipv4.ip_forward = 1
```

## 利⽤yum快速安装docker

```bash
# 提前配置好yum仓库
# 1.阿⾥云⾃带仓库 2.阿⾥云提供的docker专属repo仓库
curl -o /etc/yum.repos.d/docker-ce.repo
http://mirrors.aliyun.com/docker-ce/linux/centos/dockerce.repo
curl -o /etc/yum.repos.d/Centos-7.repo
http://mirrors.aliyun.com/repo/Centos-7.repo
# 更新yum缓存
yum clean all && yum makecache
# 可以直接yum安装docker了

# yum安装
yum install docker-ce-20.10.6 -y
# 查看源中可⽤版本
yum list docker-ce --showduplicates | sort -r

# 如果需要安装旧版本
yum install -y docker-ce-18.09.9
# 如果要卸载
yum remove -y docker-xxx

```

## **配置**docker docker**加速器**

```bash
 mkdir -p /etc/docker
 touch /etc/docker/daemon.json
 vim /etc/docker/daemon.json
 cat /etc/docker/daemon.json
 {
 "registry-mirrors" : [
 "https://8xpk5wnt.mirror.aliyuncs.com"
 ]
}
# 启动docker
systemctl daemon-reload
systemctl enable docker 
systemctl restart docker


# 
curl -sSL
https://get.daocloud.io/daotools/set_mirror.sh | sh -s
http://f1361db2.m.daocloud.io
```



# 启动docker

```
systemctl start docker
```

## 查看docker版本

```bash
docker version
```

## 查看镜像

```bash
docker images
```

### 查看具体的镜像信息

```bash
docker images nginx
```

### 只查看镜像id

```bash
docker images -q
```

### 格式化显示镜像信息

```bash
docker images --format "{{.ID}}--{{.Repository}}"
```

### 以表格形式美化

```bash
docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
```



## 查看数据存放目录

```
docker info |grep -i root
# Docker Root Dir: /var/lib/docker
# /var/lib/docker/image/overlay2/imagedb/content/sha256
# 文件为json格式
```

## 搜索镜像

```bash
docker search nginx # 镜像名
```

## 下载镜像

```bash
docker pull nginx
```

## 删除镜像

```bash
docker rmi 904b8cb13b93 # image/id
```

### 强制删除

```bash
docker rmi 904b8cb13b93 --force # image/id
```



## 启动镜像，绑定运行端口

```bash
# -d 后台运行
# -p 端口绑定

docker run -d -p 80:80 nginx
# 返回一个id
852f97524fd94602ce4f2af1259774faf6175390593548cb2f0aa000d93c9b41
```

## 查看运行中的容器

```bahs
docker ps
:<<!
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS         PORTS                               NAMES
852f97524fd9   nginx     "/docker-entrypoint.…"   6 minutes ago   Up 6 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   xenodochial_kowalevski
!
```

停止具体容器运行

```bash
docker stop 852f97524fd9 # 具体容器id
```

启动具体容器

```bash
docker start 852f97524fd9
```

# docker生命周期

![image-20230317120944118](/home/Ljw/.config/Typora/typora-user-images/image-20230317120944118.png)

# 灵活的替换linux发行版

## 利用docker下载linux发行版镜像

```bash
docker pull centos:7.8.2003
```

## 运⾏centos7.8.2003发⾏版

运⾏容器，且进⼊容器内

```bash
# 参数解释  

# -i 交互式命令操作

#-t 开启⼀个终端 afb6fca791e0 是镜像的id bash 进⼊容器后，执⾏的命令

docker run -it afb6fca791e0 bash
# [root@2c0f8217dc78 /]
```

⼩节

1.⼀个完整的系统，是由linux的内核+发⾏版，才组成了⼀个可以使⽤的完整的系统

2.利⽤docker容器，可以获取不同的发⾏版镜像，然后基于该镜像，运⾏出各种容器去使⽤

# docker镜像原理

## docker分层原理

![image-20230318102318352](/home/Ljw/.config/Typora/typora-user-images/image-20230318102318352.png)

![image-20230318102429637](/home/Ljw/.config/Typora/typora-user-images/image-20230318102429637.png)

## 进⼊到正在运⾏的容器内

命令是 docker exec

```bash
docker exec -it 1a033aef64fe bash
```

## 可写层容器

![image-20230318105241842](/home/Ljw/.config/Typora/typora-user-images/image-20230318105241842.png)

![image-20230318105309956](/home/Ljw/.config/Typora/typora-user-images/image-20230318105309956.png)

所有对容器的修改动作，都只会发生在容器层里，只有容器层是可写的，其余镜像层都是只读的。

只有当需要修改时才复制一份数据，这种特性被称作`Copy-on-Write`。可见，容器层保存的是镜像变化的部

分，不会对镜像本身进行任何修改。

这样就解释了我们前面提出的问题：容器层记录对镜像的修改，所有镜像层都是只读的，不会被容器修改

所以镜像可以被多个容器共享。

## 查看数据存放目录

```bash
docker info |grep -i root
# Docker Root Dir: /var/lib/docker
# /var/lib/docker/image/overlay2/imagedb/content/sha256
# 文件为json格式
```

## 查看具体的镜像信息

```bash
docker images nginx
```

# docker删除

## 删除镜像

```bash
docker rmi 904b8cb13b93 # 镜像id
# 前三位即可
```

## 删除容器

```bash
docker rm 2c0f8217dc78 # 容器id
```

## 批量删除镜像

```bash
docker rmi `docker images -aq`
```

## 批量删除容器

```bash
docker rm `docker ps -aq`
```

# docker镜像导入导出

## 导出镜像

```bash
docker image save nginx:latest > /opt/nginx/latest.tgz
```

## 导入镜像

```bash
docker image load -i /opt/nginx/latest.tgz
```

# 查看信息

## 查看docker服务的信息

```bash
docker info
```

## 查看镜像的详细信息

```bash
docker image inspect 904b8cb13b93 
```

```json
[
    {
        "Id": "sha256:904b8cb13b932e23230836850610fa45dce9eb0650d5618c2b1487c2a4f577b8",
        "RepoTags": [
            "nginx:latest"
        ],
        "RepoDigests": [],
        "Parent": "",
        "Comment": "",
        "Created": "2023-03-01T18:43:12.914398123Z",
        "Container": "71a4c9a59d252d7c54812429bfe5df477e54e91ebfff1939ae39ecdf055d445c",
        "ContainerConfig": {
            "Hostname": "71a4c9a59d25",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.23.3",
                "NJS_VERSION=0.7.9",
                "PKG_RELEASE=1~bullseye"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"nginx\" \"-g\" \"daemon off;\"]"
            ],
            "Image": "sha256:6716b8a33f73b21e193bb63424ea1105eaaa6a8237fefe75570bea18c87a1711",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "DockerVersion": "20.10.23",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.23.3",
                "NJS_VERSION=0.7.9",
                "PKG_RELEASE=1~bullseye"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "sha256:6716b8a33f73b21e193bb63424ea1105eaaa6a8237fefe75570bea18c87a1711",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 141838643,
        "VirtualSize": 141838643,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/510498676d927d30e7db56b32c68522c835eebb6ddea2591e17f8efa8055cd52/diff:/var/lib/docker/overlay2/d5826be01d08f9298c0b09359ef27c31e0e60dd6f0aa1c636d04748f57ba05f4/diff:/var/lib/docker/overlay2/53e34ea9c97af95b5ab864ba44989c3ee338bfc8fabe9eb07fe60b4ea6b58439/diff:/var/lib/docker/overlay2/48a9a89919496aa3b22bae011aea7dcd78ba759fef27a4048c216c7ebe9e2e0f/diff:/var/lib/docker/overlay2/9836d09cface59f842a38c48e66bddd2c35ebb24bcd864094982a9dcc2f91efb/diff",
                "MergedDir": "/var/lib/docker/overlay2/931f22cc24bbd1f9d350d78cc266931ac954e6a03abe21b9d66f1db4460ca244/merged",
                "UpperDir": "/var/lib/docker/overlay2/931f22cc24bbd1f9d350d78cc266931ac954e6a03abe21b9d66f1db4460ca244/diff",
                "WorkDir": "/var/lib/docker/overlay2/931f22cc24bbd1f9d350d78cc266931ac954e6a03abe21b9d66f1db4460ca244/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:650abce4b096b06ac8bec2046d821d66d801af34f1f1d4c5e272ad030c7873db",
                "sha256:4dc5cd799a08ff49a603870c8378ea93083bfc2a4176f56e5531997e94c195d0",
                "sha256:e161c82b34d21179db1f546c1cd84153d28a17d865ccaf2dedeb06a903fec12c",
                "sha256:83ba6d8ffb8c2974174c02d3ba549e7e0656ebb1bc075a6b6ee89b6c609c6a71",
                "sha256:d8466e142d8710abf5b495ebb536478f7e19d9d03b151b5d5bd09df4cfb49248",
                "sha256:101af4ba983b04be266217ecee414e88b23e394f62e9801c7c1bdb37cb37bcaa"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]

```

# docker容器管理

## docker run 等于创建加启动

docker run 镜像名，如果镜像不存在本地，则会在线去下载该镜像

## 运⾏容器的玩法

### 运⾏⼀个挂掉的容器

（从错误示范来学docker容器）

```bash
docker run centos:7.8.2003
```

这个写法，会产多多独⽴的容器记录，且容器内没有程序在跑，因此挂了

### 运⾏容器，进⼊容器内，且执⾏某个命令

```bash
docker run -it centos:7.8.2003 sh
```

## 丰富docker运⾏的参数

### -d 后台运⾏

### --rm 容器挂掉后⾃动被删除

### --name 给容器起个名字

### --it  进行交互并且开启一个终端

### -p 端口映射

### -P 宿主机随机端口映射到docker内端口

## 查看运行的容器

```bahs
docker ps
```

## 查看容器日志

```bash
docker logs d6bb4ef5d808  # 容器id
```

### -f 实时刷新

```bash
docker logs -f d6bb4ef5d808  # 容器id
```

## 进入到容器内部

```bash
docker exec -it d6bb4ef5d808 bash # 容器id
```

## 查看详细的容器信息

```bash
docker container inspect d6bb4ef5d808 # 容器id
```

## 查看docker端口转发情况

```bash
docker port d6bb4ef5d808 # 容器id
```

## 提交docker镜像

```bash
docker image commit d6bb4ef5d808(镜像id) nginx01(名字)
```

## docker-cp

```bash
docker cp 镜像id:/etc/nginx/nginx.conf /data/conf/nginx.conf
```

## 清除所有docker内容

```bash
docker system prune 
```



# dockerfile定制

## FROM 

这个镜像的妈妈是谁？（指定基础镜像）

## MAINTAINER 

告诉别⼈，谁负责养它？（指定维护者信息，可以没有）

RUN 你想让它⼲啥（在命令前⾯加上RUN即可）

ADD 添加宿主机的⽂件到容器内，还多了⼀个⾃动解压的功能

\# RUN tar -zxf /opt/xx.tgz # 报错！该tgz⽂件不存在！！

COPY 作⽤和ADD是⼀样的，都是拷⻉宿主机的⽂件到容器内，COPY就是仅仅拷⻉

WORKDIR 我是cd,今天刚化了妆（设置当前⼯作⽬录）

VOLUME 给它⼀个存放⾏李的地⽅（设置卷，挂载主机⽬录）

EXPOSE 它要打开的⻔是啥（指定对外的端⼝），在容器内暴露⼀个窗⼝，端⼝ EXPORT 80

CMD 奔跑吧，兄弟！（指定容器启动后的要⼲的事情）

systemctl restart 

## 实践

需求，通过dockerfile，构建nginx镜像，且运行容器后，生成的页面是超哥带你学docker

```dockerfile
from nginx
maintainer ljw
run echo '<meta charset=utf-8>超哥带你学docker' > /usr/share/nginx/html/index.html
```

### 通过dockerfile构建镜像

```bash
docker bulid .(Dockerfile文件在当前终端所在目录)
```



### 修改镜像tag

```bash
docker tag 0e9f11b728fb(镜像id) my_nginx
```

### 运行该镜像

```bash
docker run -d -p 80:80 0e9f11b728fb 
```

### 访问宿主机对应端口

ip



## COPY

```dockerfile
copy chaoge.py /home/ 
```

⽀持多个⽂件，以及通配符形式复制，语法要满⾜Golang的filepath.Match

```dockerfile
copy chaoge* /tmp/cc?.txt. /home/
```

COPY指令能够保留源文件的元数据，如权限，访问时间等等，这点很重要

## ADD

```dockerfile
ADD chaoge.tgz /home/
```

特性和COPY基本一致，不过多了些功能

1.源文件是一个URL，此时docker引擎会下载该链接，放入目标路径，且权限自动设为600，若这不是期望结果，还得增加一层RUN

2.源文件是一个URL，且是一个压缩包，不会自动解压，也得单独用RUN指令解压

3.源文件是一个压缩文件，且是gzip，bzip2，xz，tar情况，ADD指令会自动解压缩该文件到目标路径

RUN linux命令（xxx修改命令）

## CMD 

```dockerfile
# cmd ["参数1","参数2"] 
cmd ["bin/bash"]
```

```dockerfile
# 该容器运⾏时，执⾏的命令
# 等同于命令⾏的直接操作 docker run -it centos cat /etc/os-release
CMD ["cat","/etc/os-release"]
```

在指定了entrypoint指令后，用CMD指定具体的参数

docker不是虚拟机，容器就是一个进程，既然是进程，那么程序在启动的时候需要指定些运行参数，这就是CMD指令作用

例如centos镜像默认的CMD是`/bin/bash`.

直接`docker run -it centos`会直接进入bash解释器。

也可以启动容器时候，指定参数．`docker run -it centos cat /etc/os-release`

CMD运行she11命令，也会被转化为shell形式

例如

`CMD echo $PATH`



把宿主机安装，启动nginx的理念放⼊到dockerfile

```dockerfile
RUN yum install nginx

RUN 配置⽂件修改 sed

RUN systemctl start nginx 
```

容器内的程序必须是前台运⾏，你的容器是启动不了的

正确的应该是 `CMD ["nginx","-g","daemon off;"]`

## ENTRYPOINT 

作⽤和CMD⼀样，都是在指定容器启动程序以及参数。
当指定了ENTRYPOINT之后，CMD指令的语义就有了变化
⽽是吧CMD的内容当作参数传递给ENTRYPOINT指令。

```dockerfile
CMD xxxx
ENTRYPOINT xxx
```

### 准备⼀个dockerfile

```dockerfile
FROM centos:7.8.2003
RUN yum install curl -y
CMD ["curl","-s","http://ipinfo.io/ip"]
```

当有cmd命令时在运行时添加命令时会覆盖cmd的命令

### 解决cmd命令被覆盖

```bash
# 不推荐
docker run 37ef32aa6b86 curl -s http://ipinfo.io/ip -I
```

```dockerfile
# 替换cmd命令使用ENTRYPOINT
ENTRYPOINT ["curl","-s","http://ipinfo.io/ip"]
```

```bash
# 使用时直接加在后面即可
docker run run 37ef32aa6b86 -I
```

## ARG**和**ENV 

设置环境变量

```dockerfile
ENV NAME="ljw"
ENV AGE="20"
ENV MYSQL_VERSION=8.0.28
# 引用 $变量名
```

ARG和ENV⼀样 设置环境变量

区别在于ENV ⽆论是在`镜像构建时`，还是`容器运⾏`，该变量`都可以使⽤`

ARG只是⽤于`构建镜像`需要设置的变量，`容器运⾏`时就`消失`了

## VOLUME

容器在运⾏时，应该保证在存储层不写⼊任何数据，运⾏在容器内产⽣的数据，我们推荐是挂载，写⼊到宿主机上，进⾏维护。

```dockerfile
FROM centos
MAINTAINER ljw
VOLUME ["/data1","/data2"]
# 将容器内的/data1,/data2⽂件夹 ，在容器运⾏时，该⽬录⾃动挂载为匿名卷，
# 任何向该⽬录中写⼊数据的操作，都不会被容器记录，保证的容器存储层⽆状态理念
```

### docker inspetc 容器id

查看具体运行信息

1.容器数据挂载的⽅式，通过dockerfile，指定VOLUME⽬录

2.通过docker run -v 参数，直接设置需要映射挂载的⽬录

## EXPOSE

指定容器运⾏时对外提供的端⼝服务

帮助使⽤该镜像的⼈，快速理解该容器的⼀个端⼝业务

```bash
docker port 容器
docker run -p 宿主机端⼝：容器端⼝
docker run -P # 作⽤是随机宿主机端⼝：容器内端⼝
```

```dockerfile
EXPOSE 3306
```

## WORKDIR

⽤于在dockerfile中，⽬录的切换，更改⼯作⽬录

```dockerfile
WORKDIR /opt/
```

## USER

⽤于改变环境，⽤于切换⽤户

```dockerfile
USER root
USER chaoge
```

# 构造一个网站镜像

​	1.nginx，修改⾸⻚内容，htm了 ⽹站就跑起来了，web server，提供web服务，提供代理转发，提供⽹关，限流扥等。apache

2. web framework，web框架，⼀般由开发，通过某个开发语⾔，基于某个web框架，⾃⼰去开发⼀个web站点，python,django框架

> 1.⽤python语⾔，基于flask web框架，开发⼀个⾃⼰的⽹站，写了⼀个后端
>
> 的⽹站代码
>
> 2.开发dockerfile，部署该代码，⽣成镜像
>
> 3.其他⼈基于该镜像，docker run就可以在电脑跑起来你这个⽹站的router，也理解为path
>
> http://pythonav.cn/hello 
>
> http://pythonav.cn/hello
>

## py文件

```python
#coding:utf8
from flask import Flask
app=Flask(__name__)
@app.route("/")
def hello():
    return "hello docker,i am ljw...it worked"
if __name__=="__main__":
    app.run(host='0.0.0.0',port=8080)
```

## dockerfile文件

```dockerfile
FROM centos:7.8.2003
MAINTAINER ljw
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8
RUN curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo;
RUN curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo;
RUN yum makecache fast;
RUN yum install python3-devel python3-pip -y;
RUN pip3 install -i https://pypi.douban.com/simple flask
COPY  hello.py /opt
WORKDIR /opt
EXPOSE 8080
CMD ["python3","hello.py"]
```

## 构建docker镜像

```bash
docker build --no-cache -t 'ljw_flask_demo' .
```

## 运行镜像,生成容器

```bash
docker run -d --name my_flask_web_1 -p 90:8080 b0d36f316d00
```

## 修改容器内的应用

1.修改宿主机的代码，以及dockerfile，重新构建

2.你可以进⼊到以及运⾏的容器内，修改代码，重启容器即可

```bash
docker exec -it f4a2f02a1b3a(容器id) bash
 vi chaoge_flask.py
# 重启容器
docker restart 4efde9370b50
```

# 镜像推送

## 部署私有镜像仓库

```bash
docker run -d -p 5000:5000 --name registry registry:2
```

## 推送本地镜像到私有仓库

Tag the image so that it points to your registry

### 标记镜像，指向私有仓库

```bash
docker image tag ubuntu localhost:5000/myfirstimage[私有库内的镜像名]
```

### 推送

```bash
docker push localhost:5000/myfirstimage
```

### pull回来

```bash
docker pull localhost:5000/myfirstimage
```

## 查看镜像仓库元数据

```bash
curl -X GET http://127.0.0.1:5000/v2/_catalog
```

## 一个上传案例

```bash
# 我的镜像仓库是dsdcyy/ljw,我需要上传一个REPOSITORY为calico/node,TAG为v3.8.9的镜像,该怎么做可以通过以下步骤上传镜像：

# 打开终端并使用docker pull命令拉取需要上传的镜像。在这种情况下，需要拉取calico/node:v3.8.9镜像。可以执行以下命令：

docker pull calico/node:v3.8.9

# 使用docker tag命令将刚刚拉取的镜像重新命名并标记为需要上传的仓库和标签。在这种情况下，需要将calico/node:v3.8.9重新命名为dsdcyy/ljw:calico-node-v3.8.9。可以执行以下命令：

docker tag calico/node:v3.8.9 dsdcyy/ljw:calico-node-v3.8.9

使用docker push命令上传新命名的镜像。可以执行以下命令：
sh
Copy code
docker push dsdcyy/ljw:calico-node-v3.8.9
这样就可以将calico/node:v3.8.9镜像上传到dsdcyy/ljw仓库中，并将其标记为calico-node-v3.8.9标签。如果该仓库是公开的，其他用户也可以使用docker pull dsdcyy/ljw:calico-node-v3.8.9命令来拉取该镜像
```



# 部署java项目+redis

## 创建宿主机redis.conf文件

```ini

# 设置允许任何来源访问
bind 0.0.0.0 -::1
# 监听地址
# bind 127.0.0.1 -::1
# 密码
requirepass 12344321

# 是否开启AOF持久化，默认为no

appendonly yes

# 工作目录，默认在当前路径
dir ./

# 日志文件
logfile "redis.log"

```

## 启动redis镜像

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

#redis使用自定义配置文件启动

docker run -v ~/data/redis/redis.conf:/etc/redis/redis.conf \
-v ~/data/redis/data:/data \
-d --name myredis \
-p 6379:6379 \
redis:latest  redis-server /etc/redis/redis.conf

# /etc/redis/redis.conf 初始化内部脚本
```

## 编写dockerfile打包镜像

```dockerfile
FROM openjdk:21-ea-11-jdk-slim
LABEL MAINTAINER ljw
COPY redis-demo-0.0.1-SNAPSHOT.jar /opt
WORKDIR /opt
EXPOSE 8080
ENTRYPOINT [ "java","-jar","redis-demo-0.0.1-SNAPSHOT.jar"]
```

## 启动镜像

```bash
docker run --name -d -p 8080:8080 镜像id
```

