---
title: C语言main()函数标准写法
tags:
  - C
id: 670
categories:
  - 编程笔记
date: 2013-04-17 16:49:12
---

即使到了2013年的今天，还是能看到在C语言中void main()这种写法。也许是被VC++ 6宠坏了，也许是被谭浩强的《C语言程序设计》误导了（谭的第四版已使用int main()写法）。不必纠结void main()历史成因，只用接受main的正确写法。

在C语言中，正确的main函数写法只有两种：

无参数的：

<pre class="lang:c decode:1 " >int main(void){/*...*/}</pre>

或者两个参数的:

<pre class="lang:c decode:1 " >int main(int argc, char *argv[]){/*...*/}</pre>

当然，参数名称可以自定义。

这两种方式在C语言标准C11([ISO/IEC 9899:2011](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=57853))的5.1.2.2.1 Program startup处说明，C99([ISO/IEC 9899:1999](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=29237))中的规定相同，如图：![C11 Program startup](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/7163547.png)

需要注意的是，C++中，int main()等价于int main(void)，有参数的写法相同。但是请注意，这是C++的写法，C和C++是两种语言。

<menu id="userscript-search-by-image" type="context"></menu>