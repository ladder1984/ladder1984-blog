---
title: 用Chevereto搭建图床
tags:
  - phpcloud
  - wordpress
id: 434
categories:
  - 网站建设
date: 2013-04-05 00:27:11
---

# 前言

　　虽然有imgur、imgbox等免费图床，以及skydrive、新浪微博等可以做图床的网络存储，而且这些服务稳定性还不错。但是依然不能让人**100%**放心。原因如下：

1.  服务关闭或修改政策。连Google Reader都会被关闭，还有什么不会的呢？若是在未通知用户情况下突然关闭，那对用户造成的损失将会巨大的。
2.  网站被墙。天灾人祸导致图床服务器损坏，还有可能回复，而一旦被墙，就是不可抗拒的。
3.  图片被删除。就算是合法的文件，也不能保证图床服务商因为某种政策原因删除你的图片。

　　如果图床出了问题，即使你保存了图片，更换文章中的图片url也是个复杂的工程。所以，搭建自己的图床是个不错的选择，当然在专业的图床供应商处购买付费服务也是不错的选择。搭建自己的图床，使用自己的域名，即使空间出了问题，还是可以迁移的，因为引用的只是图片的url。鉴于Wordpress自带不错的图片管理功能，本文更适合空间、流量有限，并担心免费服务稳定性的人。

# 用Chevereto搭建图床

![Chevereto](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/chevereto.png)

　　图床程序有很多，以**图床程序**为关键词Google一下就有大量选择。选择Chevereto是因为它的易于部署，无需sql数据库。当然简单的代价就是功能很少，少到了你就把他当成可以外链图片的网盘就行了。当然，这是指的Chevereto免费版，其收费版是有相当丰富功能的。

## 1\. Chevereto下载

Chevereto最新的免费版是1.91，最后更新是2010年10月，下次更新估计在下世纪。

Chevereto官网：[http://chevereto.com/](http://chevereto.com/)

免费版下载：[http://chevereto.googlecode.com/files/chevereto_nb1.91.zip](http://chevereto.googlecode.com/files/chevereto_nb1.91.zip)

## 2\. Chevereto部署

**上传**：像安装Wordpress等php程序安装一样，把upload文件夹的里的所有文件上传到网站根目录，如果你没有合适的免费空间，推荐使用phpcloud，使用方法，参考[《PHPCloud安装使用WordPress图文教程》](http://www.itoldme.net/archives/368)

**修改文件权限**：修改所有文件夹权限为777，phpcloud修改的方法，同样参考[《PHPCloud安装使用WordPress图文教程》](http://www.itoldme.net/archives/368)

**设置中文**：Chevereto默认是英文，但鉴于功能十分简单，是否修改中文也无所谓。修改方法为在config.php修改define('LANG', 'cn');

**修改图片存储路径**：默认是在/images文件夹内，修改方法为在config.php修改define('DIR_IM','images/');

## 3\. 和本地同步

　　如果你嫌手动上传到图床麻烦的话，可以把图片保存到本机文件夹，利用WinSCP同步到图床服务器。具体方法参考这篇文章： [利用sftp+winscp的自动同步功能建立属于自己的伪dropbox](http://housefansblog.tk/?p=352)