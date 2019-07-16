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

形式很简洁：

* 直接使用：创建一个新的储藏
* list：列出已经存在的储藏
* show：显示（某个）储藏的具体信息
* drop：删除（某个）储藏
* pop：弹出顶部的一个储藏（看这名字就知道储藏是栈结构）
* apply
* ...

通常在什么情况下使用 `git stash` 呢？

基本上是当前工作正在进行中，突然有新需求过来，我们需要保存当前的工作进度准备一个干净的工作区和暂存区，切换到其它分支/或者就在当前分支上做其它工作。

下边我们用**master**和**stash-demo**两个分支来演示`git stash`操作，过程中会来回切换，可能会引起不适，见谅！


在**master**分支上：新建**stash.md**（就是本文档）加入暂存区，然后又对**stash.md**做了修改，之后再新建**stash-demo.txt**文件

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-5.png)

现在我们执行 `git stash` 命令，*Git* 提示将工作目录和暂存区工作进度（WIP：work in progress）放入储藏中

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-6.png)

此时 `git status -s` 查看，看到工作目录中还有一个未被跟踪对**stash-demo.txt**文件，由此我们可以判断默认情况下 `git stash` 并不保存未被跟踪对文件。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-7.png)

`git stash list` 看到当前有一个 **stash@{0}** 对存储了，还指明了储藏的位置在**master**分支的**c44a5cc**提交处，这里说明一下储藏是一个*栈（stack）*结构，后进先出，从新到旧依次0、1、2...这样编号

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-8.png)

这次我们执行 `git stash --include-untracked`，显式的指明要将未跟踪的文件放入储藏中，完事我们 `git status -s` 看到现在是一个干净的工作区了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-9.png)

`git stash list` 看到现在有两个储藏了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-10.png)

我们 `git checkout stash-demo` 切换分支，`git status -s` 确认**stash-demo**工作区是干净的

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-11.png)

在**stash-demo**分支上添加一个同名的**stash-demo.txt**文件并加入暂存区

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-12.png)

`git stash`

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-13.png)

`git stash list` 这时我们有一个在**stash-demo**分支上的储藏了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-14.png)

`git status` **stash-demo**分支进行储藏操作后，工作区也变干净了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-15.png)

`git chekcout master`回到**master**分支

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-16.png)

现在执行 `git stash apply stash@{1}` 我们应用编号1的储藏，系统提示未被跟踪的**stash-demo.txt**文件回来了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-17.png)

执行 `git stash drop stash@{1}` 删除这个已经apply的储藏，因为我们觉得它用不着了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-18.png)

此时`git stash list` 看储藏区剩下两个储藏，并且重新编号0和1了，原来的1号被删除了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-19.png)

我们`git add` `git commit`完成对这个文件的提交

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-20.png)

`git checkout stash-demo` 切换到**stash-demo**分支

这次我们使用 `git stash pop` 命令，pop会从储藏顶部弹出最近一次储藏，应用它，并删除它

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-21.png)

这个时候 `git stash list` 再看就剩下最早的一个储藏了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-22.png)

在**stash-demo**分支上完提交

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-23.png)

`git checkout master` 切换回**master**分支，`git stash pop stash@{0}` 弹出剩下最早的一个储藏，最后完成提交

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-24.png)

到此，`git stash`的演示结束。


存储命令还有一些其它的搭配，可以自己尝试玩一玩～

```
git stash apply --index # --index 选项重新应用暂存区的修改
git stash --patch # --patch 参数交互式的提示哪些改动想要储藏
git stash branch <branch name> # 从储藏创建一个分支
git clean # 清理工作目录
```