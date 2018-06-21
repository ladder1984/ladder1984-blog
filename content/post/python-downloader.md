---
title: python写的简单批量下载器
tags:
  - Python
id: 290
categories:
  - My Code
date: 2013-01-02 01:15:43
---

闲得无聊，用python写了个简易下载器，把教务系统中所有人的照片下载下来了。俗话说“人生苦短，我用python”，所以用python实现只需要几行代码。<!--more-->

python版本为2.x,使用3.x也差不多，只需要把代码第一行的from urllib改成from urllib.request即可。folder定义下载的文件夹，input_file=open打开存储每个文件文件名的列表的文件all.txt。for语句从all.txt提取每个数据保存为stucode、name，和folder组成文件的完整地址。urlretrieve是python的下载命令，第一个参数是url，第二个参数是本地保存的文件名。try语句用于跳过错误的url，比如该地址不存在照片。完整代码如下，*代表隐去该隐去的部分，all.txt也不在此提供了。
<pre class="brush:python; ">#pyhton 2.x
from urllib import urlretrieve
folder='http://*.cn/Imgxs/'
input_file=open(“all.txt”,“r”)
for line in input_file:
    line=line.strip()
    stucode=line[*]
    name=line[*]
    try:
        urlretrieve(folder+stucode+".jpg",stucode+name+".jpg")
    except:
        pass
input_file.close()
</pre>
无聊到把学校里一万多学生照下载下来的不知道有多少，不过我得感谢公开学生数据的渣渣教务系统，我是不懂入侵技术的。PS：证件照果然大家都长得不怎么样了。