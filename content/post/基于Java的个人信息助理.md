---
title: 基于Java的个人信息助理
tags:
  - Java
id: 1126
categories:
  - My Code
date: 2014-06-25 01:08:07
---

## 1 前言

**“个人信息助理（Personal Information Assistant）”**是我刚刚做完的课程设计的题目，也应该是很多人遇到的题目，但是搜索却发现没有一篇博客文章是讲这个的（也有可能是我没注意到），原因想必是这题目太水了，没什么拿出来讲的价值，我也是这么认为的。。。但鉴于博客很久没有更新了，便就此写点东西凑凑数，也算是对这次课程设计的总结。水平实在是渣，只为记录点自己的经验，顺便给后来同渣者点经验。误点进本页的大神请直接右上角（如果你的关闭按钮在左上角，呃，就左上角吧）。

由于时间关系，也由于本人水平有限，最后的成品非常粗糙，各位姑且一看。本文不会贴完整代码，代码已在github开源。文档也不会贴完整，只会选我认为重要的讲，完整文档也会放出来。

做个课设，写篇总结也是应该，虽然上交了一份厚厚的报告，但那是面向老师的，很多东西说不明白，说不清楚。而本文是面向作者自己、面向课设的。

## 2 需求分析

直接抄书了，最终也是按照书上要求实现的。

> 在现代信息社会里，人们越来越重视信息的交流与沟通，更加注重时间的有效利用，其中个人量也在高速增加，因此个人信息的管理与日常工作和生活中成为一项必不可少的内容，方便实用的个人信息管理工具对于提高信息利用率具有重要实际意义。随着个人电脑的普及，简单实用的个人信息助理工具成为人们日常个人信息管理的不错选择。
>   基本功能与要求：
>   具体需要实现以下一些基本功能。
>   1\. **权限验证**：验证用户合法身份，保障个人信息安全。
>   2\. **口令维护**：用户可以定期或不定期的更改登录口令，提高系统安全性。
>   3\. **日常记事**：记录日常生活或者工作事件，以免遗忘，并提供记录的查询、浏览、修改和删除等管理功能。
>   4\. **通讯簿**：记录朋友、亲属、同事等联系人的通信信息，以免遗忘，并提供记录的查询、浏览、修改和删除等功能。
>   5\. **密码备忘**：记录各类密码信息，以免遗忘，并提供记录的查询、浏览、修改和删除等功能。

稍微注意看下就知道3、4、5功能完全一样，都是对信息的**查询、浏览、修改、删除**，其实还有**添加**，不过既然书上没写，我就不改动了。

## 3 准备工作

### 3.1 数据库的选择

首先要确定，**不能选择**需要服务器端运行数据库，因为PIA是面向桌面用户的，不必要，也不可能要求用户安装数据库，并要求用户配置。所以需要支持无服务器模式的数据库。为了用做题、为了用数据库而用数据库是不可取的，要考虑实际需求。考虑到编程语言是JAVA，所以进入我视野的有SQLite、Access、H2 DATABASE。最终选择了Access，原因很简单：

1.  没有解决SQLite数据库加密的问题。
2.  H2感觉有点复杂，没足够时间折腾。
3.  Access具备足够稳定、令人放心，海量的资料可供查询、能遇到的坑基本前人都遇到过，同时Access应对本系统足够了。

Access也有缺点：
 1\. 这是微软的产品，不知道这算不算原罪。
 2\. 不能真正无服务器运行，至少要装个运行时。
 3\. 跨平台待考，但也看到Jackcess这种有趣的linux解决方案。

### 3.2 开发环境的选择

#### 3.2.1 IDE的选择

只能说，MyEclipse足够**卡**，卡到不用考虑是否选择它。IntelliJ IDEA足够**舒服**，舒服到放不下手。同时MyEclipse是穷学生负担不起的，推荐使用免费的IntelliJ IDEA社区版 http://www.jetbrains.com/idea/download/。

#### 3.2.1 字符编码（Character encoding）选择

选择GBK，不要问我为什么，折腾成UTF-8也会遇到很多问题，在这上面我浪费了很多时间，也就不折腾了，系统默认好了。

#### 3.2.3 操作系统的选择

Windows，既是开发环境，也是支持的平台。原因很简单：

1.  我用的Windows
2.  我选择了Access作为数据库

## 4 分析与设计

### 4.1 三层架构

> 三层架构(3-tier architecture) 通常意义上的三层架构就是将整个业务应用划分为：表现层（UI）、业务逻辑层（BLL）、数据访问层（DAL）。区分层次的目的即为了“高内聚，低耦合”的思想。
>   　　　引自 百度百科

就个人信息助理而言，**UI层**的类用于GUI显示，以及从用户获得输入数据。**BLL层**的类用于把获得的数据生成sql语句。**DAL层**的类用于打开、关闭数据库，执行sql语句。

另外写个PIA.java启动软件，并初始化。

形成如下结构：
**PIA.java**

**BLL**/LoginBLL.java

**BLL**/addrBookBLL.java

**BLL**/notepadBLL.java

**BLL**/passMaintainBLL.java

**BLL**/pwdBLL.java

**DAL**/DB.java

**UI**/MainFrame.java

**UI**/addrBookDialog.java

**UI**/dbMaintainDialog.java

**UI**/loginDialog.java

**UI**/notepadDialog.java

**UI**/passMaintainDialog.java

**UI**/pwdDialog.java

**UI**/searchDialog.java

### 4.2 功能实现

1.  登入功能
界面类：loginDialog.java
业务逻辑类：loginBLL.java
数据访问类：DB.java
①   loginDialog.java
生成登陆窗口，获得用户输入信息。
②   loginBLL.java
把用户信息与数据库内信息比较，返回校验结果。
③   DB.java
数据库访问类，负责打开数据库、关闭数据库、执行sql语句。
2.  登陆口令维护功能
界面类：passMaintainDialog.java
业务逻辑类：passMaintainBLL.java
数据访问类：DB.java
①   PassMaintainDialog.java
创建口令维护窗口，获得新用户、新密码。
②   passMaintainBLL.java
用从界面获得的新用户名、新密码更新数据库信息。
③   DB.java
参见上文
3.  数据库维护功能
类：dbMaintainDialog.java
①   dbMaintainDialog.java
备份数据库、还原数据库。
4.  密码备忘管理
界面类：notepadDialog.java
业务逻辑类：notepadBLL.java
数据库访问类：DB.java
①   notepadDialog.java
创建用户界面，获取用户添加、修改、删除、搜索的信息
②   pwdBLL.java
把用户用户添加、修改、删除的密码备忘传入数据库。
③   DB.java
参见上文
5.  日常记事管理
界面类:notepadDialog.java
业务逻辑类：notepadBLL.java
数据库访问类：DB.java
①   notepadDialog.java
创建用户界面，获取用户添加、修改、删除、搜索的日常记事
②   notepadBLL.java
③   DB.java
6.  通讯簿管理
界面类:addrBookDialog.java
业务逻辑类：addrBookBLL.java
数据库访问类：DB.java
①   addrBookDialog.java
创建用户界面，获取用户添加、修改、删除、搜索的通讯簿条目
②   notepadBLL.java
③   DB.java
参见上文
7.  显示主界面
界面类：mainFrame.java
业务逻辑类：PIA.java
①   mainframe.java
显示主界面。
②   PIA.java
启动软件。启动登陆框、启动主界面、连接数据库

**本文分析设计比较简略，详情参见完整文档**

题    目：       个人信息助理

作    者：zeroten
网    址：[http://www.itoldme.net](http://www.itoldme.net)
代码地址：[https://github.com/ladder1984/PIA](https://github.com/ladder1984/PIA)
文档说明：[http://www.itoldme.net/archives/1126](http://www.itoldme.net/archives/1126)
完整文档(OneDrive下载)：[http://1drv.ms/1v2mZHq](http://1drv.ms/1v2mZHq)