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

由于现在是一个人孤苦伶仃的在写这个文档，我先在本地cp出一份代码仓库来做演示，做一些修改（其实就是增加了当前这个文件），然后提交到远程仓库上，