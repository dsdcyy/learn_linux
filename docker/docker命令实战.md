# Docker命令实战

 常用命令  

docker主机

dockerimages

镜像

容器

dockerps-a

dockerps

dockerrun

remove

dockerexec-it

dockerlogs

日志

dockersave

run

dockerexport

ockerrmi

压缩包

容器

/data

dockerload

docker'stop

dockerrm-f

应用

镜像

dockerimport

端口80

数据

dockerrmi

remove

stop

dockercommit

dockerrun-v/hello:/data

dockerbuild

dockerrun-p88:80

dockerpull

/hello

docker-cli

Dockerfile

端口

端口

端口

端口

dockerpush

6379

8848

3306

88

dockerlogin/logout

dockerhub

http://xxxx:88

atg

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1613913/1625373590853-2aaaa76e-d5b5-446b-850a-f6cfa26ac70a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_85%2Ctext_YXRndWlndS5jb20gIOWwmuehheiwtw%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fresize%2Cw_1008%2Climit_0)





 基础实战 

 1、找镜像 

去[docker hub](http://hub.docker.com/)，找到nginx镜像







 2、启动容器 

启动nginx应用容器，并映射88端口，测试的访问







 3、修改容器内容 

修改默认的index.html 页面

 1、进容器内部修改 





 2、挂载数据到外部修改 





 4、提交改变 

将自己修改好的镜像提交







 1、镜像传输 







 5、推送远程仓库 

推送镜像到docker hub；应用市场









 6、补充 







 进阶实战 

 1、编写自己的应用 

编写一个HelloWorld应用

https://start.spring.io/



示例代码：  https://gitee.com/leifengyang/java-demo.git





 2、将应用打包成镜像 

编写Dockerfile将自己的应用打包镜像



 1、以前 

Java为例

●SpringBoot打包成可执行jar

●把jar包上传给服务

●服务器运行java -jar











 2、现在 

所有机器都安装Docker，任何应用都是镜像，所有机器都可以运行





 3、怎么打包-Dockerfile 











思考：
每个应用每次打包，都需要本地编译、再上传服务器、再进行docker构建，如果有1000个应用要打包镜像怎么办？有没有更好的方式？







 3、启动容器 

启动应用容器







Bash

复制代码

1

docker run -d -p 8080:8080 --name myjava-app java-demo:v1.0 



分享镜像





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

\# 登录docker hub

docker login

\#给旧镜像起名

docker tag java-demo:v1.0  leifengyang/java-demo:v1.0

\# 推送到docker hub

docker push leifengyang/java-demo:v1.0

\# 别的机器

docker pull leifengyang/java-demo:v1.0

\# 别的机器运行

docker run -d -p 8080:8080 --name myjava-app java-demo:v1.0 











 4、部署中间件 

部署一个Redis+应用，尝试应用操作Redis产生数据



docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

\#redis使用自定义配置文件启动

```bash
docker run -v /data/redis/redis.conf:/etc/redis/redis.conf \

-v /data/redis/data:/data \

-d --name myredis \

-p 6379:6379 \

redis:latest  redis-server /etc/redis/redis.conf
```



