---
title: git tag应用
date: 2017-02-28 14:59:13
tags: 
 - git
---


## 1. Tag概述

>像其他版本控制系统（VCS）一样，Git 可以给历史中的某一个提交打上标签，以示重要。 比较有代表性的是人们会使用这个功能来标记发布结点（v1.0 等等）。

参考：  

- <https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE>
- `git help tag`


### 1.1 命令

tag的命令与分支相似。

- 列出标签: `git tag`
- 创建标签: `git tag tag123`
	- 附注标签: `git tag -a v1.4 -m 'my version 1.4'`
	- 打在制定版本: `git tag -a v1.2 9fceb02` 
- 查看标签内容: `git show v1.4` 或 `git log v1.4`
	- 按照版本排序：`git tag --sort="version:refname`
	- 搜索： `git tag -l <pattern>`，如`git tag -l "v*"`
- push标签：git push不会推送本地标签
	- push特定标签： `git push origin [tagname]`
	- push所有标签： `git push origin --tags`
- 检出标签: `git checkout -b version2 v2.0.0`
- 删除标签：
	- 删除本地标签: `git tag -d v0.1`      批量：`git tag -l "vTEST*"|xargs git tag -d`
	- 删除远程标签: `git push origin --delete v0.1`   批量：`git tag -l "vTEST*"|xargs git push origin --delete`

<!-- more -->
## 2. 配置

### 2.1 jenkins

增加构建后操作的Publisher：

![](/images/git_tag1.png)

- v168形式的标签表示每次 jenkins上线构建的版本,使用`git tag --sort="version:refname" -l "v*"`获得按照版本排序的tag。


### 2.2 Gitlab

需要添加jenkins用户为develop权限。


## 应用

1. 比较
- 文件比较：`git diff v98 master`,此命令比较v98和master文件不同。常用于比较两个版本是否一致。
- commit：`git diff ^v98 master`，此命令比较不在v98上但是在master上的commit有哪些。常用于比较两个版本不同的commit
	- 取得远程master上没有上线的commit：
	
	```
	git fetch && git log `git tag --sort="version:refname" -l "v*"|tail -1`..origin/master
	```
	或者
	```
	git fetch && git rev-list ^`git tag --sort="version:refname" -l "v*"|tail -1` origin/master --pretty=oneline
	```
	
**注**：`--sort="version:refname" -l "v*"`要求v开头的tag都是jenkins打出来的，且要求git 2.1及以上版本
    
2. 回滚

todo

