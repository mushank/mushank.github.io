---
title: "Git Using Notes"
date: 2015-12-16 19:00:00
tag: [Skills, Git]
blog: true
layout: post

---

# Git Using Notes

## 1. Local Repo
1. 创建Git仓库  
- ` $ mkdir gitDirectory `	// 创建目录  
- ` $ cd gitDirectory `  
- ` $ pwd `	// 查看当前所在目录  
- ` $ git init `	// 初始化仓库  
- ` $ ls -ah `	// 查看当前目录所含文件

2. 添加文件到版本库  
- ` $ git add fileName `	// 提交文件至Git暂存区  
- ` $ git commit -m 'wrote a file' `	// 提交暂存区文件至Git版本库，`-m`后输入的是本次提交的的说明  
- ` $ git rm --cached fileName `	//  取消对某文件的追踪

3. 查看命令  
- ` $ git status `	// 查看Git仓库当前状态  
- ` $ git diff fileName `	// 查看文件修改内容  
- ` $ git log `	// 查看提交日志  
- ` $ git log --pretty=oneline `	// 显示简要日志信息  
- ` $ git reflog ` // 查看历史日志  

4. 版本回退
- ` $ git reset --hard commitId `	// reset回退命令，可以回退到任一提交版本  
- ` $ git reset --hard HEAD^ `	// HEAD表示当前版本，HEAD^标识上一个版本，HEAD^^表示上上个版本，HEAD~100表示往前100个版本  

5. 撤销修改
- ` $ git checkout -- fileName `	// 撤销对某文件的修改，恢复至最近一次add或commit的状态  
- ` $ git reset HEAD fileName `	// reset命令除了可以回退版本，还可以用于撤销暂存区的修改，重新放回工作区。HEAD表示最新版本  

6. 删除文件
- ` $ rm fileName `	// 从系统文件管理器删除文件  
- ` $ git rm fileName `	// 从Git版本库中删除文件  

## 2. Branch
1. 创建分支
- ` $ git branch branchName `	// 创建分支branchName  

2. 切换分支
- ` $ git checkout branchName `	// 切换至分支branchName  

3. 查看分支
- ` $ git branch `	// 会列出所有分支，带*号的为当前分支  

4. 合并分支
- ` $ git merge branchName`	// 合并指定分支到当前分支  

5. 删除分支
- ` $ git brance -d branchName `	// 删除指定分支  
- ` $ git branch -D branchName `	// 强行删除指定分支  
- ` $ $ git push origin --delete branchName `	// 删除远程仓库指定分支  
	***注意：步骤1. 2. 可由一句命令完成：  
` $ git checkout -b branchName `	// 创建分支branchName，并切换至该分支***  

### 2.1 Deal with conflicts
1. 解决冲突后，再次使用`add`、`commit`进行提交操作。  

	*** 注：使用如下命令可以查看分支合并情况：  
	- ` $ git log --graph --pretty=oneline --abbrev-commit `  
	
2. 合并分支时禁用`Fast forward`模式：  
- ` $ git merge --no-ff -m "merge with no-ff" branchName `	// 禁用`Fast forward`模式合并分支时，必定会创建一个新的commit，所以加上`-m`参数，写入commit描述  

	***注：分支策略：master分支仅用于发布版本，平时在dev分支上进行开发。团队开发时，每个人都往dev分支上合并代码，发布版本时才将dev分支往master分支上合并***  

### 2.2 Bug branch
1. 储藏现场  
- ` $ git stash `  

2. 查看现场  
- ` $ git stash list `  

3. 恢复现场  
- ` $ git stash apply stash@{0} `	// 恢复指定stash，恢复后，stash内容并不删除  

4. 删除现场  
- ` $ git stash drop `	// 删除stash内容  

	***注：步骤3. 4. 可由一句命令完成：  
- `$ git stash pop `	//恢复现场，同时删除现场***  

## 3. Remote Repo
1. 创建SSH Key  
- ` $ ssh-keygen -t rsa -c 'example@email.com' `	// 如果用户主目录下已有.ssh目录，并且.ssh目录下已包含id_rsa和id_rsa.pub两个文件，则跳过创建此步骤  

2. 登录GitHub，'Account settings' -> 'SSH Keys' -> 'Add SSH Key',复制id_rsa.pub文件内容添加新的SSH Key  

3. 在GitHub创建一个Git仓库  

4. 关联远程GitHub仓库
- ` $ git remote add origin git@serverName:path/repoName.git `	// 关联远程库  
- ` $ git push -u origin master `		// 首次推送master分支所有内容  

5. 查看远程仓库信息  
- ` $ git remote `	// 查看远程仓库信息  
- ` $ git remote -v `	// 查看远程仓库详细信息

	***注：从远程服务器Clone仓库至本地，命令如下：***  
- ` git clone git@serverName:path/repoName.git `	// 默认只clone Master分支  

6. 推送本地分支
- ` $ git push origin branchName `	

7. 抓取分支
- ` $ git checkout -b branchName origin/branchName `	// 从远程仓库拉取branchName分支  

8. 指定本地分支和远程分支的链接  
- ` $ git branch --set-upstream branchName origin/branchName `  

9. 抓去分支最新内容  
- ` $ git pull `  

## 4. Tags
1. 添加标签  
- ` $ git tag tagName `	// 给当前commit添加标签  
- ` $ git tag tagName commitId `	// 给指定commit添加标签  
- ` $ git tag -a tagName -m "remark" commitId `	// `-a`指定标签名字，`-m`指定说明文字  
2. 查看标签  
- ` $ git tag `	// 查看所有标签  
- ` $ git show tagName `	// 查看指定标签详细信息  

3. 删除标签  
- ` $ git tag -d tagName `  
- ` $ git push origin :refs/tags/tagName `	// 删除远程仓库标签（首先从本地删除标签，然后再运行此命令）  

4. 推送标签  
- ` $ git push origin tagName `	// 推送标签至远程仓库  
- ` $ git push origin --tags `	// 推送全部尚未推送的标签至远程仓库  


## 5. Custom Define
1. 让Git显示颜色  
- ` $ git config --global color.ui true `  

2. 配置别名  
- ` $ git config --global alias.customName orderName `	// 设置命令order-name的别名custom-name  
	**举例：**  
- ` $ git config --global alias.cho checkout `  
- ` $ git config --global alias.co commit `  
- ` $ git config --global alias.br branch `  
- ` $ git config --global alias.unstage 'reset HEAD' `  
- ` $ git config --global alias.last 'log -1' `  
- ` $ git config --global alias.superLog "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" `  
***注1：配置Git的时候，加上`--global`是针对当前用户起作用的;如果不加，则只针对当前仓库起作用***  
***注2：每个仓库的Git配置文件都放在.git/config文件中.  
当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中***  

## 6. Rebase
1. ` git rebase -i commitId `	// commitId为需要修改的commit的前一个commit的Id
2. ` git commit --amend `	// 进入VIM修改Commit信息
3. ` git rebase --continue `	// 修改完Commit Msg，继续操作命令
4. ` gir rebase --abort `	// 放弃本次rebase操作


## 7. Setting
- 系统级别Git配置文件：`/etc/gitconfig`
- 用户级别Git配置文件：`~/.gitconfig`
- 项目级别Git配置文件：`<Project Path>/.git/config`

1. `git config --global user.name "Jack"`	// 设置Git提交显示用户名
2. `git config --global user.email "jack@emai.com""`	// 设置Git提交显示邮箱  
***使用`--global`选项，更改的是用户级别配置，去掉`--global`选项更改的是项目级别配置***


## 8. Sth else
1. `find . -name ".git" | xargs rm -Rf`	// 删除Git仓库信息













