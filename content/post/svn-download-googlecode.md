---
title: SVN下载Google Code源代码的简单方法
tags:
  - Google Code
  - SlikSvn
  - svn
id: 301
categories:
  - About互联网
date: 2013-01-10 21:46:15
---

Google Code上的项目基本使用SVN做版本控制，window是没有原生支持SVN的，所以需要一款SVN工具。网上常见的方法是使用TortoiseSVN，不过仅仅是想下载源代码的法，这种方法略显麻烦。现在介绍使用[SlikSVN](http://www.sliksvn.com/en/download)简单下载。

1.下载SlikSVN

当然是下载[SlikSVN](http://www.sliksvn.com/en/download)，下载地址[http://www.sliksvn.com/en/download](http://www.sliksvn.com/en/download "http://www.sliksvn.com/en/download")。虽然是英文，但使用相当简单，这也无所谓了。注意有32位和64位可供选择，请根据你的系统情况选择。SlikSVN支Windows XP/Vista/7/8/2012、Mac OS X、Debian and Ubuntu Linux、FreeBSD。

2.使用

SIikSvn是命令行工具，直接在cmd命令行里就可以使用svn命令了，svn help命令可查看帮助。如图：

![QQ截图20130110213816](http://www.itoldme.net/wordpress/wp-content/uploads/2013/01/QQ截图20130110213816.png)

本文以下载HUSTOJ为示例，打开[https://code.google.com/p/hustoj/source/checkout](https://code.google.com/p/hustoj/source/checkout "https://code.google.com/p/hustoj/source/checkout")，如图所示![QQ截图20130110214023](http://www.itoldme.net/wordpress/wp-content/uploads/2013/01/QQ截图20130110214023.png)如上面写的，把<tt>**svn checkout _http_://hustoj.googlecode.com/svn/trunk/ hustoj-read-only**粘贴在cmd里就可以了。默认存储位置是“我的文档”。![QQ截图20130110214208](http://www.itoldme.net/wordpress/wp-content/uploads/2013/01/QQ截图20130110214208.png)</tt>

<menu id="userscript-search-by-image" type="context"></menu>