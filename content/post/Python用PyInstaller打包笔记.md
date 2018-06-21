---
title: Python用PyInstaller打包笔记
tags:
  - Python
id: 1242
categories:
  - 编程笔记
date: 2014-10-22 21:37:11
---

为了把python发行到没有安装python的Windows环境使用，需要打包成exe可执行文件。现在常见的python打包工具有cx_Freeze、PyInstaller和py2exe，想想我当初接触python的时候，似乎只有py2exe，而且有不少问题时光荏苒，一切过的真快。本文介绍PyInstaller打包的使用。

## 一.准备工作

### 安装PyWin32

到[http://sourceforge.net/projects/pywin32/](http://sourceforge.net/projects/pywin32/)下载 PyWin32，本文使用的是 pywin32-219.win-amd64-py2.7，地址：[http://sourceforge.net/projects/pywin32/files/pywin32/Build%20219/pywin32-219.win-amd64-py2.7.exe/download](http://sourceforge.net/projects/pywin32/files/pywin32/Build%20219/pywin32-219.win-amd64-py2.7.exe/download)

### 下载PyInstaller

到[http://www.pyinstaller.org/](http://www.pyinstaller.org/)下载PyInstaller并解压缩。本文使用PyInstaller 2.1。

**或者**使用pip安装，执行**pip install pyinstaller**

### 下载upx（可选）

到[http://upx.sourceforge.net/](http://upx.sourceforge.net/)下载upx，解压后把upx放在PyInstaller目录下，upx的作用是给生成的exe加壳，减小体积。

## 二.使用方法

cmd切换到PyInstaller文件夹，执行命令，如：

> pyinstaller myscript.py

当然也可以添加输出选项，获得更好的exe可执行文件，如：

> python pyinstaller.py --upx-dir -F xxx.py

-F用于制作独立的可执行程序，--upx-dir用于压缩文件。

**注意：**

*   网上教程常见的-X选项启用upx已经失效
*   如果upx.exe已经复制到PyInstaller文件夹下，会默认使用upx，如果不在文件夹下，可以使用--upx-dir选项，如--upx-dir=upx_dir，如--upx-dir=/usr/local/share/
*   如果upx.exe复制到了PyInstaller文件夹下，如果不想使用upx，需要添加参数 --noupx

## 三.常用参数介绍

-F 用于制作独立的可执行程序

--upx-dir 使用upx加壳从而压缩exe文件

--noconsole适用于Windows和Mac OS X，用于创建不显示控制台窗口的程序

## 其他

PyInstaller可以用于多个平台的打包，包括 Windows (32-bit and 64-bit),Linux (32-bit and 64-bit),Mac OS X (32-bit and 64-bit),experimentally Solaris and AIX。

PyInstaller也可以自定义ico文件等，完整使用手册参见[http://pythonhosted.org/PyInstaller/](http://pythonhosted.org/PyInstaller/ "官方文档")