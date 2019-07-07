---
title: shell命令+crontab+php实现每天特定时间段内随机时间执行任务
date: 2019-07-08 00:02:47
categories: Linux
tags:
- Liunx
- Crontab
top:
password:
---

之前写过使用shell+crontab实现每天随机执行任务（[linux使用crontab命令指定时间段内随机执行任务](lucent.blog/passages/linux使用crontab命令指定时间段内随机执行任务/)，但是后来想想容易出bug，

比如：第一天执行是生成的随机时间要留给下次使用，如果第一次生成时间为8：01，那么第二天就会8：01执行任务，第二天8：01执行任务时生成的随机时间是8：05，那么8：05也会执行一次任务，就会导致同一天执行两次甚至多次任务。

那么下面加入php重写,实现每天早晨8：00-8：09之间随机时间访问www.baidu.com：

**shell代码sign.sh：**

```shell
#!/bin/bash
r=$(($RANDOM%10)) ##生成10以内的随机数字
rm -f /www/wwwroot/time.txt ##删除以前的time.txt
echo “08:0″$[r] >> /www/wwwroot/time.txt ##创建并将随机数字作为时间的分钟插入time.txt
chmod 777 sign.sh ##设置shell脚本的权限
```

**crontab命令文件sign.cron:**

```shell
59 07 * * * ./sign.sh   ##定时7：59执行shell脚本生成随机时间
0-9 08 * * * curl http://服务器ip/sign/Sys.php    ##8：00-8：09没分钟执行一次http请求
```

**php文件Sys.php：**

```php
<?php
header(“Content-type: text/html; charset=utf-8”);

//打开time.txt文件，方法为只读

$myfile = fopen(“time.txt”, “r”) or die(“Unable to open file!”);

//将打开文件中的内容（这里即是shell脚本生成的随机时间）赋值给time1

$time1=fread($myfile,filesize(“time.txt”));

fclose($myfile); //关闭time.txt

$randomtime=strtotime(“$time1”);//将随机时间转换成时间戳格式

$time=date(‘H:i’,time());//获取当前时间
$now=strtotime(“$time”);//将当前时间转换成时间戳格式

//比较当前时间是否等于随机时间，若是，则执行下面代码

if($now == $randomtime){

//执行访问www.baidu.com的任务，也可以做其他任务，需要自己写代码

header(“Location: http://www.baidu.com”);

}

else{
//代码
//这里是如果当前时间不等于随机时间的时候要执行的代码，不写即什么都不做
}
```

注意：如果不将时间通过strtotime()转换成时间戳格式，将无法比较两个时间！

**原理：**

通过设定crontab定时任务，在7：59分时生成随机时间并存储到time.txt中，第二个任务在8：00-8：09之间每分钟执行一次，

**curl http://服务器ip/sign/Sys.php 命令即是向服务器发送请求，**服务器Sys.php每分钟收到一次请求，并每次判断当前时间是否与time.txt中的随机时间相等，若相等，执行预先设置好的任务，若不等，什么也不做，等待下一次请求。



通过以上方式即可实现每天在特定的时间段中的随机时间执行任务。

若你有更好的方式，欢迎留言，谢谢！