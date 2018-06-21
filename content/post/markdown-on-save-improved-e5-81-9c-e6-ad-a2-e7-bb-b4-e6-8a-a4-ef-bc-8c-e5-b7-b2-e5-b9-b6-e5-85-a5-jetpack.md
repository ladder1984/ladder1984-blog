---
title: Markdown on Save Improved停止维护，已并入Jetpack
tags:
  - Markdown
  - wordpress
id: 1074
categories:
  - About互联网
date: 2014-03-26 14:18:33
---

[墙外的梯子](http://www.itoldme.net)长期使用的markdown插件**Markdown on Save Improved**业已停止维护，并且已并入**Jetpack**，markdown功能现在是该插件的一个模块。

之前[墙外的梯子](http://www.itoldme.net)使用过多款wordoress的markdown插件，包括PHP Markdown、WP-Markdown等，最终选择了Markdown on Save Improved插件，前几天更新发现Markdown on Save Improved停止维护，惊讶之余发现好在只是并入了Jetpack。jetpack是大名顶顶的wp插件，长期待在wp热门插件榜前列，同时也是wordpress.com官方出品的插件，虽然这个插件恨过功能中国用户没法用。。。

Markdown on Save Improved最新版的说明已经变成“Deprecated. Please use the Markdown module in the Jetpack plugin!”，那我们就紧随作者脚步迁移到Jetpack。登录官网[jetpack.me](http://jetpack.me/)下载或者wp后台搜索Jetpack安装。停用Markdown on Save Improved，在后台Jetpack面板找到并启用markdown功能，如图。
![markdown in Jetpack](http://www.itoldme.net/wordpress/wp-content/uploads/2014/03/QQ%E6%88%AA%E5%9B%BE20140326143405.png)

说来对Markdown on Save Improved的终止还是有点遗憾，毕竟用了这么久，但世事皆在变化，期待Jetpack中的markdown不断进步，也期待wordpress原生支持markdown。

**注意**
Jetpack的语法和标准Markdown语法略有不同，似乎更接近[**Markdown Extra**](https://michelf.ca/projects/php-markdown/extra/)，比如:
**1.#标题写法**
Jetpack的语法是：

> # Header 1
>    ## Header 2
>    ### Header 3 
>    #### Header 4 ####
>    ##### Header 5 #####
>    ###### Header 6 ######
>   后面的#是可选的

**2.不支持两个空格产生换行**，Jetpack的解释是

> We do not support Markdown’s typical double-space to generate a line break due to our built-in auto-linebreaking function. A regular line break will generate a line break on output.

**更多语法说明参见[http://en.support.wordpress.com/markdown-quick-reference/](http://en.support.wordpress.com/markdown-quick-reference/)**