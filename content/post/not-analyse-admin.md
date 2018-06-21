---
title: Wordpress禁止统计代码计算管理员本人访问
tags:
  - wordpress
id: 299
categories:
  - 网站建设
date: 2013-01-07 07:07:34
---

如果把站长本人的浏览量计入统计统计数据，显然数据就不准了，影响分析价值，尤其是像我这样的浏览量不本来就小的网站。完全可以利用wordpress自带的**current&#95;user&#95;can()**函数消除此类误差。

**用法：**

<pre><?php current_user_can( $capability, $args ); ?>

</pre>

**参数：**

<dl>
<dt><tt>$capability</tt></dt>
<dd>(_string_) (_required_) capability or role name</dd>

<dd>Default: _None_</dd>

<dt><tt>$args</tt></dt>
<dd>(_mixed_) (_optional_) Any additional arguments that may be needed, such as a post ID. Some capability checks (like <tt>'edit_post'</tt> or <tt>'delete_page'</tt>) require this be provided.</dd>

<dd>Default: _None_</dd>
</dl>

**返回值：**

true：如果当前用户拥有该权限

false：如果当前用户不拥有该权限

**示例：**

<pre><?php if(!current_user_can( 'edit_post' )) { ?>   
    //统计代码

<?php } ?>

</pre>

我们就可以利用管理员拥有编辑文章权限这点来实现我们所需的功能。在原有统计代码外加上这么一层判断语句。

更多信息可参考Wordpress官方介绍：[https://codex.wordpress.org/Function&#95;Reference/current&#95;user_can](https://codex.wordpress.org/Function_Reference/current_user_can)

<div>
  <embed id="ciba_grabword_plugin" width="0" height="0" type="application/ciba-grabword-plugin" hidden="true" />
</div>