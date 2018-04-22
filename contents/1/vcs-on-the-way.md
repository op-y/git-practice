# 版本控制系统的演变

## 本地版本控制系统
本地版本控制可以称为版本控制的史前时代，这个时候我们理解了合理保存项目文件的各个版本的重要性，于是使用类似 `diff` + `patch` 这样的工具来做版本控制。  
本地版本控制系统的示意图如下

![local version control system](https://github.com/op-y/git-practice/blob/master/images/1/draft.1-0.png) 
 
`diff` + `patch` 本地演示  
我们在本地创建 **version0.txt** 文件，内容为

```
Hello World!
This is the version 0 of the document.

Create by op-y.
```
之后我们修改该文件，成为 **version1.txt**，内容为

```
Hello World!
Hello Git!
This is the version 1 of the document.

Create by op-y & Celine.
```
我们用 diff -u version0.txt version1.txt 可以看到这次修改操作前后文件的变化

![diff](https://github.com/op-y/git-practice/blob/master/images/1/snip.1-0.png) 

我们重定向保存文件的变化 `diff -u version0.txt version1.txt > diff.txt` ，现在我们可以利用 **version0.txt** 和 **version1.txt** 两个文件中的一个，以及差异文件（我们称之为patch）恢复另一个文件了。  
执行 `patch version0.txt < diff.txt`，然后我们看看打过patch的 **version0.txt文件** `cat version0.txt`  
![version0 to version1](https://github.com/op-y/git-practice/blob/master/images/1/snip.1-1.png)  
同样的执行 `patch -R version0.txt < diff.txt`，注意这里是反方向操作所以使用了 `-R` 参数，然后我们看看打过（去掉）patch的 **version1.txt** 文件 `cat version1.txt`  
![version1 to version0](https://github.com/op-y/git-practice/blob/master/images/1/snip.1-2.png)  

## 集中式版本控制系统
互联网时代到来后，版本控制系统需要解决一个问题，那就是如何处理不同开发者在不同系统上协同工作的问题。CVCS（Centralized Version Control Systems）应运而生。1986年 *CVS* 诞生开启了集中式版本控制时代，而随后的 *SVN*（*Subversion*）无疑是集中式版本控制系统的集大成者。  
集中式版本控制系统示意图如下

![centralized version control system](https://github.com/op-y/git-practice/blob/master/images/1/draft.1-1.png)

集中式版本控制系统带来了新的突破，也存在自身的问题。例如单点问题，中心服务器一旦挂掉，会影响所有的开发者；细节上如SVN在每个目录下都生成版本控制数据目录管理起来不是特别方便，系统的trunk、branch、tag管理也不够优雅。

## 分布式版本控制系统
互联网经历较长时间发展后，我们迎来了去中心化的时代。分布式版本控制系统在这个时期诞生，如 *Git*，*Mercurial*，*Bazaar* 以及 *Darcs* 等。相比集中式版本控制系统而已，分布式版本控制系统最大的特点是，每个开发者本地都有一个完整的版本控制数据库（我的地盘我做主！）只在需要的时候，在不同开发者之间（包括所谓官方库）进行数据交换。
分布式版本控制系统示意图如下

![distributed version control system](https://github.com/op-y/git-practice/blob/master/images/1/draft.1-2.png)

## Git的诞生
Git的创造者是大名鼎鼎的 Linus Torvalds。  
大神总有些异于凡人的认知/想法/行为，老李（原谅我这么称呼他）坚定的反对CVS和SVN（也许是集中式管理不符合他的理念？），十多年来坚持手工打patch的方式进行 *Linux kernel* 的维护。后来顶不住社区的声讨，02-05年间找到了一个叫做 *BitKeeper* 的分布式版本控制系统对项目进行管理。

剧情的发展总是很出人意料，2005年 Andrew Tridgell（*samnba* 的创造者） 出于xx目的对 *BitKeeper* 进行反向工程，这种行为惹恼了 *BitKeeper* 的所属公司，其收回了对Linux社区的授权。无奈之下老李只能动动手参考 *BitKeeper* 的设计思想，自己写一个分布式版本控制系统 -- 于是Git就这样诞生了。  

**Git 诞生历程**

* 2005年4月3日，开始Git开发
* 2005年4月6日，项目发布
* 2005年4月7日，Git可以自举了
* 2005年4月18日，第一个分支合并
* 2005年4月29日，Git性能达到老李的预期
* 2005年6月16日，*Linux kernel 2.6.12* 发布，Git已经在维护Linux内核源码了
* 2005年7月26日，老李深藏功与名，Git交由其他贡献者和社区维护

后来的故事大家都知道了，Git风靡开源社区；*BitKeeper* 固守自己的商业化策略后来日渐式微，后来为了能继续活下去，也开源了，开源后代码托管在 [Github](https://github.com) 上...  

这个故事告诉我们
> 不要随便去惹大神，分分钟吊打你...
