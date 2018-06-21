---
title: 初读《计算机程序的构造和解释》(SICP)
tags:
  - LISP
  - Scheme
  - SICP
id: 319
categories:
  - Coding Life
date: 2013-01-14 03:12:05
---

    中了[《黑客与画家》](http://book.douban.com/subject/6021440/)关于LISP介绍的毒，对LISP神往已久。虽然很难看到LISP的实际应用，相关讨论也少之又少，但偶尔看到的只言片语对于那种说不清道不明的向往已经够了。近日偶然看到《计算机程序的构造和解释》，也就是《Structure and Interpretation of Computer Programs》（简称为SICP），便什么也不顾的打乱既有计划，看了起来。虽然只读了十几页，也收获颇丰。

    如果没有一点编程基础，也许感觉不到什么，但是如果之前学过C语言，或受C影响的其他语言，如C++、Java等，看LISP就仿佛进了一个新世界。一句无厘头的话就能表明我初读SICP的感受，&ldquo;=竟然是等于号的意思，毁三观啊&rdquo;。C等语言中&ldquo;=&rdquo;表示赋值，&ldquo;==&rdquo;才是等于的意思。以我粗浅的理解，LISP中赋值用define表示，如define x 1。这也隐隐表明着LISP与C系语言的巨大不同。据说现在主流的编程语言，滥觞就是C和LISP，果然大不同。

    LISP并没有一个统一的标准，似乎在未来也不可能形成标准，取而代之的是大量方言，最主流是的common LISP和scheme，因为LISP的灵活性，创建一种新的方言也十分容易。SICP一书使用的是scheme。书如其名，该书并不是以介绍scheme为目的，而是计算机程序的构造和解释，所以很遗憾的没有在开头就看到hello world。不过Google一下就有了：(display &quot;Hello world&quot;)。是的，就是这么简单。顺便提下另外两种方言的hello world，Common LISP：(display &quot;Hello world&quot;)。Clojure：(println &quot;Hello, world!&quot;)。

    看书的最初的印象，是熟悉的分号没了，取而代之的是无穷无尽的括号()，一开始看，真的容易搞不清分号的对应关系，不过看了一会就基本习惯了。另外就是奇怪的算术方法，如+ 1 1来表示1+1。结合分号，一个算术式真的看起来够复杂，不过相信这样自有其好处。

    除了LISP本身对我的吸引外，SICP也是很有深度的一本书。[Amazon](http://www.amazon.com/Structure-Interpretation-Computer-Programs-Edition/dp/0070004846/ref=sr_1_1?ie=UTF8&amp;qid=1358103764&amp;sr=8-1&amp;keywords=Structure+and+Interpretation+of+Computer+Programs)上五星和一星居多这种完全不符合正态分布的打分，也显出本书的不烦。这本MIT发入门书，给我带来了极大的兴趣与快感。

    ![http://www.itoldme.net/wordpress/wp-content/uploads/2014/09/s1113106.jpg](http://www.itoldme.net/wordpress/wp-content/uploads/2014/09/s1113106.jpg)

    推荐阅读：

> [_**Structure and Interpretation of Computer Programs**_](http://mitpress.mit.edu/sicp/full-text/book/book.html) SICP在线阅读（英文原文）
> 
>         [**《黑客与画家》**](http://book.douban.com/subject/6021440/)豆瓣
> 
>         **[为什么Lisp语言如此先进？（译文）](http://www.ruanyifeng.com/blog/2010/10/why_lisp_is_superior.html)**
> 
>         *&nbsp; [**计算机程序的构造和解释(豆瓣)**](http://book.douban.com/subject/1148282/)&nbsp; 豆瓣
> 
>          [**https://zh.wikipedia.org/zh/LISP**](https://zh.wikipedia.org/zh/LISP "https://zh.wikipedia.org/zh/LISP")&nbsp; 维基百科LISP词条
> 
>         [**https://zh.wikipedia.org/wiki/Scheme**](https://zh.wikipedia.org/wiki/Scheme "https://zh.wikipedia.org/wiki/Scheme") 维基百科Scheme词条
> 
>         [**《Structure and Interpretation of Computer Programs, Second Edition》**](http://www.amazon.com/Structure-Interpretation-Computer-Programs-Edition/dp/0070004846/ref=sr_1_1?ie=UTF8&amp;qid=1358103764&amp;sr=8-1&amp;keywords=Structure+and+Interpretation+of+Computer+Programs)Amazon.com