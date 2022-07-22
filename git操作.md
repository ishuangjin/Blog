# git操作

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

   - 执行`git log --pretty=oneline`显示从最近到最远的提交日志和`commit id`

     > `--pretty=oneline`参数精简显示输出，非必须

   - 执行`git reset --hard HEAD^`回退到上一个版本

     > `HEAD`：当前版本
     >
     > `HEAD^`：上个版本
     >
     > `HEAD~100`：往上100个版本
   - 执行`git reflog`显示每一次命令和`commit id`，即使退出了`git bash`后也能显示之前的记录，执行` git reset --hard 1094a`回退到`commit id`为`1094a`的版本

2. 暂存区回退，撤销`git add`
   
   - `git reset HEAD <file>`可以把暂存区的修改撤销掉，重新放回工作区
   
3. 工作区回退，撤销本地修改
   
   - `git checkout -- file`可以丢弃工作区的修改，`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”
   
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

   - 打开Git Bash，创建SSH Key，其中邮箱地址换成自己的，执行后回车使用默认值，在用户主目录找到`.ssh`目录，找到`id_ras`和`id_rsa.pub`两个文件

     ```bash
     $ ssh-keygen -t rsa -C "youremail@example.com"
     ```
   
   - 注册并登录GitHub账号，右上角头像`Settings`-->`SSH and GPG keys`-->`New SSH key`，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容

2. 添加远程库`origin`

   - 右上角`+`号，`New repository`创建一个新的仓库

   - 在本地仓库运行GitHub提示的命令
     ```bash
     $ git remote add origin https://github.com/xxxx/xxxx.git
     ```
     
     > `origin`：远程仓库名，默认习惯命名
     
   - 推送本地库所有内容到远程库上
   
     ```bash
     $ git push -u origin master
     ```
   
     > `git push`：把本地库内容推送到远程，实际上是把当前分支`master`推送到远程
     >
     > `-u`：把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时可以简化命令
   - 从现在起，只要本地作了提交，就可以通过以下命令把本地`master`分支的最新修改推送至GitHub
     ```bash
     $ git push origin master
     ```

3. 删除远程库（解除绑定）
   - `git remote -v`查看远程库信息
   - 根据名字解除绑定关系`git remote rm origin`
