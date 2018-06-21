---
title: 第1章 简介——The Python Language Reference阅读笔记
tags:
  - Python
  - The Python Language Reference阅读笔记
id: 1374
categories:
  - 编程笔记
date: 2015-04-14 23:10:25
---

# 关于阅读笔记

《The Python 2 Language Reference阅读笔记》系列文章是我阅读Python 2的The Python Language Reference所记的笔记，不是翻译，也不会全部罗列Reference所讲的全部内容，而是记一些笔记和心得。

在笔记中有我对文档的一些理解，以及相关内容的资料，文档原文在[The Python Language Reference](https://docs.python.org/2/reference/index.html)，另有一份中文翻译[Python 语言参考](http://python.usyiyi.cn/python_278/reference/index.html)，没有完整阅读过翻译，翻译质量未知。

* * *

# 第一章 简介

## 1.1.可选实现

常见的Python实现有CPython、Jython、Python for .NET、IronPython、Pypy。

Cpython是最流行的的实现，C编写，也是官方的实现。Jython是Java的实现。Python for .NET和IronPython都是.Net相关的实现。PyPy是用Python本身实现的Python，值得关注。

## 1.2.记号法

Reference的词法和语法分析描述方法由巴克斯范式（BNF）修改而来，比较简单。

> name      ::=  lc_letter (lc_letter | "_")*
>   lc_letter ::=  "a"..."z"

第一行是说name是一个lc_letter，后面跟着一个零个或多个lc_letter和下划线组成的序列。接着，一个lc_letter是'a'到'z'之间任意一个单个字符。（这个规则事实上就是该文档中词法和语法规则中的名称的定义方式。）

每条规则以一个名字（这条规则定义的名字）和::=开始。竖线(|)用于分隔可选的项；它是该语法符号中绑定性最弱的操作符。星号(&#42;)表示前面项目的零个或多个重复；类似地, 加号(+)表示一个或多个重复, 而方括号([ ])表示里面的内容出现零次或一次（换句话说, 方括号中的内容是可选的）。*和+操作符的绑定性最强；圆括号用于分组。字符串字面值由引号引起来。空格只对分隔标识符有意义。规则通常包含在单独的一行中；具有许多可选项的规则可能会在第一行之后，每一行以一个竖线开始。

在词法定义中（如上面的例子），还使用了两个额外的约定：三个点号分隔的两个字符表示给出的范围内（包括这两个字符）的任何一个单个ASCII字符。尖括号(&lt;...>)中的内容表示不是定义的符号的正式描述；例如，如果需要这可以用来描述‘控制字符’的概念。