# git stash 和储藏

`git stash`是 *Git* 提供的一种保存工作进度的工具。*stash*这个单词本身就非常生动的说明了该命令的功能

```
v. 藏匿
n. 一些藏匿物
```

先通过 `git stash -h` 看看命令的帮助文档

```
usage: git stash list [<options>]
   or: git stash show [<stash>]
   or: git stash drop [-q|--quiet] [<stash>]
   or: git stash ( pop | apply ) [--index] [-q|--quiet] [<stash>]
   or: git stash branch <branchname> [<stash>]
   or: git stash save [--patch] [-k|--[no-]keep-index] [-q|--quiet]
		      [-u|--include-untracked] [-a|--all] [<message>]
   or: git stash [push [--patch] [-k|--[no-]keep-index] [-q|--quiet]
		       [-u|--include-untracked] [-a|--all] [-m <message>]
		       [-- <pathspec>...]]
   or: git stash clear
```

形式很简洁

* 直接使用：创建一个新的储藏
* list：列出已经存在的储藏
* show：显示（某个）储藏的具体信息
* drop：删除（某个）储藏
* pop：弹出顶部的一个储藏
* apply
* ...

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-5.png)
