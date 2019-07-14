# 开发模型

这一篇是一些偏重“工程”学的内容，自己在这方面也没多少经验，所以只能是道听途说了。

介绍*Git*使用的过程中，我们多次提及了*Git*对分布式特点。不同的开发者可以在本地拥有一份完整的代码仓库，同时他们之间还能相互分享自己的工作。这听上去非常的美好，但是仔细想想也会存在一些问题，其中的关键还是回到**冲突**上，大家协同工作过程中，如何减少冲突，避免代码仓库的混乱，顺利推进项目的进行，就是开发模型需要研究的内容。

这里我们使用的是 **开发模型** 这个词，另一个词语 **分支模型** 也经常出现，我个人对二者不做太大区分。自己的见解是 **分支模型** 强调分支在协同开发过程中的作用，而 **开发模型** 强调的是不同开发者的角色在开发过程中对作用（或者工作），结合在一起就是不同角色的开发者，如何利用好分支这一工具，合理地开展协作，最终顺利完成开发工作。

后续 **开发模型** 和 **分支模型** 会混着用，各位看官不要介意。后边介绍的内容也不一定是并列或者层次的关系，想一出写一出了。

同时声明一下，这一节图都是从网页上剪下来的，传统做法 -- 请提示，侵删～

## Gitflow

*Gitflow* 是 Vincent Driessen 2010/01/05（文章上的发表时间）提出的一种基于*Git*分支的开发模型。这篇文章的链接：[A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-0.png)

文章介绍了利用 *Git* 分支这一强大工具，实现多人之间分布式同时又共享中心仓库的开发模式。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-1.png)

文章引入了分支的一些分类

主要分支（main branches）

* master
* develop

支持分支（supporting branches）

* feature branches
* release branches
* hotfix branches

其中特性分支（feature branches）从开发分支（develop）创建最终合并到开发分支

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-2.png)

释出分支（release branches）从开发分支创建，最终合并到开发分支和主分支（master）

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-3.png)

修复分支（hotfix branches）从主分支创建，最终合并到开发分支和主分支

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-4.png)

当然 Vincent Driessen 表示他的经验很成功。至于你要不要用这种模式，见仁见智了，网上也有diss这种模式的：[Gitflow有害论](http://insights.thoughtworkers.org/gitflow-consider-harmful/)，毕竟没有好不好，只有合不合适嘛。

## 单主干开发模式（Trunk Based Development）

所有开发工作都在**master**分支上进行，同时利用pipeline做持续集成，利用不同的release分支进行发布和跟踪。这种模式似乎也被叫做 **先锋主干多稳定分支**

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-5.png)

## 持续发布流水线

项目如果使用了CI/CD，可能就有开发环境、测试环境、生产环境，测试环境又有测试和预发布等细分。这个时候也许你会采用这种 **持续发布流水线**

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-6.png)

## 先锋主干多稳定分支

参考单主干开发模式

## 守护主干多先锋分支

主干**master**分支是稳定分支，拉出先锋分支开发新功或者做bugfix，完成工作后merge回到主干进行发布。

目前我们应该就是采用的这种开发模式！！！

## 主干无分支

这种一条道走到底的方式估计现在很难见到了吧...

## 守护主干单分支

master + 一个branch，紧急改动在主干上进行，正常开发在分支上进行，二者双向同步。这种方式还挺有意思...

## 卖主分支（vendor branches）

这种模式主要用在对开源项目的二次开发上，所谓卖主分支就是项目中专门有一个分支和上游分支同步，一旦上游分支（开源社区项目）有新的发布，我们就fetch到本地的项目中，再做其它操作。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/pic.3-7.png)


## 协同模型

除了上述道听途说的开发模型，从资料里还了解到一些协同模型概念

* 经典Git协同模型
* Topgit协同模型
* 子模组协同模型
* ...

这些内容恕我学浅，没研究过，有机会再讨论吧～


## 总结

关于开发模型就这些了，总结一句话：没有最好，只有最合适！
