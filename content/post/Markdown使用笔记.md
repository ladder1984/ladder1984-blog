---
title: Markdown使用笔记
tags:
  - Markdown
id: 537
categories:
  - About计算机
date: 2013-04-11 08:00:07
---

Markdown很优雅、简洁，但正因为简洁，所以存在着一些功能的缺失。本文介绍一些Markdown使用的补充。

*   **HTML代码转义**
虽然Markdown可以自动处理&lt;和&amp;这样的特殊字符，参见[http://wowubuntu.com/markdown/#autoescape](http://wowubuntu.com/markdown/#autoescape)。但还是会在一些地方出现问题，比如在wordpress贴代码，所以还是在Markdown中书写HTML转义字符实体比较好。比如&lt;stdio.h&gt;最好写成&amp;lt;stdio.h&amp;gt;形式。

*   **插入表格**
Markdown本身不能不能输入表格，但好在留了兼容html这条路。同样你可以手动html的table，而这个网站可以自动转换成html代码：[http://pressbin.com/tools/excel_to_html_table/index.html](http://pressbin.com/tools/excel_to_html_table/index.html)

**示例**
<pre>&lt;table&gt;
   &lt;tr&gt;
      &lt;td&gt;John&lt;/td&gt;
      &lt;td&gt;Smith&lt;/td&gt;
      &lt;td&gt;123 Main St.&lt;/td&gt;
      &lt;td&gt;Springfield&lt;/td&gt;
   &lt;/tr&gt;
   &lt;tr&gt;
      &lt;td&gt;Mary&lt;/td&gt;
      &lt;td&gt;Jones&lt;/td&gt;
      &lt;td&gt;456 Pine St.&lt;/td&gt;
      &lt;td&gt;Dover&lt;/td&gt;
   &lt;/tr&gt;
   &lt;tr&gt;
      &lt;td&gt;Jim&lt;/td&gt;
      &lt;td&gt;Baker&lt;/td&gt;
      &lt;td&gt;789 Park Ave.&lt;/td&gt;
      &lt;td&gt;Lincoln&lt;/td&gt;
   &lt;/tr&gt;
&lt;/table&gt;</pre>
这些代码可以得到
<table>
<tbody>
<tr>
<td>John</td>
<td>Smith</td>
<td>123 Main St.</td>
<td>Springfield</td>
</tr>
<tr>
<td>Mary</td>
<td>Jones</td>
<td>456 Pine St.</td>
<td>Dover</td>
</tr>
<tr>
<td>Jim</td>
<td>Baker</td>
<td>789 Park Ave.</td>
<td>Lincoln</td>
</tr>
</tbody>
</table>
如果你想得到边框、底色等功能可以参考这两篇文章：[Markdown之表格的处理](http://www.ituring.com.cn/article/3452)以及[http://twitter.github.io/bootstrap/base-css.html#tables](http://twitter.github.io/bootstrap/base-css.html#tables) (英文)

* * *

更多关于Markdown的本站文章请看[http://www.itoldme.net/archives/tag/markdown](http://www.itoldme.net/archives/tag/markdown)

<menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu>