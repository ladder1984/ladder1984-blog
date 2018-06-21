---
title: DJango开发笔记
tags:
  - Django
  - Python
id: 1163
categories:
  - 编程笔记
date: 2014-09-12 17:50:40
---

# ![Django Unchained](http://www.itoldme.net/wordpress/wp-content/uploads/2014/09/Django-Unchained.jpg)

(图文无关，图片来自网络)

# 

# 环境

IDE:JetBrains PyCharm 3.4.1

Python: 2.7.8

Django:1.7

MySQL:5.6.20

&nbsp;

# 配置

## 数据库：

<pre class="lang:python decode:true ">#settings.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'datebaseName',
        'USER': 'username',
        'PASSWORD': 'pass',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}</pre>
相关指令
python manage.py syncdb
python manage.py makemigrations
python manage.py migrate

&nbsp;

# 参考文档

**1.7文档**：https://docs.djangoproject.com/en/1.7/

**The Django Book（中文）**：http://djangobook.py3k.cn/2.0/

models数据类型：https://docs.djangoproject.com/en/dev/ref/models/fields/

静态文件：https://docs.djangoproject.com/en/1.7/howto/static-files/

数据库迁移：https://docs.djangoproject.com/en/1.7/topics/migrations/

# 总结

使用Django真的很方便!学好英语真的很重要!

PS：本来想多写点东西，但发现东西都在文档里。。。等整理好，再更新文章。

Github：https://github.com/ladder1984/DJangoHotel_Python