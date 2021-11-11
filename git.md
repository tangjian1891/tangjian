# git

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
