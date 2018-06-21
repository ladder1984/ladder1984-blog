---
title: Wordpress主题制作笔记
tags:
  - wordpress
  - 笔记
id: 956
categories:
  - 网站建设
date: 2013-11-06 23:20:00
---

　　wordpress主题非常强大，完全可以定制出所需功能。把以下函数放入functions.php，把调用函数放入主题中所需的位置。

## 获取指定文章内容

根据指定文章的ID，获取内容，并以wordpress格式输出内容，函数：

<pre class="lang:php decode:true " >function get_post_content_by_id($post_id = ''){
    $post_content = $post_id == '' ? get_the_content() : get_post($post_id)-&gt;post_content;
    return wpautop($post_content);
}</pre>

**调用函数：**<pre class="inline:true decode:1 " >get_post_content_by_id($id)</pre>，如<pre class="inline:true decode:1 " ><?php echo get_post_content_by_id(20) ?></pre>

**参考：**[wordpress根据指定ID获取文章内容](http://www.utubon.com/43/get-content-base-on-article-id)

* * *

## 不同分类调用不同sidebar

**插件法：**安装[Widget Logic插件](http://wordpress.org/plugins/widget-logic/)

**使用方法:**在小工具的widget logic设置出填入<pre class="inline:true decode:1 " >(is_single() &amp;&amp; in_category(X))</pre>其中X是分类ID。

**参考:** [WordPress › Widget Logic « WordPress Plugins](http://wordpress.org/plugins/widget-logic/)

* * *

## 使用导航菜单

导航菜单注册函数：<pre class="inline:true decode:1 " >register_nav_menus() </pre>

导航菜单调用函数：<pre class="inline:true decode:1 " >wp_nav_menu()</pre>

多级菜单的显示需要js或者css的支持。

函数使用参考：[WordPress导航菜单函数register_nav_menus() 和 wp_nav_menu()](http://www.wpdaxue.com/register_nav_menus-and-wp_nav_menu.html)

* * *

## 文章访问量统计

**插件：**[WP-PostViews](http://wordpress.org/plugins/wp-postviews/)

该插件可以统计最多、最少被访问的页面等，函数详情参见：[http://wordpress.org/plugins/wp-postviews/faq/](http://wordpress.org/plugins/wp-postviews/faq/)

## 面包屑导航

参见[墙外的梯子](www.itoldme.net "墙外的梯子")文章[《为wordpress添加面包屑导航》](http://www.itoldme.net/archives/906)

* * *

调用友情链接

自从wordpress 3.5去去掉友情链接之后，使用此功能需要安装官方出品的[Link Manager](http://wordpress.org/plugins/link-manager/)插件。然后在所需位置调用<pre class="inline:true decode:1 " >wp_list_bookmarks</pre>函数，函数使用方法参见[WordPress友情链接函数 wp_list_bookmarks()详解](http://www.wpdaxue.com/wp_list_bookmarks.html)。

如果需要横排显示友情链接，可用CSS控制，核心是<pre class="inline:true decode:1 " >list-style:none;</pre>去掉前面的点，<pre class="inline:true decode:1 " >display:inline;</pre>把li行内显示，如<pre class="inline:true decode:1 " >.linkcat li{floag:left;list-style:none;display:inline;margin:20px}</pre>

* * *

PS：这是一篇通篇外链的文章，算是记点笔记，方便以后查找。