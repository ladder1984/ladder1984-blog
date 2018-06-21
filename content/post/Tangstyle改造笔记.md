---
title: Tangstyle改造笔记
tags:
  - wordpress
id: 477
categories:
  - 网站建设
date: 2013-04-07 13:27:28
---

[Tangstyle](http://tangjie.me/tangstyle)是款相当不错的简洁风格主题，具有自适应特性，也免去了再安装手机版主题的麻烦。为了让它符号我的习惯，做了一些改造，使其符合我的习惯。

*   **去掉段首空格，为了在编辑器所见即所得，还是改为手动输入两个[全角空格](http://www.itoldme.net/archives/518)为好。**
**方法**：找到style.css中的.text p {text-indent:20px;padding-bottom:15px;line-height:26px;}，删除text-indent:20px;

*   **加入面包屑导航，为了让用户了解当前位置，面包屑导航是不错的选择。**
**方法**：[见博客添加简陋的面包屑导航](http://www.itoldme.net/archives/275)

*   **更改摘要输出。原本的摘要不太美观，不能显示html标签效果。**
**方法**：打开模板里的index.php文件，把里面的
<pre lang="php"> 

<!--?php echo mb_strimwidth(strip_tags(apply_filters('the_content', $post--->post_content)), 0, 330,”…”); ?&gt;</pre>
替换成&lt;?php the_content(); ?&gt;。这样就是全文输出了，然后用这个中文摘要输出插件[http://www.kutailang.com/excerpt](http://www.kutailang.com/excerpt)

这样的好处是可控性提高，可以自由修改样式。

*   **修改搜索框。原本的搜索是写在代码里的，而不是小工具。**
**方法**：删除每个sidebar里的
<pre>&lt;div id="search"&gt;
    &lt;form id="searchform" method="get" action="&lt;?php bloginfo('home'); ?&gt;"&gt;
        &lt;input type="text" value="&lt;?php the_search_query(); ?&gt;" name="s" id="s" size="30" x-webkit-speech /&gt;
        &lt;button type="submit" id="searchsubmit"&gt;&lt;i class="iconfont"&gt;ő&lt;/i&gt;&lt;/button&gt;
    &lt;/form&gt;
&lt;/div&gt;
&lt;div class="sidebar"&gt;</pre>
然后可以随意改造，使用小工具里的搜索框也行，使用别的，比如本站使用的Google Custom Search插件。

*   **使用同一侧边栏，默认情况下，首页、分类页、文章页分别对应三个侧边栏，如果三个位置内容相同的话，只需要使用一个即可。**
**方法**：在index.php、single.php、category.php里最后的&lt; ?php get_sidebar('category'); ?&gt;填入需要的sidebar

*   **修改评论框，原来的是个简单而原始的评论框**
**方法**：安装第三放评论框，比如多说、友言等等

*   **增加文章归档页，在Tangstyle没有找到这个功能**
**方法**：安装WP-EasyArchives插件，效果如[http://www.itoldme.net/archive](http://www.itoldme.net/archive)（已失效）,使用方法可以Google插件的名字，很简单。

*   **添加读者墙**
**方法**：添加读者墙的方法可以搜到很多，但既然本站使用了多说插件，就用个较简单的方法：

在你想添加读者墙的页面插入如下代码：
<pre>&lt;li id="ds-recent-visitors-3" class="boxed widget ds-widget-recent-visitors"&gt;
 &lt;h3 class="widgettitle"&gt;近期评论&lt;/h3&gt;
 &lt;ul class="ds-recent-visitors" data-num-items="20"&gt;&lt;/ul&gt;
&lt;/li&gt;
&lt;script&gt;if (typeof DUOSHUO !== 'undefined')
            DUOSHUO.RecentVisitors('.ds-recent-visitors');&lt;/script&gt;</pre>
其中data-num-items是显示访客的数量，“近期评论”可以自定义。了解更多请参考多说官方说明:[http://blog.duoshuo.com/help/helps-recent-comments/](http://blog.duoshuo.com/help/helps-recent-comments/)
效果可以看本站的留言板[http://www.itoldme.net/guestbook](http://www.itoldme.net/guestbook)

*   **删除自带SEO信息**
**方法**：主题自带SEO功能，但是本人为了能够完全掌握SEO信息，决定使用Wordpress的SEO插件，比如本站现在用的All in One SEO Pack。所以要删除主题自身的SEO信息，包括描述（Description）和关键字（KeyWords）。这两样都在header.php中，搜索description和keywords找到相关语句，然后跟着感觉删吧，嘿嘿。 ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/184315.png) 我删掉了
<pre lang="php"> 

<!--?php if ( is_single() ) { ?--><meta name="description" content="&lt;?php echo trim($description); ?&gt;" /><meta name="keywords" content="&lt;?php echo rtrim($keywords,','); ?&gt;" />

<!--?php } ?-->

<!--?php if ( is_home() ) { ?--><meta name="description" content="&lt;?php echo stripslashes(get_option('tang_description')); ?&gt;" /><meta name="keywords" content="&lt;?php echo stripslashes(get_option('tang_keywords')); ?&gt;" />

<!--?php } ?--></pre>
<menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu>