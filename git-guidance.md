

## GIT教程

### 安装教程

[参考这里](https://blog.csdn.net/qq_41782425/article/details/85183250)

### 工作流程

<img src="E:\vscodefile\image\git.jpg" alt="n" style="zoom: 80%;" />

### 常用命令

#### 版本关联
```git
git init           #初始化当前文件夹为一个git版本库

git status         #查看仓库状态
git diff <file>    #查看文件修改内容
git log            #查看历史提交记录

git add <file>     #把工作目录的修改提交到暂存区
git add --all      #提交所有文件

git commit <file> -m <message>    #把暂存区的内容提交到本地仓库
git commit --all -m <message>

git reset --hard HEAD^       #回退到上一个版本
git reset --hard commitid    #回退到commitid对应的版本，commitid可用git log查看

git checkout --<file>        #撤销修改,回到最近一次git commit或git add时的状态
```










#### 远程仓库

**推荐先远程建好仓库和分支后，然后克隆到本地后进行开发。**

```
git clone git@github.com:dc-zhangy/demo.git #克隆远程仓库到本地,此时本地仓库与远程仓库已经关联
git remote add origin git@github.com:dc-zhangy/zy.git  #已有本地仓库关联已有远程仓库

git remote        # 查看本仓库是否关联远程仓库
git remote update origin --prune   # 更新远程仓库新创建的分支, orgin指的就是远程仓库

git push  #将本地仓库的最新状态更新到远程
git pull   #将远程仓库的最先状态合并到本地  git pull = git fetch + git merge
```



#### 分支管理

![](E:\vscodefile\image\branch.png)

**分支创建**

- 远程已有，本地创建

```
git branch dev orgin/dev     #此时两个分支已经关联，push和pull都是在这两分支上进行。
```

- 本地无，远程也无

```
git branch dev         #基于当前所在分支(一般是master)，创建一个branch的分支
git push -u origin dev     #本地dev分支提交到远程仓库，即创建了远程分支dev，-u表示已经建立了关联
```

```
git switch/checkout dev       #切换到dev 分支
git checkout -b dev       #创建并切换到dev分支

git branch           #查看本地分支
git branch  --all    #查看所有分支(包括远程)

git branch -d dev    #删除dev分支

git merge dev       #将dev分支的成果合并到当前所在分支，一般是master
```

