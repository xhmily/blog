---
title: Docker学习笔记（二）Docker(V18.03)安装配置
categories: Docker
date: 2019-07-06 18:00:29
tags: Docker
---

### OS要求

要安装Docker CE，您需要CentOS 7及以上版本。

### 卸载旧版本

较旧版本的Docker被称为`docker`或`docker-engine`。如果已安装这些，请卸载它们以及相关的依赖项。代码如下

```shell
$ sudo yum remove docker \
          docker-client \
          docker-client-latest \
          docker-common \
          docker-latest \
          docker-latest-logrotate \
          docker-logrotate \
          docker-selinux \
          docker-engine-selinux \
          docker-engine`
注意 "\"为shell脚本的连接符，同java的"+"
```

<span style="color: #ff6600;">如果系统未安装过docker，则提示如下</span>：

![1.jpg](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/a55ec63c0fc0581d66a717ca235d3cbf.jpg)

## 安装Docker CE（社区版，免费）

### 使用存储库安装

在新主机上首次安装Docker CE之前，需要设置Docker存储库。之后，您可以从存储库安装和更新Docker。

#### 设置存储库

#### 1.安装所需的包。`yum-utils`提供了`yum-config-manager` ，并且`devicemapper`存储驱动程序依赖`device-mapper-persistent-data`和`lvm2`。

```shell
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

<span style="color: #ff6600;">执行结果</span>：

![2.jpg](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/19cb985de4bdb509cfbfe52b1159ff3c.jpg)

#### 2.使用以下命令设置**稳定**存储库。即使你还想从**edge**或**test**存储库安装构建，你仍然需要**稳定的**存储库。

```shell
$ sudo yum-config-manager --add -repo https://download.docker.com/linux/centos/docker-ce.repo
```

<span style="color: #ff6600;">执行结果</span>：

![3.jpg](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/6f0f4402511cb1b51db93496510c2783.jpg)

#### 3.（可选）启用**edge**和**test**存储库。这些存储库包含在`docker.repo`上面的文件中，但默认情况下处于禁用状态。您可以将它们与稳定存储库一起启用。

```shell
$ sudo yum-config-manager --enable docker-ce-edge
$ sudo yum-config-manager --enable docker-ce-test
```

#### 您可以通过运行带有标志的命令来禁用**edge**或**test**存储库 。要重新启用它，请使用该标志。以下命令禁用**edge**存储库。`yum-config-manager``--disable``--enable`

```shell
$ sudo yum-config-manager --disable docker-ce-edge
```

#### <span style="color: #ff6600;">**注意**</span>：从Docker 17.06开始，稳定版本也会被推送到**边缘**并**测试**存储库。 安装DOCKER CE

1. 安装_最新版本_的Docker CE，或转到下一步安装特定版本：

   ```shell
   $ sudo yum install docker-ce
   ```

##### <span style="color: #ff6600;">如果提示接受GPG密钥，请验证指纹是否匹配</span>

##### <span style="color: #ff6600;">`060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`，</span>

##### <span style="color: #ff6600;">如果匹配 ，则接受它。</span>

<span style="color: #ff6600;">此处确认无误，选择y确定</span>：

![4.jpg](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/e9e728e4a309c8d9e6e67638a51636ac.jpg)     

<span style="color: #ff6600;">指纹比对正确，选择y继续</span>：

![5.jpg](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/e3dca310a88f200ed072a9e50f764edc.jpg)

<span style="color: #ff6600;">最终结果</span>：

![6.jpg](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/24cd06e2ed9927862d4231b155107d99.jpg)

<span style="color: #333333;"> 2.您还可以使用</span>`$ sudo yum install docker-ce-<版本号>`来安装指定版本的docker，例如安装17.06版本

```shell
$ sudo yum install docker-ce-<17.06>
```

3.启动Docker。

```shell
$ sudo systemctl start docker
```

<span style="color: #ff6600;">注意：<span style="color: #333333;">docker启动后不会有任何输出</span></span>

4.`docker`通过运行`hello-world` 映像验证是否已正确安装。

```shell
$ sudo docker run hello-world
```

<span style="color: #ff6600;">执行命令后如果你看到如下图所示，那么恭喜，docker安装成功！</span>

![7.jpg](https://raw.githubusercontent.com/xhmily/imgbed/master/images/2019/07/07/9e818831fbf2e690c7d5728a5d5c8431.jpg)

