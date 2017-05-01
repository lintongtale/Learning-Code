# Git常用命令
## 1、Git安装
	• git config --global user.name "Your name"
	• git config --global user.email "Your email"
	注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址
## 2、远程仓库
	• ssh-keygen -t rsa -C "youremail@example.com"
	在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥
	• git remote add origin git@github.com:lintongtale/test.git 将本地仓库与远程仓库关联
	• git push -u origin master 把当前分支推送到远程，-u把本地分支和远程分支关联起来，在以后的推送或拉取时可以简化命令
		○ git push origin master
	• git clone git@github.com:lintongtale/test.git 从远程克隆一个本地库
		○ ls 列出所有文件
## 3、版本控制
	• mkdir <file> 创建一个空目录
		○ cd <file>
		○ pwd 
	• git init 把当前目录变成Git可以管理的仓库
	• git add <file> 添加到暂存区
		○ git add . 将内容修改和新文件添加到暂存区，不包括删除的文件
		○ git add -u 更新已经被add的文件，不提交新文件（untracked file）
		○ git add -A 上面两个功能的合集
	• git commit -m "说明"
	• git log 显示从最近的提交日志git log --pretty=oneline
	pretty 按指定格式显示日志信息,可选项有：oneline,short,medium,full,fuller,email,raw以及format
	• git reset --hard HEAD^ 回退到上一个版本
		○ git  reset --hard HEAD^^ 回退到上上一个版本
		○ git reset --hard HEAD~100 往上100个版本
		○ git reflog 记录每一次命令，包括commit id
		○ git reset --hard <commit id>
	• git status 查看状态
	• 
	• git diff HEAD -- <file> 查看工作区和版本库里面最新版本的区别
	• git checkout -- <file> 让文件回到最近一次git commit或git add时的状态（撤销修改）
	• rm <file> 删除文件
		○ git rm <file>
		○ git commit -m "remove file"
		○ 若发现删错了，git checkout -- <file>
## 4、分支管理
	• git checkout -b test 创建test分支，并切换到test分支，-b表示创建并切换，相当于以下两条命令
		○ git branch test 创建分支
		○ git checkout test 切换分支
		○ git branch 查看当前分支
	• git merge test 将test分支合并到master分支上，Fast forward模式，<删除分支时会丢失分支信息>
		○ git merge --no-ff -m "merge with no-ff" 合并分支，no-ff表示禁用Fast forward，<删除分支时不会丢失分支信息>
	• git branch -d test 删除test分支
	• git log --graph 查看分支合并图
	当master分支和test分支各自都有新的提交时，Git无法快速合并，只能试图将各自的修改合并起来。
	• git branch -a 查看远程分支
	• Bug分支
	当你在dev分支上进行的工作还没有提交，而此时有一个代号为101的bug任务，需要另外临时创建一个issue-101来修复它
		○ git stash 把当前工作现场“储藏”起来，等以后恢复现场后继续工作，此时git status工作区是干净的
	假设需要在master分支上修复bug，创建临时分支issue-101修改并merge后
		○ git stash list 查看刚才的工作现场
		○ git stash apply 恢复
		○ git stash drop 删除stash内容
		○ git stash pop 等同于前两步
	• Feature分支
		○ 软件开发中，每添加一个功能时，应该创建一个feature分支，完成后再合并。
		○ 当分支完成并commit后，如果此时功能需要取消，要删除feature分支，此时，使用git branch –d feature-vulcan将会出现error
		○ git branch –D feature-vulcan 强行删除feature-vulcan分支
	• 多人协作
		○ git remote –v 查看远程仓库的信息
		○ git push origin master 推送分支，git push origin dev推送dev分支
		master是主分支，要时刻与远程同步；dev是开发分支，团队所有成员都需要在上面工作，需要远程同步；bug分支只用于本地修复bug，无需推送到远程；feature分支是否推送到远程，取决于你是否和其他人合作在上面开发
		○ git checkout –b dev origin/dev 创建远程origin的dev分支到本地
		此时，可以在dev分支上修改，并push到远程；若此时，另一个人同样在对dev分支的文件作了修改，并试图推送，出现erro并提示“current branch is behind”
		○ git pull 把最新的提交从远程抓下来
		○ git branch –set-upstream dev origin/dev 指定本地分支dev与远程origin/dev分支链接，之后才能git pull
		此时，将本地修改与远程merge，但是会有冲突，需要手动解决，解决后commit再push
## 5、标签管理
	• git tag v1.0 创建标签；默认标签是打在最新提交的commit上的
	• git tag 查看所有标签
	• git log –pretty=oneline –-abbrev-commit 查找到相应的commit id例如6224937；然后打标签
	• git tag v0.9 6224937
	• git tag –a v0.1 -m "version 0.1 released" 3628164 创建带有说明的标签，-a指定标签名，-m指定说明文字
	• git tag –s v0.2 -m "signed version 0.2 released" 用PGP签名标签
	• git tag –d v0.1 删除标签
	• git push origin v0.1 推送标签到远程
	• git push origin –tags 一次性推送全部尚未推送到远程的本地标签
	• git push origin :refs/tags/v0.9 删除远程标签，需先删除本地标签
## 6、自定义Git
	• git check-ignore –v App.class 检查.gitignore的规则错误
	• git config –global alias.st status 配置status的别名，此时git st等效于git status
	• git config –global alias.unstage 'reset HEAD' 此时git unstage test.py相当于git reset HEAD test.py，即把暂存区的修改撤销掉（unstage）
	• git config –global alias.last 'log -1' 此时，用git last就能显示最近一次的提交
	• git config –global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset'  --abbrev-commit"
## 二、git 命令
	• git add . 同时添加多个已修改的文件到暂存区。
	• q 退出

撤销修改

查看之前的commit
	• git checkout <commit> <file>
	• git checkout <commit>
	• git checkout <branch>

撤销公共修改
	• git revert <commit>

撤销本地修改
	• git reset
	• git clean

重写Git历史记录
	• git commit –amend
	• git rebase
	• git reflog

cd .. 返回上一级目录
cd <file> 进入某一目录
