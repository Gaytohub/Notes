# Git

创建一个目录，在该目录下执行 

```
git init 命令，则该目录会转换成一个由 git 工具管理的仓库。
```

把文件添加到仓库的命令：

```
git add 文件名
```

还需要把文件提交到仓库

```
git commit -m "描述"
```



对文件进行修改。

为了查看对文件做了哪些修改，可以使用

```
git diff 
```

把修改后的文件添加并提交到仓库需要执行：

```
git add 文件名
git commit -m "描述"
```



### 问题：Failed to connect to github.com port 443: Operation timed out

源自：https://www.jianshu.com/p/471aeba64724

1、设置代理方法即可解决：``git config --global http.proxy "localhost:port"``

2、完成后取消设置：``git config --global --unset http.proxy``



### 版本回退

每次对文件进行修改，并执行 commit 时，会生成一个版本号，对应 commit id。 

查询所有的版本信息可以使用命令：

```
git log
或者
git log --pretty=oneline
```

在 git 中，使用 HEAD 表示当前版本，上一个版本就是 HEAD^，上上个版本就是 HEAD^^，所以往上的 100 个版本就是 HEAD 加上 100 个角标。

所以可以使用下面的命令，从当前版本会退到上一个版本：

```
git reset --hard HEAD^
```

当然，之前引入了版本号的概念，也可以使用版本好进行回退：

```
git reset --hard commit_id
```

很多时候，我们不能准确记住 commit_id ,毕竟他很复杂，Git 提供了 

```
git reflog
```

命令，该命令记录了本地执行的每一次命令。



### 工作区和暂存区

把文件向 Git 版本库里添加的时候，是分两步执行的：

第一步是用：

```
git add 把文件添加进去，实际上就是把文件修改添加到暂存区。
```

第二步是用：

```
git commit 提交更改，实际上就是把暂存区的所有内容提交到当前分支。
```



### 管理修改

Git 跟踪并管理的是修改，而非文件。

对某文件做如下四次操作：

第一次修改 ->  `git add`  -> 第二次修改 ->  `git commit`

Git 管理的是修改，当使用 `git add` 命令后，在工作区的第一次修改会被放入暂存区，准备提交。但是，在工作区中的第二次修改并没由放入暂存区，所以， `git commit`只负责把暂存区的修改提交，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，可以用`git diff HEAD -- file_name` 命令查看工作区和版本库里最新版本的区别。



### 撤销修改

当对文件作出的修改，并不想保留。可以使用命令：

```
git status // 查看对文件进行的修改

git checkout -- file_name
```

命令`git checkout -- file_name`意思就是，把 file_name 文件在工作区的修改全部撤销，这里存在两种情况：一种是 file_name 文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态。一种是 file_name 文件已经添加到暂存区后，又做了修改，现在，撤销修改就是回到添加到暂存区后的状态。

**总之，就是让这个文件回到最近一次 git commit 或 git add 时的状态。**



当对文件作出了修改，并且还用 `git add` 到暂存区里了，在 ` commit` 之前，可以使用命令

```
git reset HEAD file_name
```

把暂存区的修改撤销掉，重新放回工作区。



### 删除文件

通常，会使用文件管理器删除没用的文件，或者是使用 rm 命令：

```
rm file_name
```

这时候，Git 会知道我们删除了文件，因此，工作区和版本库就不一致，使用 `git status`命令就会告诉我们哪些文件被删除了。

现在我们有两种选择，一是确实要从版本库中删除该文件，那就用 `git rm`删掉，并且`git commit`，这样文件就从版本库中删除了。

还有一种情况就是删错了，因为版本库里还有，所以可以轻松将误删的文件恢复到最新版本：

```
git checkout -- file_name
```

 

### 创建与合并分支

创建 `dev` 分支，然后切换到 `dev` 分支：

```
git checkout -b dev
```

 `git checkout` 命令加上 -b 参数表示创建并切换，相当于执行了以下两条命令：

```
git branch dev
git checkout dev
```

然后，可以使用 `git branch`命令查看当前分支。 `git branch`命令会列出所有分支，当前分支前面会标一个 * 号。

然后，就可以在 `dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

```
Creating a new branch is quick.
```

然后提交：

```
git add readme.txt 
git commit -m "branch test"
```

 现在， `dev`分支的工作就完成了，现在切换回 `master`分支：

```
git checkout master
```

切换到 `master`分支后，再查看`readme.txt`文件，会发现添加的内容不见了，因为那个提交是在 `dev` 分支上，而

`master`分支此刻的提交点并没有变。

现在，可以把`dev`分支的工作成果合并到`master`分支上：

```
git merge dev
```

上面的命令用于合并指定分支到当前分支，再查看`readme.txt`文件，就可以看到，和`dev`分支的最新提交是完全一样的。

合并完成后，可以放心的删除 `dev`分支：

```
git branch -d dev
```



创建并切换到新的`dev`分支，可以使用：

```
git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```
git switch master
```

