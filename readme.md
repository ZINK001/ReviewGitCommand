# 基本操作
> 创建SSH Key（/Users/fulibo/.ssh/id_rsa.pub）   
ssh-keygen -t rsa -C "fulibowork@163.com"

> 配置全局用户和邮箱   
git config --global user.name "fulibo"   
git config --global user.email "fulibowork@163.com"

> 初始化
git init  

> 提交本地仓库内容至远程库  
git push origin master

> 推送本地仓库至远程库（方式一）  
git remote add origin https://xxx.git   
git push -u origin master

> 克隆远程库（方式二）   
git clone https://xxx.git

> 关联本地分支与远程分支  
git branch --set-upstream-to=origin/dev dev

# 增加
> 工作区 -> 本地仓库的"暂存区"  
git add readme.md  

> 本地仓库的"暂存区" -> 本地仓库的"本地分支"   
git commit -m "init commit"

> 创建本地dev分支，并切换到本地dev分支   
git checkout -b dev    
>
> 相当于下面两个命令  
> 1. 创建分支   
git branch dev   
> 2. 切换到dev分支     
git checkout dev

> 切换到dev分支，并与远程dev分支关联  
git checkout -b dev origin/dev



# 删除
### 删除
> 移除操作系统中文件  
rm readme.md     

> 移除本地库中文件   
git rm readme.md   
git commit -m "remove readme.md"

> 删除远程文件   
git rm file   
git commit -m "remove remote file"

> 删除分支   
git branch -d dev

> 强制删除分支   
git branch -D dev 

> 删除远程分支   
git branch -r -d dev   
git push origin :dev

# 修改
> 拉取远程分支更新内容  
git pull

> 储存当前工作区（准备去别的分支上修复Bug等）   
git stash

> 回到最近的一次存储点，并删除记录  
git stash pop

> 回到某个存储点，并删除记录   
git stash pop stash@{1}

> 回到某个存储点   
git apply stash@{1}

> 删除某个储存点记录   
git drop stash@{1}

> 回退到上次提交   
git reset --hard HEAD^   

> 回退到上上次提交   
git reset --hard HEAD^^ 

> 回退到某个指定的版本（commit_id开头数字即可）    
git reset --hard f25a5f 

> 撤销修改 工作区回到最近一次add或commit状态（工作区修改的内容不想要了）  
git checkout -- readme.md 

> 撤销修改 撤销"暂存区"内容，重新放回到工作区（撤回add操作）  
git reset HEAD readme.md

> 修改远程文件名   
git mv old_file new_file  
git commit -m "modify remote file name"

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

# 查看
> 查看本地仓库的当前状态  
git status

> 查看commit记录  
git log

> 查看commit记录（简化版）  
git log --pretty=oneline   
或 git log --oneline 

> 查看操作记录  
git reflog
```
    f25a5f2              HEAD@{2}: reset: moving to f25a5f   
当前所在commit_id         执行了reset操作，移动到f25a5f
```

> 查看远程分支的信息（地址等）    
git remote -v

> 比较工作区与本地仓库的"暂存区"  
git diff readme.md

> 比较工作区与本地仓库的"本地分支"   
git diff HEAD -- readme.md

> 比较本地仓库的"暂存区"与本地仓库的"本地分支"  
git diff --cached readme.md

> 对比两次提交差异   
git diff commit_id...commit_id_other

> 查看某次提交更新的内容   
git show commit_id

> 查看所有本地分支  
git branch

> 查看所有分支  
git branch -a

> 查看分支合并图   
git log --graph --pretty=oneline --abbrev-commit

> 查看储存列表   
git stash list

# Rebase
### 单分支上多人协作，冲突解决  
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


### Rebase合并多次提交记录  
> 合并最近三次提交    
git rebase -i HEAD~3 

> 合并局部提交
> 如果想合并1-2 3-4
```
* 61f54b5 4 by me
* 011f680 3 by me
* b8c56d8 modify by other
* 022f680 2 by me
* 83fd038 1 by me
* 45fd038 init commit
```
> git rebase -i 45fd038   
> 修改如下
```
pick 83fd038   1 by me
s    022f680   2 by me
pick b8c56d8   modify by other
pick 011f680   3 by me
s    61f54b5   4 by me
```
> 保存退出, 添加/修改合并日志信息


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

### Rebase合并分支
> dev内容合并到master   
git rebase dev   
解决commit_1冲突   
git add readme.md   
git rebase --continue   
解决commit_2冲突  
git add readme.md   
git rebase --continue   
git push origin master


# 标签
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
