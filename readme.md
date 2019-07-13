### 基本操作
> 初始化仓库  
git init  

> 工作区的内容 -> 本地仓库的“暂存区”  
> 可添加多次，然后一起提交  
git add readme.md  

> 本地仓库“暂存区”的内容 -> 本地仓库的“本地分支”   
git commit -m "wrote a readme.md file"

### 比较
> 比较工作区与本地仓库的“暂存区”  
git diff readme.md

> 比较工作区与本地仓库的”本地分支“   
git diff HEAD -- readme.md

> 比较本地仓库的”暂存区“与本地仓库的”本地分支“  
git diff --cached readme.md

### 查看
> 查看本地仓库的当前状态  
git status

> 查看commit记录  
git log

> 查看commit记录（简化版）  
git log --pretty=oneline  

> 查看操作记录  
git reflog  
    f25a5f2              HEAD@{2}: reset: moving to f25a5f   
当前所在commit_id         执行了reset操作，移动到f25a5f

### 回退与撤销(恢复)
> 版本回退  
git reset --hard HEAD^   回退到上次提交 
git reset --hard HEAD^^  回退到上上次提交  
git reset --hard f25a5f  回退到某个指定的版本（commit_id开头数字即可） 

> 撤销修改 工作区回到最近一次add或commit状态（工作区修改的内容不想要了）  
git checkout -- readme.md 

> 撤销修改 撤销”暂存区“内容，重新放回到工作区（想撤回add操作）  
git reset HEAD readme.md

### 删除
> 移除操作系统中文件  
rm readme.md     

> 移除本地库中文件   
git rm readme.md   
git commit -m "remove readme.md"

### 远程库
> 推送本地仓库至远程库（方式一）  
git remote add origin https://xxx.git   
git push -u origin master

> 克隆远程库（方式二）   
git clone https://xxx.git

> 提交本地仓库内容至远程库  
git push origin master

### 分支
> 创建本地dev分支，并切换到本地dev分支   
git checkout -b dev   

> 相当于下面两个命令
> 创建分支   
git branch dev   

> 切换到dev分支     
git checkout dev

> 查看所有本地分支  
git branch

> 查看所有分支  
git branch -a

> 合并dev分支到master分支（快进模式）    
git merge dev

> 删除分支   
git branch -d dev

> 强制删除分支   
git branch -D dev  

> 查看分支合并图   
git log --graph --pretty=oneline --abbrev-commit

> 合并dev分支到master分支（禁用快进模式）, 保留dev分支commit历史
git merge --no-ff dev


> 合并dev分支到master分支（压缩模式）, 保留dev分支commit历史，并将多次commit压缩成一次  
git merge --squash dev



















