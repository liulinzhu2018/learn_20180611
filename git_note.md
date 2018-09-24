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
- git remote show origin 查看远程origin仓库的信息
- git remote rename oldname newname 重命名远程仓库
- git remote rm name 删除远程仓库


# 克隆仓库
clone两种： 
1.local到local : git clone /path/to/repository 本地路径  
2.github到local：git clone username@host:/path/to/repository  

# git status
- 仓库下面的文件大体分为两种状态：已跟踪（tracked files）和未跟踪（Untracked files）
- 对于已跟踪的文件状态又分为：已修改的文件（处于工作区），已修改且已暂存的文件（处于临时区）
- git status 查看文件的状态，可以看到文件是否跟踪，跟踪的文件处于工作区，还是临时区 
- git status -s(--short) 简短的输出，文件的状态表示有下面几种：
  - ??  未跟踪文件
  - 右M 已跟踪文件被修改，在工作区
  - 左M 已跟踪文件被修改，在暂存区
  - MM  已跟踪文件被修改，在工作区和暂存区都有
  - A   新添加到暂存区的文件

# git add / rm
- 修改文件--添加文件--提交文件
- 修改文件是在工作区进行修改的，git status查看工作区和临时区， 里面Changes not staged for commit 表示工作区的修改，
- git add filename将工作区的修改提交到临时区，git add -i交互式添加文件，git status 里面Changes to be committed 表示提交到临时区的文件。
- git rm 将文件从仓库中移除
  - git rm filename 对于工作区的文件，本地文件也会被删除
  - git rm \*~ 删除以~结尾的所有文件
  - git rm -f(--force) filename 对于已经暂存的文件，要强迫删除它，本地文件也会被删除
  - git rm --cached filename 对于已暂存的文件，本地的文件不会被删除，只是文件不会再被跟踪
  >> 当我们不小心将不需要被跟踪的文件暂存时，如执行了git add .操作，且没有设置忽略的文件，那么这时就需要把这些文件从暂存中移除，但又不删除本地的文件，这时就要执行这个命令了。

# git mv
- git mv old_filename new_filename 移动文件
- 这个命令相当于是三个命令，mv old_filename new_filename; git rm old_filename; git add new_filename
- git status 也可以看出来那些文件做了重命名操作

# git commit
- 每一次执行提交命令都是对项目做一次快照。
- git commit -m "log" 将修改提交到本地的仓库的HEAD中，仓库中包含多个commit，每个commit中包含修改的文件。
- git commit --amend 与最近commit合并，产生新的commit id, 但commit时间没有更新
- git commit -a 会自动将跟踪的文件暂存，再提交，不用自己先将文件暂存，省略自己执行git add的步骤。
- **每次暂存会对修改的文件计算校验和（SHA-1）,然后把当前版本的快照保存在git仓库中（git使用`blob对象`保存它们,每个修改的文件的快照为一个blob对象），再将校验和加入暂存区等待提交。提交的时候会先计算子目录的校验和，然后在git中这些校验和保存为`树对象`（一个树对象，记录目录结构和blob对象索引），之后创建一个`提交对象`保存指向树对象的指针和提交信息。 每一次提交，提交对象都会有一个指向前一次提交对象的指针。**

# git diff
- git diff 对比工作区和临时区文件的区别，也就是跟踪的文件还没暂存的变化
- git diff --staged(--cached) 对比临时区和本地仓库的区别，也就是跟踪的文件已经被暂存的变化
- git diff commitid1 commitid2 比较本地仓库中两次提交的区别
- git diff head filename 比较head和工作区或临时区某个文件的区别
- git show commitid 查看某次提交的更改
- git diff --check 检查空白错误

# .gitignore 文件
- 创建这个文件，里面可以自己写入忽视的文件，这样执行git status就不会看到它们，可以防止不小心被提交
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
- !xx - 对规则取反
- eg. \*.\[ao\] 表示忽略以.a或是.o为后缀的文件  
- eg. /test1 表示忽略当前目录下面的test1文件
>> 当我们执行对仓库的代码进行编译后，会自动产生一些文件，像\*.a, \*.o 这样的文件我们是不需要这些文件出现在git status当中的，这时就可以使用这个方法设置规则，将这些文件忽略。

# git log
git log --graph --oneline  
git log -n 2 或者 git log -2 查看距离当前最近两次的提交  
git log --since=2.weeks 近两周的改动
git log --stat 查看每个commit id 下面修改的文件，以及添加和删除的长度  
git log --pretty=oneline 每条日志打印一行，只打印commit id和日志信息  
git log --oneline 每条日志打印一行，只打印commit id的前7位和日志信息  
git log dir dir目录下的提交记录
git log -p 打印出提交的文件的修改内容
git log -w  
git log --decorate --oneline   
git log --oneline --decorate --graph --all  

git log --author="Paul Lewis" 过滤作者  
git log --grep=bug  
git log --grep bug  
git log -Sfunction_name 过滤出含有function_name的改动
多个过滤条件要加--all-match,否则满足任意一个过滤条件都会被打印

# git shortlog
git shortlog 可以看到汇总后每个人的提交  
git shortlog -s 统计每个人的提交数量，不带有具体每条日志  
git shortlog -s -n 按照每个人提交的数量降序排序  

# git show
git show SHA 查看SHA具体文件改动的内容。  

# git tag
- git tag 列出标签
- 标签分为两种：轻量标签，附注标签
- 轻量标签：是特定提交的引用，仅将提交的校验和存放在文件中，不保存其他标签信息。
- 附注标签：是一个完整的对象，包含标签信息，包括打标签者名字，邮箱，时间等。
- 创建标签
  - 附注标签创建：git tag -a v1.0 -m 'my version 1.0'
  - 轻量标签创建：git tag v1.1
  - 也可以对过去的提交打标签：git tag -a v2.0 SHA(提交对应的校验和)
- 删除标签：git tag -d v1.0 SHA
- 查看某个标签的信息：git show v1.0
- 将标签推送到远程服务器：git push origin v1.0 (推送多个标签添加--tags)
- 在特定的标签创建一个分支：git checkout -b branchname tagname

# git 别名
- git config --global alias.co checkout 设置checkout的别名为co

# branch
- **创建分支实际是创建一个指针指向在当前的提交对象，相当与是像一个文件写41个字节（40字符SHA-1+1字符换行）。HEAD指针指向当前的提交对象。通过git log --decorate可以看到指针的位置**
- 创建并切换到分支：git checkout -b newbranch  
- 删除分支：git checkout -d newbranch (-D 强迫删除) 
- 切换到分支：git checkout xxx  **此时HEAD的指针将移动到切换后的分支**
>> 无论是切换到master分支或是其他分支，所作的事情都是相同的，下面以切换到master分支为例，执行git checkout master会做两件事情，一是将HEAD指针指向master分支，二是将master的最后一次的提交的快照切换回工作区，如果git不能完成这样的任务那么切换分支的操作会被阻止。
- 将本地分支推送到远程仓库(origin)：git push --set-upstream origin branchname(第一次要使用这个命令, 或-u，-u是--set-upstream的简写), git push origin branchname, git push -u origin A:B 
- 查看分支
  - 查看本地分支：git branch   
  - 查看远程分支：git branch -a 
  - 查看每个分支最后一次提交：git branch -v
  - 查看已经合并到当前分支的分支：git branch --merged
  - 查看未合并到当前分支的分支：git branch --no-merged
- 删除远程分支： git push origin --delete branchname, 只会从服务器上删除分支的指针，分支实际会保留一段时间，等待垃圾回收机制进行处理。
- 分支跟踪：
  - 创建跟踪远程分支remotename/branchname的本地分支branchname：git checkout -b branchname remotename/branchname 或git checkout --track remotename/branchname
  - 已有本地分支，跟踪刚拉取下来的远程分支：git branch -u origin/branchname
  - 查看所有分支（包括没有跟踪远程的分支）：git branch -vv 可以看到本地的分支领先（eg, ahead 2领先两个提交），还是落后（eg, behind 1落后一个提交）。这个数据是本地最近一次拉取远程分支的数据，和实际远程分支可能有差异，所以要查看比较准确的数据，需要将远程分支全部在拉取一遍，执行git fetch --all,再git branch -vv.

# merge 合并分支
- git merge <other-branch>  
- 合并分支实际是修改指针，将master和head的指针指向当前的分支，删除分支只是移除指向分支的指针。
- 在合并分支时，A分支合并到B分支，1.如果B分支是A分支的直接祖先，通过移动B分支的指针到达A分支，且没有任何冲突，那么直接修改B分支的指针就实现了分支的合并，这称为`“Fast-forward”`, 在执行merge操作时，也会显示Fast-forward。2.如果B分支不是A分支的直接祖先，那么合并时git会使用A和B分支末端的快照和它们的公共祖先进行`三方合并`，创建出一个新的提交快照。在执行merge操作时，会显示Merge made by the 'recursive' strategy. 3.合并分支遇到冲突时，git会先尝试自动合并冲突，如果合并失败，会显示那些文件合并冲突失败，此时就需要自己打开冲突文件解决，也可以使用git mergetool打开可视化工具辅助合并。
- git merge branchname --no-ff -m "log info" 合并其他分支到自己的分支，--no-ff表示禁用fast-forward  
- 处理冲突，先修改冲突的文件，之后git add filename 表示冲突已经修改，再进行合并。  
- git diff <source_branch> <target_branch> 在合并前，预览差异  

合并两个分支同一个文件，如果原始文件中中有，且一个分支中有，一个分支中被删除了，那么这个最终被删除。  
如果原始文件中没有，且一个分支添加了，那么最终会被添加。  

# git rebase
- 合并分支操作除了使用merge的方法进行三方合并外，还可以采用rebase的方法，eg, 将A分支合并到B分支，rebase步骤：
  - git checkout A; git rebase B 将A的修改rebase到B上
    1. 找到这两个分支的公共祖先
    2. 对比当前分支A与该祖先的历次提交，提前相应的修改保存为临时文件
    3. 将当前分支指向基底（B分支的最后提交）
    4. 将临时文件记录的修改依次应用在当前的分支
  - git checkout B; git merge A
    1. 由于通过上一步已经使得B分支是A分支的直接祖先，那么在合并时，直接进行Fast-forward即可
    2. A和B的指针共同指向了合并后的最后一个commit
  - git branch -d A 最后合并完成后，别忘删除分支
- 使用rebase的好处在于最终commit记录为一条直线，没有分叉，看起来更加清晰，而采用merge的方式，会有分叉，不方便维护。
- 将多个commit压制（squash）为一个commit 
- git rebase -i HEAD~3 将最后三个提交合并为一个提交 
- git push -f 强制推送 

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
  - 2）merge 将自己的更新与origin/master的合并，f和d-e合并为g。  
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







------------------------------------
# 常用问题记录
git管理的某些目录和文件不小心被删除了，怎么恢复？
git checkout [branch] filename 恢复工作区和临时区的文件
