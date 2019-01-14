---
title: "Git原理之从创建项目到commit"
date: 2019-01-14T15:12:36+08:00
tags: 
 - git
---


本文试图用**底层命令**来实现**高层命令**add和commit操作来介绍git底层原理。


## 底层命令与高层命令
- 高层命令（porcelain）：诸如checkout、branch、remote 等大约 30 个完整的、用户友好的命令，它们是我们日常操作git的命令。
- 底层命令（plumbing）：用于完成底层工作的命令。 这些命令被设计成能以 UNIX 命令行的风格连接在一起，抑或藉由脚本调用，来完成工作。

首先，我们分别使用高层命令与底层命令完成从创建仓库到添加文件，到commit的过程：

### 高层命令

```
mkdir gittest
cd gittest
git init
echo aaa>a.txt
git add a.txt
git commit -m "init"
```
我们看下add和commit命令在git官方的定义：

- `git add` 命令将内容从**工作目录**添加到**暂存区**（或称为索引（index）区），以备下次提交。
- `git commit` 命令将所有通过 git add 暂存的文件内容在**数据库**中创建一个持久的**快照**，然后将当前分支上的分支**指针**移到其之上。

从中，我们可以提取出关键词：工作目录、暂存区、数据库、快照、指针。

### 底层命令

```
mkdir gittest
cd gittest
git init
echo aaa>a.txt
git hash-object -w a.txt
git update-index --add --cacheinfo 100644 72943a16fb2c8f38f9dde202b7a70ccc19c52f34 a.txt
git write-tree
git commit-tree 37057b2e8a9041ef88b805a5b7c4e0e668a03be4 -m "init"
git update-ref refs/heads/master 48d2ea321a0f1c35b2799ad853c93115e8ab98d7
```


## 仓库文件(.git目录)内容
当在一个新目录或已有目录执行`git init` 时，Git 会创建一个 .git 目录。 这个目录包含了几乎所有 Git 存储和操作的对象。

.git目录默认内容如下：
>
HEAD
config*
description
hooks/ 
info/
objects/
refs/


- description文件: 仅供 GitWeb 程序使用
- config文件: 包含项目特有的配置选项
- info目录: 包含一个全局性排除（global exclude）文件，用以放置那些不希望被记录在 .gitignore 文件中的忽略模式（ignored patterns）
- hooks目录: 目录包含客户端或服务端的钩子脚本（hook scripts）
- **HEAD文件**：指示目前被检出的分支。
- （尚待创建的）**index文件**：保存暂存区的信息。
- **objects目录**：存储所有数据内容。所谓的数据库即是指这里。
- **refs目录**：存储指向数据（分支的）提交对象的指针。
    - /refs/heads:存储本地分支
    - /refs/remotes:存储追踪分支
    - /refs/tags/: 存储tags

所以，本质上：
- 分支：一个指向某一系列提交之首的指针或引用
- 当前位置：git status或者git branch命令看到的当前位置就是HEAD文件的内容
- 切换分支：就是修改HEAD文件的内容，设置新的引用
- 新建分支：就是在refs/heads新建一个文件，内容为原分支的最新commit对象
- fetch和push：实际上就是和远程server交换objects目录里的内容
- add：就是把文件放入index文件
- commit：就是把index文件的内容生成一个commit对象



## git对象

Git 是一个内容寻址文件系统。 这意味着，Git 的核心部分是一个简单的键值对数据库（key-value data store）。

每个对象(object) 包括三个部分：类型，大小和内容。大小就是指内容的大小，内容取决于对象的类型。

### Git对象类型
- 数据对象（blob）：一个“blob”通常用来存储文件的内容。一个“blob”对象就是一块二进制数据，它没有指向任何东西或有任何其它属性，甚至没有文件名。
- 树对象（tree）：有点像一个目录，它管理一些“tree”或是 “blob”（就像文件和子目录）。一般用来表示内容之间的目录层次关系。
- 提交对象（commit）：一个“commit”只指向一个"tree"，它用来标记项目某一个特定时间点的状态。它包括一些关于时间点的元数据，如时间戳、最近一次提交的作者、指向上次提交（commits）的指针等等。
- 标签对象（tag）：标记某一个提交(commit) 

可以使用`git cat-file -p <sha1值>`命令查看对象内容。

![git对象图示](https://ws3.sinaimg.cn/large/006tNc79ly1fz63htt3o2j30l20h640z.jpg)

### git对象存储

详见：<https://git-scm.com/book/zh/v2/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-Git-%E5%AF%B9%E8%B1%A1>


前文曾提及，在存储内容时，会有个头部信息一并被保存。 让我们略花些时间来看看 Git 是如何存储其对象的。 通过在 Ruby 脚本语言中交互式地演示，你将看到一个数据对象——本例中是字符串“what is up, doc?”——是如何被存储的。

可以通过 irb 命令启动 Ruby 的交互模式：

```
$ irb
>> content = "what is up, doc?"
=> "what is up, doc?"
```

Git 以对象类型作为开头来构造一个头部信息，本例中是一个“blob”字符串。 接着 Git 会添加一个空格，随后是数据内容的长度，最后是一个空字节（null byte）：

```
>> header = "blob #{content.length}\0"
=> "blob 16\u0000"
```

Git 会将上述头部信息和原始数据拼接起来，并计算出这条新内容的 SHA-1 校验和。 在 Ruby 中可以这样计算 SHA-1 值——先通过 require 命令导入 SHA-1 digest 库，然后对目标字符串调用 Digest::SHA1.hexdigest()：
```
>> store = header + content
=> "blob 16\u0000what is up, doc?"
>> require 'digest/sha1'
=> true
>> sha1 = Digest::SHA1.hexdigest(store)
=> "bd9dbf5aae1a3862dd1526723246b20206e5fc37"
```

Git 会通过 zlib 压缩这条新内容。在 Ruby 中可以借助 zlib 库做到这一点。 先导入相应的库，然后对目标内容调用 Zlib::Deflate.deflate()：

```
>> require 'zlib'
=> true
>> zlib_content = Zlib::Deflate.deflate(store)
=> "x\x9CK\xCA\xC9OR04c(\xCFH,Q\xC8,V(-\xD0QH\xC9O\xB6\a\x00_\x1C\a\x9D"
```

最后，需要将这条经由 zlib 压缩的内容写入磁盘上的某个对象。 要先确定待写入对象的路径（SHA-1 值的前两个字符作为子目录名称，后 38 个字符则作为子目录内文件的名称）。 如果该子目录不存在，可以通过 Ruby 中的 FileUtils.mkdir_p() 函数来创建它。 接着，通过 File.open() 打开这个文件。最后，对上一步中得到的文件句柄调用 write() 函数，以向目标文件写入之前那条 zlib 压缩过的内容：

```
>> path = '.git/objects/' + sha1[0,2] + '/' + sha1[2,38]
=> ".git/objects/bd/9dbf5aae1a3862dd1526723246b20206e5fc37"
>> require 'fileutils'
=> true
>> FileUtils.mkdir_p(File.dirname(path))
=> ".git/objects/bd"
>> File.open(path, 'w') { |f| f.write zlib_content }
=> 32
```

就是这样——你已创建了一个有效的 Git 数据对象。 所有的 Git 对象均以这种方式存储，区别仅在于类型标识——另两种对象类型的头部信息以字符串“commit”或“tree”开头，而不是“blob”。 另外，虽然数据对象的内容几乎可以是任何东西，但提交对象和树对象的内容却有各自固定的格式。


#### 快照
每次提交git保存所有文件。

#### 快照存储
Git 对当时的全部文件制作一个快照并保存这个快照的索引。 为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。


即：对全部文件做快照（hash-object）->只保存修改的文件，为修改的文件通过链接的方式识别（commit的parent）->通过打包的方式，只保存文件不同版本之间的差异内容来压缩空间（可通过git gc触发）。



### 底层命令解释
我们回顾开头用底层命令完成从创建仓库到添加文件的例子

- git-hash-object - 计算对象ID并可选择从文件创建一个blob。-w参数将对象写入对象数据库。
- git-update-index - 将工作树中的文件内容注册到索引
- git-write-tree - 从当前索引创建一个树形对象
- git-commit-tree - 创建一个新的提交（commit）对象，即生成快照。
- git-update-ref - 安全地更新存储在ref中的对象名称

流程图：

![](https://ws2.sinaimg.cn/large/006tNc79ly1fz6hxj51kmj309x0fc0ta.jpg)




## 参考资料
- [Git 内部原理](https://git-scm.com/book/zh/v2/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E5%BA%95%E5%B1%82%E5%91%BD%E4%BB%A4%E5%92%8C%E9%AB%98%E5%B1%82%E5%91%BD%E4%BB%A4)
- [GIT对象模型](http://gitbook.liuhui998.com/1_2.html)
- [Plumbing Commands](https://cloud.tencent.com/developer/chapter/12781)

