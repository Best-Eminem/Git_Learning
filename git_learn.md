# Git 常用命令

**()表示可选项**

![image-20220615152415329](.\git_learn.assets\image-20220615152415329.png)

## 工作区&暂存区

工作区就是你在电脑里能看到的目录。

git init 初始化仓库

git diff readme.md/git diff 查看工作区和暂存区的差异

git add readme.md （git add在commit之前可以多次执行）

git status 查看仓库状态，可以看到commit之前暂存区的修改，commit之后，status就clean了

git restore --staged sample_data/README.md 该命令将add到暂存区的修改撤回

git commit -m 'xxxxx' 将暂存区的所有add的修改全部提交

## 撤销修改：

**每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中**

`git checkout -- file`可以丢弃工作区的修改：总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。（如果commit后修改了没有add，那就是撤销修改回到和版本库一模一样的状态；已经add到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。）

假如已经将修改add了，使用`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：再用上面的checkout命令丢弃工作区的修改。

## 版本回退

git log (--pretty=oneline) 可以查看由近到远的commit历史提交记录，按Q可以退出

**HEAD表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往上100个版本就是`HEAD~100`**

git reset --hard HEAD^ 回退到上一个版本 **（注意，该操作会修改工作区文件，返回到上一个commit之前）**

git reset --hard commit_id Git允许我们在版本的历史之间穿梭

commit_id可以通过查看历史git命令来寻找git reflog

## 删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了；这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：`git checkout -- test.txt`

## 本地仓库&远程仓库

**添加远程库：**在github上新建仓库，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。`git remote add origin git@github.com:xxxx/xxxx.git`

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。`git push -u origin master`。从现在起，只要本地作了提交，就可以通过命令：`git push origin master`

**删除远程库:** `git remote -v`查看远程库信息；git remote rm origin 此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。

**同步远程仓库**：先 `git add` 再`commit`之后 ；`git remote add upstream xxxx.git` ，再 `git fetch upstream`，若上游仓库有变更，需要先进行`rebase`: `git rebase upstream/master`, `rebase`是指将当前分支的基分支修改到`upstream/master`(因为基分支可能已经有了改动), 并将当前分支在原始基分支上的改动重新commit到新基分支上。![在这里插入图片描述](https://img-blog.csdnimg.cn/12b959efcc454da5a15b9fdec493d61b.png?)

## 分支管理

创建与合并分支：分支的创建与合并实际上是指针的创建与移动。

- 查看分支：`git branch`
- 创建分支：`git branch <name>`
- 切换分支：`git checkout <name>`或者`git switch <name>`
- 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`
- 合并某分支到当前分支：先切换到主分支，再 `git merge <name>`。在其他分支add以及`commit`以后，相当于在那条分支上生成了新的`commit`节点，在和主分支合并以前，此时若切换回主分支，会发现之前在其他分支上提交的修改消失了，此时我们再切换回其他分支，工作区的修改又恢复了，这就是`git`的奇妙之处。若其他分支`commit`过了，在主分支上我们对同一文件做出了修改，会提示`merge`失败，因为`git`不知道该如何合并，我们可以`git checkout -- <filename>` 来放弃在master分支对这个文件的修改，或者使用`git stage`存起来。
- 删除分支：`git branch -d <name>`
