---
title: emoji、Python、MySQL utf8mb4与UTF
date: 2016-03-20 21:16:31
tags:
- MySQL
- Python
---



编码、字符集是很大的内容，本文简单聊聊Python、MySQL中处理emoji这种特殊字符的问题。本文描述的Python是2.x版本，3.x情况可能有所不同

## 问题描述

如果向MySQL utf8的表中保存一个emoji字符，必然出错，因为emoji是4字节字符，而MySQL utf8只支持3字节字符。MySQL 5.5开始加入utf8mb4支持4字节字符，但是又带来了显示的问题，毕竟PC上无法正常显示emoji的，而且能显示的系统又各家有各家的样子。所以要么把emoji过滤掉、要么在显示时候做特殊处理。
<!-- more --> 
## 浅谈UTF

最常用的字符都在BMP（Basic Multilingual Plane, 基本多语言平面），范围是**U+0000至U+FFFF**。只有这些码位在UCS-2可用，也是MySQL UTF8可以存储的范围，其它字符存储在辅助平面。

其中从**U+D800到U+DFFF**之间的码位区段是永久保留不映射到Unicode字符。UTF-16就利用保留下来的0xD800-0xDFFF区段的码位来对辅助平面的字符的码位进行编码。Unicode规定这个范围不对应字符，但在使用UCS-2时会被用于映射某些字符。

## MySQL对4字节字符的支持

MySQL 5.5开始有了utf8mb4字符集，兼容utf8并且可以存储4字节字符，详见https://dev.mysql.com/doc/refman/5.5/en/charset-unicode-utf8mb4.html

## Python中的4字节字符

最开始的方案是直接识别4字节字符并直接过滤掉，3字节的范围是`\u0000-\uFFFF`，如果想直接判断一个字符是否在这个范围，**可能**会惊奇的发现一个字符被拆分成了两个字符，该字符的长度也是**2**。比如`\U0001f604`(笑脸表情，参见<https://codepoints.net/U+1F604>)会被拆分成`\ud83d`和`\ude04`，`\U0001f604`展示了以一当二的能力。至于说可能，是因为这是由Python解释器编译时候编译选项是--enable-unicode=ucs4还是--enable-unicode=ucs2决定的，后者就是前文描述的情况。查看当前Python情况可以输出sys.maxunicode，得到65535就是ucs2，详见Python文档<https://docs.python.org/2/library/sys.html#sys.maxunicode> 。ucs4下，4字节字符范围在`\U00010000-\U0010ffff`，usc2下拆分出的两个字符会在\uD800-\uDFFF范围。所以如果要过滤emoji或者其他特殊字符，可以根据此范围替换。

## 代码示例

``` 
def filter_invalid_str(text):
	"""
	过滤非BMP字符
	"""
	try:
		# UCS-4
		highpoints = re.compile(u'[\U00010000-\U0010ffff]')
	except re.error:
		# UCS-2
		highpoints = re.compile(u'[\uD800-\uDBFF][\uDC00-\uDFFF]')

	return highpoints.sub(u'_', text)

```



参考：

- https://en.wikipedia.org/wiki/UTF-16
- <https://docs.python.org/2/library/sys.html#sys.maxunicode>
- https://dev.mysql.com/doc/refman/5.5/en/charset-unicode-utf8mb4.html
- http://stackoverflow.com/questions/26568722/remove-unicode-emoji-using-re-in-python

