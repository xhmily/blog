---
title: 微信小程序获取用户UnionId
date: 2019-09-19 10:54:56
categories: 微信小程序
tags: 小程序
top:
password:
---

###### 		如果开发者拥有多个移动应用、网站应用、和公众帐号（包括小程序），可通过 UnionID 来区分用户的唯一性，因为只要是同一个微信开放平台帐号下的移动应用、网站应用和公众帐号（包括小程序），用户的 UnionID 是唯一的。换句话说，同一用户，对同一个微信开放平台下的不同应用，unionid是相同的。

能够获取UnionID的前提是**小程序已经绑定了开发者账号**

##### 微信开发平台绑定流程：

登录[微信开放平台](https://open.weixin.qq.com/)-->管理中心-->小程序-->绑定小程序

![](https://raw.githubusercontent.com/xhmily/imgbed/master/images/%7BK%7D%25%5DQII01DP%7DJZAWJKJ%7E(V.png)

#### 1.需要用到的工具

[cryptojs](https://github.com/xhmily/imgbed/raw/master/images/cryptojs-master.zip)

将文件复制到小程序项目中

![](https://raw.githubusercontent.com/xhmily/imgbed/master/images/20190919173913.png)

#### 2.代码

```javascript
const util = require('../utils/utils.js')
const WXBizDataCrypt = require('../utils/RdWXBizDataCrypt.js')
const getData = util.getData

login() {
    const that = this
    //在发起请求获取unionID之前必须先调用wx.login接口，因为需要用到session_key
    wx.login({
      success(res) {
        if (res.code) {
          //发起网络请求,调用后台的登录接口
          getData("auth/login",{
            jsCode:res.code
          },"POST").then(res=>{
  	 //后台用临时票据向微信服务器换取了用户openid和session_key，把session_key传回前端用来解密
            var pc = new WXBizDataCrypt('wxa1acc2c4af07a57b', res.data.sessionkey)
            wx.getUserInfo({
              success: function (res) {
                //拿到getUserInfo（）取得的res.encryptedData, res.iv，调用decryptData（）解密
                var data = pc.decryptData(res.encryptedData, res.iv)
                // data中就包含了openid和unionID，
                wx.setStorageSync('unionid', data.unionId)
                wx.setStorageSync('userInfo', data)
              }
            })
            if (res.msg =='success'){
              app.globalData.openid = res.res
              app.globalData.hasLogin = true
              wx.redirectTo({
                url: '/pages/start/start',
              })
            }else if(res.msg=='noAuth'){
              app.globalData.openid = res.res
              wx.redirectTo({
                url: '/pages/reg/reg',
              })
            }else{
              wx.showToast({
                title: '登录失败！',
                icon: 'none'
              })
            }
          })
```

这样即可获得用户的unionID,当然，前提是用户在授权时点了允许