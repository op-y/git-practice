# Git 的安装

### Linux 上源码安装
从Github上下载源码 [https://github.com/git/git](https://github.com/git/git)，当前最新的release是2.17.0版本

```
wget https://github.com/git/git/archive/v2.17.0.tar.gz
tar -zxf v2.17.0.tar.gz
cd git-2.17.0
make prefix=/usr/local all
sudo make prefix=/usr/local install
```

### Linux 上的包管理器安装
*Fedora* 上安装 `yum install git-core`  
*Ubuntu* 上安装 `apt-get install git`  

### Mac 上的安装
从官网下载[安装包](https://git-scm.com/download/mac) 安装  
使用 *Homebrew* 包管理器安装 `brew install git`  

### Windowns 上的安装
从官网下载[安装包](https://git-scm.com/download/win)安装

### Git 图形化管理工具
萝卜白菜各有所爱了，在Linux/Mac下工作的还是多多熟悉命令行吧。

# Git 获取帮助
* 使用自身的帮助

```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

![git help](https://github.com/op-y/git-practice/blob/master/images/1/snip.1-3.png) 
 
* 从[官网](https://git-scm.com)获取帮助文档  
* 参考书籍，如《Pro Git》《Git 权威指南》

# Git 配置
本地使用 git 过程中，有三个配置文件
 
* 系统（system）配置，对系统上所有用户都生效，Linux上位于 **/etc/config**  
* 用户全局（global）配置，对于当前用户所有代码库生效，Linux上位于 **～/.gitconfig**  
* 当前代码库的配置，仅对于当前代码库生效，位于代码库目录下 **.git/config**

这几个文件生效优先级是 **当前库配置 > 用户全局配置 > 系统配置**  

经常使用的配置: 提交用户名、提交用户邮箱、默认编辑器、diff工具、常用命令别名等

```
git config --global user.name "op-y"
git config --global user.email op-y@example.com
git config --global core.editor vim
git config --global merge.tool vimdiff
git config --global alias.co chekcout

# 查看配置
git config --list
git config <var name>
```
