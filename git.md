## vscode配置git

打开vscode，然后 文件 > 首选项 > 设置 ，然后添加 "git.path"键，值为Git目录下的cmd下的git.exe文件。如： "git.path":"D:/Program Files/Git/cmd/git.exe"

# git中ssh与https

```
1.clone项目:使用ssh方式时，首先你必须是该项目的管理者或拥有者，并且需要配置个人的ssh key。而对于使用https方式来讲，就没有这些要求。
2.push:在使用ssh方式时，是不需要验证用户名和密码，如果你在配置ssh key时设置了密码，则需要验证密码。而对于使用https方式来讲，每次push都需要验证用户名和密码。
```

```
$ git config --global  --list // 查看当前用户(global)配置
$ git config --system  --list // 查看系统config
$ git config --local  --list // 查看当前仓库配置信息
```

# 生成新 SSH 密钥并添加到 ssh-agent

```shell
$ ssh-keygen -t rsa -b 4096 -C "zhangqianlong@wljs.com"
生成的SSH密钥地址： C:\Users\EDZ\.ssh
//config配置
$ git config --global user.name "zhangqianlong"
$ git config --global user.email "zhangqianlong@wljs.com"

$ git clone git@gitlab.weilaijishi.net:future-mall-frontend/h5-client/h5-main.git
*****提示：Enter passphrase for key '/c/Users/EDZ/.ssh/id_rsa':
*****输入密码：simahe@#2019
账号：zhangqianlong@wljs.com
密码：simahe@#2019
```



```
http://localhost:3000/#/root/
用13800138001 验证码888888
```



# git基本操作

- git add 添加文件到暂存区`git add .   `
- git status 查看仓库当前的状态，显示有变更的文件`git status`
- git diff 比较文件的不同，即暂存区和工作区的差异。`git diff [file]`
- git commit 提交暂存区到本地仓库。`git commit -am '修改 hello.php 文件'`
- git reset 回退版本。`git reset  [HEAD] `
- git rm 删除工作区文件。`git rm <file>`
- git mv 移动或重命名工作区文件。`git mv -f [file] [newfile]`
- git log 查看历史提交记录`git log --oneline`
- git blame<file>以列表形式查看指定文件的历史修改记录`git blame README `
- git remote 远程仓库操作,`git remote -v`
- git fetch 从远程获取代码库`git merge`  `git fetch`
- git pull 下载远程代码并合并`git pull <远程主机名> <远程分支名>:<本地分支名>`
- git push 上传远程代码并合并`git push <远程主机名> <本地分支名>:<远程分支名>`

## DEMO

```
1、选择一个合适的地方，创建一个空目录
$ mkdir learngit
$ cd learngit
2、通过git init命令把这个目录变成Git可以管理的仓库：
$ git init 通过git init命令把这个目录变成Git可以管理的仓库：
3、添加文件，提交
$ git add readme.txt  把要提交的所有修改放到暂存区（Stage）
$ git add A
$ git add -A  提交所有变化
$ git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
$ git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
$ git commit -m "wrote a readme file" 一次性把暂存区的所有修改提交到分支。
4、时光机穿梭
$ git status  命令看看结果
$ git diff readme.txt 不清上次怎么修改的readme.txt
5、版本回退
$ git log
$ git log --pretty=oneline 嫌输出信息太多，看得眼花缭乱的
$ git reset --hard HEAD^  回退到上一个版本add distributed
$ git reset --hard 1094a
$ git reflog 用来记录你的每一次命令
6、工作区和暂存区
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
用git status查看一下状态
7、管理修改和撤销修改
每次修改，如果不用git add到暂存区，那就不会加入到commit中
$ git checkout -- readme.txt 让这个文件回到最近一次git commit或git add时的状态。
8、删除文件
$ rm test.txt 删除一个文件
$ git checkout -- test.txt 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

9、添加远程库====已有本地仓库learngit
$ git remote add origin git@github.com:zql/learngit.git 
$ git push -u origin master  把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
$ git push origin master 从现在起，只要本地作了提交，就可以通过命令：

10、从远程库克隆，最好的方式是先创建远程库，然后，从远程库克隆。
$ git clone git@github.com:michaelliao/gitskills.git
11、分支管理
===========创建与合并分支
$ git checkout -b zql-test  创建dev分支，然后切换到dev分支：相当于下面2条
$ git branch dev   创建dev分支
$ git checkout dev 换到dev分支
$ git branch 查看当前分支
$ git checkout master
$ git merge dev 我们把dev分支的工作成果合并到master分支上
$ git branch -d dev 删除dev
$ git branch 查看当前分支
============删除分支
$ git branch -d zql-test（remotes/origin/分支名） 删除本地分支：
$ git branch -D zql-test  强制删本地
$ git push --delete origin zql-test 删除远程分支：
============分支管理策略
$ git merge --no-ff -m "merge with no-ff" dev
$ git log --graph --pretty=oneline --abbrev-commit  看看分支历史
12、标签管理
$ git branch 查看分支
$ git branch -a 查看远程分支
$ git checkout master 切换分支
$ git tag v1.0  打一个新标签
$ git tag 查看所有标签
$ git show v1.0 查看标签信息
$ git tag -a v1.0 -m "version 1.0 released" 1094adb 用-a指定标签名，-m指定说明文字
$ git show v0.1 可以看到说明文字
$ git tag -d v0.1 删除标签
$ git push origin v1.0 推送某个标签到远程****
$ git push origin --tags 推送全部尚未推送到远程的本地标签合并
如果标签已经推送到远程，要删除远程标签就麻烦一点
$ git tag -d v0.9 ，先从本地删除：
$ git push origin :refs/tags/v0.9 然后，从远程删除。删除命令也是push，但是格式如下：
要看看是否真的从远程库删除了标签，可以登陆GitHub查看。


===========远程分支操作
$ git fetch  刷新
$ git branch -a 查看远程分支
$ git checkout -b release-log origin/release-log  不行就用下面这个
$ git branch -r -d origin/zql-test  删除远程分支
$ git push origin --delete  zql-test 删除远程分支：

$ git checkout -b dev-bsc origin/dev-bsc 
$ git checkout -b release origin/release 

$ git checkout -b future-1.1.9 origin/future-1.1.9
$ git fetch origin future-1.1.9:future-1.1.9
用上面这个需要第一次
$ git push -u origin future-1.1.9


 ========
 feedback分支操作
 
 git checkout -b  feedback
 git branch -d future-1.1.9
 git merge feedback
 
 
git fetch     刷新分支
git branch -a 查看分支

 git checkout -b hotfix-service origin/hotfix-service
 
 git checkout -b release-user origin/release-user
 git checkout -b future-cashier-0729 origin/future-cashier-0729
 
  git checkout -b dev origin/dev
  
  git pull --rebase origin/develop

git checkout -b future-social


 git checkout -b feature-lock origin/feature-lock
```



![git代码管理规范](D:\simahe\full-stack\Documents\git\git代码管理规范.jpg)

## Git Flow—Git团队协作最佳实践

### Git Flow不同分支的角色

结合图片，简单介绍一下不同分支的职责。

#### 1.Production分支

这个分支是发布到生产环境的代码，这个分支只能从其他分支合并，不能在这个分支直接修改。

#### 2.Develop分支

这个分支是主开发分支，包含所有要发布到下一个Release的代码，这个主要合并自其他分支，比如Feature分支。

#### 3.Feature分支

Feature 分支主要用来开发一个新的功能，一旦开发完成，合并回Develop分支，并且进入下一个Release，Feature分支可以选择删除或者保留。

#### 4.Release分支

当需要发布一个新Release的时候，基于Develop分支创建一个Release分支，Release分支在测试过程中可能会修改，完成Release后，合并到Master和Develop分支。

#### 5.Hotfix分支

当在Production发现新的Bug时候，需要创建一个Hotfix分支, 完成Hotfix后，合并回Master和Develop分支，所以Hotfix的改动会进入下一个Release。

### Git Flow使用原则

- Master分支是线上稳定分支，Release通常用作测试分支，Develop分支是开发应用的主分支
- 所有的功能开发都在Feature分支进行，然后合并到Develop分支
- Release分支发布后出现问题，直接在Release分支修改，避免Develop分支代码污染

 

## 三、Git Flow分支协作最佳实践https://yq.aliyun.com/articles/68655

我们在应用Git Flow的时候，也遇到了一些问题，比如开发结束后，在develop分支进行merge开发分支操作，出现冲突如果不能很好的解决，容易对develop分支的代码造成污染。

下面是实际开发中使用的流程，在Feature分支上合并develop代码，然后合并到develop分支上，流程更加清晰，冲突优先在开发分支解决。

**一个开发人员典型的提交流程：**

```

//新建分支
git checkout develop   
git pull origin develop
git checkout -b myfeature
 
//在分支上开发
git add ***
git commit -m "*****"
 
//在分支开发过程中合并develop分支到本分支（先把自己的工作commit到本地）
git checkout develop
git pull origin develop
git checkout myfeature
git merge develop
 
（如果没有冲突，就继续开发，如果有冲突，执行下面过程）
首先在本地解决冲突，再把冲突解决commit
git add ***
git commit -m "*****"
 
//在分支开发结束，需要将本分支合并到develop分支（先把自己的工作commit到本地）
git checkout develop
git pull origin develop
git merge myfeature
 
（如果没有冲突，就推送到远程）
git push origin develop
（如果有冲突，则解决冲突，再commit，并推送到远程：）
git add ***
git commit -m "*****"
git push origin develop

```

## git提交规范

| **类型** | **描述**                                               |
| -------- | ------------------------------------------------------ |
| build    | 编译相关的修改，例如发布版本、对项目构建或者依赖的改动 |
| chore    | 其他修改, 比如改变构建流程、或者增加依赖库、工具等     |
| ci       | 持续集成修改                                           |
| docs     | 文档修改                                               |
| feat     | 新特性、新功能                                         |
| fix      | 修改bug                                                |
| perf     | 优化相关，比如提升性能、体验                           |
| refactor | 代码重构                                               |
| revert   | 回滚到上一个版本                                       |
| style    | 代码格式修改, 注意不是 css 修改                        |
| test     | 测试用例修改                                           |


