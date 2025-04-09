---
title: Git使用指南
date: 2025-01-05
last_modified: 2025-01-05
author: Cheng Jun
desc: git不只是git clone和git push，下面是我使用git上传到github的一些经验。
tags: [Git, GitHub]
categories: Technical sharing
---

#### 当你需要和别人的代码协作的时候

**Fork 仓库**：首先，你需要将原始仓库 fork 到你自己的 GitHub 账户。这将创建一个你拥有的副本。

**克隆 Fork**：然后，从你自己的 fork 仓库克隆到本地。

**进行更改**：在本地进行代码更改和开发。

**推送到 Fork**：将更改推送到你自己的 fork 仓库。

**创建拉取请求**：在 GitHub 上，从你的 fork 向原始仓库提交一个拉取请求，请求将你的更改合并到原始仓库。

常用命令：

```bash
# git pull 实际上是 git fetch 和 git merge 的组合
git fetch origin # 可以更新你本地仓库中所有远程分支的最新状态（只是查看）
git checkout -b try origin/try # 新建的本地分支会包含远程分支的当前状态和更改
git push --set-upstream origin try = git push -u origin try # 推送并建立跟踪关系
--set-upstream # 建立关联

# fork仓库手动同步原始远程仓库的更改
git remote add upstream https://github.com/原始用户名/原始仓库.git
git fetch upstream
git checkout main  # 切换到你的主分支
git merge upstream/main  # 合并原始仓库的主分支
git push origin main


git remote -v # 查看远程仓库信息
git branch -vv # 查看本地分支的追踪情况
git remote rm origin # 移除对远程仓库的关联（注意这个名字）
git push origin --delete my_branch # 这会将本地的分支删除，并从远程仓库中删除同名的分支。

git log --stat
git status

git checkout -b <branchname>  # 从当前节点新建分支
git branch	#列举所有分支
git checkout <branchname> # 单纯的切换到某个分支
git branch -D <branchname> # 删掉特定的分支
git merge <branchname>	# 合并分支
git merge --abort # 放弃合并
```



#### 撤销git push后的一次提交

1，撤销最后一次提交，但是保留更改

```bash
git reset --soft HEAD~1 # git reset --hard HEAD~1
```

2, 查看当前更改

```bash
git status
```

3，进行新的更改，并重新提交更改

```bash
git add .  # git commit
```

4，强制推送到远程仓库

```bash
git push origin <branch-name> --force
```

