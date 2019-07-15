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

## 应用场景

## 他山之石

## 总结
