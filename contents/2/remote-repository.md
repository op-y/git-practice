# 使用远程仓库

我们一开始就了解到 *Git* 是分布式版本控制系统，所以和自己服务器上的代码仓库、其他/她开发者机器上到代码参考交互基本是不可避免到。这个时候需要用到 `git remote` 命令了。先来看一下命令到help

```
usage: git remote [-v | --verbose]
   or: git remote add [-t <branch>] [-m <master>] [-f] [--tags | --no-tags] [--mirror=<fetch|push>] <name> <url>
   or: git remote rename <old> <new>
   or: git remote remove <name>
   or: git remote set-head <name> (-a | --auto | -d | --delete | <branch>)
   or: git remote [-v | --verbose] show [-n] <name>
   or: git remote prune [-n | --dry-run] <name>
   or: git remote [-v | --verbose] update [-p | --prune] [(<group> | <remote>)...]
   or: git remote set-branches [--add] <name> <branch>...
   or: git remote get-url [--push] [--all] <name>
   or: git remote set-url [--push] <name> <newurl> [<oldurl>]
   or: git remote set-url --add <name> <newurl>
   or: git remote set-url --delete <name> <url>

    -v, --verbose         be verbose; must be placed before a subcommand
```

这条命令可以完成很多工作。包括
* 查看远程分支
* 添加远程分支
* 重命名远程分支
* 删除远程分支
* ...
需要注意到是，这里我们说到 **远程** 是站在本地仓库到视角上看的，所以相关操作首先影响到是本地到相关内容。如果站在远程代码仓库的视角上看那就是本地代码仓库了。

由于现在是一个人孤苦伶仃的在写这个文档，我先在本地cp出一份代码仓库来做演示，做一些修改（其实就是增加了当前这个文件），然后提交到远程仓库上。然后我们看看本地原始仓库和复制仓库的提交日志，如下图，明显能看到复制出的日志多一次 *add content 2-5* 的提交。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-60.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-61.png)

现在我们使用 `git remote` 和 `git remote -v` 看看原始仓库的远程仓库信息，如下图，这里就只有一个origin的远程仓库啦。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-62.png)

也可以用 `git remote show [remote-name]` 命令看看远程仓库详细信息。这个命令会去查询远程仓库的信息，发现本地仓库落后于远程仓库，就是下图中的 **local out of date** 。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-63.png)

接下来使用 `git remote add` 添加一个远程仓库，由于之前 Celine Wang fork过我这个仓库，我就用Celine的仓库地址了。完事之后再 `git remote -v` 就可以看到名为 celine 的新远程仓库了。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-64.png)

这里只是演示了add的操作，如果对celine远程仓库有读写权限，之前对于自己远程仓库的操作都可以用在celine的远程仓库上。
接着来使用 `git fetch origin` 拉取origin远程仓库，这个命令会将远程仓库有而本地仓库没有的提交（就是本地复制仓库做的那一次提交）拉取下来。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-65.png)

这个时候我们只是把远程仓库的提交信息（还包括blob文件等）拉取到了本地仓库中（详细内容可以看ref信息），但是本地仓库到master分支还是落后于origin远程仓库的master分支到。
于是我们执行 `git merge master origin/master` 将远程分支领先的提交合并到本地master分支，如下图。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-66.png)

这个时候再通过 `git remote show origin` 看，发现我们本地仓库master分支已经 **up to date** 了。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-67.png)

好了，到这里，远程仓库的简单介绍结束了。

多说一点，这里提到了 `git fetch` + `git merge` 的操作，实际上这个操作实现了 `git pull` 的功能。至于两种方式孰优孰劣，尚有争论，我们的关键是理解它们背后的机理。