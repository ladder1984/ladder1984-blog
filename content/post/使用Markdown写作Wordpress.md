---
title: 使用Markdown写作Wordpress
tags:
  - Markdown
  - wordpress
id: 427
categories:
  - 网站建设
date: 2013-04-04 17:54:42
---

## 前言

今时今日，Markdown已经越来越流行，作为最流行的博客程序——WordPress，虽然没有原生支持，但是有丰富的插件让WordPress支持Markdown。本文将介绍使用**Jetpack**插件支持Markdown，**Crayon Syntax Highlighter**插件来支持代码高亮。

## 一.什么是Mardkown

> Markdown 的目标是实现「易读易写」。
> 
>   可读性，无论如何，都是最重要的。一份使用 Markdown 格式撰写的文件应该可以直接以纯文本发布，并且看起来不会像是由许多标签或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括 Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，而最大灵感来源其实是纯文本电子邮件的格式。
> 
>   总之， Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然。比如：在文字两旁加上星号，看起来就像_强调_。Markdown 的列表看起来，嗯，就是列表。Markdown 的区块引用看起来就真的像是引用一段文字，就像你曾在电子邮件中见过的那样。
> 
>   ——摘抄自[Markdown 语法说明(简体中文版) ](http://wowubuntu.com/markdown/)

简而言之，Mardkown让我们可以抛弃糟糕的富文本编辑器，让我们简单、可控的写作。

Markdown起源于[Daring Fireball: Markdown Syntax Documentation](http://daringfireball.net/projects/markdown/syntax)，此文也是最原始版本的Markdown的规范。

更多关于Markdown的介绍请参见[《使用Markdown写作》](http://www.itoldme.net/archives/393)或者维基百科[Markdown](https://zh.wikipedia.org/zh/Markdown)词条。Markdown语法说明请参见[Markdown 语法说明(简体中文版) ](http://wowubuntu.com/markdown/)。

需要注意的是，除了原始的Mardkown，现在已经衍生出Markdown Extra、Github Flavored Markdo、CommonMark等多个版本，他们都是在原始Markdown的基础上进行功能扩充，也基本兼容原始Markdown的语法。

## 二.Wordpress的Markdown支持插件

wordpress.com已经支持markdown，但是我们所使用的WordPrss程序并不原生支持，好在WordPress从来不缺插件。WordPress的Markdown插件主要有WP-Markdown、Markdown on Save Improved、Jetpack等。其中最好的当属**Jetpack**，它也是wordpress.com实现Markdown支持的方式。另有一个JP Markdown插件，插件说明是Jetpack的Markdown模块，笔者没有使用过，读者若有兴趣，可自行尝试。

在https://wordpress.org/plugins/jetpack/下载插件，或者在WordPress后台搜索安装即可。安装后请记得在Jetpack后台激活markdown，详情见[《
Markdown on Save Improved停止维护，已并入Jetpack
》](http://www.itoldme.net/archives/1074)一文。

需要注意的是，Jetpack支持的是Markdown Extra，和原始Markdown有稍许差别。具体语法，请参见官方文档：[《Markdown quick reference》](http://en.support.wordpress.com/markdown-quick-reference/) （wordpress.com上的内容，自备梯子），或者Markdown Extra的语法说明:[Michel Fortin – PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/)

## 三.代码高亮插件

代码高亮插件并非必须，但因为众多使用者需要贴代码，所以在这也介绍下。推荐使用的代码高亮插件是**Crayon Syntax Highlighter**。

由于插件的自身的原因或者设置不当，使得代码高亮插件和Markdown插件造成冲突，这也是WordPress使用Markdown的最大障碍。而**Crayon Syntax Highlighter**和**Jetpack**完全兼容。

贴代码的方式多种多样，可以使用WordPress后台文章编辑处的添加按钮，如图：
![](http://www.itoldme.net/wordpress/wp-content/uploads/2015/01/Crayon-Syntax-Highlighter001.png)
![](http://www.itoldme.net/wordpress/wp-content/uploads/2015/01/Crayon-Syntax-Highlighter002.png)

或者使用Markdown Extra支持的语法。如：

**行内高亮：**
&#96;This is code&#96;
**效果为：**`This is code`

**区块高亮：**
 ~~~~
This is a 
piece of code 
in a block
 ~~~~

和

&#96;&#96;&#96;
This too
&#96;&#96;&#96;

**效果为：**

    This is a 
    piece of code 
    in a block
    `</pre>

    和

    <pre>`This too

需要**注意**的是，在 Crayon Syntax Highlighter 设置中**不要**勾选“捕获 &#96;反引号&#96; 为 <code> 标签”，以防和Jetpack插件冲突。

更多在WordPress上使用Markdown的技巧请关注，请关注[Markdown - 墙外的梯子](http://www.itoldme.net/archives/tag/markdown)。

* * *

**本文最后更新于：2015/1/28**