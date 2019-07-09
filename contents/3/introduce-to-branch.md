# 使用标签

在[2.0 小节: 你需要了解点Git底层实现](https://github.com/op-y/git-practice/blob/master/contents/2/first-glance-of-git-foundation.md)，中我们已经了解到 **分支** 这个概念了。其实，主流到到版本控制系统都以各自的方式实现了分支功能。

所谓分支，就是你可以把当前工作从代码仓库主干（相对分支的概念，其实也是一个分支，通常被称为master分支，你想改个名字也可以，是指master的叫法是行业认可的）上分离开来，你当前工作对代码对修改不会影响到已经对外发布的主干提交。

*Git* 的底层使用blob全量存储每一次提交所有文件快照的设计，使得 *Git* 的分支模型非常简单便捷（相对而言，SVN的分支模型就非常的重，操作也相当复杂），这也是 *Git* 如此成功的原因之一。这里我也不自主的推测，这种设计应该也是老李程序设计哲学的一个体现：围绕数据结构编程。好的数据结构是成功的关键。

在继续介绍 *Git* 分支之前，这里我们先回顾一下前文中的一副图，回忆一下我们先前对分支对介绍。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/draft.2-4.png)


*Git* 中每一次提交、文件和相关的信息都是以对象存储的，常见的对象有 commit、tree、blob 等（回忆一下！）。上图中的 master 和 bugfix 就是我们所说的 **分支** 即 **branch**，其实就是对commit对象的引用。与上一章最后介绍的标签（tag）不同的是：标签是静态引用（而且附注标签本身也是一个对象）指向一个固定的commit；而分支是一个动态引用，分支会跟踪工作的前进成为新commit对象的引用，指向新commit。图中HEAD可以说是一个分支的引用，它的功能是用来表示当前我们处于哪一个分支（或者commit位置，有时候会处于分离头指针状态的，这个分离头就是说HEAD和分支引用指向了不同的commit）。

至此，我们已经好好回忆了一下之前提到过的分支。我也不确认你对分支是不是有个形象的理解了，反正我是有了...下边我们要开始简单介绍一些分支的操作了，详细的分支操作后续的章节还会介绍的。

分支操作使用 `git branch` 命令，我们可以 `git branch -h` 看下它的帮助

```
usage: git branch [<options>] [-r | -a] [--merged | --no-merged]
   or: git branch [<options>] [-l] [-f] <branch-name> [<start-point>]
   or: git branch [<options>] [-r] (-d | -D) <branch-name>...
   or: git branch [<options>] (-m | -M) [<old-branch>] <new-branch>
   or: git branch [<options>] (-c | -C) [<old-branch>] <new-branch>
   or: git branch [<options>] [-r | -a] [--points-at]
   or: git branch [<options>] [-r | -a] [--format]

Generic options
    -v, --verbose         show hash and subject, give twice for upstream branch
    -q, --quiet           suppress informational messages
    -t, --track           set up tracking mode (see git-pull(1))
    -u, --set-upstream-to <upstream>
                          change the upstream info
    --unset-upstream      Unset the upstream info
    --color[=<when>]      use colored output
    -r, --remotes         act on remote-tracking branches
    --contains <commit>   print only branches that contain the commit
    --no-contains <commit>
                          print only branches that don't contain the commit
    --abbrev[=<n>]        use <n> digits to display SHA-1s

Specific git-branch actions:
    -a, --all             list both remote-tracking and local branches
    -d, --delete          delete fully merged branch
    -D                    delete branch (even if not merged)
    -m, --move            move/rename a branch and its reflog
    -M                    move/rename a branch, even if target exists
    -c, --copy            copy a branch and its reflog
    -C                    copy a branch, even if target exists
    -l, --list            list branch names
    --create-reflog       create the branch's reflog
    --edit-description    edit the description for the branch
    -f, --force           force creation, move/rename, deletion
    --merged <commit>     print only branches that are merged
    --no-merged <commit>  print only branches that are not merged
    --column[=<style>]    list branches in columns
    --sort <key>          field name to sort on
    --points-at <object>  print only branches of the object
    -i, --ignore-case     sorting and filtering are case insensitive
    --format <format>     format to use for the output
```

我们使用 `git branch` 或者 `git branch --list` 可以看到当前存在的分支，目前这个点我们只有一个master分支，有时候我们管master分支叫主干（trunk）分支或者就叫主干，这和后边要会介绍的开发模型有关。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/draft.3-0.png)

现在我们可以使用 `git branch demo` 命令来创建一个用于操作说明的分支。然后再使用 `git branch --list` 我们可以看到列表中已经有新的分支demo了，注意那个雪花号，表示我们虽然创建了新的分支，但是我们当前还是在master分支上。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/draft.3-1.png)

用 `git log --oneline --graph` 我们可以到提交历史和分支的状态，下图中的4个红色框框，表示了当前HEAD正引用master分支，远程origin的HEAD也指向它的master分支，同时本地仓库master分支、demo分支和远程的master分支都指向commit f0b636a。

我们可以使用 `git checkout [branch-name]` 命令来切换分支，这里我们 `git checkout demo`，系统会提示我们已经切换分支，如图。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/draft.3-2.png)

由于我们这里对代码仓库做了一些修改，并增加了当前正在编辑的这个文件，我们 `git checkout master` 回到master分支，做一次 **add content 3-0** 提交。




