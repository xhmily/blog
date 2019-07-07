---
title: Redis学习笔记（三）Redis字符串(String)
date: 2019-07-08 00:27:22
categories: Redis
tags: Redis
top:
password:
---

String（字符串）
string是redis最基本的类型，可以理解成与Memcached一模一样的类型，一个key对应一个value。
string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象 。
string类型是Redis最基本的数据类型，一个redis中字符串value最多可以是512M。

### 常用命令

```
set key value  #设置key的值为value

get key        #获取key的值

incr key       #将key中存储的数据(必须为数字)加1

incrby key 值  #将key中存储的数据(必须为数字)加上指定的值

decr key       #将key中存储的数据(必须为数字)减1

decrby key 值  #将key中存储的数据(必须为数字)减去指定的值

getrange key start end  #返回key中字符串值的子字符(获取指定区间范围内的值,类似between...and)

setrange key offset value #设置指定区间范围内的值


SETEX key seconds value #设置带过期时间的key

SETNX key value  #只有在 key 不存在时设置 key 的值

mset key1 value1 key2 value2 ...    #同时设置一个或多个 key-value 对
mget value1 value2 ... #获取所有(一个或多个)给定 key 的值


msetnx key1 value1 key2 value2 #同时设置一个或多个key-value对，当且仅当所有给定 key 都不存在
```