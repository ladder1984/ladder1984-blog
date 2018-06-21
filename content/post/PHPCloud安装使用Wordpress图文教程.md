---
title: PHPCloud安装使用Wordpress图文教程
tags:
  - phpcloud
  - wordpress
  - 免费空间
id: 368
categories:
  - About互联网
date: 2013-03-18 13:52:00
---

##### [_PHPCloud_](http://www.phpcloud.com/)  [http://www.phpcloud.com/](http://www.phpcloud.com/ "http://www.phpcloud.com/")

PHPCloud是由著名的Zend公司的提供的免费云，500M免费PHP空间，不限流量，速度不错。500M对于个人博客差不多够了。本文介绍PHPCloud安装使用Wordpress，及一些疑难问题的解决。

一.申请PHPCloud空间

> 首先打开[http://www.phpcloud.com/](http://www.phpcloud.com/ "http://www.phpcloud.com/")，点击START DEVELOPING注册，首先需要有个ZEND帐号，如果没有的话，会让你注册，跟着链接走就行了。
> 
> ![https://ihuabq.bn1.livefilestore.com/y1pstX4tj2tak4f2P3qdkM76amvnJpjySZ4doPAtds_iVPYZMcj2-CBXTpTxSxYR6CkzWO8mgZK8OTFhmGlVKDFGvqxsVOpPSUa/QQ%E6%88%AA%E5%9B%BE20130318135305.png?psid=1](https://ihuabq.bn1.livefilestore.com/y1pstX4tj2tak4f2P3qdkM76amvnJpjySZ4doPAtds_iVPYZMcj2-CBXTpTxSxYR6CkzWO8mgZK8OTFhmGlVKDFGvqxsVOpPSUa/QQ%E6%88%AA%E5%9B%BE20130318135305.png?psid=1)

二.添加Wordpress

　　PHPCloud可以一键添加Wordpress。点击左侧的My Containers [https://my.phpcloud.com/container/list](https://my.phpcloud.com/container/list "https://my.phpcloud.com/container/list")。点击Create Container ，Container Name随便填，也是赠送的免费域名。

![https://ihuabq.bn1.livefilestore.com/y1pElkOWu34LX9m7r9hg4VBMTp2uOAY69KRAt0GZQIHJ-ldd5ygBw7ClJXQeFdQanNACGWTOhJbugPlRhgyH-6EeGljjLthdccn/QQ%E6%88%AA%E5%9B%BE20130318135910.png?psid=1](https://ihuabq.bn1.livefilestore.com/y1pElkOWu34LX9m7r9hg4VBMTp2uOAY69KRAt0GZQIHJ-ldd5ygBw7ClJXQeFdQanNACGWTOhJbugPlRhgyH-6EeGljjLthdccn/QQ%E6%88%AA%E5%9B%BE20130318135910.png?psid=1)

新建的Container默认有个Default Application，最好是点击remove删除算了。

　　点击[NEW APPLICATION ](https://my.phpcloud.com/apps/gallery/container/test135)新建应用，找到Wordpress，点击[DEPLOY APPLICATION ](https://my.phpcloud.com/apps/deploy/container/test135/appId/4)。

![https://ihuabq.bn1.livefilestore.com/y1pg9O4qSLXKPmrxw6vVRbOfEscSkbb_WW1bQq8Xwdqr4IffOyj7Vxle-LdAaNuzMoqwj-GOJ1PjFniHd1yyLKRK4BrK-XMyB3g/QQ%E6%88%AA%E5%9B%BE20130318140555.png?psid=1](https://ihuabq.bn1.livefilestore.com/y1pg9O4qSLXKPmrxw6vVRbOfEscSkbb_WW1bQq8Xwdqr4IffOyj7Vxle-LdAaNuzMoqwj-GOJ1PjFniHd1yyLKRK4BrK-XMyB3g/QQ%E6%88%AA%E5%9B%BE20130318140555.png?psid=1)然后会让你设置Application Name应用名

![https://ihuabq.bn1.livefilestore.com/y1p0iUWQ0fk55zhgaRZHq34UXam5mdzo8VP7Tu53Tte1a4Fwx6ydHRHsRYUQrjwWPVNzAQOR3ZT5v9eSnlltO6S1BXlAI53dELF/QQ%E6%88%AA%E5%9B%BE20130318140640.png?psid=1](https://ihuabq.bn1.livefilestore.com/y1p0iUWQ0fk55zhgaRZHq34UXam5mdzo8VP7Tu53Tte1a4Fwx6ydHRHsRYUQrjwWPVNzAQOR3ZT5v9eSnlltO6S1BXlAI53dELF/QQ%E6%88%AA%E5%9B%BE20130318140640.png?psid=1)

Name随便填，Path默认情况下会是XXX.my,phpcloud.com/Name，如果你已经删除了Default Application，可以直接填/，省去了目录，有助后续绑定域名。然后点击Deploy Application，等待一会Wordpress就建立好了。

三.设置域名

　　PHPCloud不提供直接绑定域名，但可以通过cname方式，在你的dns设置里，添加canme记录，记录值就是你Wordpress的网址XXX.my,phpcloud.com。dns管理推荐[DNSPOD](https://www.dnspod.cn/)。同时需要在你的Wordpress设置里换上你的新域名。进入后台-Settings-General Settings，修改Site Address (URL)和WordPress Address (URL)。<font color="#ff0000">注意：</font>Wordpress帐号是admin，密码是Create Container时设置的密码。

四.文件管理

> PHPCloud不支持ftp，但可以用累死的sftp，sftp客户端建议试用WinSCP，下载地址[http://winscp.net/eng/docs/lang:chs](http://winscp.net/eng/docs/lang:chs "http://winscp.net/eng/docs/lang:chs")。安装完WinSCP，新建链接，选择sftp协议。主机名是Wordpress的网址，不用加http://，用户名是Container Name，不用输入密码，而是密钥文件。如果你没保存的话，返回PHPCloud，左侧My Account-Access Keys-Generate New Keypair，如果你用Windows，记得选择PPK格式的下载。
> 
> 　　登录后就看到了类似Ftp工具的管理窗口。

五.配置Wordpress

　　如果你已经开始使用Wordpress，大概已经发现了问题，安装插件，升级Wordpress什么的都需要你设置链接信息，但PHPCloud不支持ftp。

1.解决PHPCloud没有ftp管理文件。

> 用WinSCP找到Wordpress根目录，编辑wp-config.php，找到最下面的
> 
> require_once(ABSPATH . ‘wp-settings.php’);

在其后添加如下代码

<pre>if(is_admin()) {
add_filter('filesystem_method', create_function('$a','return "direct";' ));
define('FS_CHMOD_DIR', 0751);
}
</pre>

这样安装插件之类的问题，保险起见，在WinSCP选择所有文件右击，权限设置成777。 2.升级Wordpress 　　通过添加应用生成的Wordpress是3.1的英文版，在[http://cn.wordpress.org/](http://cn.wordpress.org/ "http://cn.wordpress.org/")下载最新中文版，解压后上传并覆盖原有文件。打开wp-config.php，define (‘WPLANG’, ”);改为define (‘WPLANG’, ‘zh_CN’);

　　这样就基本上可以正常试用Wordpress了，测试安装插件，更改主题及上传媒体文件正常。

六.总结

　　总的来说PHPCloud还是不错的，比本站使用cdn前的收费空间快许多。

　　PHPCloud没有备份，鉴于可能的风险，最好养成自己备份的习惯，Wordpress备份方法网上多的是，Google一下，你就懂的。

　　openshift二级域名已经被墙，希望大家合理利用PHPCloud。

PHPCloud探针：[http://blog.itoldme.net/tz.php](http://blog.itoldme.net/tz.php)