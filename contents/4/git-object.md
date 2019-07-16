# git cat-file 和对象

在[你需要了解点Git底层实现 对象](https://github.com/op-y/git-practice/blob/master/contents/2/first-glance-of-git-foundation.md#git-对象)一节中我们已经介绍了*Git*对象。回忆一下**.git/objects**目录！
这里我们只做一些总结。

四种对象：

* blob：数据对象
* tree: 树对象
* commit: 提交对象
* tag: 标签对象

四种对象的内容和意义经过这么长时间的练习及时不完全明白也应该基本掌握了。

我们知道对象是在*Git*键值数据库中用40位Hash值来索引，前边的练习中我们知道一个 `git cat-file`命令可以查看对象文件。现在我们系统了解一下：

```
usage: git cat-file (-t [--allow-unknown-type] | -s [--allow-unknown-type] | -e | -p | <type> | --textconv | --filters) [--path=<path>] <object>
   or: git cat-file (--batch | --batch-check) [--follow-symlinks] [--textconv | --filters]

<type> can be one of: blob, tree, commit, tag
    -t                    show object type
    -s                    show object size
    -e                    exit with zero when there's no error
    -p                    pretty-print object's content
    --textconv            for blob objects, run textconv on object's content
    --filters             for blob objects, run filters on object's content
    --path <blob>         use a specific path for --textconv/--filters
    --allow-unknown-type  allow -s and -t to work with broken/corrupt objects
    --buffer              buffer --batch output
    --batch[=<format>]    show info and content of objects fed from the standard input
    --batch-check[=<format>]
                          show info about objects fed from the standard input
    --follow-symlinks     follow in-tree symlinks (used with --batch or --batch-check)
    --batch-all-objects   show all objects with --batch or --batch-check
    --unordered           do not order --batch-all-objects output
```

* -t：查类型
* -s：查大小
* -e：没有报错情况下返回0
* -p：友好的输出内容
* ...

另外还有一些底层命令操作对象

* git hash-object
* git update-index
* git write-tree
* git read-tree
* git commit-tree
* ...

感兴趣的话可以查文档了解这些命令怎么操作*Git*对象。
