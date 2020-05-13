---
title: 'Linux安装screen命令'
date: 2019-07-07 23:48:56
tags: [Linux]
published: true
hideInList: false
feature: 
isTop: false
---

screen对于不支持SSH的虚拟主机没有作用，但是对于vps来说那可是用处大大的。
不知道朋友们有没有在配置vps环境的时候出现突然中断或者要离开但是还没有配置完成的情况呢？

我遇到很多回，往往已经快配置完成的时候出现短线等情况，那就要从头再安装配置，很是麻烦，有时要连续重新安装好几次系统才可以完成。
现在有了screen命令就不用为此烦恼了。

**screen命令是什么？**
Screen是一个可以在多个进程之间多路复用一个物理终端的全屏窗口管理器。Screen中有会话的概念，用户可以在一个screen会话中创建多个screen窗口，在每一个screen窗口中就像操作一个真实的telnet/SSH连接窗口那样。
**如何安装screen命令呢？**
Centos执行:
yum install screen
Debian/Ubuntu执行:
apt-get install screen

**怎么使用screen命令？**
1、创建一个screen会话：
`screen -S abc`
abc为创建会话的名称

2、创建好以后就可以正常安装和配置vps环境，如怕中途短线或者要离开，马上就可以使用
快捷键**Ctrl+a d(即按住Ctrl，依次再按a,d)**来保存这个会话窗口
当然程序还在自动进行不会关闭。

3、需要恢复会话的时候就需要执行
`screen -r abc`

如果在恢复会话的时候忘记了或者没有设定会话名称我们就要执行：
`screen -ls`

他会列出你所有的会话列表，然后使用：
`screen -r 会话名称`
来恢复会话窗口。

4、关闭screen的会话
`exit`
会提示：[screen is terminating]，表示已经成功退出screen会话。

5、screen命令常用的一些快捷键：

```shell
Ctrl+a c ：在当前screen会话中创建窗口
Ctrl+a w ：窗口列表
Ctrl+a n ：下一个窗口
Ctrl+a p ：上一个窗口
Ctrl+a 0-9 ：在第0个窗口和第9个窗口之间切换
```

在此记录以便查阅.