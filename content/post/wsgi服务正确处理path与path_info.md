---
title: "Wsgi服务正确处理path与path_info"
date: 2019-07-25T15:51:15+08:00
tags:
  - Python
---


## 问题

需要把原app1.yy.com,app2.yy.com.. 的域名合并成xx.yy.com/app1 , xx.yy.com/app2的部署方式，在测试合并域名部署时，发现前端发来的请求，个别页面报错，观察到的情况是

1. 前端请求http://xxx.yy.com/app1/api/get_order，希望获得app1服务的/api/get_order的接口
2. 而后端对此返回HTTP 301,重定向到了http://xxx.yy.com/api/get_order/
3. 前端服务访问http://xxx.yy.com/api/get_order/， Nginx无法找到/api/get_order/，于是返回404




## 原因

此时Nginx配置如下：

```/Users/zhutianqi/ladder1984-blog/content/post/wsgi服务正确处理path与path_info.md
server {
    listen 80;
    server_name xxx.yy.com;
    location /app1/{
           proxy_pass http://192.168.6.183:19910/;
        }
}
```

1. Nginx接受请求http://xxx.yy.com/app1/api/get_order 后通过proxy_pass转发给http://192.168.6.183:19910/
2. wsgi服务，即Django，接受到http://xxx.yy.com/api/get_order请求
3. 按照Django url匹配原则，即项目代码定义的url(r'api/get_order/$',  ...)  
    3.1 会首先认为api/get_order匹配失败，  
    3.2 根据Django默认设置APPEND_SLASH=True，会发起重定向补全“/”，根据request.host和request.path_info，得到重定向url：http://xxx.yy.com/api/get_order/
4. NGINX无法匹配 /api/get_order/，返回404


根本原因是，Django服务无法感知到path: /app1的存在，所以重定向生成的URL有误。对此，解决方案有：

1. 在Django设置APPEND_SLASH=False，关闭自动补全。但所有客户端必须正确请求携带/的URL，但排查老代码，以及规避以后代码请求出错风险较大
2. 通过Django中间件的方式在处理url匹配前自动加上/，但此方案只能处理APPEND_SLASH场景，不确定Django是否还有其他类似重定向，也使得后端服务丧失发起重定向能力，风险较大
3. 彻底解决path问题，使得后端服务正确感知到完整path是/app1/api/get_order，并能正确处理


## 方案

经查询，对于多个服务部署到同一域名的多个path下，Nginx、wsgi、Django是有支持的。需要使用SCRIPT_NAME特性。


根据wsgi定义，详见 https://www.python.org/dev/peps/pep-0333/#id19：

    SCRIPT_NAME：URL请求中路径开始的部分，对应应用程序对象
    PATH_INFO：URL请求路径剩余部分，指定请求目标在应用程序内部的虚拟位置

即完整的path是SCRIPT_NAME + PATH_INFO，以上述场景，SCRIPT_NAME是/app1，PATH_INFO是/api/get_order。


而Django能够正确处理`SCRIPT_NAME`和`PATH_INFO`，url匹配时是使用path_info，而`append_slash`这样需要完整path的场景，`get_full_path`函数能够正确识别SCRIPT_NAME，所以只要正确把SCRIPT_NAME + PATH_INFO传给Django就行了。`http://xxx.yy.com/app1/api/get_order` 请求，Django能解析到`request.path: /app1/api/get_order`，`request.path_info: /api/get_order`。


具体方案如下：


Nginx配置文件:

```
server {
    listen 80;
    server_name xx.yy.com;
    location /app1/{
 
           uwsgi_pass 192.168.6.81:9999;
           uwsgi_param SCRIPT_NAME /app1;
           include uwsgi_params;
        }
 
 
}
```

uWSGI的init配置文件：

```
...
route-run = fixpathinfo:
...
```


**注意**uWSGI需要2.0.11及以上版本，详见：https://uwsgi-docs-zh.readthedocs.io/zh_CN/latest/Changelog-2.0.11.html#fixpathinfo



参考：

https://note.qidong.name/2017/11/uwsgi-script-name/

https://uwsgi-docs-zh.readthedocs.io/zh_CN/latest/Changelog-2.0.11.html

https://www.python.org/dev/peps/pep-0333/#id19

https://uwsgi-docs-zh.readthedocs.io/zh_CN/latest/Vars.html

https://www.jianshu.com/p/9c95a28c6895