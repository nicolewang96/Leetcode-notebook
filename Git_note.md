# Git

###### Resources:
- [clone on mac](https://blog.csdn.net/qq_39832864/article/details/88617268)
- [廖雪峰git教材](https://www.liaoxuefeng.com/wiki/896043488029600/896827951938304)


## 初始设置
```
git config --global user.name "Your Name"
git config --global user.email “email”    //注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置
```

## 创建repository在本地
```
mkdir learngit      //在当前文件夹建立一个叫learngit的文件夹
cd learngit
pwd       //显示当前目录
git init     //将当前文件变成git可以管理的仓库
ls -ah      //显示隐藏文件

git status       //查看仓库当前的状态
git diff          //查看修改前后的不同
git log        //显示从最近到最远的提交日志,可以看到3次提交
git log --pretty=oneline    //简略版
git reflog     //查看之前的工作日志
```

## 版本回退

![时间线](https://github.com/nicolewang96/Leetcode-notebook/raw/main/pictures/分支1.png)

```
git reset --hard HEAD^     //退回到上一个版本（上上个:HEAD^^ 上一百个:HEAD~100）
git reset --hard commit_id  //在退回到以前的版本且想要回到最新版本，可以在后面写上部分新版本的commit id（不用写完整，可以看之前的log）
```
Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向最新的版本

## Git版本库
![Git版本库](https://github.com/nicolewang96/Leetcode-notebook/raw/main/pictures/Git版本库.jpg)


## 撤销修改
1. 仅在工作区进行了修改，但是想撤销这次修改
```
git checkout -- file    //丢弃工作区的修改;让这个文件回到最近一次git commit或git add时的状态
```

2. 已经将修改的文件添加到暂存区还没提交，但是想撤销提交暂存区前的那次修改
```
git reset HEAD file   //把暂存区的修改回退到工作区
git checkout -- file  //再对这个文件的修改操作进行撤销
```

3. 已经将修改的文件提交到了版本库
  
  结合版本回退，然后再根据场景2进行修改（但是已经将这个版本推送到了远程，就无济于事了）


## 删除文件
```
rm file  //直接在文件管理器中删除文件
```

#### 从版本库中删除文件：
```
git rm file
git commit -m "remove file”
```
#### 在文件管理器中删错了文件，可以通过版本库里有的恢复
```
git checkout -- test.txt
```
`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”




## add & commit

- 需要先通过git add将修改文件添加到暂存区，否则不会直接加入到commit中
```
cd /Users/Nicole/Desktop/xxxx  (一定要到达这个需要push的文件里)
git add .         //告诉Git需要把这个文件添加到仓库，必须要在git仓库目录内执行
git add file1.txt
git add file2.txt file3.txt        //可以add很多个文件
git commit -m "First Commit"         //用git commit告诉Git，把文件提交到仓库；可以一次提交很多文件
```

## 分支

![分支](https://github.com/nicolewang96/Leetcode-notebook/raw/main/pictures/分支2.png)

```
git switch -b dev  //创建dev分支，并切换到新分支(-b:创建、切换) —相当于git branch dev + git switch dev
git branch dev   //创建分支
```

* 然后就可以在这个新的分支上干活（更新内容不会影响另外分支的内容）
```
git branch      //查看当前分支
git switch master     //切换到master这个分支

git merge dev    //合并指定分支(dev)到当前分支,快速合并是让指针直接指向dev
git merge --no-ff -m "merge with no-ff" dev    //“--no-ff ”表示禁用Fast forward

git branch -d dev  //删除分支
git branch -D dev  //强行删除分支
(git switch 和 git checkout都是一样的，只是为了与前面的区别理解)
```

## 解决冲突

当合并分支时有冲突，需要解决冲突，比较两个分支中有冲突的文件

#### 查找冲突点方法：
* 冲突文件
* git status
* git log
* git log --graph

## 解决bug

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；
```
git stash       //把当前工作现场“储藏”起来，等以后恢复现场后继续工作
git stash list     //查看隐藏工作现场

git stash apply     //恢复隐藏工作现场
git stash drop     //删除stash内容

git stash pop     //恢复隐藏工作现场并删除
```
* 开发一个新feature，最好新建一个分支



## 标签管理
```
git tag v1.0        //打一个新标签v1.0，默认是打在最新提交的commit上
git tag     //查看所有标签,标签不是按时间顺序列出，而是按字母排序的
git show v0.9     //查看标签信息

git log --pretty=oneline --abbrev-commit   //先查找之前历史提交的commit id
git tag v0.9 f52c633   //结合上一条找到想打标签的commit id 打上标签v0.9
git tag -a v0.1 -m "version 0.1 released" 1094adb     //创建带有说明的标签

git push origin v1.0  //推送标签v1.0到远程
git push origin --tags    //一次性推送全部尚未推送到远程的本地标签

git tag -d v0.1  //删除本地标签
git push origin :refs/tags/v0.1 //如果标签已经推送到远程，先在本地删除标签后，再在远程删除
```

## 远程仓库

本地Git仓库和GitHub仓库之间的传输是通过SSH加密的

### 创建SSH Key
```
//创建SSH Key （对于一个电脑，只用操作一次）
ssh-keygen -t rsa -C “email”
```

### 关联本地与远程
```
//将本地仓库与远程仓库关联（在仓库目录下操作）
git remote add origin git@github.com:nicolewang96/learngit.git (ssh地址)    //管理远程仓库并给其一个名字（origin是默认命名）
```

```
git push -u origin master    //把本地库的所有内容推送到远程库上
git push origin master     //将本地master分支的最新修改推送到GitHub
git push origin dev     //将本地dev分支的最新修改推送到GitHub
```

* 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令

```
git remote -v    //查看远程仓库信息
git remote rm origin    //解除本地和远程的绑定关系

//克隆远程仓库
git clone git@github.com:nicolewang96/gitskills.git
```

## Note
* 当远程与本地仓库建立联系后（不论是pull还是clone），本地文件管理器进行删除添加都无所谓，如果想要同步到远程仓库，需要先 git add/rm 然后再commit到本地master分支；只有通过这样的一系列操作，再通过git push 才能将 ‘本地master分支’ 更新（更新，增加，删除等）同步到远程仓库
* 在本地想要一个目录变成仓库 需要 git init
* 如果是从远程仓库clone下来的文件，这个文件默认为仓库（里面有.git）
* 可以运行git remote add origin ssh 建立远程与本地之间的联系
