# GitHub学习

### **GitHub** 与 **Git** 的关系

1. GitHub

- 主要提供基于git的版本托管服务

2. Git

   - Git 是一款免费、开源的分布式版本控制系统。

   - 那么什么是版本控制系统？版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。

     

### GitHub有什么作用

- 学习优秀的开源项目，开源社区一直有一句流行的话叫「不要重复发明轮子」

- 多人协作

- 搭建博客、个人网站或者公司官网

- 写作

- 个人简历

  

### GitHub的基本概念

- Repository 仓库

- Issue 提问题或者处理问题

- Star 星星

- Fork 在原有项目的基础上新建了一个分支，不会影响原有项目的代码与结构

- Pull Request （PR）基于Fork，修改人修改代码后发起PR请求将修改代码合并入原代码中，原作者review修改，觉得OK会接受修改，merge代码，这样原项目就接收了修改。

- Watch  Watch 了某个项目后会收到关于这个项目的更新通知提醒

- Gist  分享代码片段

  

[GitHub 的 Pull Request 是指什么意思？ 作者：beepony](https://www.zhihu.com/question/21682976)

> 我尝试用类比的方法来解释一下 pull reqeust。想想我们中学考试，老师改卷的场景吧。你做的试卷就像仓库，你的试卷肯定会有很多错误，就相当于程序里的 bug。老师把你的试卷拿过来，相当于先 fork。在你的卷子上做一些修改批注，相当于 git commit。最后把改好的试卷给你，相当于发 pull request，你拿到试卷重新改正错误，相当于 merge。
>
> 当你想更正别人仓库里的错误时，要走一个流程：
>
> 1. 先 fork 别人的仓库，相当于拷贝一份，相信我，不会有人直接让你改修原仓库的
> 2. clone 到本地分支，做一些 bug fix
> 3. 发起 pull request 给原仓库，让他看到你修改的 bug
> 4. 原仓库 review 这个 bug，如果是正确的话，就会 merge 到他自己的项目中
>
> 至此，整个 pull request 的过程就结束了。

 

### Git常用命令

1. 本地git命令
   - git status 查看状态
   - git init (切换到仓库目录后)初始化仓库
   - git add <file> 添加追踪文件
   - git commit -m '描述信息'   提交修改到本地仓库
   - git log 查看所有commit提交信息
   - git branch 查看当前分支
   - git branch a  创建a分支。a分支与主分支一样的内容
   - git checkout a  切换到a分支上。*代表当前所在分支
   - git checkout -b a 创建a分支并切换到a分支
   - git merge a 把a分支代码合并到当前分支
   - git branch -d/D a  删除/强制删除a分支
   - git tag v1.0  给当前代码添加标签v1.0
   - git tag  查看代码历史标签记录
   - git checkout v1.0 使代码切换到v1.0的状态

>#### 这部分是本地仓库与Git远程仓库ssh加密连接
>
>- ssh-keygen -t rsa 生成公钥id_rsa.pub和密钥id_rsa，用于本机和远程linux服务器建立连接。接下来要做的就是将id_rsa.pub放到远程GitHub上，这样你本地的 id_rsa 密钥跟 GitHub 上的 id_rsa.pub 公钥进行配对，授权成功才可以提交代码。
>- ssh -T git@github,com 将id_rsa.pub内容添加到GitHub上的ssh key中后，使用此命令检查本地仓库是否能与远程仓库连接成功

2. 远程git命令

   - git push origin master 本地代码推到远程master分支

   - git pull origin master   将远程master分支拉下来到本地，一般我们在 push 之前都会先 pull ，这样不容易冲突

   - 提交代码到远程仓库的方法：

     1. clone克隆远程仓库到本地，修改提交：
        - git clone https://github.com/Liuz-ak47/test1reposity.git
        - git push origin master

     2. 关联本地已有项目：
        - git remote add origin https://github.com/Liuz-ak47/test2.repository.git   使本地仓库与远程仓库建立联系
        - git push origin master  推送代码到master分支

3. 其他

   - git config --global alias.co checkout    给某些命令起别名

   - git config --global user.name "name"  全局添加作者

   - git config --global user.email "name@mail.cn"  全局添加邮箱

   - git config --global alias.log "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)% 

     d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"

   - git diff  比较当前文件和暂存区文件差异，什么是暂存区？就是你 还没有执行 git add 的文件

   - git checkout a.md   撤销没有进暂存区(未git add)的文件修改

   - git stash 是把当前分支所有没有 commit 的代码先隐藏起来，使用后查看git status是干净的没有未提交记录的，同时查看修改的文件也没有修改的记录

   - git stash list 查看隐藏记录

   - git stash apply  将隐藏的代码还原，这时git status就能看见有待提交代码，同时cat也能查看修改的代码

   - git stash drop  删除本次隐藏记录。后面可以加ID删除特定隐藏记录

   - git merge featureA 合并代码

   - git rebase featureA 合并代码
   
   - git push origin :develop  删除远程分支
   
   - git checkout develop origin/develop  把远程的 develop 分支迁到本地
   
   - 

 