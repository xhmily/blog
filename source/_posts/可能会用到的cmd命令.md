---
title: 可能会用到的cmd命令
date: 2019-08-07 10:39:33
categories: CMD
tags: cmd
top:
password:
---

#### 查找对应的端口占用的进程，找到占用端口对应的程序的PID号：

```bash
netstat  -aon|findstr  "8080" 
```

#### 根据PID号找到对应的程序 ，找到对应的程序名：

```bash
tasklist|findstr "6676" 
```

#### 结束进程：

```bash
taskkill /f /t /im java.exe
```

#### BAT脚本通过端口号获取pid并结束进程：

```bash
@echo off
setlocal enabledelayedexpansion
for /f "tokens=1-5" %%a in ('netstat -ano ^| find ":%1"') do (
    if "%%e%" == "" (
        set pid=%%d
    ) else (
        set pid=%%e
    )
    echo !pid!
    taskkill /f /pid !pid!
)
```

#### BAT脚本调用steamcmd安装服务器

```bash
c:\steamcmd +login 账号 密码 +force_install_dir C:\dayz\ +app_update 223350
```

#### BAT调用steamcmd下载mod

```bash
steamcmd +login 账号 密码 +force_install_dir c:\mod +workshop_download_item 游戏id Modid
::下载mod需要登录的账号拥有该游戏才可下载
```

