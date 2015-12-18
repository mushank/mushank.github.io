# Git使用笔记
## 一、本地仓库
1. 创建Git仓库  
` $ mkdir learnGit `	// 创建目录  
` $ cd learnGit `  
` $ pwd `	// 查看当前所在目录  
` $ git init `	// 初始化仓库  
` $ ls -ah `	// 查看当前目录所含文件

2. 添加文件到版本库  
` $ git add fileName `	// 提交文件至Git暂存区  
` $ git commit -m 'wrote a fileName file' `	// 提交暂存区文件至Git版本库，`-m`后输入的是本次提交的的说明  

3. 查看命令  
` $ git status `	// 查看Git仓库当前状态  
` $ git diff fileName `	// 查看文件修改内容  
` $ git log `	// 查看提交日志  
` $ git log --pretty=oneline `	// 显示简要日志信息  
` $ git reflog ` // 查看历史日志  

4. 版本回退
` $ git reset --hard commit_id `	// reset回退命令，可以回退到任一提交版本  
` $ git reset --hard HEAD^ `	// HEAD表示当前版本，HEAD^标识上一个版本，HEAD^^表示上上个版本，HEAD~100表示往前100个版本  

5. 撤销修改
` $ git checkout -- fileName `	// 撤销对某文件的修改，恢复至最近一次add或commit的状态  
` $ git reset HEAD fileName `	// reset命令除了可以回退版本，还可以用于撤销暂存区的修改，重新放回工作区。HEAD表示最新版本  

6. 删除文件
` $ rm fileName `	// 从系统文件管理器删除文件  
` $ git rm fileName `	// 从Git版本库中删除文件  

## 二、远程仓库
1. 创建SSH Key  
` $ ssh-keygen -t rsa -c 'example@email.com' `	// 如果用户主目录下已有.ssh目录，并且.ssh目录下已包含id_rsa和id_rsa.pub两个文件，则跳过创建此步骤  

2. 登录GitHub，'Account settings' -> 'SSH Keys' -> 'Add SSH Key',复制id_rsa.pub文件内容添加新的SSH Key  

3. 在GitHub创建一个Git仓库  

4. 关联远程GitHub仓库
` $ git remote add origin git@server-name:path/repo-name.git `	// 关联远程库  
` $ git push -u origin master `		// 首次推送master分支所有内容  

	***注：从远程服务器Clone仓库至本地，命令如下：***  
` git clone git@server-name:path/repo-name.git `  

## 三、分支管理
1. 创建分支
` $ git branch branch-name `	// 创建分支branch-name  

2. 切换分支
` $ git checkout branch-name `	// 切换至分支branch-name  

3. 查看分支
` $ git branch `	// 会列出所有分支，带*号的为当前分支  

4. 合并分支
` $ git merge branch-name`	// 合并指定分支到当前分支  

5. 删除分支
` $git brance -d branch-name `	// 删除指定分支  

	***注意：步骤1. 2. 可由一句命令完成：  
` $ git checkout -b branch-name `	// 创建分支branch-name，并切换至该分支***  
	
## 解决冲突
1. 解决冲突后，再次使用`add`、`commit`进行提交操作。  

	*** 注：使用如下命令可以查看分支合并情况：  
	` $ git log --graph --pretty=oneline --abbrev-commit `  
	
2. 合并分支时禁用`Fast forward`模式：  
` $ git merge --no-ff -m "merge with no-ff" branch-name `	// 禁用`Fast forward`模式合并分支时，必定会创建一个新的commit，所以加上`-m`参数，写入commit描述  

	***注：分支策略：master分支仅用于发布版本，平时在dev分支上进行开发。团队开发时，每个人都往dev分支上合并代码，发布版本时才将dev分支往master分支上合并***  





