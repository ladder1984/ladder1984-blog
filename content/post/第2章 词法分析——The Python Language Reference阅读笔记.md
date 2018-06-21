---
title: 第2章 词法分析——The Python Language Reference阅读笔记
tags:
  - Python
  - The Python Language Reference阅读笔记
id: 1376
categories:
  - 编程笔记
date: 2015-04-15 23:08:51
---

# 第二章 词法分析（Lexical analysis）

一个 Python 程序由 **解析器**(parser) 读入, 输入解析器的是由 **词法分析器**(lexical analyzer) 生成的 **语言符号**(token) 流. 本章讨论词法分析器是如何把文件割成若干语言符号的.

Python使用7位ASCII字符作为程序文本（注意：Python3使用Unicode）。

## 2.1.行结构

Python程序由逻辑行组成。

### 2.1.1.逻辑行

逻辑行的结尾以“换行”（NEWLINE）为标志，语句不能出现在多个逻辑行，除非语法支持（例如符合语句，复合语句在第七章详解）。逻辑行由一个或多个显/隐式连接的物理行。

文档中没有明确给出“NEWLINE”的定义，我的理解是逻辑行形式上等同于组成它的物理行，所以物理行的结束也就是逻辑行的结束。

### 2.1.2.物理行

物理行以end-of-line（EOL）结束，这个EOL在每个平台不同。

> 以ASCII为基础的或相容的字元集使用分别LF（Line feed，0Ah）或CR（Carriage Return，0Dh）或CR+LF；下面列出各系统换行字元编码的列表
> 
>   **LF**：在Unix或Unix相容系统（GNU/Linux，AIX，Xenix，Mac OS X，...）、BeOS、Amiga、RISC OS
>   **CR+LF**：MS-DOS、微软视窗操作系统（Microsoft Windows）、大部分非Unix的系统
>   **CR**：Apple II家族，Mac OS至版本9
>   [换行 - 维基百科](http://zh.wikipedia.org/zh-cn/%E6%8F%9B%E8%A1%8C)

物理行等同于视觉上看到的行，这个是考验编辑器的事情。

### 2.1.3.注释

**comment**是以#开头的物理行，会被解释器忽略。文档中并没有提到另一种注释，三个引号(""")包围的东西，**文档注释（doc string）**。如：

<pre>
def foo():
    """文档注释"""
    return None 
print(foo.__doc__)  
help(foo)
</pre>

### 2.1.4.编码声明

声明编码，源文件中第一行或者第二行匹配正则表达式`coding[=:]\s*([-\w.]+)`的注释，通常写成 # -_- coding: utf-8 -_- 或者coding=utf8。当然声明的编码Python得认识，通常声明成utf-8就好。

**编码设置会用于整个词法分析过程, 包括字符串字面值, 注释和标识符。**

另外，Python还能识别UTF-8 byte-order mark ('\xef\xbb\xbf')，也就是微软的恶趣味BOM。通常自己编写的文件最好避免出现BOM，读取文本文件时候也要注意BOM。

### 2.1.5.显式行连接

行连接是为了把多个物理行连接成一个逻辑行，显式即是通过反斜杠（\,backslashe）。要注意的反斜杠后面不能有注释。

### 2.1.6.隐式行连接

圆括号()、方括号[]、花括号{}中多个物理行的内容不需要显式连接，并且可以添加注释。例如：

<pre>
month_names = ['Januari', 'Februari', 'Maart',  # These are the
   'April',   'Mei',  'Juni',   # Dutch names
   'Juli','Augustus', 'September',  # for the months
   'Oktober', 'November', 'December']   # of the year
</pre>

### 2.1.7.空行

简单来说，空的行在解析时会被忽略。所谓空行就是只有空格、制表符、进纸符和注释的逻辑行。在交互时，空行根据具体实现而定。

### 2.1.8.缩进

缩进用来区分语句是Python最明显的特色，缩进可以使用制表符（tab）或者空格，在解析时候tab会被先转换成空格，推荐使用四个空格的方式。

另外，用**分号(;)**将多个语句写在同一行上，语句之间用分号隔开，而这些语句也不能在这行开始一个新的代码块。例如：`a=1;a=2`。不过，不推荐这样做，也基本没见过这样写的Python代码。

Python用栈来控制缩进，详情看官方文档：[Indentation](https://docs.python.org/2/reference/lexical_analysis.html#indentation "Indentation")

### 2.1.9.语言符号间的空白符

除了在行首、字符串中这样的特殊位置，空白符的作用就是把标记分号，如把变量**ab**分隔成**a b**两个变量。

## 2.2.其他记号

除了换行（NEWLINE）、缩进（INDENT）、DEDENT（啥？），语言记号包括：标识符、关键字、字面值、运算符 和 分隔符。空白符不是。

## 2.3.标识符和关键字

标识符（Identifier）也叫做名字，就是平时所说的变量名一类，词法定义如下：

> identifier ::=  (letter|"_") (letter | digit | "_")*
>   letter     ::=  lowercase | uppercase
>   lowercase  ::=  "a"..."z"
>   uppercase  ::=  "A"..."Z"
>   digit      ::=  "0"..."9"

标识符不限长度，大小写敏感，Python的标识符要求和其他语言基本一样。
另外，标识符命名推荐使用"_"分隔，而不是大小写的驼峰法，具体推荐命名法见PEP8。

### 2.3.1.关键字

Python 2的关键字就这么多：

> and       del       from      not       while
>   as        elif      global    or        with
>   assert    else      if        pass      yield
>   break     except    import    print
>   class     exec      in        raise
>   continue  finally   is        return
>   def       for       lambda    try

顺带附上Python 3.4的关键字：

> False      class      finally    is         return
>   None       continue   for        lambda     try
>   True       def        from       nonlocal   while
>   and        del        global     not        with
>   as         elif       if         or         yield
>   assert     else       import     pass
>   break      except     in         raise

### 2.3.2.保留的标识符类型

`_*`：不会在from module import &#42;时被导入
`__*__`:系统保留的名字，所以自己写的时候就不用这种名字格式
`__*`:类的私有名

## 2.4.字面值

字面值是某些内置类型的常量值的表示法

### 2.4.1.字符串字面值

词法定义,见[String literals](https://docs.python.org/2/reference/lexical_analysis.html#string-literals)：

<pre>
stringliteral   ::=  [stringprefix](shortstring | longstring)
stringprefix    ::=  "r" | "u" | "ur" | "R" | "U" | "UR" | "Ur" | "uR"
                     | "b" | "B" | "br" | "Br" | "bR" | "BR"
shortstring     ::=  "'" shortstringitem* "'" | '"' shortstringitem* '"'
longstring      ::=  "'''" longstringitem* "'''"
                     | '"""' longstringitem* '"""'
shortstringitem ::=  shortstringchar | escapeseq
longstringitem  ::=  longstringchar | escapeseq
shortstringchar ::=  <any source character except "\" or newline or the quote>
longstringchar  ::=  <any source character except "\">
escapeseq       ::=  "\" <any ASCII character>
</pre>

前缀：
r:表示原始字符串（raw strings），即不对字符串里的\进行特殊处理
u：表示Unicode字符串
b：b在Python 中没用，它是用于告诉2to3的自动转换程序，这个字符串转换成3后应该是个字节字面值。

### 2.4.2.字符串字面值的连接

简单来说，写在一起的字符串会自动连接到一起，如"hello" 'world'会自动连接成"helloworld"

注意这个功能是在语法层次上定义的, 但却是在编译时实现的。 在运行时连接字符串表达式必须使用” +” 运算符。在字面值连接时, 不同的引用字符可以混用, 甚至原始串与三重引用串也可以混合使用.

### 2.4.4数值型字面值

Python 2有四种字面值：普通整数（plain integers）、长整数（long integers）、浮点数（floating point numbers）、虚数（imaginary numbers）（Python 3中只有三种，只有整数）。复数用实数+虚数的形式表示。

词法定义：

<pre>
longinteger    ::=  integer ("l" | "L")
integer        ::=  decimalinteger | octinteger | hexinteger | bininteger
decimalinteger ::=  nonzerodigit digit* | "0"
octinteger     ::=  "0" ("o" | "O") octdigit+ | "0" octdigit+
hexinteger     ::=  "0" ("x" | "X") hexdigit+
bininteger     ::=  "0" ("b" | "B") bindigit+
nonzerodigit   ::=  "1"..."9"
octdigit       ::=  "0"..."7"
bindigit       ::=  "0" | "1"
hexdigit       ::=  digit | "a"..."f" | "A"..."F"
</pre>

要注意的是L要大写，来和数字1区分。Python 3对整数的统一更有好些，在Python 2中，数值溢出时候会自动转换成带L后缀的长整数。长整数的取值范围没有限制，由可用内存而定。

参考阅读：[sys.maxint](https://docs.python.org/2/library/sys.html#sys.maxint)

### 2.4.5.浮点型字面值

词法定义：

<pre>
floatnumber   ::=  pointfloat | exponentfloat
pointfloat    ::=  [intpart] fraction | intpart "."
exponentfloat ::=  (intpart | pointfloat) exponent
intpart       ::=  digit+
fraction      ::=  "." digit+
exponent      ::=  ("e" | "E") ["+" | "-"] digit+
</pre>

浮点数的取值范围依赖于实现。注意整数部分和指数部分都看作是十进制的. 例如, 077e010 是合法的, 它等价于 77e10,下面这些奇奇怪怪的形式也是可以的:
3.14    10\.    .001    1e100    3.14e-10    0e0

浮点型的取值范围和机器有关，可以使用sys.float_info查看相关信息，会得到这样的结果：

> sys.float_info(max=1.7976931348623157e+308, max_exp=1024, max_10_exp=308, min=2.2250738585072014e-308, min_exp=-1021, min_10_exp=-307, dig=15, mant_dig=53, epsilon=2.220446049250313e-16, radix=2, rounds=1)

参考阅读：[sys.float_info](https://docs.python.org/2/library/sys.html#sys.float_info)

### 2.4.6.虚数字面值

词法定义：
imagnumber ::=  (floatnumber | intpart) ("j" | "J")

虚数是实部为零的复数. 复数由一对有着相同取值范围的浮点数表示. 为了创建一个非零实部的复数, 可以对它增加一个浮点数, 例如, (3+4j). 下面是一些例子:
3.14j   10.j    10j     .001j   1e100j  3.14e-10j

## 2.5.运算符

运算符包括：

<pre>
+       -       *       **      /       //      %
<<      >     &       |       ^       ~
<             <=      >=      ==      !=      <>
</pre>

其中!=和&lt;>是一个意思，不建议使用后者，并且它在Python 3中已经取消。

## 2.6.分隔符

分隔符包括：

<pre>
(       )       [       ]       {       }
,       :       .       ;       @       =
+=      -=      *=      /=      //=     %=
&=      |=      ^=      >>=     <<=     **=
</pre>