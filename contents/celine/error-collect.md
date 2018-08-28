## GIT错误集锦及解决方法  
新手在使用git的时候，难免会遇到一些不知所然的错误，在此文档，会收集常见的错误，并提供解决方法。欢迎大家进行检索，也可以一起进行补充。  

#### 1. git clone SSH连接配置问题
**执行代码：**  `git clone ...`  
**错误提示：**  Warning: Permanently added the RSA host key for IP address '192.30.252.129' to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.  
**解决方法：** 这种一般是SSH链接配置没有设置好。可以参考Hunter博客的[《Github之SSH连接配置》](http://www.linmuxi.com/2016/02/24/github-config-ssh/)，里面的图文很详细，可以直接按照步骤进行设置。

#### 2. 添加远程库后，首次push问题
**执行代码：**  `git push -u origin master`  
**错误提示：**  `error: src refspec master does not match any. `
                `error: failed to push some refs to 'git@github.com:hahaha/ftpmanage.git'. `  
**解决方法：** 这个错误是提示本地仓库为空，也就是说你还没有添加`add`和提交`commit`文件，自然就没有什么可以`push`的了。 
当你再本地电脑上新建一个项目后，想要和远程github repository连接起来并进行后续操作，一般的执行流程如下：
```
#本地仓库初始化
git init

#添加远程库
git remote add origin git@github.com:celineWong7/chosen-demo.git

#添加本地文件（工作区）到暂存区
git add ./

#将暂存区的文件提交到本地仓库
git commit -m "init"

#此时，再进行push，推送本地仓库到远程库
git push -u origin master

#若在出现`hint: Updates were rejected because the remote contains work ...`错误，一般是需要先进行pull
git pull --rebase origin master

#pull成功后再进行push
git push -u origin master
```
#### 3. 切换分支失败
**执行代码：**  `git checkout branchA`  
**错误提示：**  `error: cannot stat ‘file’: Permission denied`
**解决方法：** 这种错误一般是该分支上的文件被电脑占用（编辑器、浏览器等），无法释放。解决方法是退出各类和分支上文件相关编辑器、浏览器、资源管理器等，再进行切换 .