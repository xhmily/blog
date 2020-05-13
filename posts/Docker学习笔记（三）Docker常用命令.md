---
title: 'Docker学习笔记（三）Docker常用命令'
date: 2019-07-06 18:15:18
tags: [Docker]
published: true
hideInList: false
feature: 
isTop: false
---

## docker常用命令：

```shell
docker pull 镜像名:TAG   从仓库拉取某镜像

docker run 镜像名:TAG    运行某个镜像

Ctrl+p+q    在容器中使用此命令可退出容器（保留容器进程）

exit   在容器中使用此命令可退出容器（留容也会停止运行）

docker kill 容器id   此命令可以停止指定容器的运行

docker ps 查看当前正在运行的容器

docker ps -a 查看所有容器的状态

docker start/stop id/name 启动/停止某个容器

docker attach 容器id 进入某个容器

docker exec -it id 启动一个伪终端以交互式的方式进入某个容器

docker images 查看本地镜像
docker rm id/name 删除某个容器
docker rmi id/name 删除某个镜像
```



### docker pull 镜像名:TAG   详解:

```shell
docker pull tomcat:8.5
#如果不指定:TAG则默认拉取最新版本
```

<span style="color: #ff6600;">执行结果：</span>

![1.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/136bfbcf6e143ca71a2c3ec52b266b0d.png)

<span style="color: #ff6600;">查看本地镜像：</span>

![2.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/4e15c07d8717536a98a41367e6e4f3e7.png)

#### docker run 镜像名:TAG    详解：

```shell
docker run -it -p 1234:8080 -name MyTomcat tomcat
##可选参数## 
#-it 代表以启动一个伪终端交互式的方式运行镜像
#-p  端口映射，将宿主机的1234端口（可指定其他）映射到tomcat的8080端口
#-d  以守护进程的方式运行，即后台运行，不启动交互界面
#--name 为容器命名，如不指定该参数，系统将默认为其命名
##<span style="color: #ff6600;">注：</span>镜像只是一个模板，每运行一次镜像后将产生一个容器，即容器是镜像运行后的产物
##例如Windows镜像安装之后成为系统，修改系统文件并不影响Windows镜像，并且Windows镜像可多次使用
```

<span style="color: #ff6600;">以上代码运行结果：</span>

![3.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/d289d0f25c97176091839e1a5c12de35.png)

此时访问<span style="color: #ff6600;">localhost:1234</span>便可以看到<span style="color: #ff6600;">tomcat</span>欢迎页：

![4.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/b3102d20fa30ede621a66551e734d05d.png)

此时说明启动tomcat成功！

<span style="color: #ff6600;">查看正在运行中的容器（docker ps）：</span>

![5.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/ba4971a08b21b50d7d1324c06bd303fb.png)

### docker attach 容器id  详解：

该命令可以再次进入为停止的容器，如使用<span style="color: #ff6600;">Ctrl+p+q</span> 退出的容器

![Snipaste_2018-09-12_22-13-54.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/335a2c84b57ff90565a80f3f13309cd0.png)

<span style="color: #ff6600;">注：因为进入的是tomcat，所以只会有一个光标不停闪动，或者只有tomcat日志输出，若进入的是docker版的centos系统中，</span><span style="color: #ff6600;">将会进入到该centos系统的默认路径下。</span>

### docker kill 容器id 详解：

执行命令后，指定容器的进程将会被停止

![7.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/9d2c674e099e08e303ed22fdb3537cf9.png)

### <span style="color: #333333;">docker ps -a 详解：</span>

该命令可以查询出运行过的容器

![8.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/f2d166a0f76be38d85fbb2d090069398.png)

### docker start/stop id/name 详解：

```shell
docker start MyTomcat #启动刚才停止的tomcat容器
```

![9.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/d2e086937ce1909ca564b11962b5592b.png)

```shell
docker stop MyTomcat #停止刚才启动的tomcat容器
```

![10.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/cc66f716211f2cdc122f2fa15cc70175.png)

### docker rm 容器id/name 详解：

该命令可以删除指定容器

```shell
docker rm MyTomcat #删除名字为MyTomcat的容器
```

!![11.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/da8cf517303152fa439e4025d3d217f2.png)

### docker rmi id/name 删除某个镜像  详解：

该命令可以删除指定镜像

```shell
docker rmi hello-world:TAG #不加TAG表示删除最新版
##可选参数##
#-f 表示强制删除
```

![12.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/d229a9f3068e8ca887b7f9904458b293.png)

