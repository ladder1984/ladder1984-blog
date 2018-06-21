---
title: C语言判断输入是否是整数
tags:
  - C
id: 374
categories:
  - 编程笔记
date: 2013-03-28 00:13:14
---

首先考虑可能的输入：

*   **非数字的字符。**
*   **带小数点的浮点数。**
&nbsp;

对于第一种情况，考虑到scanf函数的返回值是成功输入的数量，用if(scanf("%d",&amp;a)==1)判断即可。但是可惜的是，如果输入的是浮点数，scanf依然认为你输入成功，并返回1。

以下提供两种解决方案：
**一.输入浮点型，并于强制转换int型的结果比较是否相等：**
代码如下：
<pre>int IsInt(float a)
{
    if((int)a==a)
        return 1;
    else
        return 0;
}</pre>
示例程序：
<pre class="lang:default decode:true">#include &lt;stdio.h&gt;

int IsInt(float a)
{
    if((int)a==a)
        return 1;
    else
        return 0;
}
int main(int argc, char *argv[])
{
	float a;
	while(1)
	{
		scanf("%f",&amp;a);
		fflush(stdin);
		if(IsInt(a))
            printf("It's an intn");
        else
            printf("It's not an intn");
	}

	return 0;
}</pre>
当然，IsInt返回值是1的话，要用(int)a强制转换成int型并赋给a。

**二.输入字符串，逐字符比较**
用字符串数组a[SIZE]接收输入，判断每个字符是否满足a[i]&gt;='0'&amp;&amp;a[i]&lt;='9'条件，有一个字符不满足则不是int型。若是则用atoi函数转换成int型。

_**如果你有更好的方法，欢迎在评论中反馈。**_

<menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu>