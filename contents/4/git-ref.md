# git引用

说完*Git*对象后，我们再回顾一下*Git*引用。上一张曾经出现过的图

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/draft.2-4.png) 

这张图我们曾经详细讲解过，经历了这么长一段*Git*之旅，你对图中**HEAD**、**master**、**bugfix**、**v0.1**有更加深入的了解了吗？

没错这些就是*Git*引用。

最简单的理解：引用就是为了不让我们费脑子记着哪些40位的Hash值，我们给有需要记忆的Hash值取了一个方便记忆的名字，只是这些名字的用法/用途各有不同。
最直观的理解：去看看**.git/refs**目录！

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-25.png)

在本地*Git*代码仓库目录**.git**下有首先能看到**HEAD**文件和**refs**目录，**HEAD**我们最后说。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-26.png)

在**refs**下有**heads**，**remotes**，**tags** 想到什么了吗？对！就是分支和标签！

* heads：本地分支
* tags：标签
* remotes：远程分支

不行我们看看这些目录下边的文件

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-27.png)

那为为啥管他们叫*Git*引用呢？我们`cat`看看几个文件

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-28.png)

全是40位的Hash值，这些Hash值就是*Git*对象，图中的这些都是提交对象。所以我们说的引用就是这些对象的引用！

顺道看一个有储藏的场景如下

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-29.png)

**refs**目录有个**stash**文件，这个文件里也是40位的Hash值，所以stash也是引用。

回到开头，我们看看**HEAD**文件

![git-practice](https://github.com/op-y/git-practice/blob/master/images/4/snip.4-30.png)

**HEAD**文件的内容是 `ref: refs/heads/master`，就是**master**这个引用，所以**HEAD**就是引用的引用，我们管这种引用叫做符号引用（symbolic reference)

至此对引用和引用的使用是不是有点恍然大悟的感觉？

* HEAD通常指向一个分支（引用）
* 分支就是一个提交对象的引用
* 标签就是一个标签对象引用
* 大部分时候 reset 就是在调整 分支（引用）同时HEAD跟随移动
* 大部分是有 checkout 就是在移动HEAD
* ...

当然关于*Git*引用也有很多底层命令

* git symbolic-ref
* git update-ref
* ...

你可以动手试试！
