
Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。
git仓库流程大致如下图：
![git处理流程](https://upload-images.jianshu.io/upload_images/10930505-807073db894a7d71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ps：图片引自[http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)

解析：
- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

#### 创建代码仓库
```
$ git init   # 表示在当前目录创建代码仓库
$ git init [dir_name]   # 表示在当前目录创建文件夹dir_name，并把它作为代码仓库
```

#### 克隆远端仓库到本地
```
$ git clone [url]  # 表示将远端仓库复制到本地
```

#### git仓库配置
git的配置文件在仓库目录下的```.git```目录下有一个config的配置文件存储了git的相关配置信息。通常使用git需要配置用户名和邮箱：
```
# 获取git配置信息：
$ git config --list

# 设置提交代码时的用户名和邮箱：
$ git config --global user.email "your_email@email.com"
$ git config --global user.name "your_username"
```

#### 分支
在仓库目录下可执行对分支的操作命令：
```
# 创建本地分支
$ git checkout -b branch_name

# 创建远程分支 
$ git push origin branch_name 

# 删除本地分支（在非本分支下删除）
$ git branch -D branch_name

# 删除远程分支
$ git push origin --delete branch_name 
$ git branch -dr [remote/branch]

# 合并指定分支到当前分支
$ git merge [branch]

#查看分支：
$ git branch
$ git branch -r   # 查看远程仓库所有分支
$ git branch -a  # 查看本地和远程仓库所有分支

#切换分支：
$ git checkout branch_name
```

#### 增加和删除文件
```
# 向暂存区提交文件
$ git add file   # file可以是文件也可以是目录

# 删除工作区文件，并同步到暂存区
$ git rm file  # file可以是文件或者目录

# 将工作区文件重命名，并提交到暂存区
$ git mv old_file new_file
```
#### 提交文件到仓库区
```
# 提交暂存区到仓库区
$ git commit -m 'commit message'

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v
```
#### 标签
```
# 列出所有tag
$ git tag

# 给最新一次commit打上标签
$ git tag -a tag_name -m 'commit'

# 删除本地tag
$ git tag -d tag_name

# 删除远程tag
$ git push origin --delete tag_name

# 查看tag信息
$ git show tag_name

# 提交指定tag
$ git push origin tag_name
```
#### 查看信息
```
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 查看缓存
git stash list

# 修改文件后再另一个分支保存缓存
$ git stash apply stash_name
```
#### 远程同步
```
# 将本地仓库同步到远程仓库
$ git push origin remote_branch

# 下载远程仓库的所有变动
$ git fetch [remote]

# 取回远程仓库的变化到工作区，并与本地分支合并
$ git pull [remote] [branch]
```
#### 撤销
```
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop

# 生成一个可供发布的压缩包
$ git archive
```






