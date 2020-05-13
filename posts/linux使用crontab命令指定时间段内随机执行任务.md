---
title: 'linux使用crontab命令指定时间段内随机执行任务'
date: 2019-07-07 23:53:49
tags: [Linux,Crontab]
published: true
hideInList: false
feature: 
isTop: false
---

crontab命令常见于[Unix](https://baike.baidu.com/item/Unix)和[类Unix](https://baike.baidu.com/item/%E7%B1%BBUnix)的操作系统之中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于"crontab"文件中，以供之后读取和执行。

cron是一个linux下 的定时执行工具，可以在无需人工干预的情况下运行作业。
`service crond start    //启动服务`
`service crond stop     //关闭服务`
`service crond restart  //重启服务`
`service crond reload   //重新载入配置`
`service crond status   //查看服务状态`

在crontab文件中如何输入需要执行的命令和时间。该文件中每行都包括六个域，其中前五个域是指定命令被执行的时间，最后一个域是要被执行的命令。
每个域之间使用空格或者制表符分隔。格式如下：
`minute hour day-of-month month-of-year day-of-week commands`
合法值 00-59 00-23 01-31 01-12 0-6 (0代表周日)

<span style="color: #ff0000;">除了数字还有几个个特殊的符号就是"*"、"/"和"-"、","，*代表所有的取值范围内的数字，"/"代表每的意思,"/5"表示每5个单位，"-"代表从某个数字到某个数字,","分开几个离散的数字。</span>

crontab -l 在标准输出上显示当前的crontab。
-r 删除当前的crontab文件。
-e 使用VISUAL或者EDITOR环境变量所指的编辑器编辑当前的crontab文件。当结束编辑离开时，编辑后的文件将自动安装。

<span style="color: #ff0000;">需要注意的是同同一用户默认只有一个crontab任务，例如root用户每次新建一个crontab任务都会覆盖之前的任务。</span>

例子：

`vi test.cron ###创建一个cron文件`

并向该文件中写入如下命令：

```shell
0 8 * * * echo "good morning ">>test.txt ###表示每天早晨8点向test.txt中插入一条"good morning"

0 8 1,3,5 * 1-5 echo"good morning" >> test.txt

###表示每年的1月3月5月中每周一到周五的早晨8点向test.txt中插入一条"good morning"
```

使用`crontab test.cron`  即可启动该命令文件，到达指定时间系统将会自动执行文件中的命令。

问题：

综上所述，crontab 命令固然好用，但是执行任务的时间是死的，每天都是同一个时间执行任务，在做某些需要随机时间的特殊任务时，就显得没那么好用了。

所以，如果需要随机时间，就要用的shell脚本了。

首先创建一个shell脚本，test.sh

```shell
#!/bin/bash

echo "good morning" >> test.txt ###向test.txt中插入一条"good morning"

r=$(($RANDOM%10)) ###随机生成一个10以内的随机数
rm -f test.cron                ###删除以前的命令文件
echo $[r]" 8 * * * ./test.sh" >> test.cron #创建并将任务写入cron文件
chmod 777 test.sh  ###给予shell脚本最高执行权限
crontab test.cron    ###启动cron任务文件，用于定时自动执行
```

<span style="color: #ff0000;">当然，该脚本必须在写完后手动执行一次，以后才会按时自动执行</span>

命令:

`[code]chmod 777 test.sh`

`./test.sh[/code]`

<span style="color: #ff0000;">原理：</span>

将随机生成的数字作为时间（在这里是作为分钟）写入cron文件，并通过按时执行shell脚本来将"good morning"插入到test.txt中，

由于该任务的时间即分钟是10以内的随机数，所以每次执行任务的时间是每天早晨8：00-8：09之间的随机时间，

通过这种方式就可以设置随机时间执行任务了。

&nbsp;

如有错误，还请大佬多多包涵，谢谢！