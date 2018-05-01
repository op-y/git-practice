# 提交你的工作到Git仓库

## 文件的状态、添加和暂存
之前的内容我们已经提到过git项目中的工作三部曲  
**修改 --> 暂存 --> 提交**  
*Git* 项目中文件状态可以用下边的状态图来表示  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/draft.2-6.png)  
在一个干净的（工作区没有变更，暂存区没有暂存新文件）项目中，执行 `git status` 可以查看当前 *Git* 项目状态（这个之前我们已经了解过）  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-31.png)  
命令输出显示当前工作区是干净的，而且我们正在master分支上。我们在终端下用 *Vim* 编辑一个新文件 **working-commit.txt** 并保存，这个时候我们再使用 `git status` 查看当前 *Git* 项目状态  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-32.png)  
命令输出了未被跟踪文件列表（当前终端使用了oh-my-zsh，提示符后有个 ✗ 提示当前工作区有改变）。这时我们使用命令 `git add working-commit.txt` 开始追踪文件，再使用 `git status` 查看 *Git* 项目状态，看到现在 **working-commit.txt** 文件已经被暂存（而且它是个新文件）  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-33.png)  
我们修改一下之前示例中用过的文件 **staging-demo.txt**，再使用 `git status` 查看状态 -- 一个新文件 **working-commit.txt** 被暂存（有待提交），一个（已被追踪的）文件 **staging-demo.txt** 被修改。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-34.png)  
使用 `git add staging-demo.txt` 暂存修改文件。 再使用 `git status` 查看状态。现在两个文件都已经被暂存。根据之前的介绍，现在我们如果执行 `git commit -m "commit message"` 就可以提交本次工作了。 
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-35.png)   
不过我们先不提交，再修改一下 **staging-demo.txt** 文件。再使用 `git staus` 查看状态，发现与之前不同的是，**staging-demo.txt** 文件己既有已经暂存的修改，又存在未暂存的修改。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-36.png)  
通过上边的操作我们看到的文件的不同状态（未被追踪、未被修改、已修改、已暂存）。  

## 忽略文件
工作过程中存在这么一些状况，C/C++这类语言编译过程中会产生一些 **\*.out** 中间文件、**a.out** 这种默认输出文件等。一般情况下我们不希望将这类文件纳入版本控制。这个时候我们就需要使用到 *文件忽略* 了，即 *Git* 项目里的 **.gitignore** 文件。  
我们 `touch test.o` 和 `touch a.out` 生成两个文件，再用 `git status` 查看状态，可以发现两个未被追踪的文件  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-37.png)  
这个时候我们在当前目录下创建一个新文件 **.gitignore**，在文件中添加内容  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-38.png)  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-39.png)  
保存后再使用 `git status` 查看状态，发现 **test.o** 和 **a.out** 已经被 *Git* 项目给忽略了（这里说明一点，如果需要忽略**.gitignore**文件，则 *.gitignore* 也需要做一行内容放入文件中，这里看不到是因为项目顶层目录下也有**.gitignore**文件将其忽略掉了）。至此文件忽略基本概念和使用已经说明了。补充一些内容。  

* **.gitignore** 可以存在项目的任何目录中，作用范围是当前目录及其子目录
* 使用 `git status --ignore` 可以查看被忽略的文件
* **.gitignore** 只忽略未被追踪的文件，已经被 *Git* 追踪的文件不受影响
* 被 **.gitignore** 忽略的文件不能直接 `git add`，需要使用 `git add -f <filename>` 强制进行追踪
* 如果不打算让其他人使用你的 **.gitignore**，将其自身忽略是一个好做法
* **.gitignore**	语法参考下边的代码段

```
#所有空行或者以注释符号 ＃ 开头的行都会被 Git 忽略。
#可以使用标准的 glob 模式匹配。
#匹配模式最后跟反斜杠（/）说明要忽略的是目录。
#要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

# 此为注释 – 将被 Git 忽略
# 忽略所有 .a 结尾的文件
*.a
# 但 lib.a 除外
!lib.a
# 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
/TODO
# 忽略 build/ 目录下的所有文件
build/
# 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
doc/*.txt
# 忽略 doc/ 目录下所有扩展名为 txt 的文件
doc/**/*.txt
```

## 查看文件diff
`git diff` 命令给我们提供了查看已提交文件、已暂存文件和工作区文件间差异的方法。接着上边的操作，我们执行 `git diff`，命令输出显示了当前工作区 **staging-demo.txt** 和暂存区 **staging-demo.txt** 的差异，包括文件对象（index 那行），文件的差异范围（a文件的2到3行，b文件的2到4行），差异内容是添加了一行 `for working-commit again`  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-40.png)  

```
diff --git a/contents/op-y/staging-demo.txt b/contents/op-y/staging-demo.txt
index 479c764..4c518ba 100644
--- a/contents/op-y/staging-demo.txt
+++ b/contents/op-y/staging-demo.txt
@@ -2,3 +2,4 @@ What's staging?
 Add a new line.

 for working-commit.
+for working-commit again.
```

`git diff` 查看到是当前工作区和暂存区文件的差异，有时候我们希望看看已暂存文件和上一次提交文件间的差异，这个时候我们可以使用 `git diff --cached`，命令输出如下  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-41.png)  

```
diff --git a/contents/op-y/staging-demo.txt b/contents/op-y/staging-demo.txt
index d942c0e..479c764 100644
--- a/contents/op-y/staging-demo.txt
+++ b/contents/op-y/staging-demo.txt
@@ -1,2 +1,4 @@
 What's staging?
 Add a new line.
+
+for working-commit.
diff --git a/contents/op-y/working-commit.txt b/contents/op-y/working-commit.txt
new file mode 100644
index 0000000..476903a
--- /dev/null
+++ b/contents/op-y/working-commit.txt
@@ -0,0 +1 @@
+I finish one task.
(END)
```

可以看到 **staging-demo.txt** 添加了一行内容，而 **working-commit.txt** 是新增文件。

## 提交更新
提交更新之前已经演示过很多遍了， `git add` 后 `git commit -m "commit message"`  
另外，之前也讲到跳过暂存直接提交的命令 `git commit -a -m "commit message"`，这种命令需要谨慎使用

## 删除文件
已经被纳入 *Git* 版本控制的文件，如果后续需不再需要，也可以将文件从 *Git* 追踪的文件中删除。我们用 **working-commit.txt** 试试。直接执行 `rm working-commit.txt` 看看好不好使。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-43.png)  
发现状态是有变更未被暂存，*Git* 友好的提示了文件被删除了。而执行 `git rm <filename>` 则直接将工作区文件删除，并且标记到暂存区，表示该文件不再纳入版本控制（之前的版本中还是有的）。  
![git workflow](https://github.com/op-y/git-practice/blob/master/images/2/snip.2-44.png)  
还有一种情况是，文件已经被暂存，我们想要删除暂存区中的文件但是需要留下工作区的文件，可以使用 `git rm --cached <filename>`  
被删除的文件如果想要恢复怎么办？可以自己想象。提示：*checkout*

## 移动文件
我们已经了解文件的删除了，文件的移动（重命名）可以类比了。这里写出一条命令： `git mv <old file> <new file>`，剩下的自己思考一下。