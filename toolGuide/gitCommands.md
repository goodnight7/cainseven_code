[TOC]

# git commands

## commands

* `git init`

* `git add <file>`
    * `git add .`

* `git commit -m <description>`

* `git status`

* `git diff <file>`
> 查看文件是否有修改 （修改没有提交到版本库中）

    * `git diff HEAD -- <file>`

    > 比较工作区与版本库的区别

* `git log`
> 查看提交历史

    * `git log--pretty=oneline`
    * `git log --graph --pretty=oneline --abbrev-commit`

* `git reset --hard HEAD^`
> HEAD 表示当前版本 上一个就是 HEAD^ 上上一个版本 HEAD^^ 前100个版本就是 HEAD~100

    * `git reset --hard <commitID>`
    > 与上面的命令完全一样 只不过根据 commit id 来实现版本的切换

    * `git reset HEAD <file>`
    > 把暂存区的修改退回工作区 也就是撤销 `git add ` 操作，将暂存区的文件清零

* `git reflog`
> 记录每一次命令 log

* `git checkout -- <filename>`
> 丢掉工作区的修改： 回到最近一次 git commit 或 git add 时的状态 
> 如果暂存区是干净的，就回到版本库的最新版本；如果暂存区有内容，就回到暂存区的内容，总之这就是把工作区的内容清零
> 本质就是用版本库替换工作区的版本，所以也可以用来恢复不小心删除的文件

* `git rm <file>`
    * `git commit -m <des>`
    > 在版本库里删除文件

* `ssh-keygen -t rsa -C <"email@email.com">`
    
    > 生成 ssh 公钥私钥 在目录`~/.ssh`

* `git remote`
    * `git remote -v`
    > 显示详细信息

    * `git remote add <originName> <sshRemoteResposity>`


* `git push -u <originName> <branches>`
> 第一次推送时 -u 是为了给远程主分支与当前主分支做关联 以后推送和拉取就可以简化命令了

    * `git push <originName> <branches>`

* `git clone <sshRemoteResposity>`
> 取下来的是主分支 且只有主分支 如果要在别的取下别的分支:

    * `git checkout -b <branchName> origin/dev`
    > 创建远程 origin 的 dev 分支到本地，并做关联，且与远程的名称最好一致

    * `git branch --set-upstream dev origin/dev`
    > 本地分支与远程分支建立连接 如果本地分支没有和远程分支做关联的话

    * `git pull`
    > 如果做了关联，直接用这个就可以了


* `git checkout -b <branchName>`
> 创建新的分支并切换到新的分支 与下面两条等效
    * `git branch <branchName>`
    * `git checkout <branchName>`
    
* `git branch`
> 查看分支

* `git branch <branchName>`
> 创建新的分支

* `git checkout <branchName>`
> 切换分支

* `git merge <branchName>`
> 合并分支到当前分支
    
    * `git merge --no-ff`

    > 禁用 fastforward 模式 如下: *好处是删除分支后 可以保存分支的信息 在 log 中可以看到*

    ![禁用 fastforward](http://www.liaoxuefeng.com/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0)

    >fastforward 模式如下 *不好的地方是删除分支后 无法保存分支的信息*:
    
    ![fastforward](http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)


* `git branch -d <branchName>`
> 删除一个已经合并过的分支

* `git branch -D <branchName>`
> 强制删除一个没有合并的分支

* `git stash`
> 临时有 bug 时可以保存现场 未完成的工作 可以多次 stash 

    * `git stash list`
    > 查看工作现场 list

    * `git stash apply`
        * `git stash apply stash@{#}`
    > 恢复到指定的现场 但是现场还没有删除 需要再执行一步删除的操作

        * `git stash drop`
        > 删除 stash 存放的内容


    * `git stash pop`
    > 恢复现场 同时删除现场

* `git tag <tagname>`
> 默认是把标签直接打在最新提交的一次 commit 上

    * `git tag`

    > 查看所有的标签

    * `git tag <tagname> <commitid>`
    > 给对应的 commit 打上标签

        * `git tag -a <tagname> -m <"desc"> <commitid>`

        > 创建带有说明的标签 

    * `git show <tagname>`
    > 查看一条标签的具体信息

    * `git tag -d <tagname>`
    > 删除远程标签有两步：一、删除本地的标签 二、如果还要进行远程库的标签删除 接下来

        * `git push <origin> :refs/tags/v0.9`


    * `git push <remoteResposity> <tagname>`
    > 标签只存储在本地，不会自动推送到远程，把标签推送到远程

    * `git push <remoteResposity> --tags`
    > 一次性推送所有尚未推送到远程的本地标签

* `git config --global color.ui true`

* .gitignore
> 在 .gitignore 里加入想要忽略的文件即可

    * `git add -f <file>`
    > 强制添加文件

    * `git check-ignore -v <file>`
    > 检查文件是否被忽略

* .gitconfig
> 文件位置在`~/.gitconfig`

> 配置别名

    * `git config --global alias.st status`
    * `git config --global alias.unstage 'reset HEAD'`
    * `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`






* 团队开发
    * 分支管理
    ![团队分支管理](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)








