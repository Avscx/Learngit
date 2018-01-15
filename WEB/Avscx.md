# GIT使用笔记
## 各系统平台安装git
    linux：
	使用命令:
    sudo apt-get install git
	分支：（debian、unbuntu linux):
	sudo apt-get git-core
    Max os:
    通过homebrew安装git，[教程]http://brew.sh/
	通过xcode-preferences-downloads-command Line tools安装
    Windows:
          1.下载mysysgit
          2.设置身份：(git Bash)
          3.git config --global user.name "your name“
          4.git config --global user.email "your email address" 
		  
## 创建版本库（本机）
     选择一个地方，创建一个空目录
     1.mkdir 目录name
     2.cd    目录name
     3.pwd   显示当前目录（git Bash 窗口上方也有显示当前目录）
	
     初始化仓库(变为可管理):git init
     添加一个文本文件：（windows下由于编码问题不能用自带笔记本编辑，可使用Notepad ++) 
     提交到暂存区:git add 文件名字
     提交到版本库（只能跟踪文本文件如txt的改动，Microsofe的word由于使用二进制编辑不能跟踪改动）:git commit -m "备注”
	 
## 时光穿梭机
     查看仓库当前状态:git status
     查看当前修改与上一次修改的difference：git diff
     查看历史记录：git log 查看历史记录（简化）：git log --pretty=online
     回退：git reset --hard commit_id （最近也可用HEAD、HEAD^、HEAD~number）
     查看历史命令：git reflog 搭配回退有神奇的效果
   
     丢弃工作区修改：git checkout --file
     撤回暂存区修改：git reset HEAD file（由于暂存区会整合最新add的修改，因此不用考虑到commit_id）（从暂存区撤回的文件会被重新调回工作区）
   
     从版本库中删除文件：git rm file
     从版本库恢复文件：git checkout --file（等价于撤销工作区修改）（工作区最新修改会消失）  
 
     注意点:只有添加（git add）到暂存区的文件才能提交（git commit）到版本库

## 远程仓库
     创建SSH KEY（可在用户主目录下查看有无.ssh目录（包含id_rsa（私钥）\id_rsa.pub（公钥）))若无,在git Bash输入命令：ssh-keygen -t rsa -C “邮件地址”
     最后
     将ssh key中的id_rsa.pub添加到github上的ssh keys中，便可向github推送文件（公开）
     注意：每台电脑的ssh key不同
    
     关联远程仓库：git remote add origin git@github.com:github名称/仓库名称（.git)
     向远程仓库推送本地分支所有内容：git push -u origin master 
     推送最新修改：git push origin master
   
     克隆一个远程仓库：git clone git@github.com:github名称/仓库名称(远程仓库）
## 分支管理
     创建分支 git branch 分支名称     
     切换分支 git checkout 分支名称
     创建并切换分支 git checkout -b 分支名称
     
     git merge命令用于合并指定分支到当前分支
     删除分支 git branch -d <name>
     用git log --graph命令可以看到分支合并图

     合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并
      
     修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；当手头工作没有完成时，先把工作现场git stash
     然后去修复bug，修复后，再git stash pop，回到工作现场
     恢复有两种办法，一种是用git stash apply进行恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；另一种方式是用git stash pop，恢复的同时把stash内容也删了 	
     可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash
   
     开发一个新feature，最好新建一个分支；如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除
   
     多人协作的工作模式：
     查看远程库信息，使用git remote -v；
     本地新建的分支如果不推送到远程，对其他人就是不可见的；
     从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
     在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
     建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
     从远程抓取分支，使用git pull，如果有冲突，要先处理冲突
     注意：
     1.首先，可以试图用git push origin branch-name推送自己的修改；
     2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
     3.如果合并有冲突，则解决冲突，并在本地提交；
     4.没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
     如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name
   
## 标签
     命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
     git tag -a <tagname> -m "blablabla..."可以指定标签信息；
     git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
     命令git tag可以查看所有标签。
## 远程库
     关联远程库 git remote add <name> <address>
## 自定义Git
     缩写命令名称 
     eg:git config --global alias.st status
     
     或者修改.gitconfig文件
     [alias]
     co = checkout
     st = status
       
   
      