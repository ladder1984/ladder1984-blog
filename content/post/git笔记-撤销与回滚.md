---
title: git笔记-撤销与回滚
tags:
- git
- git笔记
date: 2015-11-19 11:31:25
---


## 1.撤销

### 撤销文件修改
checkout以文件作为参数时会**修改**文件为指定版本的状态。
- 撤销未暂存的文件：`git checkout hello.py`，`git checkout .`为所有未暂存文件
- 撤销未commit的文件：`git checkout HEAD hello.py`
- 撤销已经commit的文件到指定版本，如e316e21：
```
git checkout e316e21 t1.txt
git add t1.txt
git commit -m '恢复t1.txt到e316e21'
```

**注**：以上撤销不会改写commit记录
<!-- more --> 
### 1.1 撤销commit（不改写commit记录）
revert命令用新的commit来撤销修改，不改写提交记录。
- revert指定commit:`git revert e316e21`
- revert多个commit:
下文的大写字母带走commitID
方案1： 撤销多个commit并生成多个revert commit:`git revert A B C`
方案2：撤销多个commit并生成一个revert commit：
```
git revert A B C --no-commit
git commit -m 'the commit message'
```
- revert指定范围：`git revert A..D`
 
### 1.2 撤销commit（改写commit记录）
使用reset或者rebase可以改写记录，但是要注意的是一旦改写，commit将会消失，并且会和远程分支、他人的本地分支冲突，push时需要用参数`-f`强制提交。所以，除非你清楚的知道重写的作用以及对他人带来的影响，不要轻易重写记录。

#### rebase

`git rebase -i commitID` commitID只要撤销的commit之前的一个或者某个commit
```
pick a5997c9 1
pick 88a1edf 2
pick dfa8d09 3
pick 57d2c26 4
pick ff3451d 5


# Rebase dfa8d09..ff3451d onto dfa8d09 (2 command(s))
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
参照git的提示，把要撤销的commit的action改为d，或者把正行删掉。

#### reset
如果这是要回滚到某个指定版本，如A，则直接`git reset A --hard`，如果要撤销的是提交记录中间的一个，则需要配合stash命令使用。
如有以下提交，要撤销其中的D提交，则：
```
A
B
C
D
E
```

```
git reset --soft D
git stash
git reset --hard E
git stash pop
git commit -am 撤销D
```

### 1.3 撤销add
当把某个文件add后，可以使用`git reset file_path`撤销add。

### 1.4 撤销最近一次commit
有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 --amend 选项的提交命令尝试重新提交：
```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
第二次提交将会覆盖之前的提交。

### 撤销（删除）未追踪的文件
`git clean -fdx`

## 2.版本回滚

版本回滚指把当前的分支切换到某个指定版本，具体而言是切换到某个commit或者tag，和上文提到的撤销类似，当从head开始撤销连续的若干commit就可达到回滚的效果。版本回滚是分支的角度，撤销是某个文件或者commit的角度。

如有以下提交，：
```
A
B
C
D
E
```
要回滚**到D**提交，有以下方法。

### 2.1 revert回滚（不改写commit记录）
revert回滚是唯一可以不修改commit记录的回滚方式，但是会产生一次revert commit。方法是上文讲到的**revert多个commit**。此时，要把D之后的提交全部revert。则：
```
git revert D..HEAD --no-commit
git commit -am 'revert to D'
```
当不清楚范围是否制定正确的时候，可以使用`git revert D..HEAD`，与上面命令的区别是没有--no-commit参数，也不用最好再commit，git会自动的revert每一个commit并每个commit生成一个revert commit，这样就能看到revert了哪些commit。

### 2.2 reset回滚（改写commit记录）
和撤销中的用法类似：
```
git reset --hard D
```

### 2.2 checkout回滚（改写提交记录）
checkout有三个目标，checkout文件是撤销中提到的改写文件，checkout分支是常用的切换分支，checkout commit就会切换到该commit，结合切换分支就可以达到回滚分支的作用。
```
git checkout D
```
此时git会给出提示:
>You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

简而言之，我们现在不在分支上，在'detached HEAD' state（游离状态），当前的代码在commit D发生时的状态，**此时的任何更改不会影响到任何分支**。我们可以不回滚代码就可让代码处于之前的某个提交，查看之前的代码或者做些实验。

从游离状态退出的方法是checkout到某个分支，你如`git checkout master`。而如果用创建分支的方法checkout -b <branch_name>就可以从当前创建分支，结合强制创建分支的参数-B，checkout回滚分支可以概括为：
```
git checkout -B test 6a26980
```
其中test是当前的分支名，6a26980是要回滚到的版本。

### 2.3 本地分支回滚到远程分支
当本地分支被改的乱七八糟时，你可能会想到重新拉取远程分支，一切重来。比较容易理解的方式可能是：
```
git checkout -B master 随便切换个别的分支
git branch -D test 把糟糕的本地分支删掉
git fetch 更新下远程引用分支
git checkout test 重新checkout test分支
```
这样子非常繁琐。有两个便捷的命令：
`git checkout origin/test -B test`或者`git reset origin/test --hard`,当然，执行前别忘了`git fetch`更新。

## 3.reflog
reflog记录了**本地仓库**的指针移动,使用`git reflog`查看,`git reflog --relative-date`可以查看到时间。，如下：
```
b37132b HEAD@{16 minutes ago}: reset: moving to origin/master
94a99ad HEAD@{16 minutes ago}: commit: s
b37132b HEAD@{21 minutes ago}: clone: from git@github.com:ladder1984/updateHosts 
```
上面的日志显示进行了reset操作，如果想恢复到reset之前的状态，可以使用`git reset 94a99ad --hard`命令。注意，reflog查看到的ID也是可以checkout命令的，所以也可以使用`git checkout 94a99ad -B master`

