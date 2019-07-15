# 变基

## 先说结论

其一：

> rebase是一个容易引起混乱的操作。

其二：

> **不要对在你的仓库外有副本的分支执行变基**。
> 
> 如果你遵循这条金科玉律，就不会出差错。否则，人民群众会仇恨你，你的朋友和家人也会嘲笑你，唾弃你。

其三：

> 永远不要rebase一个已经分享的分支，也就是说永远不要rebase一个已经在中央仓库中存在的分支，**只能rebase你自己的私有分支**。

## rebase 是啥？

我们先回过头去看一张图

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-30.png)

这是在前边章节演示两个分支合并，解决冲突后，进行了一次重新提交，最后得到的状态。两个分支最终融合到一起形成了一个 **afbc29b** 提交。

有些时候我们会想 **afbc29b** 提交存在的意义是啥？它真的有必要存在吗？我们本意上并没有想产生这次提交，只是为了完成分支合并不是吗？

从形式优雅的角度上说，我们可以将**master**分支和**auto-merge**分支工作搞成串行化，最终形成类似于：**f34c632** <-- **0189089** <-- **34c4219** <-- **735b474** 这样的线形提交历史。仔细一看，这就相当于在 **735b474** 提交后，我们进行一个操作，将**auto-merge** 分支的起点从 **0189089** 改成 **34c4219** 呀！

*Git* 还就提供了这样的操作 -- 变基，也就是 `git rebase`。

开始操作之前看看帮助 `git rebase -h`

```
usage: git rebase [-i] [options] [--exec <cmd>] [--onto <newbase>] [<upstream>] [<branch>]
   or: git rebase [-i] [options] [--exec <cmd>] [--onto <newbase>] --root [<branch>]
   or: git rebase --continue | --abort | --skip | --edit-todo

    --onto <revision>     rebase onto given branch instead of upstream
    --no-verify           allow pre-rebase hook to run
    -q, --quiet           be quiet. implies --no-stat
    -v, --verbose         display a diffstat of what changed upstream
    -n, --no-stat         do not show diffstat of what changed upstream
    --signoff             add a Signed-off-by: line to each commit
    --ignore-whitespace   passed to 'git am'
    --committer-date-is-author-date
                          passed to 'git am'
    --ignore-date         passed to 'git am'
    -C <n>                passed to 'git apply'
    --whitespace <action>
                          passed to 'git apply'
    -f, --force-rebase    cherry-pick all commits, even if unchanged
    --no-ff               cherry-pick all commits, even if unchanged
    --continue            continue
    --skip                skip current patch and continue
    --abort               abort and check out the original branch
    --quit                abort but keep HEAD where it is
    --edit-todo           edit the todo list during an interactive rebase
    --show-current-patch  show the patch file being applied or merged
    -m, --merge           use merging strategies to rebase
    -i, --interactive     let the user edit the list of commits to rebase
    -p, --preserve-merges
                          try to recreate merges instead of ignoring them
    --rerere-autoupdate   allow rerere to update index with resolved conflict
    -k, --keep-empty      preserve empty commits during rebase
    --autosquash          move commits that begin with squash!/fixup! under -i
    -S, --gpg-sign[=<key-id>]
                          GPG-sign commits
    --autostash           automatically stash/stash pop before and after
    -x, --exec <exec>     add exec lines after each commit of the editable list
    --allow-empty-message
                          allow rebasing commits with empty messages
    -r, --rebase-merges[=<mode>]
                          try to rebase merges instead of skipping them
    --fork-point          use 'merge-base --fork-point' to refine upstream
    -s, --strategy <strategy>
                          use the given merge strategy
    -X, --strategy-option <option>
                          pass the argument through to the merge strategy
    --root                rebase all reachable commits up to the root(s)
```

是不是看到这个帮助信息顿时头晕目眩？还好，有大佬给我们总结了`git rebase`的基本用法：

```
git rebase --onto <newbase> <since> <till> 
# 这是归一化用法，步骤为：
# git checkout till（改变HEAD）
# 将 since（不包括）到till（包括）标识的提交范围写到临时文件中
# git reset --hard newbase
# 从保存的临时文件的提交列表中，将提交逐一按顺序重新提交到重置之后的分支上
# 如果遇到提交已经在分支中包含，则跳过改提交
# 如果提交过程中遇到冲突，则变基暂停。然后视情况做 --continue/--skip/--abort 操作了
# 最终完成变基操作（活着回滚了）


git rebase --onto <newbase> <since>
git rebase <since> <till>
git rebase <since>
git rebase -i ... # 交互式变基
git rebase --continue # 变基过程中解决冲突后继续
git rebase --skip # 变基过程中遇到冲突跳过当时提交
git rebase --abort # 变基过程中遇到冲突放弃变基操作回到开始变基前状态
```

## 应用场景

介绍了这么多 `git rebase` 命令的信息，实际工作中用得最多的是两种场景。

### 清理提交

通常我们在自己私有分支上做特性开发/bugfix，过程中我们会时不时的做提交操作，一天之内通常也就做一两次push操作，将我们的工作推送到共享仓库上。如果直接push，会将过程中我们零零碎碎十几次甚至几十次提交一并推送，对共享仓库造成污染，这个时候我们可以在push操作前，使用rebase操作合并我们的提交，最后push一次有效的提交即可。

如下图，我们在**master**分支上做了六次提交（每次添加了一个文件，同时修改了前边添加的文件）模拟我们工作过程。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-72.png)

在我们执行push动作之前，我们执行 `git rebase -i` 命令，我们会进入交互式编辑页面，如下图

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-73.png)

这个页面信息就丰富了，首先 1-6 行是带处理提交，**#**开头的是提示信息。

每一行待处理的提交格式为：**命令 原commit-id 原commit-message**

提示信息告诉我们

* pick：使用这个提交
* reword：使用这个提交，但是修改提交信息
* edit：使用这个提交，但是暂停，修改提交
* squash：使用这个提交，但是合并到之前的一个提交上去
* fixup：同squash，但是丢弃提交信息
* exec <command>：运行shell命令
* break：在这里暂停
* drop：丢弃这个提交
* label <label>：给当前HEAD打一个标签？
* reset <label>：reset HEAD到label上
* merge ...

这里我们只需要把 2-6 行到 **pick** 改成 **squash** 就行了

如下图，由于没有冲突，*Git* 自动给我们完成了变基操作

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-74.png)

这个时候我们再看提交日志，原来到6次提交变成了1次提交，特别注意，**提交到的ID已经和原来不一样了**！

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-75.png)

再看看最后一次提交信息，已经是我们合并好之后的内容了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-76.png)

到这里我们就可以push这一个提交到共享仓库上了。

### 线性合并

线性合并是针对本节开始提出的疑问进行的操作。我们看下边这个例子。

先从**master**分支拉出一个**rebase-demo**分支，然后添加了**rebase-demo.txt**文件，并且修改前边**rebase-F.txt**文件，做了一次提交

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-77.png)

回到**master**分支上，添加了一个**master-while-rebse—demo.txt**文件（文件名单词拼写错了，将错就错了...)，做了一次提交

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-78.png)

查看提交日志，**master**和**rebase-demo**已经分叉了，这个已经是我们常见的状态了。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-79.png)

如果我们不想产生一个新的提交来将**rebase-demo**分支合并到**master**分支，怎么做?

先切换到**rebase-demo**分支，果断执行 `git rebase master`，看到 *Git* 自动为我们执行了变基操作

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-80.png)

查看提交日志，**rebase-demo**分支现在已经在**master**分支之前，而不是分叉状态了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-81.png)

切回到**master**分支，再执行`git merge rebase-demo`，由于**master**此时是单纯落后于**rebase-demo**，*Git* 自动做了一次快进，完成了合并操作

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-82.png)

最后查看提交日志，我们完成了一次线性合并。（这里我不说没有产生新的提交，理由可以自己想想）

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-83.png)


## 他山之石

* 如果是对本地私有的临时分支，则可以直接 `git rebase -i master` + `git merge` 产生一个快进，最终以一个提交展示在master分支上
* 如果是一个特别活动的分支，如feature分支、bugfix分支，那么永远不要使用rebase，而是 `git merge --no-ff`，这样该分支历史将永远存续在主分支上
* 只要你的不再有子分支的分支上需要rebase的所有提交还没有被push到共享仓库，就可以安全的使用rebase

## 小结

**变基**操作相当的复杂，我这里也只是了解了一点皮毛，实际工作中也很好碰到。最好的学习方法就是想到什么场景，自己搞个仓库，可劲折腾一番！
