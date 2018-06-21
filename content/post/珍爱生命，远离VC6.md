---
title: 珍爱生命，远离VC6
tags:
  - C
id: 379
categories:
  - Coding
date: 2013-04-01 22:50:51
---

### 一.什么是VC 6？

Microsoft Visual C++，（简称Visual C++、MSVC、VC++或VC）是微软Microsoft Visual Studio的一部分，C++开发工具，具有集成开发环境（IDE），可提供编辑C语言，C++以及C++/CLI等编程语，以下简称VC。

而VC 6则是VC的第6个版本，于1998年发布，虽然微软已于2012年发布最新的Visual C++ 2012，也就是第11个版本，但VC 6仍被广泛使用，尤其是国内大学教育。

本文将介绍VC 6的问题，以及推荐更佳的替代品。

### 二.VC6的问题所在

**1.对标准支持差**

典型的例子是void main()，这不是C标准，而是VC++自创的，维基百科上写到“void main的用法并不是任何标准制定的，是Microsoft制定的。 C语言正确的语法是int main。 在 C++ 标准中，虽然 main 的标准型态应是 int，但编译器实现中也可以自行定义型态，不过，所有实现均应接受 int main 的用法“。而且对于C99、C11标准，VC++6基本不支持。使用VC 6容易养成错误的语法习惯，这是最为危险的。

**2.较差的稳定性与兼容性**

现在的Windows系统已经发展到win7、win8，即使经过仔细的配置，未知的兼容性风险也是巨大的，如果非要在这些系统安装VC 6，可以自行Google相关教程。

在作者学校的机房中，无论配置如何的Windows XP系统，VC 6都会出现编译、链接等时彻底卡死，关闭都无法关闭。

**3.落后的IDE环境**

相对当代优秀的各种IDE，这款上个世纪90年代的老古董略显不适应时代，代码自动补全、模板等功能缺失大大加重了代码输入负担。而且，使用VC 6，20余年来IDE的发展成果你都享受不到。

&nbsp;

### 三.推荐的替代品

以下推荐一些优秀的C/C++ IDE,全部是免费软件，本文只是简单介绍下，详情请学会用Google。

&nbsp;

**1.Visual C++ 2012**

虽然VC++ 2012也具有VC 6的一些缺点，而且看起来微软也不打算支持C标准，但相对VC 6而言还是值得推荐的。微软也推出了免费的入门版VS：Visual Studio Express 2012 （集成Visual C++ 2012）。

支持平台：Windows

官方下载：[https://www.microsoft.com/visualstudio/chs/downloads](https://www.microsoft.com/visualstudio/chs/downloads "https://www.microsoft.com/visualstudio/chs/downloads")

**2.C-Free**

免费易用的C/C++ IDE，使用快捷、方便，推荐初学者使用，也是作者最常用的C语言IDE。**注意：**C-Fres生成的程序可能会被部分杀毒软件误报为病毒，但请放心使用。C-Free 5.0 专业版是收费软件，而4.0版本是免费软件，对于初学者差异不大。

支持平台：Windows

官方下载：[http://www.programarts.com/cfree_ch/download.htm](http://www.programarts.com/cfree_ch/download.htm "http://www.programarts.com/cfree_ch/download.htm")

**3.<font style="font-weight: normal">Code::Blocks</font>**

跨平台的免费C/C++ IDE，功能强大，推荐使用。

支持平台：Windows、Linux、Mac OS

官方下载：[http://www.codeblocks.org/downloads/](http://www.codeblocks.org/downloads/ "http://www.codeblocks.org/downloads/")

**4.Dev-C++**

同样简单易用，推荐使用。

支持平台：Windows、Linux

官方下载：[http://www.bloodshed.net/download.html](http://www.bloodshed.net/download.html "http://www.bloodshed.net/download.html")

&nbsp;

更多C/C++ IDE(参见维基百科[页面](https://zh.wikipedia.org/zh-cn/%E9%9B%86%E6%88%90%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)):

开放源代码的软件: <p>Anjuta · Code::Blocks · CodeLite · Dev-C++ · Eclipse · Geany · GNAT Programming Studio · KDevelop · MonoDevelop · NetBeans · QDevelop · Qt Creator · wxDev-C++ <p>免费软件: <p>Visual Studio Express · Pelles C · <p>Sun Studio · Turbo C++ Explorer · Xcode <p>商业软件: <p>C++ Builder · Visual Studio · Turbo C++ Professional <p>&nbsp; <p>提示：一些软件没有中文支持或者翻译包落后最新版，但对于初学者而言，只需要使用软件的少量的操作，所以语言不是问题，而对于高级用户，英语也应该不成问题。

&nbsp;

鉴于作者认识有限，欢迎读者留言执政、补充。