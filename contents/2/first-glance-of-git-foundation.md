
# 你需要了解点Git底层实现

## 文件的三种状态
对于任何一个文件，在 *Git* 内都只有三种状态：

* 已修改（modified）-- 已修改表示修改了某个文件，但还没有添加到暂存区和提交保存  
* 已暂存（staged）-- 把已修改的文件放在下次提交时要保存的清单中  
* 已提交（committed）-- 已提交表示该文件已经被安全地保存在本地数据库中了  

可见 *Git* 管理项目时，文件流转的三个工作区域：Git 的工作区，暂存区，以及本地仓库。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/draft.2-0.png)  
每个 *Git* 项目下都有个 **.git** 目录，它就是 *Git* 版本数据库，里边记录了版本控制的元数据和对象数据。我们每次 `git` 提交和检出操作其实都是在像这个目录下添加数据或者从这个目录中提取数据。当前项目 **.git** 目录内容如下图  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-0.png)  
暂存区是 *Git* 最成功的设计之一。暂存区是个简单的文件，一般都放在 **.git** 目录中。有时我们会把这个文件叫做索引文件（index文件），后续会有稍微详细的介绍。

## 一个提交的示例
为了更好的进行演示，我们先做一次提交 `git commit -m "clear workspace for staging demo"`  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-1.png)  
从输出结果可以看到本次提交生成的commit ID前几位是 **25012c9** （完整的commit ID是40位的 *SHA1* 哈希值，如何生成该哈希值的，后边会说到），然后我们创建一个新文件 **staging-demo.txt**，文件内容只有一行

```
What's staging?
```

这个时候我们尝试提交一下看看 `git commit -m "craete a staging-demo.txt"`，然后我们看到 *git* 提交操作并未成功，输出信息大意是 **当前我们在master分支上，我们的分支状态是最新的，但是工作区有未被Git跟踪的文件**  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-2.png)  
我们可以执行一下查看状态命令 `git status`  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-3.png)  
命令输出提示 **有未被Git跟踪文件**，用户可以使用 `git add` 命令开始跟踪文件（实际上就是新文件添加到暂存区），这个时候我们根据提示执行命令 `git add staging-demo.txt` 开始对文件 **staging-demo.txt** 进行跟踪，然后再执行命令 `git status` 看看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-4.png)  
这个时候命令输出显示变成 **有文件变更等待被提交** 了，使用 `git status -s` 使用缩略方式看看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-5.png)  
看到文件旁边有个绿色的 **A** 标记（注意位置在最左边，在不同操作系统上显示可能颜色会有差别），表示暂存区有添加（Append）新文件。这个时候我们可以执行提交了 `git commit -m "craete a staging-demo.txt"`  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-6.png)  
这次我们看到提交成功了，本次提交生成的commit ID是 **e239d7d**，到这里我们已经完成了一次提交工作。  

**接下来我们再一遍！**
修改 **staging-demo.txt** 文件新增一行内容 **Add a new line**，这个时候我们使用 `git status` 和 `git status -s` 看看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-7.png)   
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-8.png)   
命令输出提示现在是有等待被添加到暂存区的变更，缩略输出下看到文件旁边有个红色的 **M** 标记（注意位置，这次不在最左边而是在左边第二个字符位置，最左边有个空格。），表示工作区域有被修改（Modify）到文件。再演示一遍不添加暂存区直接提交的情况 `git commit -m "add a new line in staging-demo.txt"` ，这次依然是失败的  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-9.png)  
为了对比着查看状态，我们这次再创建一个文件 **staging-demo-again.txt** ，内容为
  
```
Hello World!
``` 

然后执行 `git add staging-demo.txt` 将 **staging-demo.txt** 到修改添加到暂存区 和 `git status` 查看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-10.png)  
输出显示 **staging-demo.txt** 的修改等待提交，**staging-demo-again.txt** 等待被添加到暂存区，再执行 `git add staging-demo-again.txt` 将 **staging-demo-again.txt** 添加到暂存区，用 `git status` 以及 `git status -s` 查看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-11.png)  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-12.png)  
现在两个文件都在暂存区中等待被提交，我们再修改一下 **staging-demo-again.txt** 文件（Hello World！复制了两行），在用 `git status` 以及 `git status -s` 查看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-13.png)  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-14.png)  
看到了没，这次缩略状态显示，文件左边既有绿色的暂存区状态又有红色的工作区状态（为了看到这个折腾了一圈～），继续执行 `git add staging-demo-again.txt` 将文件修改添加到暂存区，执行 `git statu -s` 红色工作区修改状态没了  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-15.png)  
终于我们可以完成这次演示工作了 `git commit -m "add a new file staging-demo-again.txt"`  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-16.png)   

这个示例，其实是想说明 *Git* 的基本操作流程： 
  
* 在工作区中修改某些文件。  
* 对修改后的文件进行快照，然后保存到暂存区。  
* 提交更新，将保存在暂存区域的文件快照永久转储到 **.git** 目录（本地仓库）中。

也就是之前我们说的 **modify --> git add --> git commit** 三部曲。需要注意的是，未被添加到暂存区的工作区文件修改其实可以使用 `git commit -a` 来提交（相当于先将所有修改添加到暂存区然后提交），不过很少这么做。

> 注意：上边示例中，命令输出在不同系统下是会有差异的，但是结论是一样的。

## 暂存区（staging）
之前我们查看 **.git** 目录下文件和说到暂存区时有提到过，暂存区主要是 **.git** 目录下的索引文件 **.git/index** (当然还有被‘暂存’的对象文件)。我们来看一个操作演示（这个图中终端显示和之前有些不同，因为是在另一台Linux机器上操作的）  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-17.png)  
我们先用 `ls --full-time` 命令查看 **index** 文件，记住此时文件最后修改时间是 **2018-04-23 11:35:58.000000000 +0800**，我们用 `git status` 查看状态，当前项目没有任何变更。  
我们用 `touch` 命令更新一下之前 **staging-demo.txt** 文件，再用 `ls --full-time` 查看一下 **index** 文件，发现时间戳没有变化。  
执行 `git status`，然后再次使用 `ls --full-time` 查看 **index** 文件，神奇的事情发生了，**index** 的最后修改时间发生了变化，现在是 **2018-04-23 21:46:48.000000000 +0800** 了。  
这个演示说明了一个事实，暂存区（ **index** 文件）记录了工作区的文件最后修改时间，`git status` 会首先使用文件修改时间等文件元信息判断工作区文件是否发生变化，只有发生变化的文件，才会读取文件，获取文件变化的内容。  
到目前为止，我们了解到 *git* 版本控制系统可以用下图表示  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/draft.2-1.png)  
**.git/index** 实际上是一个文件索引目录树，记录了文件（可以理解为看到的文件名）和对象实体（*git* 中真正存储着的文件）之间的联系。  
图中的 **HEAD**、**master** 以及那些箭头，通过后续的讲解就会知道其含义。

## Git 对象
介绍 *Git* 对象之前，先知晓一个事实。以前的许多版本控制系统，通过记录文件的变化（类似我们在1.1中看到的 **diff.txt**）进行版本控制；*Git* 则直接保存文件快照（文件在保存时刻的完整内容）进行版本控制。这些文件快照以及和这些文件快照相关的内容在 *Git* 中以二进制的形式保存，这些二进制文件就是我们要介绍的 *Git* 对象。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/draft.2-2.png)  
上图中 *Git* 版本控制，每个版本中未发生变化（相对之前版本）的文件，其实就是指向之前文件的链接。  
接下来我们从最后一次提交开始介绍 *Git* 对象。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-18.png)  
通过 `git log --pretty=raw` 提交日志查看命令（`--pretty=raw` 参数会显示提交对象的内容），我们看到最后一次提交（一个修改错别字到提价 **content 2-0 part 1 bugfix**）包含了以下信息  

```
commit f71f3b6723f58cb17491b5b9977326e324b7ea11
tree 7a0a43a6376f31d0a265a44383395cdc8682297f
parent 175f8f4f46356899465113c2d7d9089d661ef13c
author yezhiqin <ye.zhiqin@outlook.com> 1524493318 +0800
committer yezhiqin <ye.zhiqin@outlook.com> 1524493318 +0800

    content 2-0 part 1 bugfix

```

commit、tree、parent 三个40位十六进制的值（其实就是前边提到过的SHA1哈希值），作者信息author，提交人信息committer，以及单独的一行提交说明。关键我们就需要看那3个40位十六进制数表示什么了。  
接下来有请我们的 **.git** 目录了（对就是那个本地仓库）  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-19.png)  
**.git** 下有个 **objects** 目录就是用来保存 **Git** 对象（二进制表示的数据）的。我们 `ls` 看到 **object上** 下有很多2位十六进制数命名的目录，结合上边提交日志里 `commit f71f3b6723f58cb17491b5b9977326e324b7ea11` 以f7开头，我们看下 **f7** 目录，`ls` 发现里边现在有两个文件，其中一个是 **1f3b6723f58cb17491b5b9977326e324b7ea11**，你有没有发现什么？  
**f7 + 1f3b6723f58cb17491b5b9977326e324b7ea11 = f71f3b6723f58cb17491b5b9977326e324b7ea11**  
提交日志信息里的40位十六进制数其实就对应着一个objects文件。  
接下来有请一个 *Git* 底层命令 `git cat-file` ，这个命令用来查看对象文件  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-20.png)  
分别执行 `git cat-file -t f71f3b6723f58cb17491b5b9977326e324b7ea11` 和 `git cat-file -p f71f3b6723f58cb17491b5b9977326e324b7ea11` 我们可以看到 **f71f3b6723f58cb17491b5b9977326e324b7ea11** 的对象种类是 *commit* 以及对象内容（就是刚刚看到的提交信息）。  
同理可以对提交信息中tree和parent对应的40位十六进制数进行这个操作。  
我们先看 **tree 7a0a43a6376f31d0a265a44383395cdc8682297f** 对应了一个类型位tree的对象，内容为   
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-21.png)  

```
100644 blob 94a9ed024d3859793618152ea559a168bbcbb5e2	LICENSE
100644 blob 41b34509a896b02b7efce0ae4b94a8283f9840b7	README.md
040000 tree 43b40085570613966ba5141036ddaffea96f2cc2	contents
040000 tree ca8e4c1e9088b9498b3da18893b57b6dd8c262b4	images
```

其实很直观能看出对应了几个表示文件的 *Git* 对象。尝试看看 **100644 blob 41b34509a896b02b7efce0ae4b94a8283f9840b7	README.md**  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-23.png)  
是一个blob对象（big binary object），文件内容就是README.md的容。而tree对象内容对应的4个项目就是我们当前项目里的文件和目录，所以 *tree对象记录了当前提交对应文件的版本*。  
再看 **parent 175f8f4f46356899465113c2d7d9089d661ef13c** 指向了另一个commit对象。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-22.png)  