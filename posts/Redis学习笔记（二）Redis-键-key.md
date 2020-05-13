---
title: 'Redis学习笔记（二）Redis 键(key)'
date: 2019-07-08 00:23:58
tags: [Redis]
published: true
hideInList: false
feature: 
isTop: false
---

### 常用命令

```
select db #选择数据库，redis默认16个库（0-15）
keys *  #列出当前库中所有key
exists key的名字 #判断某个key是否存在
expire key seconds(秒)  #为给定的key设置过期时间
ttl key  #查看还有多少秒过期，-1表示永不过期，-2表示已过期
type key #查看key是什么类型
move key db #移动key到指定数据库，当前库就没有了，被移除了
RENAME key newkey #修改key的名字为newkey
RENAMENX key newkey #当newkey不存在时修改key为newkey
DEl key  #当key存在时删除key
```

