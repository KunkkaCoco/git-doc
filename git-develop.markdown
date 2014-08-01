# git常用命令
--------
### [git下载地址](http://git-scm.com/downloads)
### 配置用户名 
	git config --global user.name "yourname"  
### 配置用户邮箱 
	git config --global user.email "youremail"
### 下载代码 
	git clone http://gitlab.domain/project.git
### git add
	git add  /pathfile
	 #当前目录所有文件加入快照 
	git add .   
### 提交 git commit
	git commit -m '提交内容的注释'
	 #修改上一次的提交
    git commit --amend  
### 更新代码 
	# 从服务器下载最新代码，如果本地有修改则合并上去，用 git pull --rebase 也可以，注意别用 git pull，会导致git网络图混乱	
	git fetch && git rebase 
	#如果出现不能更新的情况，需要设置跟踪地址
	git branch --set-upstream-to=origin/<branch> branchname

### 上传代码	
	#将当前分支push到远程分支
	git push origin branchname
### 查看状态 		
	git status
### 查看日志 		
git log	
	
|  选项  |  说明               |
| --------     | --------            |
| -(n)	              |仅显示最近的 n 条提交|
| --since, --after    |仅显示指定时间之后的提交。|
| --since=2.weeks     | 你可以给出各种时间格式，比如说具体的某一天（“2008-01-15”）|
| --until, --before   |仅显示指定时间之前的提交。|
| --author	      |仅显示指定作者相关的提交。|
| --grep    |指定搜索关键字|
| --committer	      |仅显示指定提交者相关的提交。|
	 
### 切换分支  		
	git checkout branchname
### 恢复版本  		
	git reset --hard HEAD   
	或git reset head .  
### 设置远程站点
	git remote add origin http://192.168.82.98/wb-group/xh-relationship-center.git
	git remote set-url origin http://192.168.82.98/wb-group/xh-relationship-center.git	
	 #显示远程地址 
	git remote -v   
	 #重命名远程仓库 
	git remote rename pb paul   
	 #删除远程地址 
	git remote rm paul  
### 取最新代码 		  
	git fetch
### 重新定义起点	 
	git  rebase
### 查看分支 
	#本地显示本地分支
	git branch 	  
	#包括远程分支在内的所有分支
	git branch -a  
### 创建分支 	
	#从当前分支创建出branchname	  
	git checkout -b branchname
	git branch branchname
	#将远程分支下载到本地 
	git checkout -b branchname origin/branchname  
### 删除本地分支
	#已合并的分支
	git branch -d branchname  
	#强制删除。注意！ 
	git branch -D branchname   
### 应用分支的开发流程
	#特性分支要从develop分支分出
	#本地没有develop的时候
	git checkout -b develop origin/develop
	#本地有develop分支的时候
	git checkout develop
	#创建特性分支 
	git checkout -b feature-name
	#将特性分支push的origin
	git push origin feature-name
	#更新当前分支代码 
	git fetch && git rebase  
	#更新当前分支代码并衍合develop分支共通代码，需要rebasedevelop的时候确保develop为最新版本
	git fetch && git rebase develop
	#开发完成在gitlab发起mergeRequest
	
	

### 添加upstream 有不同的远程仓库的时候。(目前项目组只有一个gitlab仓库)
	 git remote add upstream git://gitlab.com/group/project.git 
	 #跟踪原始代码 
	 git fetch upstream 
	 #提交代码更新到自己的代码库 git push origin master 
	 #获取原始代码库的更新 
	 git fetch upstream 
	 git merge upstream/master 

### 如果当前分支上有些修改未commit，但此时需要切换到别的分支上工作
	# 储存当前未提交的内容
	git stash save 
	# 切换到别的分支工作
	git checkout other_branch 
	# 完成后切换会原分支
	git checkout origin_branch 
	git stash pop # 回复到未提交内容

	#你也可多次使用'git stash'命令,　每执行一次就会把针对当前修改的‘储藏’(stash)添加到储藏队列中. 用'git
	#stash list'命令可以查看你保存的'储藏'(stashes):
	git stash list
	stash@{0}: WIP on book: 51bea1d... fixed images
	stash@{1}: WIP on m
### git tag 打标签
	git tag -a v1.4 -m 'my version 1.4'
	#查看标签 
	git tag  
	git show v1.4
	#像分支一样可以检出 
	git checkout v1.4   
### git 自定义命令 
	#将日志信息一行显示
	git config --global alias.inline 'log --pretty=format:"%h - %an, %ar : %s"'
	
### git 数据恢复 
	#查看本地的操作日志
	git reflog
	#运行 `git log -g` 会输出 reflog 的正常日志，包含详细的commit信息
	git log -g
	#从日志中找到丢失的commit的SHA 例如'ab1afef' 找到丢失代码复制出来或合并分支均可
	git branch recover-branch ab1afef



## 分支模型 （参考）
	http://nvie.com/posts/a-successful-git-branching-model/
	http://www.oschina.net/translate/a-successful-git-branching-model

### git分支管理 开发流程 hotfix
	#确保hotfix是从master检出
	#Switched to a new branch "hotfix-1.2.1"
	 git checkout -b hotfix-1.2.1 master
	--------------------------
	 
	#Files modified successfully, version bumped to 1.2.1.
	 git commit -a -m "Bumped version number to 1.2.1"
	
	[hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1
	1 files changed, 1 insertions(+), 1 deletions(-)
	--------------------------

	git commit -m "Fixed severe production problem"
	
	[hotfix-1.2.1 abbe5d6] Fixed severe production problem
	5 files changed, 32 insertions(+), 17 deletions(-)
	--------------------------------

	#Switched to branch 'master'
	git checkout master
	git merge --no-ff hotfix-1.2.1
	
	Merge made by recursive.
	(Summary of changes)
	 git tag -a 1.2.1
	---------------------------

	#Switched to branch 'develop'
	 git checkout develop
	 git merge --no-ff hotfix-1.2.1
	
	Merge made by recursive.
	(Summary of changes)
	-----------------------------
	
	#Deleted branch hotfix-1.2.1 (was abbe5d6).
	git branch -d hotfix-1.2.1
