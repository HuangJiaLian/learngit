# 理解

为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件

`git checkout -- test.txt`
`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改 第一次才要加入`-u`参数

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放`http`端口的公司内部就无法使用`ssh`协议而只能用`https`。

# 分支管理
分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。


## 创建分支dev, 并切换到dev分支
`git checkout -b dev`
## 查看当前分支
`git branch`
```bash
jack@company ~/Desktop/Github/learngit $ git branch 
* dev
  master
```
## 切回到主分支
`git checkout master`

## 合并dev到master分支
`git merge dev`
```bash
Updating f15012b..7a18d44
Fast-forward
 readme.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 readme.txt
```
## 删除dev分支
`git branch -d dev`
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。

## 分支我理解, 但是后面怎么合并, 不会出问题吗? 
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000

## 更直观地查看log
git log --graph --pretty=oneline --abbrev-commit

## 分支管理策略
常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
`git merge --no-ff -m "merge with no-ff" dev`
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

## Bug分支
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

## Feature分支
软件开发中，总有无穷无尽的新的功能要不断添加进来。
添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

## 多人协作
查看远程仓库的信息
`git remote -v`
推送分支
推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
推送到master分支
`git push origin master`
推送到其他分支,例如`dev`分支
`git push origin dev`


