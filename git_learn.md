# Git 常用命令

**()表示可选项**

![image-20220615152415329](.\git_learn.assets\image-20220615152415329.png)

## 工作区&暂存区

git init 初始化仓库

git diff readme.md/git diff 查看工作区和暂存区的差异

git add readme.md （git add在commit之前可以多次执行）

git status 查看仓库状态，可以看到commit之前暂存区的修改，commit之后，status就clean了

git restore --staged sample_data/README.md 该命令将add到暂存区的修改撤回

git commit -m 'xxxxx' 将暂存区的所有add的修改全部提交

## 版本回退

git log (--pretty=oneline) 可以查看由近到远的commit历史提交记录，按Q可以退出

**HEAD表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往上100个版本就是`HEAD~100`**

git reset --hard HEAD^ 回退到上一个版本 **（注意，该操作会修改工作区文件，返回到上一个commit之前）**

git reset --hard commit_id Git允许我们在版本的历史之间穿梭

commit_id可以通过查看历史git命令来寻找git reflog

## 本地仓库&远程仓库