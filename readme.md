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

> 查看远程分支的信息（地址等）    
git remote -v

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
> 创建SSH Key（id_rsa.pub）   
ssh-keygen -t rsa -C "youremail@example.com"

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

> 切换到dev分支，并与远程dev分支关联  
git checkout -b dev origin/dev

> 拉取远程分支更新内容
git pull

> 关联本地分支与远程分支  
git branch --up-stream-to dev origin/dev

> 查看所有本地分支  
git branch

> 查看所有分支  
git branch -a

> 删除分支   
git branch -d dev

> 强制删除分支   
git branch -D dev  

> 查看分支合并图   
git log --graph --pretty=oneline --abbrev-commit

> 合并dev分支到master分支（快进模式）    
git merge dev  
``` 
* f162b63 modify at dev   
* 7b75ac9 modify at master      
* e98ef9d commit readme.md    
```

> 合并dev分支到master分支（禁用快进模式）, 保留dev分支commit历史   
git merge --no-ff -m "merge no fast mode" dev
```
* ab4e15b Merge branch 'dev'
|\  
| * 7af8cc5 modify at dev
|/      
* 7b75ac9 modify at master      
* e98ef9d commit readme.md 
```

> 合并dev分支到master分支（压缩模式）, 保留dev分支commit历史，并将多次commit压缩成一次  
git merge --squash dev   
git commit -m "finish function"
```
* 54565d7 finish function
* 7b75ac9 modify at master      
* e98ef9d commit readme.md 
```

### 储存工作区
> 储存工作区（准备去别的分支上修复Bug等）   
git stash

> 查看储存列表   
git stash list

> 回到最近的一次存储点   
git stash pop

### Rebase
> 单分支上多人协作，冲突解决  
> 方式一，使用git pull    
git push origin master 提交失败，因为有冲突 
git pull    
一次性解决所有commit冲突   
git add readme.md   
git commit -m "fix conflict by me"   
git push origin master
```
*   24ad2b0 fix conflict by me
|\  
| * afd5c22 modify by other
| * 61f54b5 second modify by me
* | e1843f8 first modify by me
|/  
* 34d264f   init status by me
```

> 方式二，使用git pull --rebase   
git push origin master 提交失败，因为有冲突    
git pull --rebase   
解决commit_1冲突   
git add readme.md   
git rebase --continue   
解决commit_2冲突  
git add readme.md   
git rebase --continue   
git push origin master   
```
* 61f54b5 second modify by me
* 011f680 first modify by me
* b8c56d8 modify by other
* 83fd038 init status by me
```

> git rebase --abort 放弃rebase操作, 回到git pull --rebase之前状态   
git rebase --skip  忽略冲突的commit, 保留别人的commit


##### Rebase合并多次提交记录  
git rebase -i HEAD~3 合并前三次提交

> git rebase -i (start end] 指定合并范围   
git rebase -i da123n 1n2n121

```
pick:   保留该commit（缩写:p）
reword: 保留该commit，但我需要修改该commit的注释（缩写:r）
edit:   保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
squash: 将该commit和前一个commit合并（缩写:s）
fixup:  将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
exec:   执行shell命令（缩写:x）
drop:   我要丢弃该commit（缩写:d）
```

> 如异常退出vi标记窗口   
git rebase --edit-todo    
git rebase --continue

##### Rebase合并分支
> dev内容合并到master   
git rebase dev   
解决commit_1冲突   
git add readme.md   
git rebase --continue   
解决commit_2冲突  
git add readme.md   
git rebase --continue   
git push origin master


### 标签
> 打标签   
git tag v1.0   
git tag -a v1.0 -m "1.0标签"

> 在某个commit上打标签   
git tag v0.9 f52c633   
git tag -a v0.9 f52c633 -m "0.9标签"

> 查看所有标签   
git tag  

> 查看标签详情   
git show v0.9

> 删除本地标签   
git tag -d v0.1

> 推送标签到远程库   
git push origin v1.0

> 一次性推送全部尚未推送到远程的本地标签    
git push origin --tags

> 删除远程标签  
git tag -d v0.9   
git push origin :refs/tags/v0.9











