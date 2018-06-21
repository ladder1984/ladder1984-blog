---
title: peewee笔记(2)——构建自已的peewee文档
tags: 
- peewee
- MySQL
- Python
date: 2016-03-28 20:17:01

---

## 背景

开源软件的一大好处就是我们可以根据自己的需要进行定制或优化，但这带来的问题是，修改可能无法合并的回社区。随着时间流逝，自己的版本可能和官方版本差别越来越大，也无法合并官方的代码更新。如果后续开发者一直阅读官方文档就会有问题，所有需要构建自己的文档。

打开peewee官方文档<http://docs.peewee-orm.com/en/latest/>，我们可以看到只提供latest、stable版本文档，可能和我们定制的已经千差万别。好在peewee文档使用Sphinx构建，易于修改部署。
<!-- more --> 
## 部署文档
好在github上我们可以找到历史版本的文档，以2.6.4为例。
1. 安装Sphinx：`pip install sphinx`
2. 从官方git（<https://github.com/coleifer/peewee>）下载peewee源码并切换到tag 2.6.4。
3. 进入项目内doc/目录，执行`make.bat html`(Windows)或`make html`(Linux)生成静态网站目录_build/index/ 。
4. 把2的目录找个服务器放。


