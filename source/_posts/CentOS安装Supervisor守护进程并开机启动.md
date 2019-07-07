---
title: CentOS安装Supervisor守护进程并开机启动
date: 2019-07-08 00:04:36
categories: Linux
tags:
- Centos
- Linux
top:
password:
---

**安装：**

centos6.9安装方法

`easy_install supervisor`

centos7安装方法

`yum install supervisor`

在/etc/目录下建立配置文件

`echo_supervisord_conf > /etc/supervisord.conf`

修改配置文件

`vi /etc/supervisord.conf`

在末尾加入配置信息

一般配置信息都是[program:xxx]开头的

**配置：**

<span style="color: #ff0000;">command为真实安装路径!</span>

[][program:frp]

```shell
user=root
command=/root/frp/frpsa/frps -c /root/frp/frpsa/frps.ini
startsecs=1
startretries=100
autorstart=true
autorestart=true
stderr_logfile=/tmp/err-frps.log
stderr_logfile_maxbytes=50MB
stderr_logfile_backups=10
stdout_logfile=/tmp/out-frps.log
stdout_logfile_maxbytes=50MB
stdout_logfile_backups=10
```

**使用：**

直接启动supervisor

`supervisord`

或指定配置文件启动

`supervisord -c /etc/supervisord.conf`

#配置开机自启
`systemctl enable supervisord`

#验证一下是否为开机启动

`systemctl is-enabled supervisord`