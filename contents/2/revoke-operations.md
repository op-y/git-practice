# 撤销操作

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-29.png)  

```
mkdir init-demo
ls -al
vim hello #内容为 Hello World!
git status
git init
git status
cd .git
ls -al
```
*Git* 是一个分布式版本控制系统，意味着不同地区的开发者之间可以共享工作，所以第二种获取项目 *Git* 仓库的方法是从其他项目仓库克隆，即 `git clone`。我们以当前[git-practice](https://github.com/op-y/git-practice)项目为例。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-30.png)  
通过 `git clone https://github.com/op-y/git-practice` 我们把git-practice项目克隆到了本地 **git-practice** 目录，现在我们拥有了一个和[Github](https://github.com)上项目一样的 *Git* 仓库。  
使用 `git help clone` 可以看克隆命令的帮助  
