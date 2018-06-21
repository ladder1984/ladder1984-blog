---
title: Iconic One主题更改字体
tags:
  - wordpress
id: 1110
categories:
  - 网站建设
date: 2014-06-04 02:27:34
---

## 一、起因

[墙外的梯子](www.itoldme.net)一直使用Iconic One主题，但近期网络不稳定，导致本站也访问缓慢，经查其中一个原因是主题默认字体Ubuntu放在放在Google的网站font.googleapis.com上。平时还好，但是像最近这样的特殊时期则影响较大。
<pre class="toolbar:2 lang:css decode:true ">&lt;link rel='stylesheet' id='themonic-fonts-css'  href='http://fonts.googleapis.com/css?family=Ubuntu:400,700&amp;#038;subset=latin,latin-ext' type='text/css' media='all' /&gt;</pre>
这行代码大大拖累了网站访问速度。

## 二、方法

字体可以设置为
> Helvetica, Tahoma, Arial,  "Microsoft YaHei",  SimSun,  Heiti, sans-serif !important;
&nbsp;

1.修改css，修改的方法有2两种，一种是直接替换style.css中的字体设置，一种是在custom.css添加。

方法1：在style.css搜索**font-family**，更改字体。
比如改为
> font-family: Helvetica, Tahoma, Arial, STXihei, "华文细黑", "Microsoft YaHei", "微软雅黑", SimSun, "宋体", Heiti, "黑体" ,sans-serif !important;
方法2：在custom.css添加。

如：
> body {> 
> font-size: 14px;> 
> font-family: Helvetica, Tahoma, Arial,  "Microsoft YaHei",  SimSun,  Heiti, sans-serif !important;> 
> text-rendering: optimizeLegibility;> 
> color: #444;> 
> }> 
> 
> .entry-content code,> 
> .comment-content code {> 
> font-family: Helvetica, Tahoma, Arial,  "Microsoft YaHei",  SimSun,  Heiti, sans-serif !important;> 
> font-size: 12px;> 
> line-height: 2;> 
> }> 
> 
> .entry-content pre,> 
> .comment-content pre {> 
> border: 1px solid #ededed;> 
> border-radius: 20px;> 
> color: #666;> 
> font-family: Helvetica, Tahoma, Arial,  "Microsoft YaHei",  SimSun,  Heiti, sans-serif !important;> 
> font-size: 12px;> 
> line-height: 1.514285714;> 
> margin: 24px 0;> 
> overflow: auto;> 
> padding: 24px;> 
> }
2.搜索**Ubuntu**，在functions.php中删除如下代码。
<pre class="toolbar:2 lang:php decode:true ">/*
     * Loads the awesome readable ubuntu font CSS file for Iconic One.
*/
    if ( 'off' !== _x( 'on', 'Ubuntu font: on or off', 'themonic' ) ) {
        $subsets = 'latin,latin-ext';
        $protocol = is_ssl() ? 'https' : 'http';
        $query_args = array(
            'family' =&gt; 'Ubuntu:400,700',
            'subset' =&gt; $subsets,
        );
        wp_enqueue_style( 'themonic-fonts', add_query_arg( $query_args, "$protocol://fonts.googleapis.com/css" ), array(), null );
    }</pre>
3.更新网站缓存,如果网站有使用缓存插件或者CDN的话。

## 三、后话

但愿有一天不用这么折腾，我们的网络能畅通。