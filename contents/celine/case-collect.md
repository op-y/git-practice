## GIT具体场景应用 
当你基础入门了git指令后，比如 添加`add` 、提交`commit` 、分支`branch` 、`stash`等等，但等遇到具体一些场景时候，又不太知道从何操作起。本文主要是从使用需求出发，来描述操作过程。

#### 1. 保存工作现场
**场景：**  线上运行着稳定版本stable1.0，你在QA上开发新版本dev2.0。突然stable1.0出现bug了，你需求切分支过去修改bug。但是，dev2.0的代码不过完整，还不想提交。肿么办？  

**解决：**  这种情况相当于是要在本地保存修改，但有想切换分支（当前分支若有文件修改没有提交，会无法切分支）。这时候我们可以使用`git stash`来保存当前工作现场。  
具体操作如下：
```
#在dev2.0分支下，保存工作现场
git stash

#切换到stable1.0去修改bug
git checkout stable1.0

#创建修复分支
git checkout -b stable1.0-bug1

#coding...修改好了之后
git add ./
git commit -m "fix bug1"

#合并修复分支，并删除修复分支
git checkout stable1.0
git merge --no-ff -m "merge fix bug1" stable1.0-bug1
git branch -d stable1.0-bug1

#接下来回到dev2.0，继续愉快的玩耍吧
git checkout dev2.0

#查看保存的工作现场
git stash list

#恢复stash内容
git stash apply

#删除stash内容
git stash drop

#以上的恢复和删除 stash内容 可以合并为 pop指令
git stash pop

#嗯，一切如常，仿佛啥也没发生过一样...

```

#### 2. 本地更改保存分支替换
**场景：**  你在分支A愉快的玩耍着，但突然发现这些本地修改出现了点问题，比如改乱了，或者是在研究阶段等等，总之就是不能保存在分支A上面了，可是又想要保留这些修改。于是你想创建一个新分支来放置这些修改，可是现在在分支A上，怎么优雅地挪动这些修改呢？

**解决：**  这种情况有两种解决方案。   
1.  直接从分支A创建一个新分支B，然后在新分支B上面进行add和commit即可。  
2.  利用stash把本地修改保存在当前工作现场，然后在任意分支创建一个新分支B，然后在新分支B下进行stash pop即可。
具体操作如下：
```
#方法1
git branch -b branch-B
git add ./
git commit -m "some edit..."

#方法2
git stash
git checkout -b branch-B
git stash pop
git add ./
git commit -m "some edit..."

```

#### 3. 给分支添加描述
**场景：**  修复bug创建分支，临时创意创建分支，突然想备份个版本又创建分支……（一不小心还忘记删掉无用分支）久而久之，你突然不知道你为什么创建分支，而且这些分支该何去何从呢？这时候你突然想，要是创建分支能像commit那样写个备注啥的就好了。

**解决：**  给分支添加描述，不是不可能，只是操作方式并和commit不一样。   
具体操作如下：
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
