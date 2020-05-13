---
title: 'Redis学习笔记（四）主从复制及启动哨兵模式'
date: 2019-07-08 00:38:08
tags: [Redis]
published: true
hideInList: false
feature: 
isTop: false
---

Redis主从复制，主机数据更新后根据配置和策略，自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主，优点是读写分离，容灾恢复。

### 启动三台redis

首先准备三个配置文件并分别配置三个不同端口，例如6379，6380，6381

![1.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/27e80318e2d1c960c9e1b82cc5c31f6c.png)

![2.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/f40441601ba0bdd9edc725fdd4073d80.png)

Pid文件名字，Log文件名字，Dump.rdb名字请根据需要自行设置

然后分别启动三台redis

![3.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/46393812ad848d9d6644013557fb7da0.png)

下图可见三台redis均启动成功

![4.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/e497a8c339625bdb2b2be15a178e2a53.png)

下面分别连接三台redis并查看当前redis的状态

![5.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/a4fa56a46d00b1757604a7874ff7bdc4.png)

![6.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/3349d3274cc1ae756b93e336c3bc8614.png)

![7.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/7acc36c6623ec8c57175c43df7337174.png)

可见，所有redis默认均为主库，并且没有从库连接

### 配置主从复制

主从服务器的配置原则是：配从(库)不配主(库)，即在从库进行配置，主库不用配置

注：无这里使用端口为6379的库作为主库，6380，6381库作为从库

配置代码：

```
SLAVEOF 127.0.0.1 6379 #需要在两个从库中分别执行
```

再次查看三个库的状态：

`Info replication`
主库

![8.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/adec81fa42dfadb0e40042861b5efa02.png)

从库

![9.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/7e583c8163247cbc78b81921a14411d1.png)

![10.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/a4259fcb18c90eca1c0915de8a3dba41.png)

注：每次与master断开之后，都需要重新连接，除非你配置进redis.conf文件

测试数据同步,在主库中执行：

```
set k1 v1 #从库是只读的，不能执行写操作
```

在两个从库中取出k1数据：

```
get k1
```

可见主从复制已经配置成功

### 知识点

1.上一个Slave可以是下一个slave的Master，Slave同样可以接收其他slaves的连接和同步请求，那么该slave作为了链条中下一个的master,可以有效减轻master的写压力

2.中途变更转向:会清除之前的数据，重新建立拷贝最新的

3. 主机shutdown或者挂掉后，从机不会自动上位成为主机，而是保持不变等待主机

![11.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/3eccff218c84219e84bf5dcb3ca10d26.png)

当主机再次上线时会自动连接

![12.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/7f1906dee03903145bb0810491252303.png)

4.反客为主，可以使用命令：SLAVEOF no one 使当前数据库停止与其他数据库的同步，转成主数据库，这种方式并不方便，因为需要手动操作

### 启动哨兵模式

反客为主的自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库

1.在你的配置文件目录下新建sentinel.conf文件，名字绝不能错,并填入如下内容

```
sentinel monitor 被监控库的名字(自定) 主库ip 端口 1  #示例，1 表示当票数大于1时即可成为主库
sentinel monitor host6379 127.0.0.1 6379 1  #我的
#一组sentinel能同时监控多个Master
```

2.开启哨兵

```
redis-sentinel /myredis/config/sentinel.conf  #配置文件路径请自行修改,启动成功如下图
```

![13.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/020c2e2a8a71d380a24e33c0055ec08e.png)

3.当主库发生故障

![14.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/44a3da883658255212e93bf97afd97e2.png)

哨兵发现问题，开始进行投票，选出新主库

![15.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/a46d4fe35b8a26a61e06064f1aabed05.png)

最终端口为6381的库成为主库，查看此时库的状态

![16.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/8656628abc73d39c9fc0b2bf3e4b7107.png)

![img](https://depth.team/wp-content/uploads/2018/09/Snipaste_2018-09-30_16-53-52.png)

4.如果此时库6379恢复上线，不会发生主库冲突，哨兵会将6379连接至新的主库

![17.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/fba2b883ad0d4e55e5524f44cf89167f.png)

![18.png](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/08/4a0167d623ad716bc20b03e81f374489.png)

5.如果需要让哨兵后台运行，可以使用screen命令

### 主从复制的缺点

由于所有的写操作都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。