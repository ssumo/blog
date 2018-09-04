---
title: 学习Git的总结
date: 2016-12-20 22:55:46
tags: Git
categories: Git
---
博客迁移,这还是刚入行2015年时的总结,基础但还是感觉还是有些价值的。

<!-- more -->

第一次学习**Git**是完全按照[廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)老师的教程学习的,学的过程中基本上没有遇到什么问题,但是自己实际操作就问题不断了.

首先,还是按照惯例,来膜拜一下[廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)老师精简的教程知识吧,中间穿插一些自己的感悟.


<!-- more -->

**安装**
git:msysgit是Windows版的Git，从http://msysgit.github.io/下载，然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

安装完成后，还需要最后一步设置，在命令行输入

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
**创建repository**
```
$ mkdir learngit
$ cd learngit
$ pwd

```
**初始化项目**`$ git init`

添加文件(此处可将整个文件添加 使用`git add filename` 后面也可以空格之后再加其他的文件...
```
$ git add readme.txt
$ git commit -m "wrote a readme file"

```
add所有文件`$ git add .`

add并rm所有`$ git add -A`

提交modify和rm（新加的文件不会自动提交）`$ git commit -a`

>**注意**:此处的`git commit -m "xxx"` 里的`"xxx"` 最好写上,属于给你提交的文件一个tag,利人利己,何乐不为呢.

>初始化一个Git仓库，使用**git init**命令。

>添加文件到Git仓库，分两步：

>第一步，使用命令**git add <file>**，注意，可反复多次使用，添加多个文件；

>第二步，使用命令**git commit**，完成。

**查看状态与版本倒退**

查看你刚才操作的状态`$ git status`

查看修改内容`$ git diff readme.txt` 

查看log，后面参数可选(数字)`$ git log (--prety=oneline)`


回退上个版本（HEAD指向master的指针，^个数表示上几个版本）`$ git reset --hard HEAD^`

回退n个版本`$ git reset --hard HEAD~100` 
`
回退到commit为3628164版本`$ git reset --hard 3628164`

查看每一次命令`$ git reflog`　

把readme.txt文件在工作区的修改全部撤销`$ git checkout --readme.txt`
　　　　　　　　
>这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

命令中的“`--`”很重要，没有“`--`”，就变成了“创建一个新分支”的命令`$ git checkout -- file`

可以把暂存区的修改撤销掉（unstage），重新放回工作区`$ git reset HEAD readme.txt`

从版本库中删除文件`$ git rm readme.txt`

如果在文件管理器中误删了文件，因为版本库里还有呢，可以用这句恢复`$ git checkout -- readme.txt`

**远程库**

关联一个远程库，使用命令

```
$ git remote add origin https://github.com/yourgithubname/gitdemo.git
```
也可以是这种(前提在ssh建立之后,账户密码填写之后)

```
$ git remote add origin git@github.com:yourgithubname/gitdemo.git
```

关联后，使用命令,第一次推送master分支的所有内容

```
$ git push -u origin master
```
每次本地提交后，只要有必要，就可以使用命令推送最新修改

```
$ git push origin master
```
克隆repository。创建一个空的repository，然后用命令

```
$ git clone https://github.com/cloneurl/clonetest
```
**分支与冲突**

查看分支：`$ git branch`

创建分支：`$ git branch name`

切换分支：`$ git checkout name`

创建+切换分支：`$ git checkout -b name`

合并某分支到当前分支：`$ git merge name`

 -no-ff参数，表示禁用“Fast forward”  会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```
$ git merge --no-ff -m "merge with no-ff" dev 
```

删除分支：`$ git branch -d name`
强行删除分支（当在分支上修改后，不想并入其他分支，想删掉，就要强行删除）

```
$ git branch -D branch-name
```
解决分支合并产生的冲突：

手动修改冲突后提交就行

命令可以看到分支合并图`$ git log --graph`


当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue -101来修复它，但是，等等，当前正在dev上进行的工作还没有提交

stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：git stash（有修改还没add并commit的时候调用后，现场保留，分支清空）

命令查看工作现场`$ git stash list`

恢复有两个办法：

一是用`$ git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`$ git stash drop`来删除；

另一种方式是用`$ git stash pop`,恢复的同时把stash内容也删了

查看远程库的信息`$ git remote`

显示更详细的信息`$ git remote -v`

抓取与推送
```
$ git remote -v
origin https://github.com/githubname/gitdemo.git (fetch)　　
origin https://github.com/githubname/gitdemo.git (push)　　
```
显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址

推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上

```
$ git push origin master
```

如果要推送其他分支，比如dev，就改成

```
$ git push orgin dev
```

抓取分支

小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支

```
$ git checkout -b dev origin/dev
```

现在，就可以在dev上继续修改，然后，时不时地把dev分支push到远程

```
$ git push origin dev
```

碰巧你也对同样的文件作了修改，并试图推送：推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突

先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送

```
$ git pull
```

失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接

```
$ git branch --set-upstream dev origin/dev
```

再pull就行了，但合并会有冲突。需要手动解决


**标签管理**

发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

**创建标签**

切换到需要打标签的分支上`$ git checkout dev`

1、敲命令`$ git tag name`就可以打一个新标签`$ git tag v1.0`


2、查看所有标签`$ git tag`

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了

```
$ git log --pretty=oneline --abbrev-commit
```

对应的commit id是“ef51d63”，敲入命令$ `git tag v0.9 ef51d63`

3、可以用`$ git show tagname`查看标签信息`$ git show v0.9`

4、可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

```
$ git tag -a v0.1 -m "version 0.1 released" 02172e4
```

5、可以通过-s用私钥签名一个标签

```
$ git tag -s v0.2 -m "signed version 0.2 released" a8093be
```

签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错

用命令`$ git show tagname`可以看到PGP签名信息

用PGP签名的标签是不可伪造的，因为可以验证PGP签名。验证签名的方法比较复杂

 

**操作标签**

标签打错了，也可以删除`$ git tag -d v1.0`

推送某个标签到远程，使用命令`$ git push origin tagname：`

一次性推送全部尚未推送到远程的本地标签`$ git push origin --tags`

从远程删除。删除命令也是`$ git push origin :refs/tags/tagname`

 
**配置别名**

如果敲`$ git st`就表示`$ git status`那就简单多了

1、只需要敲一行命令，告诉Git，以后st就表示status`$ git config --global alias.st status`

-2、-global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

3、命令`$ git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个unstage别名

```
$ git config --global alias.unstage 'reset HEAD'
```

敲入命令`$ git unstage test.py`

实际上Git执行的是`$ git reset HEAD test.py`

4、配置一个`$ git last`，让其显示最后一次提交信息`$ git config --global alias.last 'log -l'`

用`$ git last`就能显示最近一次的提交


**报错:**
>如果输入`$ git push origin master`
    提示出错信息：error:failed to push som refs to .......
    解决办法如下：
    1、先输入`$ git pull origin master` //先把远程服务器github上面的文件拉下来
    2、再输入`$ git push origin master`
    3、如果出现报错 fatal: Couldn't find remote ref master或者fatal: 'origin' does not appear to be a git repository以及fatal: Could not read from remote repository.
    4、则需要重新输入`$ git remote add origin git@github.com:yourgithubname/gitdemo.git`


>如果输入`$ git remote add origin git@github.com:yourgithubname/gitdemo.git`
    提示出错信息：fatal: remote origin already exists.
    解决办法如下：
    1、先输入`$ git remote rm origin`
    2、再输入`$ git remote add origin git@github.com:yourgithubname/gitdemo.git` 
    3、再输入`-f`为强制推送`$ git push -f origin master`


使用git在本地创建一个项目的过程
    `$ makdir ~/hello-world`    //创建一个项目hello-world
    `$ cd ~/hello-world`       //打开这个项目
    `$ git init`             //初始化 
    `$ git add README`        //更新README文件
    `$ git commit -m 'first commit'`     //提交更新，并注释信息“first commit”
    `$ git remote add origin git@github.com:yourgithubname/gitdemo.git`     //连接远程github项目  
    `$ git push -u origin master`     //将本地项目更新到github项目上去


国外网友制作的Git Cheat Sheet，建议打印出来备用：

[Git Cheat Sheet](http://www.git-tower.com/blog/git-cheat-sheet/)

Git的官方网站：[http://git-scm.com](http://git-scm.com)，英文自我感觉不错的童鞋，可以经常去官网看看.

报错部分欢迎更新提醒.

