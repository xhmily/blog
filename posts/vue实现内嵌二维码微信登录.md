---
title: 'vue实现内嵌二维码微信登录'
date: 2019-09-23 17:13:08
tags: [vue]
published: true
hideInList: false
feature: 
isTop: false
---

#### 前提

使用微信登录必须在[微信开放平台](https://open.weixin.qq.com/cgi-bin/index?t=home/index&lang=zh_CN)注册并完成开发者认证，创建应用完成后即可使用微信登录接口

登录后按如下进行创建：

![](https://raw.githubusercontent.com/xhmily/imgbed/master/images/20190923173336.png)

应用审核通过后即可查看appid

#### 在vue页面中使用微信内嵌二维码登录

首先在index.html中引入：

```javascript
<script type='text/javascript' src="https://res.wx.qq.com/connect/zh_CN/htmledition/js/wxLogin.js"></script>
```

代码：

```html
<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">基石FBA管理系统</div>
            <el-form label-width="0px" class="ms-content">
                <div id="qrcode"></div>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        wxlogin(){
        //用于生成二维码的方法
            var obj=new WxLogin({
                self_redirect:false,
                id:'qrcode',//显示二维码的容器id
                appid:'wx551b22825a4bxxxx',//在开放平台申请到的appid
                scope:'snsapi_login',
                redirect_uri:encodeURIComponent('https://www.rock-fba.com/#/dashboard'),//扫码后跳转的链接，网址必须是在开放平台申请时填写的那个
                state:'123456',
                style:'black',
                href:'',
            })
        },

    },
    mounted() {
        this.wxlogin()//当页面加载成功后执行，生成微信二维码
    },

};
</script>
```

微信扫码并确认后，网页会重定向到上面设置好的链接，但是我们不让它直接重定向到相关页面:

```javascript
//通过路由守卫对请求进行处理
router.beforeEach((to, from, next) => {
	//如果地址中包含code(微信扫码后返回用于换取用户信息的临时票据)，则拦截并执行登录逻辑
	   if(window.location.href.indexOf('code')>=0){
        //如果url中包含code,则保存到store中
        let code = window.location.href.split("?")[1];
        code = code.substring(5,code.indexOf('&'));
        if (code!=null&&""!=code&&code!=undefined){
            console.log("开始执行微信登录")
            //向后台登录接口发送数据
            request.post('auth/login', {
                'code':code
            }).then((r)=>{
            //后端成功返回token则视为登录成功
                if (r.token){
                    Message.success('登录成功!');
                    localStorage.setItem('token', r.token);
                    localStorage.setItem('user', JSON.stringify(r.admin));
                    console.log("登录成功")
               		//进入网站欢迎页
                    next('/');
                }else {
                  Message.error('登录失败!');
                }
            })
        }
    }
    
    //更多逻辑...
}
```

后端用于实现登录的相关代码(Java)示例：

```java
/**
     * 用户登录
     *
     * @param Admin
     * @return
     */
    public LoginUser login(LoginUser admin) {
        String url="https://api.weixin.qq.com/sns/oauth2/access_token?appid=开放平台查看&secret=开放平台查看&code="+admin.getCode()+"&grant_type=authorization_code";
        Admin admin1= null;
        LoginUser loginUser=new LoginUser();
        JSONObject json=null;
        try {
            String msg= HttpUtil.sendGet(url);//用临时票据code向微信服务器换取用户信息
            json= (JSONObject) JSONObject.parse(msg);
            Map<String, Object> map = new HashMap<>();
            map.put("unionid", json.getString("unionid"));
            admin1=adminDao.selectByMap(map).get(0);//查询该用户的unionID是否在数据库中
        } catch (Exception e) {
        //发生异常则说明不在数据库中，返回相关数据给前端，前端负责提示该用户进行注册
            e.printStackTrace();
            admin1=new Admin();
            admin1.setRoleid(2);
            admin1.setOpenid(json.getString("openid"));
            admin1.setUnionid(json.getString("unionid"));
            loginUser.setAdmin(admin1);
            return loginUser;
            //throw new MyException("登录失败!");
        }
        if (admin1.getStatus()==0){
            throw new MyException("您已被限制登录!");
        }
        //以上检查全部通过则说明用户可以登录，用相关工具类生成token返回给前端
        loginUser.setAdmin(admin1);
        //根据管理员代码和openid加密生成token
        String token = JwtUtil.sign(admin1.getAdminCode(), admin1.getUnionid(),"web");
        loginUser.setToken(token);
        return loginUser;
    }
```

以上即为Vue项目中使用微信登录的示例.