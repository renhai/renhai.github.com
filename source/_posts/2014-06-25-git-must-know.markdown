---
layout: post
title: "Git必知必会"
date: 2014-06-25 12:21:52 +0800
comments: true
categories: 
---


##git 配置查看
---
	git config -l

<!--more-->

##Log 查看
---
```
git log
git log --stat 查看提交文件信息
git log --graph 图形化查看提交log
```

##分支
---
###本地新建一个分支并关联到远程分支

	git checkout -b “本地新分支名” “远程分支名”

###本地删除分支

	git branch -d “本地分支名”

###分支合并

	git merge --no-ff 分支名

###干掉远程已删除分支

	git remote prune origin

##tag 操作
---
###显示标签

	git tag -l -n1

###创建本地标签
```
git tag -m "message" V5.8.1
git tag -a V7.3.1 -m "V7.3.1 发布"
```
###将本地标签 push 到远程

	git push origin V5.8.1

##常规操作流程
---
###git pull --rebase 冲突解决

1. 确定当前分支状态

	当发生冲突时，会进入到临时的 rebase 状态，可使用 git status 或者 git branch 命令确定当前分支状态。

2. 修改冲突

	推荐使用 vi 修改冲突，使用 vi /<<<<<<< 命令定位冲突代码行，修改冲突，n 命令可向下搜索下一处冲突行，N 命令为向上搜索，直到确定文件没有冲突标记后才可退出编辑。
修改冲突时，如果是代码冲突，需要两位当事人在场确定冲突修改方案，如果没有询问其他代码作者擅自删除他人代码，导致工程功能不完整，事后会追究责任。

3. 标记冲突解决

	使用 git add xxx 命令标记冲突解决，使用 git status 命令查看确定冲突状态是否解决。

4. rebase 继续

	解决完某 commit 冲突后，使用 git rebase --continue 命令继续 rebase 操作，如果遇到冲突再重复以上步骤，逐步解决所有冲突，直到 rebase 操作继续，回到原分支。

5. 如果发生意外
	
	如果发生意外操作，使用 git rebase --abort 命令放弃本次 rebase 操作，之后 使用 git pull --rebase 命令重新操作

###修改审核失败Patch流程

1. 暂存本地修改

	使用 git stash 命令暂存本地修改。

2. 退回到审核失败的patch commit

	使用 git rebase -i HEAD^ 命令退回到审核失败的Patch，^的数量为需修改 commit 距 HEAD 的commit 个数。
使用 commit sha号或者 HEAD~n 替代 HEAD^ 也可以。

3. 在 rebase 选择界面，将需要修改的 commit 标识置为 edit
	*	pick 为不需修改，直接使用 commit；
	*	reword 为只修改 commit message;
	*	edit 为使用 commit，但会暂停到该 commit，暂停时可以修改该 commit，最后使用 amend 方法提交;
	*	squash 跟前一个 commit 合并，但是保留 commit log 信息;
	*	fixup 同 squash 类似，跟前一个commit合并，但是放弃 commit log 信息;
	*	exec 运行命令行；

4. 修改审核失败的 commit

	开始 rebase 操作，当暂停在需修改 commit 时，可以对该 commit 做任何操作，按 Gerrit 中的审核意见修改 commit。

5. 保存修改并提交

	修改完成后，类似常规 commit 操作，使用 git add . 添加修改，使用 git commit --amend 命令将修改合并到当前 commit 中。

6. 继续 rebase 后面的 commit

	使用 git rebase --continue 命令继续 rebase 后面所有 commit

7. 将修改完成后最新的 commit 提交到服务器。

	使用 git push 命令将修改后的 commit 提交到代码服务器。

8. 取回本地暂存修

	使用 git stash pop 命令取回第一步中暂存到本地的修改。
