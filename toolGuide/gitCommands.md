[TOC]

# git commands

## commands

* `git init`

* `git add <file>`
    * `git add .`

* `git commit -m <description>`

* `git status`

* `git diff <file>`
    * `git diff HEAD -- <file>`
    > 比较工作区与版本库的区别

* `git log`
    * `git log--pretty=oneline`
    * `git log --graph --pretty=oneline --abbrev-commit`

* `git reset --hard HEAD^`
    * `git reset --hard <commitID>`
    * `git reset HEAD <file>`
    > 把暂存区的修改退回工作区 也就是撤销 `git add ` 操作

* `git reflog`

* `git checkout -- <filename>`
> 丢掉工作区的修改

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
    * `git push <originName> <branches>`

* `git clone <sshRemoteResposity>`
> 取下来的是主分支 如果要在别的取下别的分支 :

    * `git checkout -b <branchName> origin/dev`

    * `git branch --set-upstream dev origin/dev`
    > 本地分支与远程分支建立连接

    * `git pull`


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
    fastforward 模式如下 *不好的地方是删除分支后 无法保存分支的信息*:
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


    * `git stash pop`
    > 恢复现场 同时删除现场

* `git tag <tagname>`

    * `git tag`

    * `git tag <tagname> <commitid>`

        * `git tag -a <tagname> -m <"desc"> <commitid>`

    * `git show <tagname>`

    * `git tag -d <tagname>`
    > 删除本地的标签 如果还要进行远程库的标签删除 接下来

        * `git push <origin> :refs/tags/v0.9`


    * `git push <remoteResposity> <tagname>`

    * `git push <remoteResposity> --tags`

* `git config --global color.ui true`

* .gitignore
    * `git add -f <file>`
    > 强制添加文件

    * `git check-ignore -v <file>`
    > 检查文件是否被忽略

* .gitconfig
> 文件位置在`~/.gitconfig`

    * `git config --global alias.st status`
    * `git config --global alias.unstage 'reset HEAD'`
    * `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`






* 团队开发
    * 分支管理
    ![团队分支管理](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)








