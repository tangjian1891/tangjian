---
description: origin master是远程分支    origin/master是远程分支的指针，需要fetch手动更新
---

# git

### 获取仓库

```
git init            将尚未进行版本控制的目录转化为Git仓库
git clone <url>     克隆已有的仓库
```

仓库中的每个文件，只有两种状态。 **已跟踪** 或 **未跟踪**，**已跟踪**就是git知道的文件。

```
git add README        指定追踪README文件,并放入暂存区staged
git add .              递归追中当前目录下的所有文件staged

git commit -m        提交更新暂存区的代码，此命令需要保证暂存区内至少有文件内容,否则需要git add
如果你没有写-m注释，会强制进入vim编辑模式。此时在上面输入，按i进入编辑模式,:wq退出,:wq!强退。

git status -s        简短查看文件状态？？是未追中的，红色的是未放入暂存的

git log                查看提交历史
```

远程仓库

```
git remote -v     查看远程仓库，简写与其对应的URL

git remote add <shortname> <url>    手动添加一个新的Git仓库,shortname一般都是origin

查看分支相关
git branch        查看本地所有分支，当前所属分支
git branch -a        查看本地和远程所有分支
git branch -vv        查看本地与远程分支的追踪状态。方便git pull,git push简写

fetch获取,获取远程仓库的信息(仅仅是获取已追踪的分支)。
git fetch <remote>       从远程仓库中获取信息，移动对应的origin/<branch>指针，不会合并修改。

推送
git push <remote> <branch>       推送分支
git push origin dev_tangjian_feat              将本地分支推送到远程的同名分支上
git push origin dev_tangjian_feat:dev_branch    将本地分支推送到远程的dev_branch上，会出现一个指针origin/dev_btranch


追踪分支(从一个远程分支检出一个本地分支会自动跟踪，也就是upstream"上游分支")
 git branch --set-upstream-to origin/dev_tangjian_feat dev_tangjian_feat 
 git branch --unset-upstream    手动取消追踪
 
 手动检出远程分支(会自动追踪)
git checkout origin/hahaha --track        检出一个远程分支并追踪 
 
```

### 操作

#### 提交

git commit

创建分支

git branch \<your-branch-name>

切换分支

git checkout \<your-branch-name>

创建分支并切换

git checkout -b \<your-branch-name>

### git reset重置

回退版本。分为三种模式 --soft --mixed --hard ，三种模式都能更改HEAD指针，回退到某次提交

* \--soft 软合并-保持本地更改。将已commit的记录和已有暂存区内容合并后放入暂存区。
* \--mixed 混合合并-将所有本地更改，暂存区，已commit记录合并后放入本地改动。（不加参数默认这个）
* \--hard 丢弃本地，丢弃暂存，丢弃已commit记录。全部丢弃。

```
git reset HEAD   在当前节点重置，可以暂存区的内容取出放入更改中。HEAD指针不动
git reset master  未分离HEAD时，master分支就是HEAD，与1一致
git reset HEAD~1  回退1个提交，这里使用git reset master~1同理，后面只用HEAD演示
git reset e830a3b 根据hash回退到指定的commit。
```

### 推送/追踪/查看本地对应远程

#### 已追踪的分支，可以直接push推送/pull拉取

git pull \<remote> \<branch>

```
git push
```



如果失败了，并且说这个分支没有upstream上游分支，就是说此分支没有追踪远程分支，不允许直接推，可以选择手动指定远程分支推送

#### 手动追踪

```
 git branch --set-upstream-to origin/dev_tangjian_feat dev_tangjian_feat
```

#### 手动取消追踪

```
git branch --unset-upstream
```

#### 查看本地分支与远程分支对应情况

```
git branch -vv
```



### 分支合并

把本地分支合并到test，把本地分支合并到bugFix分支。

切换到test分支，merge一下本地分支。

也就是说，永远不要merge test。test代码不能污染其余分支

### 变基

git rebase 目标分支,将自己分支移动端目标分支的最前方

### 在提交树上移动

git checkout 某次提交，这样head移动,这样就只能修改git树了

操作符(^),寻找目标节点的父节点。main^就是寻找main节点的父节点 main^^同理 HEAD^也可，\~10向上移动10步

### 强制移动 &#x20;

git branch -f main HEAD\~3  让指定的分支强行移动，并不是单纯的checkout。

### 撤销变更

撤销变更由底层部分(暂存区)和上层部分(变更到底是通过哪种方式被撤销)组成。

git reset重置，git revert回滚

git reset HEAD\~1&#x20;

git revert HEAD&#x20;

### cherry-pick

git cherry-pick <提交号>

可以把其余分支的某次hash提交，复制提交当前HEAD分支上面

git cherry-pick c1 c2

### rebase可以说是基于cherry-pick

git rebase -i main\~3 会出现一个交互界面，供你拖拽.调整分支的顺序

本地栈式提交



git tag.

git tag v1 hash  git tag v2 当前HEAD上打

git describe



合并提交



### 问题

### http协议，每次都要输入账号密码

在输入账号密码认证成功完成操作后，紧接着设定永久保存登录凭证

```
git config --global credential.helper store
```

``
