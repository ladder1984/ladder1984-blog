---
title: 约瑟夫环的数学解法
tags:
  - C
id: 255
categories:
  - 编程笔记
date: 2012-12-22 20:57:40
---

<pre>int Josephus(int n, int m)//总人数为n，从第一人开始数到m的退出，
{
    int i, r = 0;
    for (i = 2; i &lt;= n; i++)
        r = (r + m) % i;
    return r+1;//最后一个人的初始位置
}
</pre>