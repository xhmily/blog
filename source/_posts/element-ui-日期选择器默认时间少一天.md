---
title: 解决element ui 日期选择器默认时间少一天的问题
date: 2019-09-05 18:00:44
categories: vue
tags: element-ui
top:
password:
---

今天在使用element-ui 的日期选择器的时候出现一个奇怪的问题，选中的日期在获取值之后会比选中的日期少一天：

```html
<el-date-picker
        class="mr10"
        v-model="selectedDate"
        type="daterange"
        unlink-panels
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期">
</el-date-picker>
```

结果：

![](https://raw.githubusercontent.com/xhmily/imgbed/master/images/20190905180454.png)

在网上搜索之后发现很多人遇到这个，最终找到一个最简单好用的解决方法：

就是加上这一段代码 **value-format="yyyy-MM-dd"**如下

```html
<el-date-picker
        class="mr10"
        v-model="selectedDate"
        type="daterange"
        unlink-panels
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        value-format="yyyy-MM-dd">
</el-date-picker>
```

结果如下图：

![](https://raw.githubusercontent.com/xhmily/imgbed/master/images/20190905180920.png)