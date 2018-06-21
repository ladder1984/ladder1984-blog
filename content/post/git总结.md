---
title: git总结
date: 2017-08-24 21:06:07
tags:
  - git
  - 第一站
---

## git中的位置

### 仓库
![仓库](http://wx1.sinaimg.cn/large/663aa05agy1fidv3clywuj20ih07wdgn.jpg)


- **工作目录**：对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

- **暂存区**：是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作`‘索引’'，不过一般说法还是叫暂存区域。

- **Git仓库目录**：是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

	- **远程仓库**： 在网络上的仓库，和远程仓库交互的命令只有`git fetch`和`git push`，注意`git pull`实际上执行了fetch后自动执行了merge(默认)或rebase。 
	- **本地仓库**：位于本地的仓库（.git\refs目录），包含本地的数据，有本地分支和追踪分支。

基本的 Git 工作流程如下：

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

- HEAD: 一个特殊指针，只想当前所在的本地分支，可以理解为当前分支的别名
- head: 指向当前的commit


参见: <https://ndpsoftware.com/git-cheatsheet.html>
<!-- more --> 
### 分支
- 远程分支：如**origin master**，位于远程仓库
- 追踪分支：如**origin/master**，位于本地仓库（.git\refs\remotes），即git fetch到的位置。
- 本地分支:如**master**，位于本地仓库(.git\refs\heads)，执行merge、rebase、cherry后更新的分支

### 数据流
![数据流](http://wx3.sinaimg.cn/large/663aa05agy1fidv3axzisj20sg0ifacj.jpg)


## 基本命令

参见：[Appendix C: Git 命令](https://git-scm.com/book/zh/v2/Appendix-C%3A-Git-%E5%91%BD%E4%BB%A4-%E8%AE%BE%E7%BD%AE%E4%B8%8E%E9%85%8D%E7%BD%AE)


### 快照基础
- `git add`命令将内容从工作目录添加到暂存区（或称为索引（index）区），以备下次提交
- `git status` 命令将为你展示工作区及暂存区域中不同状态的文件
- `git diff` 当需要查看任意两棵树的差异时你可以使用 git diff 命令。
- `git commit` 命令将所有通过 git add 暂存的文件内容在数据库中创建一个持久的快照，然后将当前分支上的分支指针移到其之上。
- `git reset` 命令主要用来根据你传递给动作的参数来执行撤销操作。理解为**重置到**。**会修改commit记录，谨慎操作**
	- `git reset file.txt`： 取消暂存的文件
	- `git reset e43a6fd3e94888d76779ad79fb568ed180e5fcdf`：重置到某个点
		- `--soft`（默认）：把文件从已commit的分支里扔到暂存区
		- `--mixed`：把文件从已commit的分支里扔到暂存区（soft的操作），再把暂存区里新添加的文件扔出暂存区
		- `--hard`:把文件从已commit的分支里扔到暂存区（soft的操作），再把所有东西**扔出硬盘**

### 分支与合并
- `git branch` 命令实际上是某种程度上的分支管理工具。 它可以列出你所有的分支、创建新分支、删除分支及重命名分支。
- `git checkout` 命令用来切换分支，或者检出内容到工作目录。
	- `git checkout`：检出某个内容，如本地分支（切换分支）、追踪分支origin/master、某个commit 85bdab6b15、relog中的某个位置71a0f628d
	- `git checkout -b/-B`：checkout为本地分支时切换分支，其他参数则检出内容后处在游离态('detached HEAD' state),使用-b参数创建分支,-B参数强制创建分支
	- `git chekcout -- a.txt`:此命令用于丢弃工作目录的修改
- `git merge `工具用来合并一个或者多个分支到你已经检出的分支中。 然后它将当前分支指针移动到合并结果上。
	- 在merge中途，可以使用`git merge --abort`退出merge并取消修改
- `git log` 命令用来展示一个项目的可达历史记录，从最近的提交快照起
- `git stash` git stash 命令用来临时地保存一些还没有提交的工作，以便在分支上不需要提交未完成工作就可以清理工作目录。
 

### 项目分享与更新

- `git fetch` 命令与一个远程的仓库交互，并且将远程仓库中有但是在当前仓库的没有的所有信息拉取下来然后存储在你本地数据库中
- `git pull` 实际上就是fetch+merge（默认）或者fetch+rebase
- `git push` 命令用来与另一个仓库通信，计算你本地数据库与远程仓库的差异，然后将差异推送到另一个仓库中。 

### 检查与比较

- `gut show`命令可以以一种简单的人类可读的方式来显示一个 Git 对象。 


### 补丁
- `git cherry-pick `命令用来获得在单个提交中引入的变更，然后尝试将作为一个新的提交引入到你当前分支上。
- `git rebase`命令基本是是一个自动化的 cherry-pick 命令。 它计算出一系列的提交，然后再以它们在其他地方以同样的顺序一个一个的 cherry-picks 出它们。**会修改commit记录，谨慎操作**
	- 在rebase中途，可以使用`git rebase --abort`退出并取消修改
- `git revert` 命令本质上就是一个逆向的 git cherry-pick 操作。 它将你提交中的变更的以完全相反的方式的应用到一个新创建的提交中，本质上就是撤销或者倒转。

## 使用场景

### 拣选工作流

- 场景： 提取某个commit到当前分支。

- 命令： `git cherry-pick e43a6fd3e94888d76779ad79fb568ed180e5fcdf`

- 注意事项： 如果要提取多个commit，先提取前面的。

- 参考：[变基与拣选工作流](https://git-scm.com/book/zh/v2/%E5%88%86%E5%B8%83%E5%BC%8F-Git-%E7%BB%B4%E6%8A%A4%E9%A1%B9%E7%9B%AE#_rebase_cherry_pick)


### 比较两个分支的commit

 - 命令:
	 - `git rev-list ^master HEAD` ： 在当前分支但不在master分支的commit
	 - `git rev-list ^master HEAD|tail -1` : 寻找当前分支的开发起点
	 - `git rev-list A ^B --conut` 查找不同commit数量

- 注意事项：
	- 可以增加`--pretty`参数美化日志结果，如`--pretty=oneline`

### 压缩commit

- 场景：为了让log变得清晰，可以把多个commit合并成一个。但是如果commit较多，又或者经历过merg、cherry等命令，rebase将变得很麻烦。可以使用reset。

- 命令：
```
git rev-list ^origin/master HEAD|tail -1 # 得到commit xxx
git reset --soft xxx~1
git commit -m "获得干净的提交"
```

- 注意事项：为了防止误操作，可以使用`git branch bak1`备份分支，执行完上述命令后使用`git diff HEAD　bak1`进行比较差异，应得到没有差异的结果。


### 回滚分支合并

- 场景： 一次分支合并时候，希望进行回滚

- 命令: `git revert -m parent-number commit`,如`git revert -m 1 08096c7c71429ac044a90bbd43f67aef13a91102`


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
-m 参数指定要回滚到parent commit，值为1或2，即第1个还是第2个。如上图，我们想回滚掉feature分之，回到合并之前的master，则选择1。


- 注意事项：通常使用Gitlab的merge request合并的分之都选1，当不确定时可以使用`git show xxxx`查看一下两个父节点分别是什么内容。


### 回滚到某个提交

- 场景：如果上线失败，想回滚到上次上线的版本，上线到昨天的某个版本等待。得到版本号后就可以回滚**到**那个版本。
- 命令：
```
git reset --hard a4aebedf433805eb15767f6f1905f3c236ce48bd
git reset --soft origin/master
git commit -am "revert to a4aebedf433805eb15767f6f1905f3c236ce48bd"
```

### 撤销本地操作

- 场景： 如删除错分支，reset命令错commit时，可以使用relog查看操作日志，并撤销操作。
- 命令:
```
git reflog # 查找需要恢复到的id
git reset --head XXX
```
- 注意事项：reflog只记录HEAD的值，即每一次提交或改变分支，所以没有commit的内容无法用reflog恢复。
- 参考：[维护与数据恢复](https://git-scm.com/book/zh/v2/Git-%E5%86%85%E9%83%A8%E5%8E%9F%E7%90%86-%E7%BB%B4%E6%8A%A4%E4%B8%8E%E6%95%B0%E6%8D%AE%E6%81%A2%E5%A4%8D)



### 把本地重置到远程分支的版本

- 场景: 如获取最新的master或某个分支覆盖本地代码
- 命令：

把当前的分支变为远程某分支
```
git fetch 
git reset --hard origin/master
```

或

检出某个远程分支强制到某本地分支

```
git fetch
git checkout origin/master -B master
```



### 二分查找问题commit

- 场景：比如一次上线有多个pr带来的代码，本地分支commit几次后发现有问题，用二分查找快速的定位问题
- 命令：
	- 暴力checkout法：手动git checkout  HEAD~1，HEAD~10,HEAD~5这样手动切换到某个commit
	- bisect命令：
		```
		git bisect start #标记开始
		git bisect bad # 标记当前有问题 
		git bisect good v1.0 标记一个没问题的起点
		```
		而后git会自动开始二分查找，用户使用git bisect bad或者git bisect good进行标记当前commit的情况，直到能确定因为问题的commit。


## 总结

git有工作目录、暂存区、仓库三个位置，里面有所有的git追踪的内容。使用`git checkout`能够**检出（获得）**分支中的内容，`git reset`能够**重置**到某个点。`git diff`比较文件，`git rev-list`比较commit。`git log`查看commit日志，`git reflog`查看本地`HEAD`移动记录。

## 学习资料

- 《Pro Git》 <https://git-scm.com/book/zh/v2>
	- [Appendix C: Git 命令](https://git-scm.com/book/zh/v2/Appendix-C%3A-Git-%E5%91%BD%E4%BB%A4-%E8%AE%BE%E7%BD%AE%E4%B8%8E%E9%85%8D%E7%BD%AE)
- Reference <https://git-scm.com/docs>
- User manual: <https://git-scm.com/docs/user-manual.html>
- https://www.atlassian.com/git
- git everyday <https://git-scm.com/docs/everyday>
- <http://ladder1984.github.io/tags/git/>