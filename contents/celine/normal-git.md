## Git 常用指令（入门级）
> 小叶哥在目录里，已详细对每个git操作做出分析（还包含了很多原理级别的）。而我自己当初，在要接触git时，也是徘徊在门外兜兜转转，最后因为工作需要，天天摸索，才总算入了门。所以，“熟能生巧”真的一点没错。  
> 之前把一些常用的操作记录在纸质笔记本上，平时偶尔忘记指令可以回翻。现在我把这些操作总结出来，希望对大家有所帮助。主要还是给自己做个电子档备份。O(∩_∩)O哈哈~  

### 概述  
Git的整个操作要素，可以用下图的流程概括：
![git简图，来自阮大大博客](../../images/celine/git-flow.png)  
**几个主要的结构体有：**   
1. Workspace 工作区：`git init`后，当前文件夹（自己电脑上，
2. 眼睛看得见的那个和平常文件夹一毛一样的文件存储区域）就是工作区。  
2. Index 暂存区：一个临时放置工作区编辑后文件的区域。`git add`后，有变动的文件会跑到这里来。  
3. Repository 本地仓库：相当于是在自己电脑上的一个仓库。`git init`后，你会发现当前文件夹多了一个`.git`的隐形文件夹，这里就存放着本地仓库的版本相关内容。  
4. Remote 远程仓库：这个是一个联网的在某个服务器上的库，我们一般使用github或者gitlab等。若希望别人能远程下载`clone`你的文件，就必须放到远程仓库。  

**接下来，我们简单捋一捋这个过程：**  
下行路程：作为小前端的我在本地电脑（**工作区**）上的一个文件夹下进行初始化`git init`，紧接着马不停蹄的撸了一把代码，保存为file.html  ------------> 通过 `add` ，把file.html放到了**暂存区** ------------>  然后又通过 `commit`，把file.html送到了**本地仓库** ------------>  最后通过 `push`，把file.html推送到了远程仓库。  

上行路程：这个月拿到激励金的我买了台笔记本，想把之前撸过的file.html工程下载到这台电脑上。先获得远程库里对应工程的地址（一个xxx.git的下载地址）------------> 通过 `clone` ，把file.html所在工程下载到了**本地仓库**（操作指令的文件路径下，会自动初始化为本地仓库）。------------> 通过 `checkout` ，把file.html检出到了**工作区**。

###  常用指令说明  
我的入门参考过阮大大博客，也入手过图灵的教材。但真正比较有感知的还是廖雪峰大大的[git教程系列](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。这个教程虽然有一点点年代了，但作为入门一点都不过时。只要按照步骤一步一步的走一遍，就能大体体味Git操作到底是什么个鬼东西了。  
此处我是精炼出指令来，以备查阅。  

#### 1 创建版本库
```
#初始化版本库
git init

#添加工作区变动到暂存区
git add

#添加暂存区内容到本地仓库
git commit

#查看仓库状态（工作区的变动文件是红色，暂存区的文件是绿色，也会有相关提示操作的语句）
git status

#查看文件修改（本地仓库和工作区比较）
git diff file1.txt
```

#### 2 版本回退
```
#查看提交日志（`git commit` 时，附录的说明）
git log

#查看提交日志（单行显示，看起来会更简洁）
git log --pretty=oneline

#回退上一个版本
git reset --hard HEAD^

# HEAD     当前版本
# HEAD^    上一个版本
# HEAD^^   上上一个版本
# HEAD~100 往上100个版本

#查看每一次命令（可以查看commit id，用以查找版本）
git reflog

#指定到commit id为3628164版本（实际的commit id数字串很长，可以不写全）
git reset --hard 368164

```

#### 3 撤销修改
```
#把工作区的修改撤销（丢弃工作区修改）
git checkout -- file1.txt

# 注意：
# 1. 若file1.txt没有add，上面的指令会让工作区回到版本库相同的状态；
# 2. 若file1.txt已经add到了暂存区，上面的指令会让工作区回到暂存区状态上
# 补充说明：checkout 是检出的意思，也就是从本地仓库提取指定文件来覆盖工作区的文件。
#           暂存区是工作区和本地仓库中间的一个桥梁，所以检出时候会先到暂存区去探究下，若没有再到本地仓库取。

#把暂存区的修改撤销，修改只保留在了工作区
git reset HEAD file1.txt

#若希望把已添加暂存区的修改丢弃，就得分别执行两步: 先撤销暂存区的修改（此时修改还在工作区），再撤销工作区的修改。
git reset HEAD file1.txt
git checkout -- file1.txt

# 注意：以上两行代码会让临时修改的代码彻底消失，谨慎操作。

```

#### 4 删文件
```
#1. 直接在工作区的文件夹把文件删除（右键-删除）

#2. 若确定要删除，继续执行以下指令
git rm file2.txt
git commit 

#3. 若误删了，可以使用撤销修改
git checkout -- file2.txt

```

#### 5 远程库 origin
(1) 添加远程库
```
#1. 关联
git remote add origin git@server:path/repo.git

#2. 第一次推送 -u：是对本地master与远程master分支进行关联
git push -u origin master

#3. 以后的推送
git push origin master

# 或者
git push

```

(2) 克隆远程库
```
#1. 关联
git clone git@server:path/repo.git

#也支持https协议，但速度会偏慢

```

(3) 查看远程库
```
# 列出远程库，远程库默认名称origin
git remote

#显示远程库信息
git remote -v

#查看远程库详细信息
git remote show [origin]

```

(4) 向远程库推送分支
```
# 推送 master 分支
git push origin master

# 推送 dev 分支
git push origin dev

```

#### 5 分支管理 branch
##### (1) 创建并合并分支

```
# 创建并切换到分支dev
git checkout -b dev

# 上面的指令等于下面两条指令的集合

# 创建分支
git branch dev

# 切换分支
git checkout dev

# 查看当前所有分支
git branch


# ... 在dev分支上做编辑，并add、commit等操作后

# 切回master分支
git checkout master

# ... dev分支上的修改不会出现在master分支上


# 把dev分支合并到master分支：可以把dev上的修改同步到master
# git merge dev

# 删除dev分支（注意：不能在dev分支上删除dev分支）
git branch -d dev

# 推送分支：把dev分支推送到远程库
git push origin dev

```

```
# 查看两个分支区别，将结果输出到指定文件

#1. 覆盖方式：若执行指令的路径下已经有file1.diff，会进行覆盖
git diff [branchA] [branchB] > file1.diff

#2. 追加方式
git diff [branchA] [branchB] >> file1.diff


# 查看分支创建时间
git reflog show --date=iso branchA
```

##### (2) 解决冲突

当使用`merge`操作合并时，若两个分支修改了相同位置内容，就会出现冲突。  
可以具体查看发生冲突的文件，文件里头会有` <<<< ` ` ==== ` ` >>>> ` 来标记出同一位置，不同分支各自的内容块。然后由我们来选择保留想要留下的内容，删除不要的内容以及标记符号。保存、add、commit，即可。
```
# ... 假设当前在master分支上，要合并dev分支
git merge dev

# ... 发生了冲突
# 1. 查看冲突发生文件：一般在进行git merge 时就会有提示文字
git status

# 2. 处理冲突：打开产生冲突的文件，进行主观编辑，然后保存

# 3. 之后进行正常的更新文件操作
git add
git commit -m "fix conflict"

# 最后，如果想要查看分支合并图
git log --graph --pretty=oneline --abbrev-commit

```

##### (3) 分支管理策略
分支是版本管理工具的一大特色。如何来管理分支，不同人不同团队采用的也不一样。  
有两篇博文可以参考下[源代码主干分支开发四大模式](https://blog.csdn.net/zhangmike/article/details/38613429) 和 英文版的[A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)。  
团队开发中，一般会有以下几种分支分类（仅供参考）：
1. master - 主分支，时刻与远处同步
2. dev    - 开发分支，团队成员在此分支上工作
3. bug    - 本地修复bug分支，可以不推送到远程
4. feature - 新功能分支，若是团队合作，可以push到远程

关于分支合并，分为两种模式：快速模式和普通模式。
```
# 快速模式：上面使用的合并模式。
git merge dev

# 普通模式：以非 “Fastforward” 模式合并，合并后历史记录有分支，能看出合并过
git merge --no-ff -m "merge with noff" dev

# 目前我都是用普通模式。 0 0 因为我使用快速模式的时候，都会自动进入 -m 编写提示文字模块。还没有去仔细研究为何这样。

```

##### (4) Bug分支  git stash
场景模拟：  
... 在dev分支上热火朝天干活ING ... 但完整模块还没有完成，并不想提交  
... 但线上出现紧急bug需要修复，必须切到master分支修改bug ...  
... 该如何是好 ... 别怕！我们有 git stash 来保存当前工作现场  

```
# 在dev分支上，保存当前工作现场
git stash


# ... 切到master分支上，进行bug修复 ...
git checkout master

# 创建修复bug新分支 issue-101
git checkout -b issue-101

# ... 修改代码后
git add
git commit -m "fix bug 101"
git checkout master
git merge --no-ff -m "merged bug fix 101" issue-101
git branch -d issue-101


# ... 切回到dev分支，要继续之前的工作 ...
git checkout dev

# 查看保存的工作现场
git stash list

# 恢复并删除stash内容
git stash pop


# 以上 git stash pop 等同于下面两条指令

# 恢复工作现场
git stash apply

# 删除stash内容
git stash drop

```
注意：在dev分支创建的stash内容，是属于本地库的，而不隶属于分支。所以如果当前的工作现场，我希望在其他分支进行编辑提交，就可以在其他分支上进行 `git stash pop`。

##### (5) 常见标签 Tag
标签的作用相当是 commit版本号的一个别称，能更方便我们回退到哪个commit。  
1)  创建标签
```
# 切换到要打标签的分支上
git checkout master

# 给最新提交打上标签 v1.0
git tag v1.0


# 若想给历史提交打标签，可以git log查看其版本号
git log --pretty=oneline --abbrev-commit

# 假如查阅到的 commit id 是 6224937****
git tag v0.9 6224937

# 查看标签
git tag

# 查看标签信息
git show v0.9

# 创建带有说明文字的标签
git tag -a v1.0 -m "version 1.0 released"

# 把标签推送到远程
git push origin v0.1

# 一次性全部推送
git push origin --tags
```

2)  删除标签
```
#本地删除标签
git tag -d v1.0

# 若标签已推送到远程，要删除就得两步：先本地删除、再远程删除
git tag -d v1.0
git push origin :refs/tags/v1.0
```

##### (6) 给分支添加描述
```
#给当前分支添加描述
git branch --edit-description

#执行上面指令，界面会进入一个编辑界面，可以按【insert】键，然后在里面噼里啪啦写下你的感言
#写完后，【Q】键退出编辑，然后输入":wq!"保存退出。

#查看分支描述
git config branch.<branch>.description

```
注意：  
1. 分支描述是保存在`.git/config`下的，是本地存储，所以不能被推送。当删除分支时，对应的分支描述也会一起删除。  
2. 设置 `git config --global branchdesc true`, 就可以将此描述推送到合并提交。即`git merge --log<branch>`,分支描述会添加到合并提交消息。~~（此条规则我还没有测试，你们可以先测测看。）~~
