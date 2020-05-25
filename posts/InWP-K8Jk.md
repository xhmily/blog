---
title: '实现个人免签支付宝收款(官方回调通知)-前期准备'
date: 2020-05-25 10:32:13
tags: [支付宝]
published: true
hideInList: false
feature: 
isTop: false
---

**在开发之前需要先开通支付宝当面付，个人可以直接在支付宝APP申请**
<!-- more -->

### 注册商家
- ##### 打开支付宝APP，搜索`蚂蚁金服商家平台`，关注并进入

![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525105910.PNG)

- ##### 点击右上角开通支付

![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525105911.PNG)

- ##### 点击立即签约

![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525105909.PNG)

- ##### 填写信息提交即可，门头照可以百度图片或者美团

![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525105908.PNG)

 ### 开发准备
- ##### 获取支付宝公钥和私钥

签约成功之后登陆蚂蚁金服开放平台`https://open.alipay.com/platform/manageHome.htm`，点击右上角秘钥管理
![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525114403.png)

- ##### 根据提示配置公钥和私钥
私钥根据提示下载支付宝RAS秘钥生成器即可`https://docs.open.alipay.com/291/105971 `,
生成的私钥保存下来，代码中需要用到
![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525115351.png)

- ##### 保存APPID和公钥
![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525120146.png)
**注意公钥是保存支付宝公钥**
![](https://test-1251540275.cos.ap-shanghai.myqcloud.com/img/20200525120254.png)

到这里开发前需要准备的资料都已经齐全了。

