---
description: origin master是远程分支    origin/master是远程分支的指针，需要fetch手动更新
---

# git

远程跟踪分支是远程分支状态的引用。它们是你无法移动的本地引用。\<origin>/\<branch> 只要fetch后，就会同步\<origin> \<branch>到\<origin>/\<branch>引用上面，包括你想克隆远程分支的，实际上克隆的是引用，也就是\<origin>/\<branch>

## 一定注意:主分支上不要rebase。不要merge test分支。

### 初始化/仓库相关

```
git init            将尚未进行版本控制的目录转化为Git仓库
git clone <url>     克隆已有的仓库
git remote -v     查看远程仓库，简写与其对应的URL
git remote add <shortname> <url>    手动添加一个新的Git仓库,shortname一般都是origin
```

仓库中的每个文件，只有两种状态。 **已跟踪** 或 **未跟踪**，**已跟踪**就是git知道的文件。

```
git add README        指定追踪README文件,并放入暂存区staged
git add .              递归追中当前目录下的所有文件staged

git commit -m        提交更新暂存区的代码，此命令需要保证暂存区内至少有文件内容,否则需要git add
如果你没有写-m注释，会强制进入vim编辑模式。此时在上面输入，按i进入编辑模式,:wq退出,:wq!强退。


```

### fetch与pull

fetch仅仅是将\<remote> \<branch>拉到\<origin>/\<branch>引用上，这样你就能用git branch -vv看到追踪的引用与本地的差异了。**fetch很安全，不会合并。**

pull是fetch+merge的升级版，需要本地分支有追踪才可以。

```
git pull                          分支如果有track追踪，那么可以直接简写，否则只能使用下面全拼
git pull <remote> <origin>        拉取一个远程分支到引用上，并merge一下这个引用

git pull origin master        你当前所处非master，拉了一下远程，这会更新远程的引用，并merge master
一般来说，都是本地拉对应的追踪远程，然后自动merge一下远程(引用)，当然想dev_tangjian_feat需要merge一下master的情况，此时可以直接拉取对应的origin master

```

### 增加

```
推送
git push                       需要有追踪的分支，会自动推送
git push <remote> <branch>    将本地分支内容，指定推送到远程同名分支
git push <remote> <branch本地:branch远程>   指定本地某个分支，推送到远程的某个，一般不这么干

本地分支新增
git branch <branch>        在当前所属分支上创建一个分支
git checkout -b <branch>    检出分支在当前所属分支上创建一个分支并切换到新建分支上

根据“远程”分支检出本地分支
git checkout -b <branch> <remote>/<branch>       常用命名，会自动--track追踪

根据“本地”分支新增远程分支
git push <remote> <branch>    推送到远程。没有的话，会新建，但不会追踪


手动增加追踪
git branch -u <remote>/<branch>       给当前分支设置一个上游upstream追踪分支,追踪引用即可. -u或--set-upstream-to

```

### 删除

```
git branch -d <branch>        删除分支，仅针对已合并的分支
git branch -D <branch>        强制删除本地分支，对于未merge fully的分支也能删
git push -d <remote> <branch>    删除某个远程分支
git push -D <remote> <branch>    强制删除某个远程分支 --delete --force

删除追踪
git branch --unset-upstream        手动取消本地分支的上游追踪。例如远程分支被删除了，需要手动解除一下
```

### 查询

```
普通查看
git status -s        简短查看文件状态？？是未追中的，红色的是未放入暂存的
git log              查看提交历史

查看分支
git branch        查看本地所有分支，当前所属分支
git branch -a     查看本地和远程所有分支,--all
git branch -vv    本地分支与远程分支追踪状态

查看HEAD操作commit等记录
git reflog show --date=iso
git reflog show --date=iso test   查看指定分支的记录。这个好用，丢失分支大概率会被merge一次

```



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

### 找回分支(本地)

#### 本地分支被删除后，从git reflog上看，分支名消失，只能看到HEAD@{12}，此时可从相关分支定位范围，找到最后一次分支的commit。例如

08821ef HEAD@{10}:commit 修复某某bug。    此记录为删除分支的最后一次提交

#### 或者记住分支名称，因为大多数分支最终会merge into到别的分支上,寻找一下

08821ef HEAD@{8}:merge **test**:Fast-forward

55813db(origin/master) HEAD@{9} checkout ：moving from **test** to master

#### 找到切换分支也行，git reflog是可以记住分支的切换的,顺着切换的hash往上找

checkout: moving from master to dev\_tangjian\_feat &#x20;

最后使用 git checkout -b \<branchName>  08821ef   即可



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
