# git学习笔记

## 安装

1. 从官网[下载安装程序](https://git-scm.com/downloads)

2. 新建文件夹作为本地仓库存放路径

3. 右键菜单或开始菜单`Git Bash`

4. 配置全局用户名和Email地址，把`Your Name`和`email@example.com`替换成自己的名字和仓库后执行

   ```bash
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   ```
5. 使用`git config --list`命令查看设置

## 创建版本库

1. 初始化，在创建好的文件夹中执行`git init`

2. 工作区修改，本地修改文件，例如新建文件`readme.md`

3. 添加到暂存区，执行`git add readme.md`

4. 提交到当前分支，执行`git commit -m "新建readme.md文件"`提交更改

   > `-m`参数记录本次提交的备注信息，建议每次提交都要带上该参数

5. `git status`可以查看当前git的状态

6. `git diff`可以查看工作区修改的具体内容


## 版本控制

1. 回退版本，撤销`git commit`

   1. 执行`git log --pretty=oneline`显示从最近到最远的提交日志和`commit id`

     > `--pretty=oneline`参数精简显示输出，非必须

   2. 执行`git reset --hard HEAD^`回退到上一个版本

     > `HEAD`：当前版本
     >
     > `HEAD^`：上个版本
     >
     > `HEAD~100`：往上100个版本
   3. 执行`git reflog`显示每一次命令和`commit id`，即使退出了`git bash`后也能显示之前的记录，执行` git reset --hard 1094a`回退到`commit id`为`1094a`的版本

2. 暂存区回退，撤销`git add`
   
   `git reset HEAD <file>`可以把暂存区的修改撤销掉，重新放回工作区
   
3. 工作区回退，撤销本地修改
   
   `git checkout -- file`可以丢弃工作区的修改，`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”
   
   > `file`是要撤销修改的文件，有两种情况：
   >
   > 一种是自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
   >
   > 一种是已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态
   
4. 删除文件

   本地删除文件，执行`git status`能够提示工作区与暂存区不一致

   - 执行`git rm <file>`将工作区的删除操作提交到暂存区，并且`git commit`提交到版本库
   - 撤销删除，执行`git restore --staged <deleted file>`丢弃工作区的change

## 远程仓库

1. [GitHub](https://github.com/)仓库配置

   1. 打开Git Bash，创建SSH Key，其中邮箱地址换成自己的，执行后回车使用默认值，在用户主目录找到`.ssh`目录，找到`id_ras`和`id_rsa.pub`两个文件

     ```bash
     $ ssh-keygen -t rsa -C "youremail@example.com"
     ```

   2. 注册并登录GitHub账号，右上角头像`Settings`-->`SSH and GPG keys`-->`New SSH key`，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容

2. 添加远程库`origin`

   1. 右上角`+`号，`New repository`创建一个新的仓库

   2. 在本地仓库运行GitHub提示的命令
     ```bash
     $ git remote add origin  git@github.com:/xxxx/xxxx.git
     ```
   
     > `origin`：远程仓库名，默认习惯命名
   
   3. 推送本地库所有内容到远程库上
   
     ```bash
     $ git push -u origin master
     ```
   
     > `git push`：把本地库内容推送到远程，实际上是把当前分支`master`推送到远程
     >
     > `-u`：把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时可以简化命令
   4. 从现在起，只要本地作了提交，就可以通过以下命令把本地`master`分支的最新修改推送至GitHub
     ```bash
     $ git push origin master
     ```

3. 删除远程库（解除绑定）
   1. `git remote -v`查看远程库信息
   2. 根据名字解除绑定关系`git remote rm origin`

4. 从远程库克隆

   1. 登录GitHub创建一个新的仓库

   2. 克隆到本地库
     ```bash
     $ git clone git@github.com:xxxx/xxxx.git
     ```

## 分支管理

1. 创建与合并分支

   1. 创建`dev`分支并切换到`dev`分支
     ```bash
     $ git switch -c dev
     ```

     > `-c`：创建并切换分支，相当于`git branch dev`+`git switch dev`
     >
     > `git branch dev`：创建分支`dev`
     >
     > `git switch dev`：切换分支到`dev`
     >
     > `git branch`：查看当前分支，标记`*`号的是当前分支
   2. 修改`dev`分支(`git add`和`git commit`)后合并到`master`分支
     ```bash
     $ git switch master
     $ git merge dev
     ```

     > `git merge`命令用于合并指定分支到当前分支，合并前需要先切换分支
   3. 删除`dev`分支
     ```bash
     $ git branch -d dev
     ```
     > `-D`：强制删除分支

2. 解决冲突

   1. 多个分支修改了同一个文件，合并时会造成冲突

   2. `git status`查看冲突的文件，查看文件内容，Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容

     ```
     <<<<<<< HEAD
     Creating a new branch is quick & simple.
     =======
     Creating a new branch is quick AND simple.
     >>>>>>> feature1
     ```

   3. 修改文件内容后提交，可以用`git log`查看分支合并情况

3. 分支管理策略

   1. 通常合并时Git会用`Fast forward`模式，删除分支后，会丢掉分支信息

   2. 使用`--no-ff`参数进行`git merge`，强制禁用`Fast forward`模式，会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

     ```bash
     $ git merge --no-ff -m "merge with no-ff" dev
     ```

   3. 分支策略
       `master`：稳定分支，仅用来发布新版本
       `dev`：平时提交的分支，仅在发布新版本时会合并到`master`分支上
       `xxx`：团队其他人每个人自己都有一个分支，偶尔合并到`dev`分支

4. Bug分支

   1. 使用`git stash`存储当前工作区状态

   2. 切换到要修复Bug的分支，创建临时分支`issue-101`
     ```bash
     $ git checkout master
     $ git checkout -b issue-101
     ```

   3. 修复Bug后切换到`master`分支，完成合并，删除`issue-101`分支
     ```bash
     $ git switch master
     $ git merge --no-ff -m "merged bug fix 101" issue-101
     $ git branch -d issue-101
     ```

   4. 返回dev分支，恢复之前存储的工作区状态
     ```bash
     $ git switch dev
     $ git stash list
     $ git stash drop
     ```

     > `git stash list`：查看之前存储的工作区状态
     >
     > `git stash pop`：恢复的同时把stash内容也删除
     >
     > `git stash apply`：恢复后stash内容并不删除
     >
     > `git stash apply stash@{0}`：恢复指定的stash

   5. 修复当前dev分支上的Bug
     ```bash
     $ git cherry-pick 4c805e2
     ```

     > `cherry-pick`：复制一个特定的提交到当前分支

5. 多人协作

   1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
   2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
   3. 如果合并有冲突，则解决冲突，并在本地提交；
   4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！
   5. 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

## 标签管理

1. 标签是什么

   Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针，相当于别名

2. 创建标签
   1. 切换到需要打标签的分支上
      ```
      $ git branch
      $ git checkout master
   
   2. `git tag <name>`打一个新标签
   
      ```bash
      $ git tag v1.0
      ```
   
   3. `git tag`查看所有标签
   
   4. 默认标签是打在默认为`HEAD`上的，给历史commit打标签
      ```bash
      $ git log --pretty=oneline --abbrev-commit
      $ git tag v0.9 f52c633
      ```
   
   5. `git show <tagname>`查看标签信息
   
      ```bash
      $ git tag -a v0.1 -m "version 0.1 released" 1094adb
      ```
   
      > `-a`指定标签名，`-m`指定说明文字

 3. 操作标签

    - `git tag -d v0.1`删除本地标签

    - `git push origin <tagname>`推送某个标签到远程

    - ` git push origin --tags`一次性推送全部尚未推送到远程的本地标签

    - 删除远程标签，本地删除后从远程删除
      ```bash
      $ git tag -d v0.9
      $ git push origin :refs/tags/v0.9
      ```

## 使用GitHub

1. 在要参与的项目点击`Fork`在自己账号下克隆一个仓库
2. 从自己的账号下`clone`该仓库到本地
3. 本地修改后提交到自己的仓库
4. 如果希望给官方仓库贡献代码，可以在GitHub上发起一个`pull request`

## 使用Gitee

1. 注册Gitee账号后，本地配置用户名、邮箱、SSH公钥

2. 在Gitee上创建一个新的项目

3. 在本地库上使用命令`git remote add`命令关联Gitee的远程库

   > 关联时注意不要和已有的远程库重名

## 自定义Git

1. 忽略特殊文件

   - 使用`.gitignore`文件，可参考官方[配置文件](https://github.com/github/gitignore)，以下是一个示例
     ```gitignore
     # Windows:
     Thumbs.db
     ehthumbs.db
     Desktop.ini
     
     # Python:
     *.py[cod]
     *.so
     *.egg
     *.egg-info
     dist
     build
     
     # My configurations:
     db.ini
     deploy_key_rsa
     ```

   - 把指定文件排除在`.gitignore`规则外的写法就是`!`+文件名

   - `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理

2. 设置别名

   1. eg：
      ```bash
      $ git config --global alias.st status
      $ git config --global alias.st status
      $ git config --global alias.co checkout
      $ git config --global alias.ci commit
      $ git config --global alias.br branch
      $ git config --global alias.unstage 'reset HEAD'
      $ git config --global alias.last 'log -1'
      $ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
      ```

   2. 配置文件

      > 配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用

      - 每个仓库的Git配置文件都放在`.git/config`文件中
      - 当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中
      - 配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置

## [Sourcetree](https://www.sourcetreeapp.com/)

推送至远程仓库时出现报错提示：`If you trust this host, enter "y" to add the key`

解决方案：工具->选项->SSH客户端：更换默认的`Putty`为`openSSH`

## 其他

搭建Git服务器和使用SourceTree相关教程可直接移步[廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/896043488029600)，感谢廖老师的教程~

