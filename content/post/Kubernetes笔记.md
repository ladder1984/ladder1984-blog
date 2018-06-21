---
title: Kubernetes笔记
date: 2017-09-09 00:33:43
tags:
  - docker
  - kubernetes
---



## 1. 简介
Kubernetes一个用于容器集群的自动化部署、扩容以及运维的开源平台。

### 1.1 基本概念
- master：主节点，负责k8s调度，不应部署容器
- node：master之外的主机节点
- pod：一组容器的集合，在一个pod只部署一个容器时，可基本等价于容器实例
- deployment：一个容器、编排部署集合
- service：一个服务，简单理解deployment通过service暴露在网络中

所以，用deployment管理一组pod，用service暴露deployment。

* Pod: 若干 container 组成的一个功能单元，共享相同的 IP / Namesapce / Volume
<!-- more --> 
### 1.2 基本架构

* Single Master: 
  * kube-apiserver - 响应 REST 请求，更新 etcd 信息
  * kube-scheduler - Pods 调度
  * kube-controller-manager - 所有其它控制功能
* Nodes: 
  * kube-proxy - 网络代理和负载均衡器
  * kubelet - 与 master 交互，管理 Pod

Internet --> kube-proxy --> Pods

注：1.2版之前是由运行在 userspace 的 kube-proxy 分发请求，1.2 版 kube-proxy 监视服务变化，生成相应 iptables rules

### 1.3 部署模型

* Deployment/Replication controller - Pod 池，确保一个 Pod 有一定份数运行，可用作负载均衡和水平扩展
* Service - 定义了一组 Pods 的逻辑集合和访问这个集合的策略，提供一个 Micro Service
* Endpoints - 一个 API，更新 Service 中的 Pods 集合

## 2. k8s技术介绍


### 2.1 网络
使用flannel组件建立容器间网络，也是运维管理维护最麻烦的地方，因此：
- 有三个网络，只需要运维了解，业务开发时不需要关心：
	- Cluster Network：虚拟局域网，pod所处的实际网络，pod获得Cluster IP。Cluster Network内部互通，内部pod可以访问公网、物理局域网
	- 物理局域网：外部主机所处的物流局域网
	- 公网
- 容器继承Node的DNS配置，不继承hosts
- pod访问外部时，通过所在Node的网络
- 只能在Master通过NodePort，把service的端口和物理机连接，把service暴露给公网，这就造成Master的网络压力较大



### 2.3 服务发现
k8s提供两种发现服务的方式，环境变量和DNS。由于数据库类资源通常使用外部资源，所以有用的是DNS自动给每个service自动注册name，由此可大大简化api-gateway的配置管理工作。
#### 2.3.1 api-gateway
由于当前网络模式不支持使用宿主机的hosts，且k8s有访问发现机制，所以，
1. gateway地址使用**api-gateway**，免去自建DNS开销。
2. app地址使用k8s提供的Internal endpoints，即app:port形式，统一使用80端口，则简化为app
3. app统一使用HTTP协议提供服务
例子：
```
    location /xxx/ {
        rewrite ^/xxx/(.*) /$1 break;
        proxy_pass xxx;
    }
```

需要说明的是，api-gateway和app都应该存在于cluster network内。对于cluster network外的api service，Nginx规则按照老形式配置IP:Port形式；对于cluster network外的需要访问api-gateway的app，则访问外部Nginx，外部Nginx通过NodePort反向代理把请求发送给内部Nginx。


### 2.4 负载均衡
请求是首先访问service，然后service根据设置的策略访问pod。简单负载均衡模式和负载均衡器模式可以无缝切换。

#### 2.4.1 简单负载均衡

在service不使用LoadBalancer模式时，，service以**随机**的方式选择pod。

注：service的实现基于kube-proxy，kube-proxy有两种模式。**userspace**会**循环**选择pod，**iptables**模式会**随机**选择pod，**iptables**是默认模式且性能更好。

#### 2.4.2 使用阿里云SLB负载均衡

可以直接使用阿里云负载均衡服务<https://intl.aliyun.com/zh/product/server-load-balancer>。` kubectl expose deployment nginx --port=80 --target-port=80 --type=LoadBalancer` 即可。具


### 2.5 容器调度
#### 2.5.1 扩容和缩容
只需要修改deployment配置文件中的spec.replicas参数，k8s会立刻执行调度。

#### 2.5.2 自动调度
- k8s会根据Node负载情况分配pod
- 当有Node不可用时，k8s会自动把pod调度到可用Node
- 当有docker 容器被关闭时，k8s会根据deploy根据spec.replicas，保证有正确数量的容器在运行

#### 2.5.3 pod健康检查

k8s提供探针对容器进行检测，可以后续开发

#### 2.5.4 自动扩容

K8s提供**HorizontalPodAutoscaler**实现自动扩容，即根据性能指标自动增加pod的数量，但目前性能指标只支持CPU使用率。



## 3. k8s的使用

### 3.1 更新容器

可以使用Jenkins调度api或者kubectl

#### 3.1.1 滚动升级

k8s支持滚动升级，升级过程中，pod逐个替换，直到替换完成，在此过程中旧Pod依然会被使用。可设置minReadySeconds表示启动容器后等待程序启动的时间。

只有更新PodTemplateSpec时才会触发滚动升级，如果镜像使用latest tag，则需要使用`kubectl patch deployment <name> -p "{\"spec\":{\"template\":{\"metadata\":{\"labels\":{\"date\"‌​:\"$(date +%s)\"}}}}}"`。


#### 3.1.2 配置更新

如果需要修改环境变量、pod数量等，可以更新配置文件然后使用`kubectl apply -f <filename>`。

### 3.2 配置文件管理

#### 3.2.1 deployment和service配置文件管理

一个应用接入k8s只需要编写一个配置文件，包含deployment和service两项配置，如：

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: simples
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: simples
    spec:
      containers:
      - name: master
        image: imgage.xxx.com/test/simples:latest  # or just image: redis
        env:
        - name: ENV_VAR
          value: "10086"
        ports:
        - containerPort: 9999
      imagePullSecrets:
      - name: keyyy

      
apiVersion: v1
kind: Service
metadata:
  name: aaa
  labels:
    app: aaa
spec:
  ports:
    # the port that this service should serve on
  - port: 80
  selector:
    app: aaa      
```

每个应用自定义的内容只有环境变量和镜像地址。所有配置使用配置文件统一管理，配置文件使用git管理。

#### 3.2.2 环境变量管理
通用变量，如数据库配置、mns配置放入configMap，app引用configMap的值。各app自己的变量写入自己的配置文件。

### 3.3 dashboard

官方web UI项目：https://github.com/kubernetes/dashboard

dashboard可以查看相关信息，以及容器日志，不建议在dashboard编辑配置，应该使用配置文件。

### 3.4 容器管理

可以使用`kubectl`命令类似`docker`命令地关闭、重启、进入容器等。

#### 3.3.1 账户登录

dashboard 不提供用户登录系统。

使用Nginx的Basic HTTP authentication功能为dashboard提供登录保护，参见<http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html>和<http://www.ttlsa.com/nginx/nginx-basic-http-authentication/>，可以为不同用户设置不同密码。

#### 3.3.2 权限管理

暂无好方案实现账户权限管理

## 4. k8s接入备忘录

### api-gateway改造
1. api service有uwsgi协议改为http协议
2. api-gateway地址可直接使用service的地址
3. api service统一使用80端口

### 环境变量改造
让和环境有关的变量都使用环境变量管理，包括：

- MySQL数据库
	- host
	- 密码
- redis地址
- Mongo地址
- MNS配置


## 其他

### 静态资源管理
静态资源可以直接用Nginx挂在物理机上。

## 总结

因为解决了宿主机端口冲突问题，方便的环境变量管理，可以做到一键创建环境，极大的减少了运维管理成本。

注：kubernetes更新十分迅速，部分信息可能失效，本文根据kubernetes 1.7探索所得。

