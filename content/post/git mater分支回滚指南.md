---
title: git master分支回滚指南
slug: How-to-revert-on-git-master-branch
tags: 
 - git
date: 2016-08-24 23:44:27
---

## 1. 概述
### 1.1 master分支回滚的特殊性

1. 多人协作，修改提交历史的方式易造成冲突
2. 希望所有操作的记录都是保存的，避免丢失代码
3. 希望没有master权限的人也能按照正常合并流程进行回滚

所以，以下的方法都是不会修改提交历史了，操作可以使用正常合并流程。
<!-- more --> 
### 1.2 场景描述

1. 回滚某个或某些commit
2. 回滚分支合并（merge commit）
3. 回滚**到**某个提交

## 2. 回滚方法

### 2.1 回滚某个或某些commit

```
git revert commit
```
如：`git revet 08096c7`

### 2.2 回滚分支合并（merge commit）
```
git revert -m parent-number commit
```

如`git revert -m 1 08096c7c71429ac044a90bbd43f67aef13a91102`

一个典型的merge commit：
```
commit a4aebedf433805eb15767f6f1905f3c236ce48bd
Merge: 6ee7688 d593ca7
Author: XXX <XXX@qq.com>
Date:   Mon Aug 22 15:40:16 2016 +0800

    XXXXX
```
其中merge字段中的6ee7688 d593ca7就是两个parent commit，
```
        a4aebedf4
           /\
   6ee7688| |d593ca7
  (master)| | (feature)
          | |
          | |
```
-m 参数指定要回滚到parent commit，值为1或2，即第1个还是第2个。如上图，我们想回滚**掉**feature分之，回**到**合并之前的master，则选择1.通常使用Gitlab的merge request合并的分之都选1.

### 2.3 回滚到某个提交

如要把master回滚到tag v100:
```
git reset --hard v100
git reset --soft origin/master
git commit -am "revert to v100"
```
回滚完毕后，可以使用`git diff head v100`比较是否还有不同，如果有不同，则说明操作错误。


