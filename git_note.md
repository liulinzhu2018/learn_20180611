github  
ussername: liulinzhu2018  
password:  llz920127  
https://github.com/orgs/liulinzhu1/people/liulinzhu2018  
https://github.com/liulinzhu2018/learn_20180611  

git config --global user.name "liulinzhu2018"  
git config --global user.email "13683318620@126.com"  

git remote add origin git@github.com:liulinzhu2018/learn_20180611  
git pull origin master --allow-unrelated-histories  
git push --set-upstream origin master  

最新git源码下载地址：
https://github.com/git/git/releases
https://www.kernel.org/pub/software/scm/git/

Git Bash 本地配置
1. 生成key: id_rsa.pub, id_rsa
ssh-keygen -t rsa -C "13683318620@126.com"
2. 将公钥id_rsa.pub添加到git上面

参考：
https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000  

windows : FC txt1 txt2  
linux   : diff -u txt1 txt2

----------------------------------------------------
版本控制

git config --global color.ui auto 有颜色的git输出 
git --version 


# 创建仓库
- 在本地创建仓库，执行git init命令创建仓库，之后产生.git目录，存放元数据
在网页的github上能够创建仓库，创建文件，修改，提交，等等
```
# 创建一个远程仓库的命令
echo "# my-travel-plans" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/liulinzhu/xxx.git 创建一个远程仓库的链接
git push -u origin master
```
- 在github网页上创建仓库，再在本地添加远程库
在github上创建一个仓库xxx，再在本地xxx仓库下执行git remote add将本地和远程的仓库关联。
```
git remote add origin https://github.com/liulinzhu/xxx.git
git push -u origin master 
```
- git remote 查看远程仓库名称
- git remote -v 查看远程仓库的url


# 克隆仓库
clone两种： 
1.local到local : git clone /path/to/repository 本地路径  
2.github到local：git clone username@host:/path/to/repository  

# git status
- 查看文件的状态，可以看到文件的改动处于工作区，还是临时区 
- 对于存放文件分为三个区：
  - 工作区working direction
  - 临时区stating area
  - 仓库repository

# git log
git log --graph --oneline  
git log -n 1 查看距离当前最近的一条日志  
git log --stat 查看每个commit id 下面修改的文件，以及添加和删除的长度  
git log --pretty=oneline 每条日志打印一行，只打印commit id和日志信息  
git log --oneline 每条日志打印一行，只打印commit id的前7位和日志信息  

git log -p  
git log -w  
git log --decorate --oneline   
git log --oneline --decorate --graph --all  

git log --author="Paul Lewis" 过滤作者  
git log --grep=bug  
git log --grep bug  

# git shortlog
git shortlog 可以看到汇总后每个人的提交  
git shortlog -s 统计每个人的提交数量，不带有具体每条日志  
git shortlog -s -n 按照每个人提交的数量降序排序  

# git show
git show SHA 查看SHA具体文件改动的内容。  

# git add / rm
- 修改文件--添加文件--提交文件
- 修改文件是在工作区进行修改的，git status查看工作区和临时区， 里面Changes not staged for commit 表示工作区的修改，
- git add filename将工作区的修改提交到临时区，git add -i交互式添加文件，git status 里面Changes to be committed 表示提交到临时区的文件。
- git rm filename 删除文件
- git rm --cached filename 从缓存区删除文件，工作区则不做出改变。

# git commit
- git commit -m "log" 将修改提交到本地的仓库的HEAD中，仓库中包含多个commit，每个commit中包含修改的文件。
- git commit --amend 与最近commit合并，产生新的commit id, 但commit时间没有更新

# git diff
- git diff 对比工作区和临时区文件的区别
- git diff --staged 对比临时区和本地仓库的区别
- git diff commitid1 commitid2 比较本地仓库中两次提交的区别
- git diff head filename 比较head和工作区或临时区某个文件的区别
- git show commitid 查看某次提交的更改

# .gitignore 文件
- 里面可以自己写入忽视的文件
- 格式：
- blank lines can be used for spacing
- \# - marks line as a comment
- \* - matches 0 or more characters
- ? - matches 1 character
- [abc] - matches a, b, _or_ c
- \*\* - matches nested directories - a/**/z matches
	- a/z
	- a/b/z
	- a/b/c/z

# git tag
- git tag -a v1.0 SHA
- git tag -d v1.0 SHA

# branch
git checkout -b newbranch 创建并切换到分支  
git checkout -d newbranch 删除分支  
git branch 查看分支  
git checkout xxx 切换到分支  
git push origin branchname 将分支推送到远程仓库(origin)  

# merge 合并分支
- git merge <other-branch>  
- 实际是修改指针，将master和head的指针指向当前的分支，之后删除分支，只是移除指向分支的指针。  
- git merge branchname --no-ff -m "log info" 合并其他分支到自己的分支，--no-ff表示禁用fast-forward  
- 处理冲突，先修改冲突的文件，之后git add filename 表示冲突已经修改，再进行合并。  
- git diff <source_branch> <target_branch> 在合并前，预览差异  

合并两个分支同一个文件，如果原始文件中中有，且一个分支中有，一个分支中被删除了，那么这个最终被删除。  
如果原始文件中没有，且一个分支添加了，那么最终会被添加。  

# git revert
- git revert SHA  
- git reflog 查看所有日志，包括回退版本后，在它之后的提交日志（git log只能看到当前版本之前的日志），和所有回退版本的日志。  

# git reset
- 撤销对最近一次提交的文件
	- \-\-hard HEAD^ 彻底删除
	- \-\-soft HEAD^  将最近一次提交的文件移动到临时区
	- \-\-mixed HEAD^ 将最近一次提交的文件移动到工作区
```
^ – indicates the parent commit
~ – indicates the first parent commit
HEAD^^^
HEAD~3
HEAD^^^2 is the 4c9749e commit (this is the grandparent's (HEAD^^) second parent (^2))
```  
  
- git reset filename 撤销临时区的修改
- git reset --hard 撤销工作区或临时区的所有修改
- git reset head -- filename 撤销工作区或临时区对文件的修改
- git reset --hard origin/master 用远程服务器的origin/master替换本地、暂存区、版本库

# git checkout
- git checkout head . 或 git checkout head -- filename 撤销工作区和临时区对文件的修改  
- git checkout . 或 git checkout -- filename 用临时区替换工作区的改动  

# git slash
git stash 隐藏工作区，当工作区在做一些修改时，这时又有另一个任务需要修改，此时可以先隐藏工作区，再创建新的分支，这个分支是不包括当前的改动的，再在新分支上完成另一个任务，进行提交，再合并到主分支，再删除分支，之后回到之前开发一半的分支，恢复隐藏，继续工作。  
git stash list 查看隐藏的工作区  
git stash apply 恢复stash内容,但stash并不删除，需要用git stash drop来删除  
git stash pop 恢复stash的同时把stash也删了  

------------------------------------

# git push origin master
git push <remote-shortname> <branch>

# git pull origin master
拉取远程仓库的提交，本地origin/master指针和本地master指针都移动到最新位置  

# git fetch 
- git fetch origin master 拉取远程仓库的提交，本地origin/master指针移动到最新位置，而本地的master指针不移动  
- git merger origin/master 将本地master指针移动到origin/master指针的位置  
- git fetch使用场景，当本地和远程都有改动和更新，且需要本地的更改合并到远程master时，就要分三步进行   
  - 1）fetch 拉取远程的更新d-e，  
  - 2）merger 将自己的更新与origin/master的合并，f和d-e合并为g。  
  - 3）git pull origin master 推送到远程仓库 
  
![image](https://github.com/liulinzhu2018/learn_20180611/blob/master/img/git_fetch.png)

# fork
复制仓库，并成为这个仓库的所有者。在github网页上操作  

# pull request
副本仓库的改动请求合并到原始仓库的步骤：  
- 第一种情况：当副本仓库和原始仓库同步，且副本仓库有改动时，  
在github网页上操作点击new pull request，请求原始仓库拉取副本仓库的改动，再合并到原始仓库中。  

- 第二种情况：当副本仓库和原始仓库都有改动时，
  - 1）git remote add upstream https://xxx 建立本地到原始仓库的链接。因为clone下来的副本仓库后，会自动建立本地到副本仓库（origin）的链接，所以需要再建立一个到原始仓库（upstream）的链接。
其中upstream是自己定义的链接名称，可以通过git remote rename upstream newname进行修改。
  - 2）git fetch upstream master 原始仓库的改动
  - 3）git merge upstream master 在本地合并自己和原始仓库的改动
  - 4）git push origin master 推送到副本仓库中
  - 5）请求合并到原始仓库，按照第一种情况操作
 
 ![image](https://github.com/liulinzhu2018/learn_20180611/blob/master/img/pull_request.png)

# git rebase
- 将多个commit压制（squash）为一个commit 
- git rebase -i HEAD~3 将最后三个提交合并为一个提交 
- git push -f 强制推送 





------------------------------------
# 常用问题记录
git管理的某些目录和文件不小心被删除了，怎么恢复？
git checkout [branch] filename 恢复工作区和临时区的文件
