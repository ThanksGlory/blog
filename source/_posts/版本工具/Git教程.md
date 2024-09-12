---
title: Git的学习教程
cover: /images/cover/git.jpg
top_img: false
tags: git
comments: true
categories:
  - 版本管理
  - Git
abbrlink: 1
date: 2022-01-11 11:14:29
---

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

#### 创建版本库

```shell
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

#### 添加到暂存区

```shell
$ git add readme.txt
# 添加全部文件
$ git add .
# 或者
$ git add -A
```

#### 提交到本地库

```shell
$ git commit -m "wrote a readme file"
# 添加多个文件
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."

[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

#### 查看提交状态

```sh
$ git status
# 精简查看行数加-s参数
$ git status -s
```

#### 对比文件

```sh
$ git diff <file>
```

#### 查看日志

```sh
$ git log
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
# 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数
$ git log --pretty=oneline
58af5f5d49d9d8666d77bb613cb9dc24012cf37a (HEAD -> master, origin/master, origin/HEAD) add test.txt
e88f93d407e28363d0b390c2fa410c4e25cfd893 remove test.txt
b3f7a63ad3701856347c743fde406ed1dfff2673 add test.txt
65a1855363347f1bf5bfd95fddaf78634f94946e Git tracks changes of files
51e88be7d9c7c3e6ca531086c9f7cffdba3adfd4 git tracks changes
e14d050157d575c3e26bd5f783529b5b8536bf4c understand how stage works
d6ca7e5a282214dc236e11bf7e9c471397fa87fa append GPL
a0fbf8b959ce54cb0ba0e18a586fc9c7664968e1 add distributed
2c012850ecfa5a46b079d9fe5aeb244d95441182 wrote a readme file
a8752ebc7948638eae5c6fe3c952af9a362e32f4 Initial commit
```

#### 回退版本

首先，Git 必须知道当前版本是哪个版本，在 Git 中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交 ID 和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上 100 个版本写 100 个`^`比较容易数不过来，所以写成`HEAD~100`。

```sh
$ git reset --hard HEAD^
# 回退到某一个提交
$ git reset --hard <commitid>
$ git reset --hard 1094a
```

#### 查看历史提交 HEAD

```SH
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

#### 撤销修改

```sh
$ git checkout -- <filename>
```

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

#### 撤销暂存区

```sh
$ git reset HEAD <filename>Unstaged changes after reset:M	readme.txt
```

#### 删除文件

```sh
$ git rm test.txtrm 'test.txt'$ git commit -m "remove test.txt"[master d46f35e] remove test.txt 1 file changed, 1 deletion(-) delete mode 100644 test.txt
```

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```sh
$ git checkout -- test.txt
```

#### 远程仓库

**1、重新生成 ssh** (连续回车三次)

```sh
$ ssh-keygen -t rsa -C "youremail@example.com"
```

**2、查看你的 public key**

```sh
#以ssh-rsa 开头，以账号的注册邮箱结尾的
$ cat ~/.ssh/id_rsa.pub

# 详细信息如下
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC0HnLUqRbt0/pi0rVvJw/NWTrpQS9aKEMj+jK77v5dRiEHzJ6q46nx3rmtRSks5sjuldOu3d5MY+4UyhoECB3fwGn9v3IAzNWBZX4hS9BpHb6QTPKC2G79dO/0FqUX2d/vc9ua7nzicsa/ncz+S/hXCa3Uds2fNR7Y3g2kTsXF6pGZg1vXfKLqpvKulRoSdcgSqYzwT+LajbfwhTr75I4FVJVnN5loodX/B9mNoz7wprAxHqV7JOLzaOGG+QjfIeuQKvis9wsO4k4pTaslcFOQC+CzMKsiBeUZmAD+cDL08cfLvWd3PWYGUqUR8Ujxuy7LmPoqzUS0EAZbyQH/HqTl 1043732762@qq.com
```

**3、将它添加到码云，添加地址 https://gitee.com/profile/sshkeys**

#### 添加远程库

```sh
# 添加远程仓库
$ git remote add origin git@github.com:michaelliao/learngit.git# 提交到远程仓库$ git push -u origin masterCounting objects: 20, done.Delta compression using up to 4 threads.Compressing objects: 100% (15/15), done.Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.Total 20 (delta 5), reused 0 (delta 0)remote: Resolving deltas: 100% (5/5), done.To github.com:michaelliao/learngit.git * [new branch]      master -> masterBranch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git 不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

**注意：如果推送失败需要将远程仓库和本地仓库同步一次**

```sh
$ git pull --rebase origin master
```

#### 删除远程库

```sh
$ git remote -vorigin  git@github.com:michaelliao/learn-git.git (fetch)origin  git@github.com:michaelliao/learn-git.git (push)
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到 GitHub，在后台页面找到删除按钮再删除。

#### 从远程库克隆

```sh
$ git clone git@github.com:michaelliao/gitskills.gitCloning into 'gitskills'...remote: Counting objects: 3, done.remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3Receiving objects: 100% (3/3), done.
```

#### 创建与切换分支

首先，我们创建`dev`分支，然后切换到`dev`分支：

```sh
$ git checkout -b devSwitched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```sh
# 创建分支$ git branch dev# 切换分支$ git checkout devSwitched to branch 'dev'
```

然后，用`git branch`命令查看当前分支：

```sh
$ git branch* dev  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

#### 合并分支

```sh
$ git merge devUpdating d46f35e..b17d20eFast-forward readme.txt | 1 + 1 file changed, 1 insertion(+)
```

#### git switch

我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。

实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的 Git 提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：

```sh
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```sh
$ git switch master
```

使用新的`git switch`命令，比`git checkout`要更容易理解。

**小结**

Git 鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

#### 解决冲突

这种情况下，Git 无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

```sh
$ git merge feature1Auto-merging readme.txtCONFLICT (content): Merge conflict in readme.txtAutomatic merge failed; fix conflicts and then commit the result.
```

果然冲突了！Git 告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

```sh
$ git statusOn branch masterYour branch is ahead of 'origin/master' by 2 commits.  (use "git push" to publish your local commits)You have unmerged paths.  (fix conflicts and run "git commit")  (use "git merge --abort" to abort the merge)Unmerged paths:  (use "git add <file>..." to mark resolution)	both modified:   readme.txtno changes added to commit (use "git add" and/or "git commit -a")
```

我们可以直接查看 readme.txt 的内容：

```tex
Git is a distributed version control system.Git is free software distributed under the GPL.Git has a mutable index called stage.Git tracks changes of files.<<<<<<< HEADCreating a new branch is quick & simple.=======Creating a new branch is quick AND simple.>>>>>>> feature1
```

Git 用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```
Creating a new branch is quick and simple.
```

再提交：

```sh
$ git add readme.txt $ git commit -m "conflict fixed"[master cf810e4] conflict fixed
```

用带参数的`git log`也可以看到分支的合并情况：

```sh
$ git log --graph --pretty=oneline --abbrev-commit*   cf810e4 (HEAD -> master) conflict fixed|\  | * 14096d0 (feature1) AND simple* | 5dc6824 & simple|/  * b17d20e branch test* d46f35e (origin/master) remove test.txt* b84166e add test.txt* 519219b git tracks changes* e43a48b understand how stage works* 1094adb append GPL* e475afc add distributed* eaadf4e wrote a readme file
```

最后，删除`feature1`分支：

```sh
$ git branch -d feature1Deleted branch feature1 (was 14096d0).
```

用`git log --graph`命令可以看到分支合并图。

#### 分支管理策略

通常，合并分支时，如果可能，Git 会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git 就会在 merge 时生成一个新的 commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```sh
$ git switch -c devSwitched to a new branch 'dev'
```

修改 readme.txt 文件，并提交一个新的 commit：

```sh
$ git add readme.txt $ git commit -m "add merge"[dev f52c633] add merge 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`：

```sh
$ git switch masterSwitched to branch 'master'
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```sh
$ git merge --no-ff -m "merge with no-ff" devMerge made by the 'recursive' strategy. readme.txt | 1 + 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的 commit，所以加上`-m`参数，把 commit 描述写进去。

合并后，我们用`git log`看看分支历史：

```sh
$ git log --graph --pretty=oneline --abbrev-commit*   e1e9c68 (HEAD -> master) merge with no-ff|\  | * f52c633 (dev) add merge|/  *   cf810e4 conflict fixed...
```

可以看到，不使用`Fast forward`模式，merge 后就像这样：

#### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如 1.0 版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布 1.0 版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

#### Bug 分支

软件开发中，bug 就像家常便饭一样。有了 bug 就需要修复，在 Git 中，由于分支是如此的强大，所以，每个 bug 都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号 101 的 bug 的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```sh
$ git statusOn branch devChanges to be committed:  (use "git reset HEAD <file>..." to unstage)	new file:   hello.pyChanges not staged for commit:  (use "git add <file>..." to update what will be committed)  (use "git checkout -- <file>..." to discard changes in working directory)	modified:   readme.txt
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需 1 天时间。但是，必须在两个小时内修复该 bug，怎么办？

幸好，Git 还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```sh
$ git stashSaved working directory and index state WIP on dev: f52c633 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被 Git 管理的文件），因此可以放心地创建分支来修复 bug。

首先确定要在哪个分支上修复 bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```sh
$ git checkout masterSwitched to branch 'master'Your branch is ahead of 'origin/master' by 6 commits.  (use "git push" to publish your local commits)$ git checkout -b issue-101Switched to a new branch 'issue-101'
```

现在修复 bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

```sh
$ git add readme.txt $ git commit -m "fix bug 101"[issue-101 4c805e2] fix bug 101 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```sh
$ git switch masterSwitched to branch 'master'Your branch is ahead of 'origin/master' by 6 commits.  (use "git push" to publish your local commits)$ git merge --no-ff -m "merged bug fix 101" issue-101Merge made by the 'recursive' strategy. readme.txt | 2 +- 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的 bug 修复只花了 5 分钟！现在，是时候接着回到`dev`分支干活了！

```sh
$ git switch devSwitched to branch 'dev'$ git statusOn branch devnothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```sh
$ git stash liststash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git 把 stash 内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash 内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把 stash 内容也删了：

```sh
$ git stash popOn branch devChanges to be committed:  (use "git reset HEAD <file>..." to unstage)	new file:   hello.pyChanges not staged for commit:  (use "git add <file>..." to update what will be committed)  (use "git checkout -- <file>..." to discard changes in working directory)	modified:   readme.txtDropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用`git stash list`查看，就看不到任何 stash 内容了：

```sh
$ git stash list
```

你可以多次 stash，恢复的时候，先用`git stash list`查看，然后恢复指定的 stash，用命令：

```sh
$ git stash apply stash@{0}
```

在 master 分支上修复了 bug 后，我们要想一想，dev 分支是早期从 master 分支分出来的，所以，这个 bug 其实在当前 dev 分支上也存在。

那怎么在 dev 分支上修复同样的 bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的 bug，要在 dev 上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到 dev 分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个 master 分支 merge 过来。

为了方便操作，Git 专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```
$ git branch* dev  master$ git cherry-pick 4c805e2[master 1d4b803] fix bug 101 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git 自动给 dev 分支做了一次提交，注意这次提交的 commit 是`1d4b803`，它并不同于 master 的`4c805e2`，因为这两个 commit 只是改动相同，但确实是两个不同的 commit。用`git cherry-pick`，我们就不需要在 dev 分支上手动再把修 bug 的过程重复一遍。

有些聪明的童鞋会想了，既然可以在 master 分支上修复 bug 后，在 dev 分支上可以“重放”这个修复过程，那么直接在 dev 分支上修复 bug，然后在 master 分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从 dev 分支切换到 master 分支。

**小结**

修复 bug 时，我们会通过创建新的 bug 分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复 bug，修复后，再`git stash pop`，回到工作现场；

在 master 分支上修复的 bug，想要合并到当前 dev 分支，可以用`git cherry-pick <commit>`命令，把 bug 提交的修改“复制”到当前分支，避免重复劳动。

#### Feature 分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个 feature 分支，在上面开发，完成后，合并，最后，删除该 feature 分支。

现在，你终于接到了一个新任务：开发代号为 Vulcan 的新功能，该功能计划用于下一代星际飞船。

于是准备开发：

```sh
$ git switch -c feature-vulcanSwitched to a new branch 'feature-vulcan'
```

5 分钟后，开发完毕：

```sh
$ git add vulcan.py$ git statusOn branch feature-vulcanChanges to be committed:  (use "git reset HEAD <file>..." to unstage)	new file:   vulcan.py$ git commit -m "add feature vulcan"[feature-vulcan 287773e] add feature vulcan 1 file changed, 2 insertions(+) create mode 100644 vulcan.py
```

切回`dev`，准备合并：

```sh
$ git switch dev
```

一切顺利的话，feature 分支和 bug 分支是类似的，合并，然后删除。

但是！

就在此时，接到上级命令，因经费不足，新功能必须取消！

虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

```sh
$ git branch -d feature-vulcanerror: The branch 'feature-vulcan' is not fully merged.If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

销毁失败。Git 友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。。

现在我们强行删除：

```sh
$ git branch -D feature-vulcanDeleted branch feature-vulcan (was 287773e).
```

终于删除成功！

**小结**

开发一个新 feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

#### 多人协作

当你从远程仓库克隆时，实际上 Git 自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

```sh
$ git remoteorigin
```

或者，用`git remote -v`显示更详细的信息：

```sh
$ git remote -vorigin  git@github.com:michaelliao/learngit.git (fetch)origin  git@github.com:michaelliao/learngit.git (push)
```

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到 push 的地址。

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git 就会把该分支推送到远程库对应的远程分支上：

```sh
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```sh
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug 分支只用于在本地修复 bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个 bug；
- feature 分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在 Git 中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

#### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把 SSH Key 添加到 GitHub）或者同一台电脑的另一个目录下克隆：

```sh
$ git clone git@github.com:michaelliao/learngit.gitCloning into 'learngit'...remote: Counting objects: 40, done.remote: Compressing objects: 100% (21/21), done.remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0Receiving objects: 100% (40/40), done.Resolving deltas: 100% (14/14), done.
```

当你的小伙伴从远程库 clone 时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看：

```sh
$ git branch* master
```

现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```sh
$ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```sh
$ git add env.txt$ git commit -m "add env"[dev 7a5e5dd] add env 1 file changed, 1 insertion(+) create mode 100644 env.txt$ git push origin devCounting objects: 3, done.Delta compression using up to 4 threads.Compressing objects: 100% (2/2), done.Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.Total 3 (delta 0), reused 0 (delta 0)To github.com:michaelliao/learngit.git   f52c633..7a5e5dd  dev -> dev
```

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```sh
$ cat env.txtenv$ git add env.txt$ git commit -m "add new env"[dev 7bd91f1] add new env 1 file changed, 1 insertion(+) create mode 100644 env.txt$ git push origin devTo github.com:michaelliao/learngit.git ! [rejected]        dev -> dev (non-fast-forward)error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'hint: Updates were rejected because the tip of your current branch is behindhint: its remote counterpart. Integrate the remote changes (e.g.hint: 'git pull ...') before pushing again.hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git 已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```sh
$ git pullThere is no tracking information for the current branch.Please specify which branch you want to merge with.See git-pull(1) for details.    git pull <remote> <branch>If you wish to set tracking information for this branch you can do so with:    git branch --set-upstream-to=origin/<branch> dev
```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```sh
$ git branch --set-upstream-to=origin/dev devBranch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再 pull：

```sh
$ git pullAuto-merging env.txtCONFLICT (add/add): Merge conflict in env.txtAutomatic merge failed; fix conflicts and then commit the result.
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后，提交，再 push：

```sh
$ git commit -m "fix env conflict"[dev 57c53ab] fix env conflict$ git push origin devCounting objects: 6, done.Delta compression using up to 4 threads.Compressing objects: 100% (4/4), done.Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.Total 6 (delta 0), reused 0 (delta 0)To github.com:michaelliao/learngit.git   7a5e5dd..57c53ab  dev -> dev
```

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

**小结**

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

#### Rebase

在上一节我们看到了，多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后 push 的童鞋不得不先 pull，在本地合并，然后才能 push 成功。

每次合并再 push 后，分支变成了这样：

```sh
$ git log --graph --pretty=oneline --abbrev-commit* d1be385 (HEAD -> master, origin/master) init hello*   e5e69f1 Merge branch 'dev'|\  | *   57c53ab (origin/dev, dev) fix env conflict| |\  | | * 7a5e5dd add env| * | 7bd91f1 add new env| |/  * |   12a631b merged bug fix 101|\ \  | * | 4c805e2 fix bug 101|/ /  * |   e1e9c68 merge with no-ff|\ \  | |/  | * f52c633 add merge|/  *   cf810e4 conflict fixed
```

总之看上去很乱，有强迫症的童鞋会问：为什么 Git 的提交历史不能是一条干净的直线？

其实是可以做到的！

Git 有一种称为 rebase 的操作，有人把它翻译成“变基”。

先不要随意展开想象。我们还是从实际问题出发，看看怎么把分叉的提交变成直线。

在和远程分支同步后，我们对`hello.py`这个文件做了两次提交。用`git log`命令看看：

```sh
$ git log --graph --pretty=oneline --abbrev-commit* 582d922 (HEAD -> master) add author* 8875536 add comment* d1be385 (origin/master) init hello*   e5e69f1 Merge branch 'dev'|\  | *   57c53ab (origin/dev, dev) fix env conflict| |\  | | * 7a5e5dd add env| * | 7bd91f1 add new env...
```

注意到 Git 用`(HEAD -> master)`和`(origin/master)`标识出当前分支的 HEAD 和远程 origin 的位置分别是`582d922 add author`和`d1be385 init hello`，本地分支比远程分支快两个提交。

现在我们尝试推送本地分支：

```sh
$ git push origin masterTo github.com:michaelliao/learngit.git ! [rejected]        master -> master (fetch first)error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'hint: Updates were rejected because the remote contains work that you dohint: not have locally. This is usually caused by another repository pushinghint: to the same ref. You may want to first integrate the remote changeshint: (e.g., 'git pull ...') before pushing again.hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

很不幸，失败了，这说明有人先于我们推送了远程分支。按照经验，先 pull 一下：

```sh
$ git pullremote: Counting objects: 3, done.remote: Compressing objects: 100% (1/1), done.remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0Unpacking objects: 100% (3/3), done.From github.com:michaelliao/learngit   d1be385..f005ed4  master     -> origin/master * [new tag]         v1.0       -> v1.0Auto-merging hello.pyMerge made by the 'recursive' strategy. hello.py | 1 + 1 file changed, 1 insertion(+)
```

再用`git status`看看状态：

```sh
$ git statusOn branch masterYour branch is ahead of 'origin/master' by 3 commits.  (use "git push" to publish your local commits)nothing to commit, working tree clean
```

加上刚才合并的提交，现在我们本地分支比远程分支超前 3 个提交。

用`git log`看看：

```sh
$ git log --graph --pretty=oneline --abbrev-commit*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit|\  | * f005ed4 (origin/master) set exit=1* | 582d922 add author* | 8875536 add comment|/  * d1be385 init hello...
```

对强迫症童鞋来说，现在事情有点不对头，提交历史分叉了。如果现在把本地分支 push 到远程，有没有问题？

有！

什么问题？

不好看！

有没有解决方法？

有！

这个时候，rebase 就派上了用场。我们输入命令`git rebase`试试：

```sh
$ git rebaseFirst, rewinding head to replay your work on top of it...Applying: add commentUsing index info to reconstruct a base tree...M	hello.pyFalling back to patching base and 3-way merge...Auto-merging hello.pyApplying: add authorUsing index info to reconstruct a base tree...M	hello.pyFalling back to patching base and 3-way merge...Auto-merging hello.py
```

输出了一大堆操作，到底是啥效果？再用`git log`看看：

```sh
$ git log --graph --pretty=oneline --abbrev-commit* 7e61ed4 (HEAD -> master) add author* 3611cfe add comment* f005ed4 (origin/master) set exit=1* d1be385 init hello...
```

原本分叉的提交现在变成一条直线了！这种神奇的操作是怎么实现的？其实原理非常简单。我们注意观察，发现 Git 把我们本地的提交“挪动”了位置，放到了`f005ed4 (origin/master) set exit=1`之后，这样，整个提交历史就成了一条直线。rebase 操作前后，最终的提交内容是一致的，但是，我们本地的 commit 修改内容已经变化了，它们的修改不再基于`d1be385 init hello`，而是基于`f005ed4 (origin/master) set exit=1`，但最后的提交`7e61ed4`内容是一致的。

这就是 rebase 操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

最后，通过 push 操作把本地分支推送到远程：

```sh
Mac:~/learngit michael$ git push origin masterCounting objects: 6, done.Delta compression using up to 4 threads.Compressing objects: 100% (5/5), done.Writing objects: 100% (6/6), 576 bytes | 576.00 KiB/s, done.Total 6 (delta 2), reused 0 (delta 0)remote: Resolving deltas: 100% (2/2), completed with 1 local object.To github.com:michaelliao/learngit.git   f005ed4..7e61ed4  master -> master
```

再用`git log`看看效果：

```sh
$ git log --graph --pretty=oneline --abbrev-commit* 7e61ed4 (HEAD -> master, origin/master) add author* 3611cfe add comment* f005ed4 set exit=1* d1be385 init hello...
```

远程分支的提交历史也是一条直线。

**小结**

- rebase 操作可以把本地未 push 的分叉提交历史整理成直线；
- rebase 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

#### 创建标签

在 Git 中打标签非常简单，首先，切换到需要打标签的分支上：

```sh
$ git branch* dev  master$ git checkout masterSwitched to branch 'master'
```

然后，敲命令`git tag <name>`就可以打一个新标签：

```sh
$ git tag v1.0
```

可以用命令`git tag`查看所有标签：

```sh
$ git tagv1.0
```

默认标签是打在最新提交的 commit 上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的 commit id，然后打上就可以了：

```sh
$ git log --pretty=oneline --abbrev-commit12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 1014c805e2 fix bug 101e1e9c68 merge with no-fff52c633 add mergecf810e4 conflict fixed5dc6824 & simple14096d0 AND simpleb17d20e branch testd46f35e remove test.txtb84166e add test.txt519219b git tracks changese43a48b understand how stage works1094adb append GPLe475afc add distributedeaadf4e wrote a readme file
```

比方说要对`add merge`这次提交打标签，它对应的 commit id 是`f52c633`，敲入命令：

```sh
$ git tag v0.9 f52c633
```

再用命令`git tag`查看标签：

```sh
$ git tagv0.9v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```sh
$ git show v0.9commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)Author: Michael Liao <askxuefeng@gmail.com>Date:   Fri May 18 21:56:54 2018 +0800    add mergediff --git a/readme.txt b/readme.txt...
```

可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```sh
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

用命令`git show <tagname>`可以看到说明文字：

```sh
$ git show v0.1tag v0.1Tagger: Michael Liao <askxuefeng@gmail.com>Date:   Fri May 18 22:48:43 2018 +0800version 0.1 releasedcommit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)Author: Michael Liao <askxuefeng@gmail.com>Date:   Fri May 18 21:06:15 2018 +0800    append GPLdiff --git a/readme.txt b/readme.txt...
```

注意：标签总是和某个 commit 挂钩。如果这个 commit 既出现在 master 分支，又出现在 dev 分支，那么在这两个分支上都可以看到这个标签。

**小结**

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个 commit id；
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

#### 操作标签

如果标签打错了，也可以删除：

```sh
$ git tag -d v0.1Deleted tag 'v0.1' (was f15b0dd)
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

```sh
$ git push origin v1.0Total 0 (delta 0), reused 0 (delta 0)To github.com:michaelliao/learngit.git * [new tag]         v1.0 -> v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签：

```sh
$ git push origin --tagsTotal 0 (delta 0), reused 0 (delta 0)To github.com:michaelliao/learngit.git * [new tag]         v0.9 -> v0.9
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```sh
$ git tag -d v0.9Deleted tag 'v0.9' (was f52c633)
```

然后，从远程删除。删除命令也是 push，但是格式如下：

```sh
$ git push origin :refs/tags/v0.9To github.com:michaelliao/learngit.git - [deleted]         v0.9
```

要看看是否真的从远程库删除了标签，可以登陆 GitHub 查看。

**小结**

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

#### 配置别名

有没有经常敲错命令？比如`git status`？`status`这个单词真心不好记。

如果敲`git st`就表示`git status`那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉 Git，以后`st`就表示`status`：

```sh
$ git config --global alias.st status
```

好了，现在敲`git st`看看效果。

当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`：

```sh
$ git config --global alias.co checkout$ git config --global alias.ci commit$ git config --global alias.br branch
```

以后提交就可以简写成：

```sh
$ git ci -m "bala bala bala..."
```

`--global`参数是全局参数，也就是这些命令在这台电脑的所有 Git 仓库下都有用。

在[撤销修改](https://www.liaoxuefeng.com/wiki/896043488029600/897889638509536)一节中，我们知道，命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个 unstage 操作，就可以配置一个`unstage`别名：

```sh
$ git config --global alias.unstage 'reset HEAD'
```

当你敲入命令：

```sh
$ git unstage test.py
```

实际上 Git 执行的是：

```sh
$ git reset HEAD test.py
```

配置一个`git last`，让其显示最后一次提交信息：

```sh
$ git config --global alias.last 'log -1'
```

这样，用`git last`就能显示最近一次的提交：

```sh
$ git lastcommit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2Merge: bd6ae48 291bea8Author: Michael Liao <askxuefeng@gmail.com>Date:   Thu Aug 22 22:49:22 2013 +0800    merge & fix hello.py
```

甚至还有人丧心病狂地把`lg`配置成了：

```sh
$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

来看看`git lg`的效果：

为什么不早点告诉我？别激动，咱不是为了多记几个英文单词嘛！

#### 配置文件

配置 Git 的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的 Git 配置文件都放在`.git/config`文件中：

```sh
$ cat .git/config [core]    repositoryformatversion = 0    filemode = true    bare = false    logallrefupdates = true    ignorecase = true    precomposeunicode = true[remote "origin"]    url = git@github.com:michaelliao/learngit.git    fetch = +refs/heads/*:refs/remotes/origin/*[branch "master"]    remote = origin    merge = refs/heads/master[alias]    last = log -1
```

别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

而当前用户的 Git 配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：

```sh
$ cat .gitconfig[alias]    co = checkout    ci = commit    br = branch    st = status[user]    name = Your Name    email = your@email.com
```

配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

**小结**

给 Git 配置好别名，就可以输入命令时偷个懒。我们鼓励偷懒。
