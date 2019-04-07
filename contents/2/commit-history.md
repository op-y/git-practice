# 查看提交记录

时隔一年，我来继续完成这个关于git的练习了。

## 查看提交历史
之前的很多实例中已经我们已经操作过使用 `git log` 查看提交历史了。这里不再图示。 `git log` 不加参数的话，会按照提交时间列出所有的提交，最近的提交显示在上边。同时会显示出每个提交的 **SHA1** 校验和、作者名字和email、提交的时间以及提交的说明。
其实 `git log` 还有一些参数可以使用。

`git log -p` 可以查看每次提交内容的差异。

`git log -p -2` 还可以限定查看提交的个数
 
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-44.png)

`git log --stat` 可以查看每次提交的简略统计信息。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-45.png)

`git log --pretty=oneline` pretty 参数可以指定不同的显示格式。可用选项有 oneline、short、full、fuller、format等。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-46.png)

其中format可以指定显示记录等格式，如 `git log --pretty=format:"%h - %an, %ar : %s"`

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-47.png)

一些格式字符串如下

* %H 提交对象的完整Hash串
* %h 提交对象的简短Hash串
* %T 树对象的完整Hash串
* %t 树对象的简短Hash串
* %P 父对象的完成Hash串
* %p 父对象的简短Hash串
* %an 作者名字
* %ae 作者的email
* %ad 作者修订日期
* %ar 作者修订日志（按照多久以前方式显示）
* %cn 提交者名字
* %ce 提交者email
* %cd 提交日期
* %cr 提交日期（按照多久以前方式显示）
* %s 提交说明

另外 `git log --graph` 可以模拟图形的方式展示提交历史信息。

![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-48.png)

除了显示格式， `git log` 还有一些限制输出的选项。

`git log --since=2.weeks` 仅显示两周内的提交

`git log -Sfunction_name` 仅显示添加或移除function_name的提交

限制输出的选项如下

* -n
* --since, --after
* --until, --before
* --author
* --committer
* --grep
* -S

最后 `git log` 可以在最后加上文件path，只显示那些我们关系的文件或者目录的提交历史。因为是放在最后的位置上，需要用 `--` 隔开之前的参数选项和后边的文件路径

