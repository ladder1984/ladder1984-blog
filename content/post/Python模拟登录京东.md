---
title: Python模拟登录京东
date: 2015-09-13 23:16:01
tags: 
 - Python
---




## 模拟登录的方法
真实用户在登录网站时，是在登录页面填写自己的账号、密码信息，并点击登录按钮。从实现的角度上讲，实际上是执行了一个Http Post请求，向网站服务器提交数据。模拟登录就是通过程序对这种行为进行模拟，所以一是要准备账号、密码等信息，二是要像用户一样进行post。

但是，通常网站都会限制程序的模拟登录，通常的方法也就是在上面提到的两点上进行检测。一是增加要提交的校验信息，二是检测post的行为。

本文介绍使用Python进行模拟登录京东网，使用PyQuery来捕捉京东增加的校验信息，用requests执行post请求。


## 数据准备

首先找到京东的登录页面https://passport.jd.com/new/login.aspx ,随便提交一次，发现post的地址是https://passport.jd.com/uc/loginService ，以及post数据为：
```
uuid:53c85802-XXX-ebdd952dda93
machineNet:
machineCpu:
machineDisk:
SGIARTgLfH:dJGep
loginname:XXXX
nloginpwd:XXX
loginpwd:XXX
chkRememberMe:on
authcode:
```

其中,`loginname`是用户名，`nloginpwd`和`loginpwd`都是密码，`uuid`、loginname上面的一组数据(本例中为SGIARTgLfH:dJGep)三者都是用于验证登录的随机字符串。`authcode`为验证码,`chkRememberMe`为是否自动登录。其他的项留空。

所以，重要的是获取uuid和那组随机数据。这些数据都在登录页面https://passport.jd.com/new/login.aspx 中。检查该页面源码，这些数据都在<form id="formlogin">表格中，使用pyquery进行选择，代码为：
```
uuid = d("#uuid").attr("value")
left = d('#formlogin input[type="hidden"]:eq(4)').attr("name")
right = d('#formlogin input[type="hidden"]:eq(4)').attr("value")
```

另外一个重要数据是验证码，在多次尝试登录或者其他京东不喜欢的情况下，会要求填写验证码。同样使用pyquery查找验证码，代码为：
```
auth_img_url = d('#JD_Verification1').attr("src2")
```
然后下载验证码，在不是很频繁登录的情况下，手动填写就够了。

需要**注意**的是，京东的网页随时可能修改，post的数据和数据的来源都要根据实际情况确定。


## 进行登录

登录操作使用requests，需要注意的是，要使用requests中的Session，首先创建`s = requests.Session()`，然后使用`s.post`和`s.get`进行操作。`s.post(login_post_url, data=login_data)`即可。返回`({"success":"http://www.jd.com"})`表示登录成功，可以使用s会话进行后续操作。


## 完整代码

完整代码如下：

{% gist 2a1231184de59a725f64 login_jingdong.py %}



或访问gist查看<https://gist.github.com/ladder1984/2a1231184de59a725f64>


