

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




