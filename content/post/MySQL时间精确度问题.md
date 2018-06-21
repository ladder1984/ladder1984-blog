---
title: MySQL时间精确度问题
date: 2015-09-20 00:17:59
tags: 
- MySQL
- Django
---

## 问题背景
在项目最近的自动化测试中，会出现一个随机出现的报错，表现为根据生成时间排序的item有时候会和预期不符。经确认，该问题是由MySQL中默认情况下时间只存储到秒引起的，所以，在自动化测试服务器快速生成item时，可能出现先后创建的项目记录为同一个生成时间，在相关页面以生成时间排序时顺序可能和预期不符。

## 问题探究
在MySQL默认情况下， 时间只会精确到秒。在MySQL 5.6.4之前，时间只能精确保存到秒，在MySQL 5.6.4之后以分数秒（Fractional Seconds）的形式保存更精确的时间，格式形如'2010-12-10 14:12:09.019473'。

### fractional seconds的使用
截止到MySQL 5.7，时间的最大精确度是微妙（microsecond）。使用fractional seconds需要使用语法`type_name(fsp)`,type_name可为TIME, DATETIME, 和TIMESTAMP类型，fsp表示精度，取值范围是0-6，6时即表示微妙。例如`CREATE TABLE t1 (t TIME(3), dt DATETIME(6));`。以下为MySQL官方文档中的示例：

	mysql> CREATE TABLE fractest( c1 TIME(2), c2 DATETIME(2), c3 TIMESTAMP(2) );
	Query OK, 0 rows affected (0.33 sec)
	
	mysql> INSERT INTO fractest VALUES 
	     > ('17:51:04.777', '2014-09-08 17:51:04.777', '2014-09-08 17:51:04.777');
	Query OK, 1 row affected (0.03 sec)
	
	mysql> SELECT * FROM fractest;
	+-------------+------------------------+------------------------+
	| c1          | c2                     | c3                     |
	+-------------+------------------------+------------------------+
	| 17:51:04.78 | 2014-09-08 17:51:04.78 | 2014-09-08 17:51:04.78 |
	+-------------+------------------------+------------------------+
	1 row in set (0.00 sec)

更多信息，参考MySQL官方文档：[Fractional Seconds in Time Values](http://dev.mysql.com/doc/refman/5.6/en/fractional-seconds.html)


至于fractional second为什么直到MySQL 5.6.4才支持以及需要手动指定精确度，个人猜测是多数情况下不需要如此高的精确度，以及高精确度带来的性能问题。

### Django中的支持
似乎到了Django 1.8才对fractional seconds进行支持，创建model时可以使用`DATETIME(6)`,参见Django 1.8的[release notes](https://docs.djangoproject.com/en/dev/releases/1.8/#database-backends)和[文档](https://docs.djangoproject.com/en/dev/ref/databases/#mysql-fractional-seconds)。
需要注意的是：
1. 使用fractional seconds的最低版本为MySQL 5.6.4和MySQLdb 1.2.5。
2. Django不会更新已有的数据库，需要执行SQL语句进行修改数据库，或执行Data Migrations操作
3. 对于早期版本，是有hack方案的，参见stackoverflow上的问题[How to create a Django custom Field to store MYSQL DATETIME(6) and enable fractional seconds (milliseconds and or microseconds) in Django/MySQL?](http://stackoverflow.com/questions/17846504/how-to-create-a-django-custom-field-to-store-mysql-datetime6-and-enable-fracti)


## 解决方案

显然，在业务需求精确到秒即可满足的情况，为了自动化测试而更改已经部署的MySQL、Django版本及代码是得不偿失的。所以最终较好的解决方案是在自动化测试的代码中把item的生成时间根据生成次序进行二次处理。