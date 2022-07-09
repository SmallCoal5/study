### git初始化

* 生成密钥
```
”cmd“中输入ssh-keygen命令生成SSH密钥
```


目录：C:\Users\cl\.ssh

![1655879561049](C:\Users\cl\AppData\Roaming\Typora\typora-user-images\1655879561049.png)

* 免密登录

![1655880544449](C:\Users\cl\AppData\Roaming\Typora\typora-user-images\1655880544449.png)



### 创建仓库（一般gitee新建的时候也是会出现以下代码的，直接复制就行了）

全局设置（如用户名、邮箱。）

``` git
git config --global user.name "SmallCoal5"      // 设置全局用户名
git config --global user.email "944635212@qq.com"    // 设置邮箱
```

除了用户名、邮箱之外，还有很多的配置可以用来自定义 Git，如：

```
git config --global color.ui true        // 让 Git 显示不同的颜色
git config core.ignorecase true            // 让 Git 对仓库中的文件大小写敏感
```

查看所有的已经做出的配置：

```lua
git config -l
```

### 创建 Git 版本库

在本地创建 Git 版本库，需要使用 `git init` 命令。

首先，你需要新建一个存放版本库的目录，然后进入到该目录所在路径，然后执行：

```csharp
git init
```

将文件添加到暂存区，使用的是 `git add`：

```
git add Readme.md        // 添加单个文件到暂存区git add .                // 将当前目录下所有修改添加到暂存区，除按照规则忽略的之外
```

将暂存区中的文件，提交到仓库中。需要使用 `git commit`：

```
git commit        // 如果暂存区有文件，则将其中的文件提交到仓库git commit -m 'your comments'         // 带评论提交，用于说明提交内容、变更、作用等
```

### 查看仓库的状态

不论我们是新建了文件，将文件加入暂存区，或者其他的修改等等，我们都可以通过：

```lua
git status
```

`git diff` 来查看具体的修改内容。

```javascript
git diff    // 查看版本库中所有的改动
```

查看历史提交记录

```
git log     // 显示所有提交的历史记录
git log --pretty=oneline    // 单行显示提交历史记录的内容
```

版本回退

```
git reset --hard HEAD^        // 回退到上一个提交版本
git reset --hard HEAD^^        // 回退到上上一个提交版本
git reset --hard 'commit_id'    // 会退到 commit_id 指定的提交版本
```

往未来回退

```
git reflog
git reset --hard 'commit_id'
```

撤销修改

丢弃工作区中文件的修改

```
git checkout -- Readme.md    // 如果 Readme.md 文件在工作区，则丢弃其修改
git checkout -- .            // 丢弃当前目录下所有工作区中文件的修改
```

丢弃已经进入暂存区的修改

```
git reset HEAD Readme.md // 将 Readme.md 恢复到 HEAD 提交版本的状态
```

删除文件

```
git rm Readme.md // 删除已经被提交过的 Readme.md
```

### 分支管理

分支是版本控制系统中很重要的一个概念，在 Git 中新建、合并等分支的操作非常轻量便捷，因此我们会很经常的用到。

查看分支

```
git branch        // 查看本地分支信息
git branch -v     // 查看相对详细的本地分支信息
git branch -av     // 查看包括远程仓库在内的分支信息
```

注意：在 git branch 的输出内容中，有一个分支，前面带有 * 号，这标识我们当前所在的分支。

创建分支

```
git branch dev // 新建一个名称为 dev 的分支
```

切换分支

```
git checkout dev // 新建完 dev 分支以后，通过该命令切换到 dev 分支
```

创建并切换分支

```
git checkout -b dev // 新建 dev 分支，并切换到该分支上
```

合并分支
当我们修复完成一个 Bug，或者开发完成一个新特性，我们就会把相关的 Bug 或者 特性的上修改合并回原来的主分支上，这时候就需要 git merge 来做分支的合并。

首先需要切换回最终要合并到的分支，如 master：

```
git checkout master        // 切换回 master 分支
git merge dev            // 将 dev 分钟中的修改合并回 master 分支
```


合并回主分支的时候，后面可能会面临到冲突的问题。冲突的解决暂不在这里说明。

删除分支
当之前创建的分支，完成了它的使命，如 Bug 修复完，分支合并以后，这个分支就不在需要了，就可以删除它。

```
git branch -d dev // 删除 dev 分支
```

### 远程仓库

上面的所有命令都是针对本地仓库的操作。当我们希望多个人来协作时，会将代码发布到一个统一的远程仓库，然后多个人在本地操作以后，在推送到远程仓库。其他人协作时，需要先同步远程仓库的内容，再推送自己的修改。

从远程仓库克隆

```
git clone https://github.com/git/git.git     // 通过 https 协议，克隆 Github 上 git 仓库的源码
git clone linfuyan@github.com/git/git.git    // 通过 ssh 协议，克隆 Github 上 git 仓库的源码
```

注意： git clone 后面的仓库地址，可以支持多种协议，如 https， ssh 等。

添加远程仓库
如果你已经有了一个本地仓库，如之前创建的 git-guide，然后你打算将它发布到远程，供其他人协作。那么使用：

```
git remote add origin your_remote_git_repo // 为本地仓库添加远程仓库
```

推送本地的内容到远程仓库
当本地仓库中，代码完成提交，就需要将代码等推送到远程仓库，这样其他协作人员可以从远程仓库同步内容。

```
git push -u origin master // 第一次推送时使用，可以简化后面的推送或者拉取命令使用
git push origin master    // 将本地 master 分支推送到 origin 远程分支
```


注意： git push -u origin master，第一次使用时，带上 -u 参数，在将本地的 master 分支推送到远程新的 master 分支的同时，还会把本地的 master 分支和远程的 master 分支关联起来。

从远程仓库获取最新内容
在多人协作过程中，当自己完成了本地仓库中的提交，想要向远程仓库推送前，需要先获取到远程仓库的最新内容。

可以通过 git fetch 和 git pull 来获取远程仓库的内容。

```
git fetch origin master  
git pull origin master
```


git fetch 和 git pull 之间的区别：

git fetch 是仅仅获取远程仓库的更新内容，并不会自动做合并。
git pull 在获取远程仓库的内容后，会自动做合并，可以看成 git fetch 之后 git merge。
注意：建议多使用 git fetch。

查看远程仓库信息

```
git remote [-v] // 显示远程仓库信息
```

建立本地分支和远程分支的关联
在本地仓库中的分支和远程仓库中的分支是对应的。一般情况下，远程仓库中的分支名称和本地仓库中的分支名称是一致的。有的时候，我们会需要指定本地分支与远程分支的关联。

```
git branch --set-upstream 'local_branch' origin/remote_branch
```

修改本地仓库对应的远程仓库地址
当远程的仓库地址发生变化时，需要修改本地仓库对应的远程仓库的地址。主要应用在工程迁移过程中。

```
git remote set-url origin url
```

### 标签管理

在项目开发过程中，当一个版本发布完成时，是需要对代码打上标签，便于后续检索。获取处于其他的原因，需要对某个提交打上特定的标签。

创建标签

```
git tag -a 'tagname' -m 'comment' 'commit_id'
-a 参数指定标签名， -m 添加备注信息， 'commit_id' 指定打标签的提交。
```

查看所有标签

```
git tag // 查看本地仓库中的所有标签
```

查看具体标签信息

```
git show tagname
```

删除本地标签
如果打的标签出错，或者不在需要某个标签，则可以删除它。

```
git tag -d tagname
```

删除远程标签

```
git push origin :refs/tags/tagname

git push origin --delete tagname

git push origin :tagname
```

推送标签到远程仓库
打完标签以后，有需要推送到远程仓库。

6.1 推送单个标签到远程仓库

```
git push origin tagname
```


6.2 一次性推送所有标签到远程仓库。

```
git push origin --tags
```



### 进阶操作

临时保存修改
在执行很多的 Git 操作的时候，是需要保持当前操作的仓库/分支处于 clean 状态，及没有未提交的修改。如 git pull， git merge 等等，如果有未提交的修改，这些将无法操作。

但是做这些事情的时候，你可能修改了比较多的代码，却又不想丢弃它。那么，你需要把这些修改临时保存起来，这就需要用到 git stash。

临时保存修改，这样仓库就可以回到 clean 状态。

```
git stash // 保存本地仓库中的临时修改。
```


注意：可以多次的 git stash 来保存不同的临时修改。

查看临时保存。当你临时保存以后，后面还是要取回来的，那它们在哪里呢？

```
git stash list // 显示所有临时修改
```


当我们处理完其他操作时，想要恢复临时保存的修改。

```
git stash apply        // 恢复所有保存的临时修改
git stash pop        // 恢复最近一次保存的临时修改
```


或者，我们后面觉得临时保存不想要了，那可以丢弃它。

```
git stash clear // 丢弃所有保存的临时修改
```

