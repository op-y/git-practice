# 获取项目的Git仓库

获取项目仓库的第一种方法非常简单，使用 `git init` 命令将当前目录初始化为一个 *Git* 项目  
![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-29.png)  
图中演示做了以下操作  

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
我们看到新创建的目录 **init-demo** 中没有 *Git* 相关的任何信息，执行 `git init` 后当前目录变成了一个 *Git* 项目，并且生成了 **init-demo/.git** 本地仓库目录，同时 **hello** 文件变成了一个待加入暂存区的文件。  

*Git* 是一个分布式版本控制系统，意味着不同地区的开发者之间可以共享工作，所以第二种获取项目 *Git* 仓库的方法是从其他项目仓库克隆，即 `git clone`。我们以当前[git-practice](https://github.com/op-y/git-practice)项目为例。  
![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-30.png)  
通过 `git clone https://github.com/op-y/git-practice` 我们把git-practice项目克隆到了本地 **git-practice** 目录，现在我们拥有了一个和[Github](https://github.com)上项目一样的 *Git* 仓库。  
使用 `git help clone` 可以看克隆命令的帮助  

```
GIT-CLONE(1)                      Git Manual                      GIT-CLONE(1)

NAME
       git-clone - Clone a repository into a new directory

SYNOPSIS
       git clone [--template=<template_directory>]
                 [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
                 [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
                 [--depth <depth>] [--recursive] [--] <repository> [<directory>]

...
```

请仔细阅读一下帮助信息！其中还有关于协议的说明，后续会再讲解。
