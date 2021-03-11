# Git

## 1.常用命令

### git clone

从git服务器拉取代码

```
git clone https://github.com/gafish/gafish.github.com.git
```

代码下载完成后在当前⽂件夹中会有⼀个gafish.github.com的⽬录，通过cd gafish.github.com命令进⼊⽬录。

### git config

配置开发者⽤⼾名和邮箱

```
git config user.name gafish
git config user.email gafish@qqqq.com
```

每次代码提交的时候都会⽣成⼀条提交记录，其中会包含当前配置的⽤⼾名和邮箱。

### git branch

创建、重命名、查看、删除项⽬分⽀，通过Git做项⽬开发时，⼀般都是在开发分⽀中进⾏，开发完成后合并分⽀到主⼲。

```
git branch daily/0.0.0
```

创建⼀个名为daily/0.0.0的⽇常开发分⽀，分⽀名只要不包括特殊字符即可。

```
git branch -m daily/0.0.0daily/0.0.1
```

如果觉得之前的分⽀名不合适，可以为新建的分⽀重命名，重命名分⽀名为daily/0.0.1

```
git branch
```

通过不带参数的branch命令可以查看当前项⽬分⽀列表

```
git branch -d daily/0.0.1
```

如果分⽀已经完成使命则可以通过-d参数将分⽀删除，这⾥为了继续下⼀步操作，暂不执⾏删除操作

### git checkout

切换分⽀

```
git checkout daily/0.0.1
```

切换到daily/0.0.1分⽀，后续的操作将在这个分⽀上进⾏

### git status

查看⽂件变动状态

通过任何你喜欢的编辑器对项⽬中的README.md⽂件做⼀些改动，保存。

```
git status
```

通过git status命令可以看到⽂件当前状态Changesnotstagedforcommit:（改动⽂件未提交到暂存区）

### git add

添加⽂件变动到暂存区

```
git add README.md
```

通过指定⽂件名README.md可以将该⽂件添加到暂存区，如果想添加所有⽂件可⽤gitadd.命令，这时候可通过gitstatus看到⽂件当前状态Changestobecommitted:（⽂件已提交到暂存区）

### git commit

提交⽂件变动到版本库

```
git commit -m '这⾥写提交原因'
```

通过-m参数可直接在命令⾏⾥输⼊提交描述⽂本

### git push

将本地的代码改动推送到服务器

```
git push origin daily/0.0.1
```

origin指代的是当前的git服务器地址，这⾏命令的意思是把daily/0.0.1分⽀推送到服务器，当看到命令⾏返回如下字符表⽰推送成功了。

### git pull

将服务器上的最新代码拉取到本地

```
git pull origin daily/0.0.1
```

如果其它项⽬成员对项⽬做了改动并推送到服务器，我们需要将最新的改动更新到本地，这⾥我们来模拟⼀下这种情况。

进⼊Github⽹站的项⽬⾸⻚，再进⼊daily/0.0.1分⽀，在线对README.md⽂件做⼀些修改并保存，然后在命令中执⾏以上命令，它将把刚才在线修改的部分拉取到本地，⽤编辑器打开README.md，你会发现⽂件已经跟线上的内容同步了。

如果线上代码做了变动，⽽你本地的代码也有变动，拉取的代码就有可能会跟你本地的改动冲突，⼀般情况下Git会⾃动处理这种冲突合并，但如果改动的是同⼀⾏，那就需要⼿动来合并代码，编辑⽂件，保存最新的改动，再通过gitadd.和gitcommit-m'xxx'来提交合并。

### git log

查看版本提交记录

```
git log
```

通过以上命令，我们可以查看整个项⽬的版本提交记录，它⾥⾯包含了提交⼈、⽇期、提交原因等信息，

提交记录可能会⾮常多，按J键往下翻，按K键往上翻，按Q键退出查看

### git tag

为项⽬标记⾥程碑

```
git tag publish/0.0.1
git push origin publish/0.0.1
```

当我们完成某个功能需求准备发布上线时，应该将此次完整的项⽬代码做个标记，并将这个标记好的版本发布到线上，这⾥我们以publish/0.0.1为标记名并发布，当看到命令⾏返回如下内容则表⽰发布成功了

### .gitignore

设置哪些内容不需要推送到服务器，这是⼀个配置⽂件

```
touch .gitignore
```

.gitignore不是Git命令，⽽在项⽬中的⼀个⽂件，通过设置.gitignore的内容告诉Git哪些⽂件应该被忽略不需要推送到服务器，通过以上命令可以创建⼀个.gitignore⽂件，并在编辑器中打开⽂件，每⼀⾏代表⼀个要忽略的⽂件或⽬录，如：

```
demo .html
build/
```

以上内容的意思是Git将忽略demo.html⽂件和build/⽬录，这些内容不会被推送到服务器上

## 2.基本概念

### ⼯作区（WorkingDirectory）

就是你在电脑⾥能看到的⽬录，⽐如上⽂中的gafish.github.com⽂件夹就是⼀个⼯作区.

### 本地版本库（LocalRepository）

⼯作区有⼀个隐藏⽬录.git，这个不算⼯作区，⽽是Git的版本库。

### 暂存区（stage）

本地版本库⾥存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们⾃动创建的第⼀个分⽀master，以及指向master的⼀个指针叫HEAD。

### 远程版本库（RemoteRepository）

⼀般指的是Git服务器上所对应的仓库

### 以上概念之间的关系

⼯作区、暂存区、本地版本库、远程版本库之间⼏个常⽤的Git操作流程如下图所⽰：

![](D:\workspace\Java-Interview-Offer\images\git001.png)

### 分⽀（Branch）

分⽀是为了将修改记录的整个流程分开存储，让分开的分⽀不受其它分⽀的影响，所以在同⼀个数据库⾥可以同时进⾏多个不同的修改

### 主分⽀（Master）

前⾯提到过master是Git为我们⾃动创建的第⼀个分⽀，也叫主分⽀，其它分⽀开发完成后都要合并到master

### 标签（Tag）

标签是⽤于标记特定的点或提交的历史，通常会⽤来标记发布版本的名称或版本号（如：publish/0.0.1），虽然标签看起来有点像分⽀，但打上标签的提交是固定的，不能随意的改动，

### HEAD

HEAD指向的就是当前分⽀的最新提交

## 3.操作⽂件

### git add

添加⽂件到暂存区

```
git add -i
```

通过此命令将打开交互式⼦命令系统

```
git add -p
```

直接进⼊交互命令中最有⽤的patch模式

这是交互命令中最有⽤的模式，其操作⽅式和update类似，选择后Git会显⽰这些⽂件的当前内容与本地版本库中的差异，然后您可以⾃⼰决定是否添加这些修改到暂存区，在命令⾏Stagedeletion[y,n,q,a,d,/,?]?后输⼊y,n,q,a,d,/,?其中⼀项选择操作⽅式

```
git add -u
```

直接进⼊交互命令中的update模式

它会先列出⼯作区修改或删除的⽂件列表，新增的⽂件不会被显⽰，在命令⾏Update>>后输⼊相应的列表序列号表⽰选中该项，回⻋继续选择，如果已选好，直接回⻋回到命令主界⾯

```
git add --ignore -removal.
```

添加⼯作区修改或新增的⽂件列表，删除的⽂件不会被添加

### git commit

把暂存区的⽂件提交到本地版本库

```
git commit -m '第⼀⾏提交原因' -m '第⼆⾏提交原因'
```

不打开编辑器，直接在命令⾏中输⼊多⾏提交原因

```
git commit -am '提交原因'
```

将⼯作区修改或删除的⽂件提交到本地版本库，新增的⽂件不会被提交

```
git commit --amend -m '提交原因'
```

修改最新⼀条提交记录的提交原因

```
git commit -C HEAD
```

将当前⽂件改动提交到HEAD或当前分⽀的历史ID

### git mv

移动或重命名⽂件、⽬录

```
git mv a.md b.md -f
```

将a.md重命名为b.md，同时添加变动到暂存区，加-f参数可以强制重命名，相⽐⽤mva.mdb.md命令省去了gitadd操作

### git rm

从⼯作区和暂存区移除⽂件

```
git rm b.md
```

从⼯作区和暂存区移除⽂件b.md，同时添加变动到暂存区，相⽐⽤rmb.md命令省去了gitadd操作

```
git rm src/ -r
```

允许从⼯作区和暂存区移除⽬录

### git status

```
git status -s
```

以简短⽅式查看⼯作区和暂存区⽂件状态，⽰例如下：

Mdemo.html

??test.html

```
git status --ignored
```

查看⼯作区和暂存区⽂件状态，包括被忽略的⽂件

## 4.操作分⽀

### git branch

查看、创建、删除分⽀

```
git branch -a
```

查看本地版本库和远程版本库上的分⽀列表

```
git branch -r
```

查看远程版本库上的分⽀列表，加上-d参数可以删除远程版本库上的分⽀

```
git branch -D
```

分⽀未提交到本地版本库前强制删除分⽀

```
git branch -vv
```

查看带有最后提交id、最近提交原因等信息的本地版本库分⽀列表

### git merge

将其它分⽀合并到当前分⽀

```
git merge --squash
```

将待合并分⽀上的commit合并成⼀个新的commit放⼊当前分⽀，适⽤于待合并分⽀的提交记录不需要保留的情况

```
git merge --no -ff
```

默认情况下，Git执⾏"快进式合并"（fast-farwardmerge），会直接将Master分⽀指向Develop分⽀，使⽤--no-ff参数后，会执⾏正常合并，在Master分⽀上⽣成⼀个新节点，保证版本演进更清晰。

```
git merge --no -edit
```

在没有冲突的情况下合并，不想⼿动编辑提交原因，⽽是⽤Git⾃动⽣成的类似Mergebranch'test'的⽂字直接提交

### git checkout

切换分⽀

```
git checkout -b daily/0.0.1
```

创建daily/0.0.1分⽀，同时切换到这个新创建的分⽀

```
git checkout HEAD demo.html
```

从本地版本库的HEAD（也可以是提交ID、分⽀名、Tag名）历史中检出demo.html覆盖当前⼯作区的⽂件，如果省略HEAD则是从暂存区检出

```
git checkout --orphan new_branch
```

这个命令会创建⼀个全新的，完全没有历史记录的新分⽀，但当前源分⽀上所有的最新⽂件都还在，真是强迫症患者的福⾳，但这个新分⽀必须做⼀次gitcommit操作后才会真正成为⼀个新分⽀。

```
git checkout -p other_branch
```

这个命令主要⽤来⽐较两个分⽀间的差异内容，并提供交互式的界⾯来选择进⼀步的操作，这个命令不仅可以⽐较两个分⽀间的差异，还可以⽐较单个⽂件的差异。

### git stash

在Git的栈中保存当前修改或删除的⼯作进度，当你在⼀个分⽀⾥做某项功能开发时，接到通知把昨天已经测试完没问题的代码发布到线上，但这时你已经在这个分⽀⾥加⼊了其它未提交的代码，这个时候就可以把这些未提交的代码存到栈⾥。

```
git stash
```

将未提交的⽂件保存到Git栈中

```
git stash list
```

查看栈中保存的列表

```
git stash show stash@{0}
```

显⽰栈中其中⼀条记录

```
git stash drop stash@{0}
```

移除栈中其中⼀条记录

```
git stash pop
```

从Git栈中检出最新保存的⼀条记录，并将它从栈中移除

```
git stash apply stash@{0}
```

从Git栈中检出其中⼀条记录，但不从栈中移除

```
git stash branch new_banch
```

把当前栈中最近⼀次记录检出并创建⼀个新分⽀

```
git stash clear
```

清空栈⾥的所有记录

```
git stash create
```

为当前修改或删除的⽂件创建⼀个⾃定义的栈并返回⼀个ID，此时并未真正存储到栈⾥

```
git stash store xxxxxx
```

将create⽅法⾥返回的ID放到store后⾯，此时在栈⾥真正创建了⼀个记录，但当前修改或删除的⽂件并未从⼯作区移除

## 5.操作历史

### git log

显⽰提交历史记录

```
git log -p
```

显⽰带提交差异对⽐的历史记录

```
git log demo.html
```

显⽰demo.html⽂件的历史记录

```
git log --since="2weeksago"
```

显⽰2周前开始到现在的历史记录，其它时间可以类推

```
git log --before="2weeksago"
```

显⽰截⽌到2周前的历史记录，其它时间可以类推

```
git log -10
```

显⽰最近10条历史记录

```
git log f5f630a..HEAD
```

显⽰从提交IDf5f630a到HEAD之间的记录，HEAD可以为空或其它提交ID

```
git log --pretty=oneline
```

在⼀⾏中输出简短的历史记录

```
git log --pretty=format:"%h"
```

格式化输出历史记录

Git⽤各种placeholder来决定各种显⽰内容，我挑⼏个常⽤的显⽰如下：

- %H:commithash
- %h:缩短的commithash
- %T:treehash
- %t:缩短的treehash
- %P:parenthashes
- %p:缩短的parenthashes
- %an:作者名字
- %aN:mailmap的作者名
- %ae:作者邮箱
- %ad:⽇期(--date=制定的格式)
- %ar:⽇期,相对格式(1dayago)
- %cn:提交者名字
- %ce:提交者email
- %cd:提交⽇期(--date=制定的格式)
- %cr:提交⽇期,相对格式(1dayago)
- %d:ref名称
- %s:commit信息标题
- %b:commit信息内容
- %n:换⾏

### git cherry -pick

合并分⽀的⼀条或⼏条提交记录到当前分⽀末梢

```
git cherry -pick 170a305
```

合并提交ID170a305到当前分⽀末梢

### git reset

将当前的分⽀重设（reset）到指定的<commit>或者HEAD

```
git reset --mixed <commit>
```

--mixed是不带参数时的默认参数，它退回到某个版本，保留⽂件内容，回退提交历史

```
git reset --soft <commit>
```

暂存区和⼯作区中的内容不作任何改变，仅仅把HEAD指向<commit>

```
git reset --hard <commit>
```

⾃从<commit>以来在⼯作区中的任何改变都被丢弃，并把HEAD指向<commit>

### git rebase

重新定义分⽀的版本库状态

```
git rebase branch_name
```

合并分⽀，这跟merge很像，但还是有本质区别

合并过程中可能需要先解决冲突，然后执⾏gitrebase--continue

```
git rebase -i HEAD~~
```

将第⼀⾏的pick改成Commands中所列出来的命令，然后保存并退出，所对应的修改将会⽣效。如果移动提交记录的顺序，将改变历史记录中的排序。

### git revert

撤销某次操作，此次操作之前和之后的commit和history都会保留，并且把这次撤销作为⼀次最新的提交

```
git revert HEAD
```

撤销前⼀次提交操作

```
git revert HEAD --no-edit
```

撤销前⼀次提交操作，并以默认的Revert"xxx"为提交原因

```
git revert -n HEAD
```

需要撤销多次操作的时候加-n参数，这样不会每次撤销操作都提交，⽽是等所有撤销都完成后⼀起提交，微信搜索Java后端关注，获取更多推送。

### git diff

查看⼯作区、暂存区、本地版本库之间的⽂件差异，⽤⼀张图来解释

```
git diff --stat
```

通过--stat参数可以查看变更统计数据

```
test.md | 1 -
1 file changed, 1 deletion(-)
```

### git reflog

reflog可以查看所有分⽀的所有操作记录（包括commit和reset的操作、已经被删除的commit记录，跟gitlog的区别在于它不能查看已经删除了的commit记录

## 6.远程版本库连接

如果在GitHub项⽬初始化之前，⽂件已经存在于本地⽬录中，那可以在本地初始化本地版本库，再将本地版本库跟远程版本库连接起来

### git init

在本地⽬录内部会⽣成.git⽂件夹

### git remote

```
git remote -v
```

不带参数，列出已经存在的远程分⽀，加上-v列出详细信息，在每⼀个名字后⾯列出其远程url

```
git remote add origin https://github.com/gafish/gafish.github.com.git
```

添加⼀个新的远程仓库，指定⼀个名字，以便引⽤后⾯带的URL

### git fetch

将远程版本库的更新取回到本地版本库

```
git fetch origin daily/0.0.1
```

默认情况下，gitfetch取回所有分⽀的更新。如果只想取回特定分⽀的更新，可以指定分⽀名。

## 7.问题排查

### git blame

查看⽂件每⾏代码块的历史信息

```
git blame -L 1,10 demo.html
```

截取demo.html⽂件1-10⾏历史信息

### git bisect

⼆分查找历史记录，排查BUG

```
git bisect start
```

开始⼆分查找

```
git bisect bad
```

标记当前⼆分提交ID为有问题的点

```
git bisect good
```

标记当前⼆分提交ID为没问题的点

```
git bisect reset
```

查到有问题的提交ID后回到原分⽀

## 8.更多操作

### git submodule

通过Git⼦模块可以跟踪外部版本库，它允许在某⼀版本库中再存储另⼀版本库，并且能够保持2个版本库完全独⽴

```
git submodule add https://github.com/gafish/demo.gitdemo
```

将demo仓库添加为⼦模块

```
git submodule update demo
```

更新⼦模块demo

### git gc

运⾏Git的垃圾回收功能，清理冗余的历史快照

### git archive

将加了tag的某个版本打包提取

```
git archive -v --format=zip v0.1>v0.1.zip
```

--format表⽰打包的格式，如zip，-v表⽰对应的tag名，后⾯跟的是tag名，如v0.1。

## 9.最常⽤的Git命令总结

### 分⽀操作

1.gitbranch创建分⽀

2.gitbranch-b创建并切换到新建的分⽀上

3.gitcheckout切换分⽀

4.gitbranch查看分⽀列表

5.gitbranch-v查看所有分⽀的最后⼀次操作

6.gitbranch-vv查看当前分⽀

7.gitbrabch-b分⽀名origin/分⽀名创建远程分⽀到本地

8.gitbranch--merged查看别的分⽀和当前分⽀合并过的分⽀

9.gitbranch--no-merged查看未与当前分⽀合并的分⽀

10.gitbranch-d分⽀名删除本地分⽀

11.gitbranch-D分⽀名强⾏删除分⽀

12.gitbranchorigin:分⽀名删除远处仓库分⽀

13.gitmerge分⽀名合并分⽀到当前分⽀上

### 暂存操作

1.gitstash暂存当前修改

2.gitstashapply恢复最近的⼀次暂存

3.gitstashpop恢复暂存并删除暂存记录

4.gitstashlist查看暂存列表

5.gitstashdrop暂存名(例：stash@{0})移除某次暂存

6.gitstashclear清除暂存

### 回退操作

1.gitreset--hardHEAD^回退到上⼀个版本

2.gitreset--hardahdhs1(commit_id)回退到某个版本

3.gitcheckout--file撤销修改的⽂件(如果⽂件加⼊到了暂存区，则回退到暂存区的，如果⽂件加⼊到了版本库，则还原⾄加⼊版本库之后的状态)

4.gitresetHEADfile撤回暂存区的⽂件修改到⼯作区

### 标签操作

1.gittag标签名添加标签(默认对当前版本)

2.gittag标签名commit_id对某⼀提交记录打标签

3.gittag-a标签名-m'描述'创建新标签并增加备注

4.gittag列出所有标签列表

5.gitshow标签名查看标签信息

6.gittag-d标签名删除本地标签

7.gitpushorigin标签名推送标签到远程仓库

8.gitpushorigin--tags推送所有标签到远程仓库

9.gitpushorigin:refs/tags/标签名从远程仓库中删除标签

### 常规操作

1.gitpushorigintest推送本地分⽀到远程仓库

2.gitrm-r--cached⽂件/⽂件夹名字取消⽂件被版本控制

3.gitreflog获取执⾏过的命令

4.gitlog--graph查看分⽀合并图

5.gitmerge--no-ff-m'合并描述'分⽀名不使⽤Fastforward⽅式合并，采⽤这种⽅式合并可以看到合并记录

6.gitcheck-ignore-v⽂件名查看忽略规则

7.gitadd-f⽂件名强制将⽂件提交

### git创建项⽬仓库

1.gitinit初始化

2.gitremoteaddoriginurl关联远程仓库

3.gitpull

4.gitfetch获取远程仓库中所有的分⽀到本地

### 忽略已加⼊到版本库中的⽂件

1.gitupdate-index--assume-unchangedfile忽略单个⽂件

2.gitrm-r--cached⽂件/⽂件夹名字(.忽略全部⽂件)

### 取消忽略⽂件

1.gitupdate-index--no-assume-unchangedfile

### 拉取、上传免密码

1.gitconfig--globalcredential.helperstore

## 10.Git常⽤命令

### 初始化

#在当前⽬录新建⼀个Git代码库

$git init

#下载⼀个项⽬和它的整个代码历史[Gitonly]

$git clone [url]

### 配置

#列举所有配置

$git config -l

#为命令配置别名

$git config --globalalias.cocheckout

$git config --globalalias.cicommit

$git config --globalalias.ststatus

$git config --globalalias.brbranch

#设置提交代码时的⽤⼾信息

$git config [--global] user.name "[name]"

$git config [--global] user.email "[emailaddress]"

Git⽤⼾的配置⽂件位于~/.gitconfigGit

单个仓库的配置⽂件位于~/$PROJECT_PATH/.git/config

### 增删⽂件

#添加当前⽬录的所有⽂件到暂存区

$git add.

#添加指定⽂件到暂存区

$git add <file1><file2>...

#添加指定⽬录到暂存区，包括其⼦⽬录

$git add <dir>

#删除⼯作区⽂件，并且将这次删除放⼊暂存区

$git rm [file1] [file2]...

#停⽌追踪指定⽂件，但该⽂件会保留在⼯作区

$git rm --cached [file]

#改名⽂件，并且将这个改名放⼊暂存区

$git mv [file-original] [file-renamed]

把⽂件名file1添加到.gitignore⽂件⾥，Git会停⽌跟踪file1的状态。

### 分⽀

#列出所有本地分⽀

$git branch

#列出所有本地分⽀和远程分⽀

$git branch -a

#新建⼀个分⽀，但依然停留在当前分⽀

$git branch [branch-name]

#新建⼀个分⽀，并切换到该分⽀

$git checkout -b [new_branch] [remote-branch]

#切换到指定分⽀，并更新⼯作区

$git checkout [branch-name]

#合并指定分⽀到当前分⽀

$git merge [branch]

#选择⼀个commit，合并进当前分⽀

$git cherry -pick [commit]

#删除本地分⽀，-D参数强制删除分⽀

$git branch -d [branch-name]

#删除远程分⽀

$git push [remote]:[remote-branch]

### 提交

#提交暂存区到仓库区

$git commit -m [message]

#提交⼯作区与暂存区的变化直接到仓库区

$git commit -a

#提交时显⽰所有diff信息

$git commit -v

#提交暂存区修改到仓库区，合并到上次修改，并修改上次的提交信息

$git commit --amend -m [message]

#上传本地指定分⽀到远程仓库

$git push [remote] [remote-branch]

### 拉取

#下载远程仓库的所有变动(Gitonly)

$git fetch [remote]

#显⽰所有远程仓库(Gitonly)

$git remote -v

#显⽰某个远程仓库的信息(Gitonly)

$git remote show [remote]

#增加⼀个新的远程仓库，并命名(Gitonly)

$git remote add [remote-name] [url]

#取回远程仓库的变化，并与本地分⽀合并，(Gitonly)

$git pull [remote] [branch]

#取回远程仓库的变化，并与本地分⽀变基合并，

$git pull --rebase [remote] [branch]

### 撤销

#恢复暂存区的指定⽂件到⼯作区

$git checkout [file]

#恢复暂存区当前⽬录的所有⽂件到⼯作区

$git checkout.

#恢复⼯作区到指定commit

$git checkout [commit]

#重置暂存区的指定⽂件，与上⼀次commit保持⼀致，但⼯作区不变

$git reset [file]

#重置暂存区与⼯作区，与上⼀次commit保持⼀致

$git reset --hard

#重置当前分⽀的指针为指定commit，同时重置暂存区，但⼯作区不变

$git reset [commit]

#重置当前分⽀的HEAD为指定commit，同时重置暂存区和⼯作区，与指定commit⼀致

$git reset --hard [commit]

#新建⼀个commit，⽤于撤销指定commit

$git revert [commit]

#将未提交的变化放在储藏区

$git stash

#将储藏区的内容恢复到当前⼯作区

$git stash pop

### 查询

#查看⼯作区⽂件修改状态

$git status

#查看⼯作区⽂件修改具体内容

$git diff [file]

#查看暂存区⽂件修改内容

$git diff --cached [file]

#查看版本库修改记录

$git log

#查看某⼈提交记录

$git log --author=someone

#查看某个⽂件的历史具体修改内容

$git log -p [file]

#查看某次提交具体修改内容

$git show [commit]

