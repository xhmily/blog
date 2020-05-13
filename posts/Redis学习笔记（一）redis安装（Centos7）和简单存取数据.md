---
title: 'Redis学习笔记（一）redis安装（Centos7）和简单存取数据'
date: 2019-07-08 00:20:57
tags: [Redis]
published: true
hideInList: false
feature: 
isTop: false
---

redis是非关系型数据库（NoSql），NoSQL(NoSQL = Not Only SQL )，意即“不仅仅是SQL”，泛指非关系型的数据库。随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题，包括超大规模数据的存储。

### redis的安装

1.创建myredis文件夹

```
mkdir myredis
```

2.下载redis最新版https://redis.io/

可以直接下载再传到Centos系统中，也可以在Centos中使用如下命令直接下载

```
cd myredis #进入文件夹
wget http://download.redis.io/releases/redis-4.0.11.tar.gz  #官网获取的最新版下载地址
```

![1.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/93208aaf8c9e9eda078781d97446e5df.png)

解压文件

```
tar -zxvf redis-4.0.11.tar.gz
```

![2.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/25903b2c36c69056aef412f63c16ac69.png)

安装gcc（redis是用C语言开发的，需要gcc编译）

```
yum install gcc-c++
```

![3.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/e650d043a789e8145a8fa2924b9c1ac0.png)

进入 redis-4.0.11 文件夹并安装

```
cd redis-4.0.11
```

![4.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/c4eaec43d4e699459ff52749c4883306.png)

```
make  #编译
```

![5.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/aa08428fb3d6fe9e0db2974a95c46a74.png)

```
make install  #安装
```

![6.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/28574d869b698bf640bc38e4fe298c47.png)

配置redis

![7.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/71d669562446b3f742908e02ef00eb7f.png)

```
vi redis.conf
#将GENERAL中 daemonize 设置为yes
```

![8.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/ef49c702af0dcbbd1b8240e42c96095d.png)

### 启动redis

进入 /root/myredis/redis-4.0.11/src 目录

```
cd /root/myredis/redis-4.0.11/src
```

![9.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/4754f06480636416931deb80b67cebff.png)

启动redis

```
redis-server ../redis.conf
```

![10.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/176154826b9137d9058a207706a02c01.png)

查看进程是否启动

```
ps -ef|grep redis
```

![11.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/832ef01f844d7c0904152da427aa9464.png)

可见默认端口是 **6379** 服务端已经启动

### 简单的数据存取

在redis中数据是以键值对（Key-value）形式存储的

启动客户端

```
redis-cli
```

![Snipaste_2018-09-19_23-18-04.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/8a615f99da40dc273a8c72ed047ec621.png)

存数据

```
set myFirstData hello-world
```

![12.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/9765d9e2bd31b990e98f7c693bbcdb55.png)

取数据

```
get myFirstData
```

到这里，如果redis存取正常，那么恭喜你，安装完成！

### 退出客户端

```
exit
```

### 关闭服务端

```
redis-cli -p 6379 shutdown
# -p为可选项 默认6379，多个redis运行的情况下需要指明要关闭redis的端口
```

![13.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/d6f31ecc534d517806fa020389a7454c.png)

可见redis已经被关闭