---
title: git rebase实践
date: 2015-10-28 19:54:15
tags: 
 - git
---

## 准备知识
### 分支
同一个分支有三种表现形式
1. 远程分支
2. 远程引用分支 origin/master
3. 本地分支	master


fetch会更新远程引用分支，本地分支merge远程引用分支之后才会更新本地分支。

### pull
pull实际执行了`git fetch`和`git merge FETCH_HEAD`两条指令


### rebase的意义
rebase的作用是重写提交记录。
- rebase优点：
	1. 合并commit（与前分支rebase）
	2. 避免merge commit（与master rebase）
	3. 重写提交记录

- rebase缺点：
1. 操作不慎会丢失commit
2. 影响不到的冲突（由merge commit引起）

在操作不熟练时，可以先备份分支`checkout -b bak_xxx`。

merge+revert的方式可以保证代码的安全性，但提交记录混乱。rebase+reset的方式能够保证提交记录的清晰。


## 新建分支
### 创建新的分支
开始需求时，需要建立一个新分支，并把它推送到远程仓库。新建分支的方法多种多样，唯一的原则是从master新建分支。
1. 更新master
	1. `git checkout master`
	2. `git pull`
2. 从master创建feature分支：
	`git checkout master -b test` 或 `git checkout origin/master -b`
3. 推送分支当远程
	 `git push -u origin test:test` 本地分支名在前
	-u是--set-upstream-to 的简写，设置追踪分支。

### 拉取分支
其他人在此分支协作时的获取。
	`git checkout --track origin/test`


## 更新分支

### 从master更新
1. **方案1**
	1. `git checkout master`
	2. `git pull`
	3. `git checkout test`
	4. `git rebase master`
2. **方案2（推荐）**
	`git pull --rebase origin master`
	**注意：**此种方法不会更新本地master分支

### 从test更新
`git pull --rebase` 
**注意**：需要先设置跟踪分支

## 完成后合并到master
在test分支开发**完成**后，需要通过rebase合并commit，此时rebase的目标不再是分支，而是在test分支中的commit。

### 准备工作
1. 从master更新test分支，方法见上文
2. 寻找rebase点，使用`git log`查看test分支中最后一个commit，如
66fc050b 
3. `rebase -i 66fc050b`开始rebase


### 交互式rebase
在执行rebase -i命令会打开sublime编辑器
```
pick 66fc050 Test:git pull --rebase origin master
pick a8b6d2f 2
pick c2c1903 xsas

# Rebase 749e319..c2c1903 onto 749e319 (3 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

内容以`动作 commitID commit信息`的形式展现，66fc050、a8b6d2f、c2c1903就是三次提交，如果需要合并三次提交，修改成
```
pick 66fc050 Test:git pull --rebase origin master
f a8b6d2f 2
f c2c1903 xsas
```
然后保存文件并退出sublime。

如果rebase过程中遇到冲突，解决完冲突后执行`git rebase --continue`，需要注意的是，即使强制关闭控制台，依然会处于rebase状态，需要执行`git rebase --abort`取消rebase，版本库会会滚刀rebase之前的状态。或者执行`git rebase --edit-todo`重新打开rebase文件。

如果产生冲突，rebase会逐个释放冲突，处理完冲突后，执行`git rebase --continue`继续。

### 推送到远程仓库
test分支经过rebase后，显然已经和远程分支冲突了，
需要强制提交:
`git push -f`
或者先删除远程分支:
1. `git push origin --delete test`
2. `git push origin test:test`


### 管理员合并分支
最终由管理员把test分支合并到master，需要注意的是合并前需要把test分支从master分支rebase更新，避免merge commit。

## 其他

### 使用Sublime Text作为外部文本编辑器

对git进行设置通常使用git config命令或者直接编辑.config文件

`git config --global core.editor "'C:/Program Files/Sublime Text 2/sublime_text.exe' -w"`
或
```
[core]
	editor = 'C:\\Program Files\\Sublime Text 2\\sublime_text.exe' -w
```

### rebase取代merge
`git config --global branch.autosetuprebase always`
或
```
[branch]
	autosetuprebase = always
```

### reset在合并提交中的应用
`git reset commitId` commitId 的位置同rebase，reset会撤销提交，一切回滚到`git add`和`git commit`之前的状态，所以我们可以把所有修改一次commit，达到合并commit的作用。

需要注意的是，reset后，提交人信息和提交信息(commit msg)都会丢失。


### git_hooks_go
通过git hooks的方式为提交信息添加用户名、分支名等信息，方便rebase。
运行git_hooks_go目录下的install.py安装。

## 总结
1. 从master创建分支
2. 使用rebase更新
3. 使用rebase -i 合并commit
4. 管理员合并分支

更新git：
http://git-scm.com/download/

git文档：
1. http://git-scm.com/book/zh/v2
2. https://www.atlassian.com/git/tutorials/

