
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
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-3.png)    命令输出提示 **有未被Git跟踪文件**，用户可以使用 `git add` 命令开始跟踪文件（实际上就是新文件添加到暂存区），这个时候我们根据提示执行命令 `git add staging-demo.txt` 开始对文件 **staging-demo.txt** 进行跟踪，然后再执行命令 `git status` 看看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-4.png)  
这个时候命令输出显示变成 **有文件变更等待被提交** 了，使用 `git status -s` 使用缩略方式看看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-5.png)  
看到文件旁边有个绿色的 **A** 标记（注意位置在最左边，在不同操作系统上显示可能颜色会有差别），表示暂存区有添加（Append）新文件。这个时候我们可以执行提交了 `git commit -m "craete a staging-demo.txt"`  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-6.png)  
这次我们看到提交成功了，本次提交生成的commit ID是 **e239d7d**，到这里我们已经完成了一次提交工作。  

接下来我们再一遍！  
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
现在两个文件都在暂存区中等待被提交，我们在修改一下 **staging-demo-again.txt** 文件（Hello World！复制了两行），在用 `git status` 以及 `git status -s` 查看状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-13.png)  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-14.png)  
看到了没，这次缩略状态显示，文件左边既有绿色的暂存区状态又有红色的工作区域状态（为了看到这个折腾了一圈～），继续执行 `git add staging-demo-again.txt` 将文件修改添加到暂存区，执行 `git statu -s` 红色工作区修改状态没了  
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
## Git 对象