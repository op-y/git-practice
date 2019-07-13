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

### 自动合并

*Git* 非常智能，当发现不同分支中存在冲突时，能判断冲突类型，如果冲突能自动的解决（在自动合并的策略下）*Git* 会尝试自动解决冲突。这一类场景通常包括快进（Fast-Forwarding）合并，不同分支添加/修改了不同的文件，不同分支修改相同文件的不同部分等。下边我们就演示一把这类场景。

### 逻辑冲突解决
### 文件冲突解决
### 树冲突解决
