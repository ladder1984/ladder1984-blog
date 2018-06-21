---
title: Code::Blocks的中文汉化
tags:
  - C
id: 1025
categories:
  - About计算机
date: 2014-01-03 12:47:42
---

## 什么是Code::Blocks

Code::Blocks是一个免费、开源、跨平台的集成开发环境，使用C++开发，并且使用wxWidgets做为GUI库。Code::Blocks使用了插件架构，其功能可以使用插件自由地扩充。目前， Code::Blocks主要针对开发C／C++程序而设计。
Code::Blocks目前支持Windows、Linux及Mac OS X数种平台。用户亦能够在FreeBSD环境中建设Code::Blocks

——[Code::Blocks - 维基百科，自由的百科全书](https://zh.wikipedia.org/zh-cn/Code::Blocks)

## 汉化过程

1.  下载mo文件
翻译文件的格式是.mo，需要到[https://translations.launchpad.net/codeblocks](https://translations.launchpad.net/codeblocks)下载最新版mo文件，如下网盘是一个2015年1月最新的mo文件：

        *   OneDrive下载：[http://1drv.ms/1hMfA9f](http://1drv.ms/1hMfA9f)
    *   DropBox下载：[https://www.dropbox.com/s/xg23tyud5p7pmlo/codeblocks.mo](https://www.dropbox.com/s/xg23tyud5p7pmlo/codeblocks.mo)
2.  放置mo文件
把mo文件放入**\CodeBlocks\share\CodeBlocks\locale\zh_CN\codeblocks.mo**，其中locale文件夹和zh_CN文件夹需要自己创建。
3.  设置Code::Blocks
Code::Blocks内 setting-->Environment-->viwe-->Internationlization
4.  重启Code::Blocks

## 其他

如果想自己翻译Code::Blocks，可以在[https://translations.launchpad.net/codeblocks](https://translations.launchpad.net/codeblocks)下载po文件，用Poedit编辑并生成mo文件。