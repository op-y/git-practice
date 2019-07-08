# 使用标签

标签（tag）是 *Git* 中对提交对象对一种静态引用。通常我们会给重要对历史提交、release版本、bugfix版本等打上标签。
标签通过 `get tag` 命令来操作。

```
usage: git tag [-a | -s | -u <key-id>] [-f] [-m <msg> | -F <file>] <tagname> [<head>]
   or: git tag -d <tagname>...
   or: git tag -l [-n[<num>]] [--contains <commit>] [--no-contains <commit>] [--points-at <object>]
		[--format=<format>] [--[no-]merged [<commit>]] [<pattern>...]
   or: git tag -v [--format=<format>] <tagname>...

    -l, --list            list tag names
    -n[<n>]               print <n> lines of each tag message
    -d, --delete          delete tags
    -v, --verify          verify tags

Tag creation options
    -a, --annotate        annotated tag, needs a message
    -m, --message <message>
                          tag message
    -F, --file <file>     read message from file
    -e, --edit            force edit of tag message
    -s, --sign            annotated and GPG-signed tag
    --cleanup <mode>      how to strip spaces and #comments from message
    -u, --local-user <key-id>
                          use another key to sign the tag
    -f, --force           replace the tag if exists
    --create-reflog       create a reflog

Tag listing options
    --column[=<style>]    show tag list in columns
    --contains <commit>   print only tags that contain the commit
    --no-contains <commit>
                          print only tags that don't contain the commit
    --merged <commit>     print only tags that are merged
    --no-merged <commit>  print only tags that are not merged
    --sort <key>          field name to sort on
    --points-at <object>  print only tags of the object
    --format <format>     format to use for the output
    --color[=<when>]      respect format colors
    -i, --ignore-case     sorting and filtering are case insensitive
```

使用 `git tag` 或者 `git tag --list` 可以查看当前已经存在的标签。如下图，我们这个项目里已经存在 **v0.9** 和 **v1.0** 两个很久之前Celine Wong创建的标签。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-68.png)

现在我们项目节点是刚刚完成2.5，我们使用 `git tag -a milestone-2-5 -m 'chapter 2 section 5'` 创建一个附注标签（通常建议创建附注标签）。再使用 `git tag` 查看该标签已经存在于标签列表中。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-69.png)

附注标签包含了很多信息，是一个完整的对象，还可以使用GPG签名和验证。但是有时候我们需要一些临时标签。这个时候可以去掉 `-a` `-m` 等参数，直接使用 `git tag ms2.5-lw` 打一个轻量级标签。完成后使用 `git tag` 查看该标签已经存在于标签列表中。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-70.png)

上边的我们都在对当前最后一个提交打标签，有时候我们会想给历史上重要提交补上标签。这也是可以做到的。通过 `git log` 我们找到最近有两个 **finish** 提交

```
878948c finish content 2-4
70a0421 finish content 2-2
```

这里我们使用 `git tag -a milestone-2-4 878948c` `git tag -a milestone-2-2 70a0421` 可以给这两个历史提交打上标签。再使用 `git tag` 查看该标签已经存在于标签列表中。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-71.png)

通过 `git show [tag-name]` 的方式可以查看标签对应的提交信息。观察 **milestone-2-5** 和 **ms2.5-lw** 我们可以看到附注标签和轻量级标签区别，附注标签带有tag信息，而轻量级标签没有。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-72.png)

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-73.png)

标签创建后只是存在于本地仓库，如果希望将标签信息共享给其他开发者，需要使用 `git push [repository-name] [tag-name]`

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-74.png)

使用 `git push [repository-name] --tags` 可以推送全部标签信息。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-75.png)

如果要删除标签，可以使用 `git tag -d [tag-name]`。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-76.png)

同理，删除标签也仅仅影响本地仓库，如果要同步删除信息，可以使用 `git push origin [tag-ref]` 这里使用 `git push origin :refs/tags/ms2.5-lw` 。

![git-practice](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-77.png)

好了，标签就讲这么多了。下一节将开启新的一篇，开始讲开发者之间的协作。