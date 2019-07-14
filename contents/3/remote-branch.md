# 远程分支

远程分支这一节，是 2.5.[使用远程仓库](https://github.com/op-y/git-practice/blob/master/contents/2/remote-repository.md) 的延续。

介绍 *Git* 的时候，我们就盛赞了它分布式协作的特点。使用 *Git* 的每一个开发者，在本地都有一个完整的代码仓库，而且可以随心所欲地在本地仓库进行 分支添加删除合并/标签添加删除 操作；同时 *Git* 也允许不同开发者之间进行代码共享，开发者本地仓库和服务器上共享仓库进行代码共享。这样就引入了一系列问题。

* 如何避免push本地开发用的临时分支到中心代码仓库上
* 如何避免不同开发者做不同feature开发时使用了同名分支
* 如何避免开发者在本地随意创建的标签影响共享代码仓库
* 如何方便开发者向共享仓库或者其他开发者的仓库push代码
* 如果有效区分多个开发者的代码仓库
* 如何指定默认交互的代码仓库
* ...

这些就是远程分支（远程仓库）需要解决的问题。

## 远程仓库

在 2.5.[使用远程仓库](https://github.com/op-y/git-practice/blob/master/contents/2/remote-repository.md) 中我们已经简单介绍过 **远程仓库** 了，当时我们只是为了强调 *Git* 的分布式。我们还使用

`git remote add celine http://github.com/celineWong7/git-practice`

添加了一个远程仓库，参考之前的图

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-64.png)

接下来我们要稍微深入远程仓库和远程分支了，在这之前我们回到 **.git** 目录，对，就是那个绕不开 *Git* 仓库目录，我们可以执行

`cat config`

看看这个配置文件

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-50.png)

图中config有4个小节（ini格式）分别是

* core
* remote "origin"
* branch "master"
* remote "celine"

其中**core**是对*Git*本身对一些参数设置可以从其文档中查询，这里我们按下不表。剩下3个小节 **remote "origin"** **branch "master"** **remote "celine"** 从名字我们就能看（猜）出是远程仓库origin、celine和本地branch分支有关的配置了。

**remote** 节中，有 url、fetch配置项，其实还有pushurl等配置项，这些配置项从名字也能猜出来它们的含义

* url：远程仓库的地址
* fetch：如果从这个远程仓库执行fetch操作，拉取回来引用和本地引用如何处理对应关系；我们现在看到的 **+**号表示强制引用替换；**:**号前是本地分支引用目录，后边远程分支引用目录，**\***通配符表示所有分支，这里配置即表示fetch回来的远程分支和本地同名分支进行引用替换
* pushurl：如果将本地仓库push到这个远程仓库，实际使用的URL地址

**branch** 节中，有 remote、merge配置项，这些配置含义也很清晰

* remote：追踪的远程仓库
* merge：远程仓库的分支引用

有这两项配置，我们在本地仓库 **master** 分支上进行`git pull` `git push` 操作时就知道要与 **origin** 远程仓库即 **git@github.com:op-y/git-practice.git** 的 **master** 分支进行交互。这里说到了**跟踪**的概念，下一段会介绍。

同时我们在本地仓库 `git branch feature-remote-demo` 创建了一个新的分支 **feature-remote-demo**

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-51.png)

为啥在**config**文件中没有看到有关这个分支的小节呢？因为这个分支仅仅是一个本地分支，并且没有和其他分支建立任何联系，所以正在**config**文件中就看不到了。
 
## 跟踪分支

从一个远程跟踪分支检出一个本地分支会自动创建 **跟踪分支** （被跟踪到分支叫做 **上游分支** ），**跟踪分支是与远程分支有直接关系到本地分支**。如果在一个跟踪分支上不带参数的执行 `git pull` `git push` 操作，*Git* 能自动地识别区哪个远程仓库的哪个分支上做相应操作。例如我们从*Github*上克隆这个仓库的时候，*Git*就自动将*Github*上的仓库设置成了**origin**远程仓库，**origin/master** 是远程分支，在本地仓库创建了**master** 分支。

我们可以执行 `git branch -vv` 看到本地分支和远程分支的关系

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-52.png)

顺道一提，我们执行 `git branch -r` 可以看到远程分支信息

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-53.png)

当前我们只是看到了**origin**这一个远程仓库的分支信息，没有看到**celine**远程仓库的信息，这是因为我们仅仅是添加了**celine**远程仓库，没有拉取任何该仓库的数据。我们执行 `git fetch celine` 再次用 `git branch -r` 就能看到了

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-54.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/3/snip.3-55.png)

## 从远程分支拉取：git fetch 和 get pull

## 推送到远程分支

## 删除远程分支

## 总结
