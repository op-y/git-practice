# 撤销操作

人们常说 “世界上买不到后悔药”，可是 *Git* 就是这么牛，给我们提供了很多“后悔”的方法。常见的撤销操作有几种

* 通过 `git commit --amend` 来修改提交
* 通过 `git reset` 来撤销commit
* 通过 `git checkout` 来撤销对工作区的修改

下边通过实例在做说明。
先准备 **revoke-demo.txt** 文件，输入三行内容，如下图所示。

```
# git commit --amend

1st line.
```

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-49.png)  

保存好后，我们 `git add` `git commit` 做一次提交，提交信息为 *add content 2-4* ,如下图。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-50.png) 

接着往文件中添加两行内容，再做一次提交，提交信息为 *add one line in revoke-demo.txt* ,如下图。

```
# git commit --amend

1st line.
2nd line.
3rd line.
```

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-51.png) 

这时候回过神来，我们惊奇的发现，我擦，明明加了两行，提交信息却写了增加一行，这怎么能行。
这个时候我们就可以执行 `git commit --amend` 命令了，执行命令后回打开交互式页面，让我们填写新的提交信息，我们看到 *add one line in revoke-demo.txt* 赫然写在上边，然后还有一些友情提示，如图所示。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-52.png) 

我们把 *add one line in revoke-demo.txt* 改成了 *add two lines in revoke-demo.txt* 然后保存退出。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-53.png) 

这个时候再使用 `git log --oneline` 看下，我们发现没有产生新的commit，而是最后一次commit的信息被修改了，如下图。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-54.png) 

到这里我们就通过 `git commit --amend` 完成了对提交的修改。
继续往demo文件里添加两行如图。

```
# git commit --amend

1st line.
2nd line.
3rd line.
4th line.
5th line.
```

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-55.png) 

然后通过 `git add` 提交到暂存区，这个时候通过 `git diff --cached revoke-demo.txt` 可以看到暂存区和最后一次commit中该文件到diff，`git status` 看到的状态是有暂存文件待提交，没有工作区修改文件待暂存，见下图。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-56.png) 

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-57.png) 

这个时候如果我们想要撤销暂存咋整？
那我们可以执行 `git reset HEAD revoke-demo.txt` 来撤销暂存，之后我们 `git status` 看到修改的文件不是待提交，而是待暂存了，如下图所示。 

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-58.png) 

这里值得一提的是，`git reset` 这个命令绝不是仅仅用来撤销暂存的，这里只是因为我们指定了 `HEAD` 参数，又使用了默认 `--mix` 方式，所以实现的是撤销暂存的效果。`git reset` 主要功能是撤销提交，功能很强大，可以进一步区了解，这里暂时不多说。

如果我们想要彻底恢复到上一次提交后到状态（即没有添加最后两行），这个时候我们可以执行 `git checkout -- revoke-demo.txt` 命令，用最后一次 commit 中该文件覆盖当前工作区中文件，如下图所示，现在 **revoke-demo.txt** 文件中最后添加到两行就没了。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-59.png) 

同样的，`git checkout` 命令也有多种使用场景，需要深入去了解。

现在我们已经知道以上几种撤销操作了。但是上头的几个 `git` 命令的功能远不止如此。所以还需要结合 工作区/暂存区/项目仓库 三个不同的目标，深入的熟悉 `git reset` `git checkout` 命令的使用。此外还以了解 `git rebase` 命令的功能。
