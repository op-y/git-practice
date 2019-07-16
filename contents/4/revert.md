# git revert

`git revert` 不知道怎么就放到补充部分来介绍了，这个操作我们可以管它叫**反转提交**，就是说，使用这个命令 *Git* 会对指定提交做一个反向的操作，然后应用到当前提交上。

`git revert` 常用于撤销历史上的某些提交。你可能要问题了，我们有resert，rebase这样的操作，为啥还要搞出一个revert操作来撤销提交呢？

这就涉及到一个很尴尬的情况了，如果你在和其他开发者共享的分支上做了一些错误操作，完事还给push到了共享仓库中，后来还被其他开发者给fetch/pull过去了，这个时候你想fix掉你的错误操作，你要怎么处理？

在本地仓库reset或者rebase再push或者强制push到共享仓库，你会被干死的。。。`git revert`提供了一种机制，将历史上提交做反向操作（添加文件就删除，修改文件就改回去），然后再提交再push修复错误。其它开发者此时能fetch/pull同步你的修复操作。

看一个演示：

我们添加了一个**revet-demo.txt**文件，做了三次提交，每次在文件里写了一行（心里话）

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-0.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-1.png)

其实心里话是不应该说出来的，所以我们打算撤销掉这三次操作

执行 `git revert 40fc3d4..659fd44`

*Git* 会切换到交互是编辑界面，针对每一个历史提交，会有一次反转提交，下图是撤销最后一次提交到反转提交编辑界面

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-2.png)

*Git* 友好地给我们提示了这个反转提交做了什么

分别做完三次反转提交后输出提示如下

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-3.png)

查看提交日志，多出了三次提交，撤销了前边的三次提交

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-4.png)

这个时候**revet-demo.txt**文件已经没了。