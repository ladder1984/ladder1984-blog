---
title: git技巧

date: 2015-11-23 10:44:27
---

git技巧，不断更新中

1. 查找分支某个commit: 
	1. `git log|grep commitID`
	2. `git branch --contains commitID` 可以配合grep指定分支

2. 比较分支不同的commit
	`git rev-list A ^B`查找在B但不在A分支的commit

3. 查找分支开发起点
	`git rev-list A ^B|tail -1` B分支从A分支checkout出后的开发起点

4. 查找不同commit数
	`git rev-list A ^B --conut`

5. 强制更新本地分支到远程状态
```
git fetch
git reset --hard origin/master
```
或
```
git fetch
git checkout origin/master -B master
```

