![](img/1615552022122-60553145-52c3-486c-84a9-ea3a2b414eaa.png)


- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库
### 配置
Git的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。
```shell
# 编辑配置文件（-e 以文件形式编辑）
$ git config -e --global

# 设计提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
$ git config [--global] user.password "[passwd]"
其他参数
--local：只对某个仓库有效
--global：对当前用户的所有仓库有效
--system：对系统登录的所有用户有效

# 显示当前git配置信息
$ git config --list [--global]
# 查看某个本地仓库下的配置信息
$ git config --list --local
```
### 新建版本库
```shell
# 1. 将已有的项目代码纳入git管理
# 新建代码库,在当前的文件夹下
$ cd [project_folder]
$ git init
# 2. 或初始化一个目录为代码库
$ git init [project-name] # 会在当前路径下创建和项目名称同名的文件夹
```
### 添加/删除文件
```shell
# 添加指定文件、指定目录、当前目录所有文件到暂存区
$ git add [file1] [file2] # 添加多文件
$ git add [dir] # 添加文件夹
$ git add . # 提交新文件和修改文件，但不包含被删除文件
$ git add -A # 提交所有变化
$ git add -u # 提交被修改和被删除文件

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p


# 删除工作区文件，并且将这次删除放入暂存区，需要commit
$ git rm [file1] [file2] …
# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]
# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]

# 可以丢弃工作区的修改(在工作区的修改全部撤销)
# 1.第一种是文件自修改后还没有被放到暂存区，撤销修改就回到和版本库一样的状态；
# 2.第二种是文件已经添加到暂存区后又作了修改，撤销修改就回到添加到暂存区后状态。
# 就是让这个文件回到最近一次git commit或git add时的状态
# 注意一定要加上--，否则是切换分支操作
$ git checkout -- filename
```
> Q：输入`git add readme.txt`，得到错误：`fatal: not a git repository (or any of the parent directories)`。
> A：Git命令必须在Git仓库目录内执行（`git init`除外），在仓库目录外执行是没有意义的。
> Q：输入`git add readme.txt`，得到错误`fatal: pathspec 'readme.txt' did not match any files`。
> A：添加某个文件时，该文件必须在当前目录下存在，用`ls`或者`dir`命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。

### 给文件重命名
```shell
# 常规方法
# 1. 修改文件夹中文件名
$ mv readme readme.md
# 2. 将新文件加入暂存区
$ git add readme.md
# 3. 删除暂存区原文件
$ git rm readme

# 先还原上面的原暂存区操作
$ git reset --hard ##危险操作，不要轻易尝试

# 简便方法
$ git mv readme readme.md
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/2472959/1617424752767-e8fae187-ad64-4bc2-a999-fad638773d09.png#height=115&id=Hntac&margin=%5Bobject%20Object%5D&name=image.png&originHeight=229&originWidth=819&originalType=binary&ratio=1&size=126088&status=done&style=none&width=409.5)
### 提交
```shell
# 提交暂存区代码到本地仓库，-m后接提交的描述
$ git commit -m"commit meaasge"
# 提交指定文件到本地仓库
$ git commit [file1] [file2] … -m"commit meaasge"
# 需要修改描述
$ git commit --amend
# 修改老旧commit的描述
$ git rebase -i [版本号]
```
### 查看日志、查看状态、比较差异
```shell
# 用于查看在你上次提交之后是否有对文件进行再次修改。即查看变更情况
$ git status


# 比较某文件工作区和暂存区的差异
$ git diff [filepath]
# 比较暂存区和工作区的所有差异
$ git diff
# 比较某文件暂存区和HEAD的差异（本地库中最近一次commit的内容）
$ git diff --cache [filepath]
# 比较某文件暂存区和HEAD的所有差异
$ git diff --cache
# 查看工作区和版本库里面最新版本的区别
$ git diff HEAD -- [filepath]
# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD
# 显示两次提交之间的差异
$ git diff [first-branch]…[second-branch]
# 显示今天你写了多少行代码
$ git diff --shortstat “@{0 day ago}”


# 显示当前分支的版本历史
$ git log
# 简洁用一行显示
$ git log --oneline # 其实就等于 --pretty=oneline
# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat
# 显示过去5次提交
$ git log -5 --oneline 
# 显示所有分支的版本历史
$ git log --all
# 可以树状查看
$ git log --all -graph
# 搜索提交历史，根据关键词
$ git log -S [keyword]
# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s
# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature
# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]
# 显示指定文件相关的每一次diff
$ git log -p [file]
# 可以web查看其他命令
$ git help --web log

# 图像化查看log
$ gitk


# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn
# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示某次提交的元数据和内容变化
$ git show [commit]
# 显示某次提交发生变化的文件
$ git show --name-only [commit]
# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]


# 显示当前分支的最近几次提交 (常用)
$ git reflog
```
### 分支操作
```shell
# 查看本地和远端所有分支
$ git branch -av
# 查看远端所有分支
$ git branch -rv
# 查看本地所有分支
$ git branch -v
# 查看当前工作在哪个分支上
$ git branch [-v]
# 删除指定分支，-D为强制删除
$ git branch -d [branch name] # 如果-d删除不了，请更换为-D


# 基于当前分支创建新分支
$ git branch [new branch name]
# 创建分支并切换到该分支
$ git checkout [new branch name]

# 删除已合并到master分支的所有本地分支
$ git branch --merged master | grep -v '^\*\| master' | xargs -n 1 git branch -d
# 删除远端origin已不存在的所有本地分支
$ git remote prune origin

## 分支合并
# 把某分支合并入当前分支，且为merge创建commit
$ git merge [某分支名]
# 把A分支合并到B分支，且为merge创建commit
$ git merge [A] [B]

# rebase可以把它理解成是“重新设置基线”，将你的当前分支重新设置开始点。这个时候才能知道你当前分支于你需要比较的分支之间的差异。
# 把当前分支基于某分支做rebase，以便把该分支合入当前分支
$ git rebase [某分支名]
# 把A分支基于B分支做rebase，以便把B分支合入到A分支
$ git rebase [B] [A]
# 用 mergetool 解决冲突
$ git mergetool

```
### 远程库
```shell
# 在本地仓库文件夹下执行命令，将本地仓库绑定到远程库
# 关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名，可省略
$ git remote add origin git@github.com:[username]/[repoxxx].git

# 把本地库master分支的所有内容推送到远程库上
# 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
# 注意这后面所有的origin都是基于add时的名称origin
$ git push -u origin master
# 第一次后的简化命令
$ git push origin master
# 强制推送
$ git push origin master -f
# 向远端提交指定标签
$ git push origin [标签名]
# 向远端提交所有标签
$ git push origin --tags

# 查看远程库信息
$ git remote -v
# 根据名字删除远程库，如删除origin
$ git remote rm origin
# 修改远程库名称
$ git remote rename [旧名称] [新名称]

# 从github或者其他代码托管平台上，下载一个项目
# url可以是git协议，也可以是https协议，特点：https不仅速度慢而且每次推送都必须输入口令
# clone默认master分支
$ git clone [url]
# clone指定分支
$ git clone -b [brench] [url]

# 将远端所有分支和标签的变更都拉到本地
$ git fetch [remote]
# 将远端分支的变更拉到本地，且merge到本地分支
$ git pull [remote] [分支名]
# 更新远程数据到本地
$ git pull
# 上面命令默认是使用merge方式拉取代码，可以添加-r，使用rebase方式拉代码
$ git pull -r 
```
### 打标签
打标签的作用,就是给项目的开发节点,加上语义化的名字,也即功能版本的别名。 打上标签名的同时,写上附带信息,可以方便项目日后维护过程中的回溯和复查。 另外,也可以通过标签记录,大致了解当前项目的向下兼容性、API的修改和迭代情况。
```shell
$ git tag [标签名] [commit的id]
```
### 版本回退（重点）
```shell
结合git reflog/git log来获得版本号
$ git reflog # 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）
$ git log # 可以显示所有提交过的版本信息
$ git log --oneline# 只显示历史的版本

# 回退到上个版本
$ git reset --hard
$ git reset --hard HEAD^
# 回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard HEAD~3 
# 退到/进到 指定commit的sha码（推荐使用）
$ git reset --hard commit_id 

#强制回退远程版本
$ git reset --hard origin/newdev
```
#### git reset三个参数对比

1. soft参数：仅仅在本地库移动HEAD指针
1. mixed参数：在本地库移动HEAD指针，重置暂存区
1. hard参数：在本地库移动HEAD指针，重置暂存区、重置工作区
#### 其他

1. 当永久删除文件后，可以使用“git reset --hard 版本号”或者“git reset --hard HEAD^”回退到删除前的版本，即可找回删除后的文件。
1. 添加到暂存区的删除文件找回：当执行了删除操作，且使用“git add 文件名”将删除操作添加到暂存区，若想找回删除的文件，可以使用“git reset --hard HEAD”命令找回删除文件。
1. 从工作区撤销没有追踪的文件：git clean -f -d
1. 想在git commit之后push之前，删除个别的commit文件

第一种方法
```shell
$ git rm --cached <file_name>
$ git commit "删除了<file_name>文件"
# git rm -- cached <file_name> will remove the file from the stage.
# 撤销commit
git reset --hard 提交id
```
第二种方法
```shell
$ git log --pretty=oneline　　 # 查看当前提交的日志
$ git reset --soft XXX 　　#XXX是commitID(d6cdbba417....) 回退当前工作空间的上一个版本,并且保留代码更改
$ git log --pretty=oneline 　　#再次查看当前提交的日志,确认是否成功撤销,保险起见
$ git push origin master --force 　　#强制提交当前版本号，以达到撤销版本号的目的.必须添加参数force进行强制提交，否则会提交失败,报错原因：本地项目版本号低于远端仓库版本号。(master 代表分支名称,默认是 master，或者也可以直接用 git push --force，或者-f)

https://www.cnblogs.com/Super-scarlett/p/8183348.html
```
### 加塞临时任务处理（重点）
```shell
# 1. 将未处理完的变更先保存到stash
$ git stash

# 2. 临时任务处理完后继续之前的工作
$ git stash pop 
# 或者下面的命令，和 pop 不同的是， apply相当于从栈顶把任务取出来，但是不过从栈中把任务移除
$ git stash apply

# 3. 查看所有stash
$ git stash list

# 4. 取回某次stash的变更
$ git stash pop stash @{N} #数字N
```
### 提交但不想创建新的commit
我们的仓库的内容每次变更执行 commit 的时候，都会生成一个新的 commit，不过有时候，我们不想产生新的 commit，而是想要通过修改之前的 commit 来变更仓库的内容，那么就可以使用如下命令了
```shell
# 1. 修改最后一次commit
# 在工作区中修改文件
$ git add
$ git commit --amend

# 2. 修改中间的commit（假设代号为N）
$ git rebase -i X前面的一个 commit 的 id
# 在工作区修改文件
$ git add
$ git rebase --contiue 
```
### 其他
```shell
# 查看哪些文件没有被Git管控
git ls-files --others 
```

