# git study
## 准备
###安装
1. ubuntu : sudo apt-get install git
2. windows : 下载Git-1.9.4-preview20140815.exe进行安装
###设置
安装完毕后进行设置
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
####todo:
1. 如何用不用账号关联不同的git服务器？
2. 不同的git仓库如何用不同的账号？

### 初始化
git init : 初始化一个git仓库

### 添加文件
添加文件到Git仓库，分两步：
1. 使用命令git add <file>，注意，可反复多次使用，添加多个文件；
2. 使用命令git commit，完成。
####添加所有文件
1. git add .
2. git commit .

### 删除文件
```
git rm test.txt
git commit -m "remove test.txt"
```
## 版本穿梭
```
git log 命令显示从最近到最远的提交日志
git log --pretty=oneline

git reset --hard HEAD^
git reset --hard 3628164

git reflog记录每一次命令
```
HEAD指向的版本就是当前版本，我们使用命令```git reset --hard commit_id```即可在版本的历史之间穿梭

- git log可以查看提交历史，以便确定要回退到哪个版本
- git reflog查看命令历史，以便确定要回到未来的哪个版本。

## 查看状态
- git status
- 如果git status告诉你有文件被修改过，用git diff可以查看修改内容

## 工作区、暂存区、分支区

## 管理修改
git管理的是修改

## 撤销修改
- git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
 1. 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
 2. 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
- 总之，就是让这个文件回到最近一次git commit或git add时的状态。

- git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令

- 用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区
- git rset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本
- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步：
  1. 用命令git reset HEAD file，就回到了场景1，
  2. 按场景1操作。
- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。

# 远程仓库
-----------------------
1. 创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
    - 你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可;
    - 如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，id_rsa.pub是公钥。
2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面，然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容

## 添加远程库
将本地库与远程库关联
1. 先建立远程库, github就会告诉你如何关联或克隆
2. git remote add origin git@github.com:yourname/yourrepository.git
3. git push -u origin master
4. 以后如果有更新，只需git push origin master即可

## 克隆远程库
1. 获取远程库的地址
2. git clone git@github.com:username/repositoryname.git

## 分支管理
-----------------------
1. 创建并切换分支: git checkout -b dev
2. 等价于
```
git branch dev
git checkout dev
```
3. 查看分支: git branch
4. 合并分支: git merge <name>
5. 删除分支: git branch -d <name>
6. 如果删除没有合并的分支：git branch -D <name>


## 解决冲突
------------------------
- 当有冲突时，先解决冲突后再合并
- 用git log --graph可以查看分支合并图

## 禁用Fast forward
---------------------
- 通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
- 如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
```
git merge --no-ff -m "merge with no-ff" dev
``

## 查看历史
```git log --graph --pretty=oneline --abbrev-commit```

## 保存当前工作现场
- 场景：
    - 当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交；可以用git status查看状态
    - 并不是你不想提交，而是工作只进行到一半，还没法提交，但是，必须马上修复该bug，怎么办？此时可以用Git提供的stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。
    - git stash: 保存当前工作现场
    - git stash list: 查看工作现场
    - git stash apply: 恢复工作现场
    - git stash drop: 删除工作现场
    - git stash pop: 恢复并删除工作现场
     - git stash pop == git stash apply & git stash drop
```
$ git stash list
stash@{0}: WIP on dev: 6224937 add merge
$ git stash apply stash@{0}
```

## 多人协作
工作模式：
    - 首先，可以试图用git push origin branch-name推送自己的修改；
    - 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
    - 如果合并有冲突，则解决冲突，并在本地提交；
    - 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
    - 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令:
    ```git branch --set-upstream branch-name origin/branch-name```

## 查看远程库信息
    - git remove: 
    - git remote -v: 详细信息
    - 从本地推送分支，使用```git push origin branch-name```，如果推送失败，先用git pull抓取远程的新提交；
    - 在本地创建和远程分支对应的分支，使用```git checkout -b branch-name origin/branch-name```，本地和远程分支的名称最好一致；
    - 建立本地分支和远程分支的关联，使用```git branch --set-upstream branch-name origin/branch-name```；
    - 从远程抓取分支，使用```git pull```，如果有冲突，要先处理冲突。

## 标签管理
    - 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
    - git tag -a <tagname> -m "some comment"可以指定标签信息；
    - git tag -s <tagname> -m "some comment"可以用PGP签名标签；
    - 命令git tag可以查看所有标签。
    - 命令git push origin <tagname>可以推送一个本地标签；
    - 命令git push origin --tags可以推送全部未推送过的本地标签；
    - 命令git tag -d <tagname>可以删除一个本地标签；
    - 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

## 忽略特殊文件

在Git工作区的根目录下创建一个特殊的.gitignore文件，
然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
所有配置文件可以直接在线浏览：https://github.com/github/gitignore

## 配置别名
```
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
$ git config --global alias.unstage 'reset HEAD'
$ git config --global alias.last 'log -1'
$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
## 配置文件

配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

每个仓库的Git配置文件都放在.git/config文件中,
而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中;

