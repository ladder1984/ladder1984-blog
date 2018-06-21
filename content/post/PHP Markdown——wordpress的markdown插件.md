---
title: PHP Markdown——wordpress的markdown插件
tags:
  - Markdown
  - wordpress
id: 664
categories:
  - 网站建设
date: 2013-04-15 00:32:32
---

　　**[《使用Markdown写作WordPress》](http://www.itoldme.net/archives/427)**一文已经介绍了在wordpress通过WP-Markdown插件写markdown，但是WP-Markdown也有问题，那就是如果你插入js代码、iframe之类的东西，虽然可以正确显示，但是当你发布完文章后，编辑器里这些东西就消失了。也就是说当你再需要修改这篇文章时候，你得重新找来这些代码。这种编辑器乱改文章的事情是相当糟糕的。
　　幸运的是，另一款markdown插件PHP Markdown不存在这个问题，当然没有实时预览是这个插件的一个缺点。

### 安装PHP Markdown

　　在wordpress后台的插件搜索中搜不到PHP Markdown，所以还是去官网下载吧。

**下载地址**:[http://michelf.ca/projects/php-markdown/classic/](http://michelf.ca/projects/php-markdown/classic/),如果你不需要PHP Markdown Extra的特性的话，就下载上面的PHP Markdown好了。最新版为PHP Markdown 1.0.1q，直接下载地址为:[http://littoral.michelf.ca/code/php-markdown/php-markdown-1.0.1q.zip](http://littoral.michelf.ca/code/php-markdown/php-markdown-1.0.1q.zip)

下载完毕后，和安装其他wordpress插件一样上传到plugins目录，或者通过后台上传功能安装

### 配置PHP Markdown

　　遗憾的是，没有找到WP-Markdown一样的配置选项，所以就直接写吧。似乎只有在html编辑器中可以使用markdown，这样也好，直接放弃可视化编辑器吧，避免编辑器过程中带来不必要的麻烦。
**设置方法**:用户-所有用户-&lt;你的用户名>-勾选“撰写文章时不使用可视化编辑器”

PS:段乱开头的空格可以使用html实体＆nbsp;实现。

* * *

推荐阅读：
　　**[《使用Markdown写作WordPress》](http://www.itoldme.net/archives/427)**