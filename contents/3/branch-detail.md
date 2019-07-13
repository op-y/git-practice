# 分支创建、合并、管理以及冲突解决

在上一小节中已经简单介绍过 **分支** 了。我们还做了 查看分支/创建分支/合并分支 操作。接下来要详细讲解分支的内容。

这一块思来想去不知道怎么讲。主要是 分支，远程分支，分支合并，冲突解决，多人协作 这些概念杂糅在一起傻傻分不清。这些概念中通常又是你中有我、我中有你也不大好分开来讲，例如，讲分支就必然讲合并；有本地仓库分支必然有远程仓库分支（不然就没有分布式的意义了）；有多人协作那就有每个人仓库里的相同分支；自己仓库里不同分支可能有冲突合并；不同开发者仓库里分支要合并可能有冲突合并... 所以这部分就先声明一个重点，然后详细讲解，如此这般进行。

## 分支创建
其实在上一节中我们已经操作过创建分支了，我们当时用 `git branch demo` 创建了一个demo分支。这也是大部分情况下的创建分支操作：暂停手头的工作，从当前状态下创建一个新分支，切换到新分支上完成某些新特新的开发，新特性测试OK之后合并回原有分支。

为了不做重复的演示，这里我们考虑另一种场景：很久之前我们开发完某些功能（这里我们就创建一些文件做一些提交），并且以 v1.1 版本（tag）对外发布，正当我们开心地写着代码，突然产品经理说 v1.1 版本有个Bug让我修复！这个时候我们咋处理？我们需要从 v1.1 对应的位置创建一个新分支，就叫 bugfix-v1.1 吧。然后切换到 bugfix-v1.1 上进行bugfix工作。

开始吧！

新建 **v1.1-release.txt**、**after-v1.1-1.txt**、**after-v1.1-2.txt** 文件，分别做一次提交。**v1.1-release.txt** 这次提交当成是v1.1版本发布，我们做个标签 **v1.1** 。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-9.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-10.png)

发现v1.1的Bug，我们从v1.1拉出新分支 **bugfix-v1.1**：`git checkout -b bugfix-v1.1 v1.1`

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-11.png)

这个时候我们查看提交日志，已经有 master和bugfix-v1.1两个分支了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-12.png)

来，把Bug修了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-13.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-14.png)

完成Bug修复，做一次 **finish v1.1 bugfix** 的提交，图形化查看提交日志如下

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-15.png)

至此我们已经完成了在新分支上的Bug修复（也演示了从历史提交上创建新分支）

## 分支合并

继续上边的任务，我们演示分支的合并。分支合并使用 `git merge` 命令，这个命令功能也挺多，看一下help

```
usage: git merge [<options>] [<commit>...]
   or: git merge --abort
   or: git merge --continue

    -n                    do not show a diffstat at the end of the merge
    --stat                show a diffstat at the end of the merge
    --summary             (synonym to --stat)
    --log[=<n>]           add (at most <n>) entries from shortlog to merge commit message
    --squash              create a single commit instead of doing a merge
    --commit              perform a commit if the merge succeeds (default)
    -e, --edit            edit message before committing
    --ff                  allow fast-forward (default)
    --ff-only             abort if fast-forward is not possible
    --rerere-autoupdate   update the index with reused conflict resolution if possible
    --verify-signatures   verify that the named commit has a valid GPG signature
    -s, --strategy <strategy>
                          merge strategy to use
    -X, --strategy-option <option=value>
                          option for selected merge strategy
    -m, --message <message>
                          merge commit message (for a non-fast-forward merge)
    -F, --file <path>     read message from file
    -v, --verbose         be more verbose
    -q, --quiet           be more quiet
    --abort               abort the current in-progress merge
    --continue            continue the current in-progress merge
    --allow-unrelated-histories
                          allow merging unrelated histories
    --progress            force progress reporting
    -S, --gpg-sign[=<key-id>]
                          GPG sign commit
    --overwrite-ignore    update ignored files (default)
    --signoff             add Signed-off-by:
    --verify              verify commit-msg hook
```

我们先 `git checkout master` 切回到master分支上

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-16.png)

使用 `git merge bugfix-v1.1` 命令，我们将 bugfix-v1.1 分支合并到 master分支上，由于这里我们仅仅是在 **bugfix-v1.1** 上添加文件和修改了文件，*Git* 很智能的帮我们判断出两个分支没有冲突直接自动合并了（冲突问题后边还会说，是否自动合并我们也可以修改*Git*默认合并策略）

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-17.png)

合并后从提交日志里可以看到两个分支状态如下，由于合并产生了一个新的提交。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-18.png)

## 分支管理

继续上边的任务，除了分支创建和合并，我们还能做一些分支管理操作。例如分支删除。

上边 **bugfix-v1.1** 分支合并入 **master** 分支后，Bug修复工作已经完成了。我们可以删除这个分支了。

`git branch -d bugfix-v1.1` 删除之

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-19.png)

再次查看提交日志，已经看不到 **bugfix-v1.1** 分支了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-20.png)

除了分支删除操作，我们还可以

`git branch -l` 列出存在的分支

`git branch -m` 重命名某个分支

等等...

## 冲突解决

工作过程中，有很多操作都有可能产生我们这里说到的 **冲突**（conflict）。

* 本地仓库不同分支上添加了新文件要合并
* 本地仓库不同分支上修改相同文件要合并
* 远程仓库相同/不同分支上自己/其他开发者提交了文件修改需要拉取/合并（fetch+merge or pull）到本地仓库
* ...

当遇到以上这些操作时，我们就要做冲突解决。

### 自动合并与手动合并

*Git* 非常智能，当发现不同分支中存在冲突时，能判断冲突类型，如果冲突能自动的解决（在自动合并的策略下）*Git* 会尝试自动解决冲突。这一类场景通常包括快进（Fast-Forwarding）合并，不同分支添加/修改了不同的文件，不同分支修改相同文件的不同部分等。

当遇到不同分支修改了同一个文件的相同部分时，*Git* 就无法为我们自动合并了，毕竟 *Git* 不能代替人来做决定。这个时候就需要我们人工修改有冲突的文件，*Git* 的厉害之处在于他会在文件中标示出当前分支和并入分支的修改，方便我们解决冲突。我们只要删除 *Git* 引入的提示符（如 >>>>>>> 、<<<<<<<、=======）并把文件修改成我们想要的样子就行。解决完冲突后，我们同样的来一次 `git add` 和 `git commit` 操作就算是完成了合并。

下边我们就演示一把这类场景。

在master分支上新建了一个 **branch-master.list** 文件，同时在 **merge-demo.txt** 文件增加了 **第七** 行 `Line7: 《精通Puppet配置管理工具》`

创建了 **auto-merge** 分支，在 **auto-merge** 分支上新建了一个 **branch-auto-merge.list** 文件，同时在 **merge-demo.txt** 文件增加了 **第七** 行 `New book: 《咖啡世界地图》`

完成这些操作状态如下边3个图。


![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-21.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-22.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-23.png)

现在我们在 **master** 分支上执行 `git merge auto-merge`，虽然我们希望 *Git* 能帮我们自动合并，但是很遗憾提示有冲突了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-24.png)

`git status`	我们看到出现了之前我们没有看到过的状态 **Unmerged  paths**，*Git* 友好的提示我们 **fix conflicts and run "git commit"**，即解决冲突然后做一次提交。

这里我们还发现对于 **branch-auto-merge.list** 文件，提示的是 **new file**，表示 *Git* 发现这个是一个新文件，在当前master分支上没有冲突（是否冲突还是由我们自己说了算，我们强行认为它冲突也可以直接删了它...），所以自动给我们合并了。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-25.png)

在解决冲突之前，我们稍微深入看看 *Git* 做了一些啥。在很早之前我们介绍过的 **.git** （还记得吧？）目录下执行 `ls -l` 我们可以看到 **MERGE_HEAD**、**MERGE_MODE**、**MERGE_MSG** 文件，`cat`一下三个文件，看看内容。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-26.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-27.png)

这些文件就是 *Git* 在处理合并冲突过程中保存的一些信息

* MERGE_HEAD：合并分支（auto-merge）的commit id
* MERGE_MODE：合并状态
* MERGE_MSG：合并信息

在工作区中，我们用 *Git* 工具 `git ls-files -s` 可以看看暂存区文件

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-28.png)

发现有3个同名文件 **merge-demo.txt**， 这三文件编号分别是1、2、3，编号分别表示

* 1：当前冲突的共同祖先版本（即各分支修改以前的样子）
* 2：当前冲突文件在当前分支中的副本
* 3：当前冲突文件在并入分支中的副本

由于忘记截图了，顺道说一下，当前工作区中的 **merge-demo.txt** 由于merge操作，已经变成了引入 *Git* 冲突提示的样子（加入了 >>>>>>> 、<<<<<<<、======= 和不同分支里做的修改）。

现在我们按照自己的需求，修改好 **merge-demo.txt** 并完成提交，再到 **.git** 目录中 `ls -l`，发现这些MERGE开头的状态文件已经消失了。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-29.png)

这时的提交日志状态如下，这个模式的图形我们已经见过好几次了。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-30.png)

### 远程分支合并

冲突解决中还有这么一类常见的情况，就是本地仓库的工作要提交（push）到远程仓库中，系统提示无法合并，原因是其他开发者已经 **抢先** 将他的工作提交到远程仓库上。这个时候我们需要将他的提交拉取到本地做一次合并。

来，开始我们的演示。

我们从一个副本代码仓库创建一个文件 **git-practice-replica.txt**，做一次提交并推送（push）到远程仓库

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-31.png)

编辑完成本地代码仓库文件后，我们 `git status` 查看当前状态，`git remote show origin` 查看远程**origin**仓库状态，现在是本地仓库有改动，并且落后远程仓库了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-32.png)

我们 `git add` `git commit` `git push` 三连，发现果然报错了，还给了提示，就是："远程仓库有我们本地仓库还没有包含的工作，可能是其他仓库推送上去的，我们需要集成远程仓库的这些工作，然后重新推送“

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-33.png)

来，`git pull` 直接拉取自动合并了（因为没有冲突） 

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-34.png)

然后 `git push` 完成提交到远程仓库。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-35.png)

### 逻辑冲突解决
### 树冲突解决
