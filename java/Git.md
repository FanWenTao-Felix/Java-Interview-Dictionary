# Git

### 1.git命令

git --version，检查一下git的版本号。

git config，对git环境进行各种配置。

### 2.git的配置文件

（1）/etc/gitconfig，对当前机器上所有的用户和git项目都生效，使用git config --system，即可对这个配置文件进行操作，也不是很重要，因为我们一般很少去配置这个文件，因为git的配置一般都是对某个用户和项目的范围去生效的。

（2）~/.gitconfig，当前用户主目录的.gitconfig文件，对当前用户有效，使用git config --global，即可对这个配置文件进行操作，windows的话，是在这里：/c/Documents and Settings/All Users/Application Data/Git。 

（3）项目的.git目录下的config文件，仅仅对当前git管理的这个项目有效，直接git config操作的就是这个文件。

我们其实一般都不会直接修改git的配置文件来进行配置，一般都是执行git config命令，来修改我们的各个不同的范围的配置，比如全局的配置，或者是项目特殊的配置。

git config --global的意思，就是设置当前用户范围内的配置，对机器上的其他用户是无效的。

git config --system，就是对当前机器上所有用户都生效。

git config，就是对当前所在的git项目本身生效。

查看所有的配置项：git config --list

查看某个配置项：git config user.name

### 3.帮助文档

如果对某个git命令不太熟悉，可以用git的帮助文档 

git add --help

### 4.git init

我们一般都是先创建一个项目，这个项目处于一个目录下，此时如果要让git来管理这个项目，就可以在这个项目的根目录下面执行该命令。

执行了git init命令之后，意思就是说，要让git托管这个项目的代码，后续这个项目中的所有代码的变动，都可以作为一个版本提交到git中，git会管理这个项目的每个版本的代码，同时后续我们就可以基于git对这个项目进行各种各样的版本控制的功能 。

git init命令执行之后，做的第一件事情，此时就会创建一个.git隐藏目录，这里包含了git的所有的文件，就是每个版本的代码都会存储在.git目录中。

.git目录中，实际上就是存储了你的每个代码文件的每个版本，每个版本就是一个独立的文件。历史版本都被存储在了.git目录中。

但是此时git还没有开始对你的项目进行版本控制。如果要对你的项目代码进行版本控制，需要执行以下命令：

git add --all .

git commit -m 'initial project version'

把上面两个事情给做了之后，实际上就是代表的是，将项目代码当前的版本，提交到git中去，保存到.git目录中去，开始让git管理当前项目的所有代码文件的后续的版本变更。

我们可以自己手动创建一个.gitignore文件，在里面每行一个，放入不需要被git托管的目录或者文件。

### 5.用git托管代码

（1）git init，初步让git准备托管我们的代码，创建了一个.git目录，其中是git用来存放版本数据的。

（2）git add和git commit两个命令，完成了第一个版本的提交，代码提交到了git中，之后所有这些代码文件的变动，都会由git来管理版本。

（3）每次我们修改完一块代码，比如开发了一个小功能，或者是修改了一个小bug，就可以用git add和git commit命令，将修改后的最新版本的代码提交到git中。

（4）循环往复，通过每次修改代码+提交修改后的代码到git中，git就完成了对我们的每个代码文件的每个版本的数据的存储，.git目录中。

（5）接下来，git才能为我们提供各种版本控制的功能。

### 6.git本地仓库结构

git项目有3个主要的部分组成：工作区（working directory / working tree），暂存区（staging area），版本库（git directory / repository）。 

git directory / repository：git版本库，其实就是git用于存储自己的元数据，以及文档数据库的地方，默认就是在项目的.git隐藏目录中。

working directory / working tree：工作区，保存的是一个项目当前的一个版本对应的所有文件，这些文件是从git版本库中的压缩后的数据库中提取出来，然后放到我们的磁盘上去。

staging area：暂存区，就是一个文件，包含在git版本库中，主要是保存了下一次要提交到的那些文件信息。在git中，对暂存区有另外一个名称，叫做index，也就是索引。 

上面三个区域的协作关系大致如下：

（1）首先会在工作区修改某个版本的文件。

（2）将某些修改后的文件放入git暂存区中，准备下一次提交到git版本库中去。

（3）执行一个提交操作，将暂存区中的文件保作为一个快照保存到git版本库中去。

如果一个文件，已经有一个版本被保存到了版本库，那么就是committed状态；如果这个文件被修改了，同时被加入了暂存区，那么就是staged状态；如果这个文件修改了，还没有加入暂存区，那么就是modified状态。

工作区，working directory。

所谓的工作区，指的就是当前你的git管理的项目，在本地的那个目录，也就是你能直接看到，编辑的那个目录，这就是工作区。

### 7.git的重要机制

（1）快照机制

Git用的不是这种维护每次提交的差异，而是用的快照。每次提交文件，都是保存一份这个文件当前这个状态的一个完整快照，同时对这次提交维护一个指针，指向这个文件快照。

（2）本地化操作

大多数的git版本控制操作，只要在本地执行即可，所有的版本文件都在本地，因此操作是非常快速的。相比较于那些依赖网络的集中式版本控制系统来说，他们的大多数操作要依赖网络，速度是很慢的。

比如说通过git查看提交历史，比较历史文件的差异，都可以在本地完成，不需要通过服务器做任何事情。

（3）完整性保证

git在存储任何文件之前，都会对其执行一个校验和，然后用校验和指向那个文件。这是git内核保证的，这样我们是不可以手工修改git版本库中的任何文件的，因为修改了文件之后，会导致计算出来的校验和与之前保存的校验和不匹配，文件会破损。 

git用的是SHA-1 hash算法来计算校验和，这是一个40位的字符串，基于文件的内容计算出来的，看起来大概是这样的： 

24b9da6552252987aa493b52f8696cd6d3b00373

如果手动破坏.git中存储的文件的内容，git会不承认，因为内容变化之后，会导致内容计算出来的SHA-1 40位的hash值变化，跟之前存储的hash值不同，就认为文件破损。 

（4）仅仅添加数据

git通常来说，仅仅会在自己的数据库中添加数据，因此提交文件到git之后，很少会丢失，而且如果我们定期提交文件到远程服务器，就更少丢失。

### 8.git status

git status，查看当前git项目的各种状态。

代码文件分成两种，一种是tracked，一种是untracked。tracked文件就是已经提交到git版本库中的文件，后面可以处于modified或者staged状态；untracked文件，就是从来没有提交到git版本库的代码文件（也从来没有放入暂存区）。

如新建一个TestServiceImpl.java，然后执行git status命令。

此时git status会告诉你，有文件没有被提交到git仓库中或者是暂存区中过，就是untracked，就是从来没有被git追踪过它的版本，甚至第一个版本都没有被git追踪。

接下来执行命令：git add --all .

git add .，就是将当前新增或者是修改过的文件，加入暂存区，但是加了--all之后，如果有文件被删除，也会将文件被删除的状态加入暂存区中。

从untracked状态，变成了new状态。

git commit -m 'add TestServiceImpl class'。

此时，这个文件就进入了一个tracked状态，已经被git开始追踪了。

然后对TestServiceImpl.java进行修改，加入一个方法，接着看一下状态。

此时变成了modified状态，就是已经被追踪了，但是后来又被修改了，但是是changes not staged for commit，就是被修改了，但是没有被staged，staged就是还没有被加入暂存区。

再次执行git add --all .命令。

此时的状态是，changed to be committed，modified，此时文件被放入了暂存区，modified状态，同时等待被commit提交到git仓库中去。

此时文件变成了committed，已经提交了。

#### 总结

git管理的文件有三种状态：提交状态（committed），修改状态（modified），暂存状态（staged）。

提交状态：我们的文件已经安全的保存在git的本地数据库中了。

修改状态：我们修改了文件，但是还没有提交到git的数据库中去。

暂存状态：将修改后的文件标记为即将通过下一次提交，保存到git数据库中去。

（1）新文件刚创建：untracked，此时仅仅停留在工作区中

（2）git add 新文件：new file，此时已经被追踪了，放入了暂存区中 => staged

（3）git commit 新文件：committed，已经被追踪了，放入了git仓库中 => committed

（4）修改那个文件：modified，changes not staged to be committed，没有加入暂存区，被修改的内容仅仅停留在工作区中 => modified

（5）git add 修改文件：modified，changes to be committed，修改的文件版本被已经加入暂存区 => staged

（6）git commit 修改文件：committed，修改后的新版本提交到了git仓库中 => committed

一般自己创建的版本库，刚开始文件都是untracked，然后git add和git commit命令执行之后，就被提交了第一个版本到git版本库，此时就全部都是tracked了。如果是从远程版本库克隆下来的，那么刚开始就是tracked。

接着对tracked的文件修改之后，就是modified；然后对modified文件再执行git add命令之后，就是staged，进入了暂存区；接着执行git commit命令之后，就将暂存区中的文件都提交到了版本库中，此时就是unmodified，and tracked。

### 9.git log查看提交历史

使用git log命令，可以看到每次commit的信息，包括了commit的SHA-1、作者、日期、提交说明。

包含了一个commit的SHA-1 hash值，40位的字符；作者，从之前安装好git之后做的设置来的；date；提交备注，inital commit，git commit -m ''。

分支，就是一份代码拷贝，基于这份独立的代码拷贝，去写自己的代码。

最后将多个分支合并在一起，就是将不同的人写的代码合并到一起。

git里面，每次git init之后，默认初始就创建一个分支，叫做master分支。

我们每次写代码，然后git commit提交，都会形成一次新的commit，然后默认就是在master分支对应的代码拷贝上，进行代码修改和提交的。

每次提交之后，master分支就会对应着最新的一次提交。

HEAD，指针，指向了我们当前所处的分支，因为当前我们默认就处于master分支上，所以HEAD指针就是指向master的。

git log --patch -2，--patch可以显示每次提交之间的diff，同时-n可以指定显示最近几个commit。这个是很有用的，可以看最近两次commit之间的代码差异，进行code review是比较方便的。 

用git log --stat，可以显示每次commit的统计信息，包括修改了几个文件，有多少行插入，多少行删除。 

用git log --pretty=oneline，可以每个commit显示一行，就是一个commit SHA-1和一个提交说明。用git log --pretty=format:"%h - %an, %ar : %s"，可以显示短hash、作者、多长时间以前、提交说明。

用git log --oneline --abbrev-commit --graph，这是最有用的，可以看到整个commit树结构，包括如何合并的，就显示每个commit的SHA-1和提交说明，同时SHA-1显示短值。

--oneline：显示一行，不要显示多行那么多东西，一行里，就显示commit的标识符，SHA-1 hash值，40位的；提交备注；显示分支和HEAD指向哪个commit。

--abbrev-commit：commit的标识符，每一次commit，都有一个唯一的标识符，就是一个SHA-1 hash值，40位，显示一个短值，默认显示前7位，就是说前7位就可以唯一定位这个commit了，不需要完整的40位。

--graph：显示图形化的commit历史，这个大家后面学习到分支那里就知道了，如果有分支的话，commit历史会形成一棵树的形状，这个时候用--graph可以看清楚这颗commit树长什么样子。

在工作区修改代码之后，执行git add命令，会将代码放入版本库中的暂存区；接着执行git commit命令之后，会将暂存区中的代码提交到版本库中的master分支上，而HEAD指针就指向master分支的最新一次提交。

每次我们执行一次commit，git都会存储一个commit object，这个commit object中会包含一个指针，指向这次提交文件的快照。这个commit object同时也包含作者的姓名和邮箱，提交说明，以及对上一次commit object的指针。

将一个文件版本放入暂存区的时候，就会计算一个校验和，然后提交的时候会将文件内容以blob的方式放入版本库中，同时在暂存区放入这个文件版本的校验和。接着git会创建一个commit object，其中会包含元数据，以及一个指针指向版本库中的文件快照。

也就是说，每次执行一次提交，都会在版本库中包含这么几个东西：一个blob，每个文件都会有一个blob来存储这个文件的本次提交的快照；一个tree，这个tree会包含对本次提交的所有文件的blob的指针；一个commt object，指向了tree的指针，作者，等信息。 

接着如果再次执行一个提交，那么下一个提交同样会包含那些东西：每个文件一个blob，一个tree指向所有blob，一个commit object指向那个tree，同时这个commit object会有一个指针，指向上一个commit object。 

最后多次提交，就会得到一颗完整的commit树。 

分支是啥？分支就是一个轻量级的指针，默认的分支是master，那么每次提交，master分支的指针默认就是指向最新的那个commit object的。每次提交一次，master指针就会挪动，继续指向最新的commit object。

同时如果你当前工作在某个分支上，比如工作在master分支上，那么git还维护了一个特殊的指针，HEAD，这个指针指向master分支指针。如果你创建了其他的分支，那么其他的分支也会指向某个commit object，而且此时如果工作在那个分支上，那么HEAD指针会指向那个分支。

#### 总结

git add，其实就是可以多次修改代码，多次git add，然后将每次修改的代码，都放入暂存区中；而git commit，就是一次性将暂存区中的代码，全部提交到master分支上，master分支会出现一个最新的commit，也就是一个最新的代码版本；而HEAD作为一个指针，永远指向master分支的最新一次commit的代码版本。

### 10.版本控制

git reset --hard HEAD^，就可以回退到上一个版本。

HEAD^，代表了什么？

HEAD -> master -> commit（add methods for classes）。

HEAD^，代表的是commit（add methods for classes）的上一个commit（add two files）。

git reset --hard HEAD^，就是将仓库、暂存区、工作区，全部恢复到上一个commit（add two files）对应的状态。 

git reset --hard HEAD~5，退回到HEAD之前的倒数第5个commit的状态。

git reset --hard d324644，指定一个commit的hash值，回退到很老的版本。

git reset这个命令，可以任意穿梭到历史的任何一个版本上去。

如果我要回来呢？重新回到之前add methods for classes那个commit对应的版本。

git reflog

### 11.分支

（1）分支的介绍

所谓的分支功能，就是可以同时拉出来多个代码副本，然后在不同的代码副本上，可以进行对应功能的开发。完成开发之后，可以将多个分支合并在一起，形成最终的代码。 

分支，其实就是指针。

在git中，每一个项目，不管你有多少个分支，不管你在哪个分支上开发。

最终都会形成一个完整的提交历史，树形结构。

每个分支，其实就只是一个指针而已，分支就指向了提交历史中的某个commit object。

每个commit object就代表了这个项目的所有代码在那次提交的时候一个完整的快照版本，包含了之前没有变更的代码文件，也包括了这次提交的最新的修改/新增/删除的代码文件。

每个commit object就代表了项目的一个版本。

所以你的分支实际上就是指向了历史上某个时刻的一个版本的代码，就是一个commit object。

（2）commit object与分支的原理 

Git并不是存储一系列的文件差异，而是存储一系列的文件快照的。实际上每次我们执行一次commit，git都会存储一个commit object，这个commit object中会包含一个指针，指向这次提交文件的快照。这个commit object同时也包含作者的姓名和邮箱，提交说明，以及对上一次commit object的指针。

将一个文件版本放入暂存区的时候，就会计算一个校验和，然后提交的时候会将文件内容以blob的方式放入版本库中，同时在暂存区放入这个文件版本的校验和。接着git会创建一个commit object，其中会包含元数据，以及一个指针指向版本库中的文件快照。

也就是说，每次执行一次提交，都会在版本库中包含这么几个东西：一个blob，每个文件都会有一个blob来存储这个文件的本次提交的快照；一个tree，这个tree会包含对本次提交的所有文件的blob的指针；一个commt object，指向了tree的指针，作者，等信息。

接着如果再次执行一个提交，那么下一个提交同样会包含那些东西：每个文件一个blob，一个tree指向所有blob，一个commit object指向那个tree，同时这个commit object会有一个指针，指向上一个commit object。

最后多次提交，就会得到一颗完整的commit树。 

分支是啥？分支就是一个轻量级的指针，默认的分支是master，那么每次提交，master分支的指针默认就是指向最新的那个commit object的。每次提交一次，master指针就会挪动，继续指向最新的commit object。 

同时如果你当前工作在某个分支上，比如工作在master分支上，那么git还维护了一个特殊的指针，HEAD，这个指针指向master分支指针。如果你创建了其他的分支，那么其他的分支也会指向某个commit object，而且此时如果工作在那个分支上，那么HEAD指针会指向那个分支。

（3）创建一个分支 

git branch testing，可以创建一个新的分支，此时这个分支的指针会指向当前你所在的分支所指向的commit object上。

如何查看各个分支指向哪个commit object呢？使用git log --oneline --decorate命令即可，会给你显示出来。

（4）切换分支 

git checkout testing，此时就可以切换到testing分支，此时HEAD指针会指向testing分支指针。

此时如果在testing分支上提交代码，那么会commit树会长出来一个新的commit object，而testing分支指正会指向最新的commit object，HEAD继续指向testing分支指针，而master指针还是指向之前的那个commit object。 

git checkout master，会切换回master分支，此时HEAD指针会指向master指针，同时将master指针指向的那个commit object，对应的tree和其中的blob，也就是对应的文件快照恢复到工作区中来。 

再次在master分支上提交一个修改，此时从这个commit object会再长出来一个新的commit object，看起来就是跟testing分支当前指向的commit object形成了两个分叉。

此时如果要查看commit树，可以用命令：git log --oneline --decorate --graph --all，这个命令很有用，可以打印出整颗commit树，同时告诉你各个分支当前指向哪个commit object。 

在Git中，分支升级上就是一个很简单的文件，其中包含了一个40位的SHA-1校验和，就是分支指向的那个commit object的SHA-1。所以创建分支的代价是很低的，不过就是创建这么一个文件罢了。而一些集中式版本控制系统，对于分支可能需要拷贝一个完整的文件副本，导致速度很慢，磁盘空间占用很大。

（5）远程分支

在本地你可以创建一个分支，然后开发代码，但是后续的话，你肯定是要将本地分支推送到远程仓库里面去的。

git push -u origin 分支名称 。

这个的意思，就是将本地分支推送到远程仓库里，形成一个同名的远程分支，同时-u这个选项就是将本地分支和远程分支关联起来。

下一次如果修改了这个分支的代码，直接执行git push origin 分支名称，就直接可以将本地分支的代码推送到远程分支。

如果其他人要将某个远程仓库的分支拉取下来，应该执行一个命令，叫做git fetch origin，就会抓取下来远程仓库新增了哪些分支。

接下来可以执行一个命令，git checkout -b 本地分支 origin/远程分支，用这个命令，origin/远程分支，就代表了远程仓库里的那个分支，然后这个命令一执行，就是在本地创建一个远程分支对应的本地分支，互相关联起来。

以后呢，每次如果别人更新了那个分支的代码，push到了远程仓库，你可以执行git pull命令，将这个分支在远程仓库的代码拉取下来，跟本地分支的代码进行合并。

远程版本库的分支，在本地都有追踪分支，remote-tracking 分支。

使用git push origin serverfix，可以将你在本地的分支推送到远程版本库上去，此时你本地的commit树也会推送到远程版本库，跟远程版本库的commit树进行合并。

下一次别人执行git fetch origin的时候，就会获取到一个remote -tracking分支，origin/serverfix分支，这个远程分支是只读的。接着你使用git checkout -b serverfix origin/serverfix命令，就可以在本地获取到一个分支跟origin/serverfix关联起来，然后就可以跟你一起对一个分支进行修改了。

使用git checkout -b serverfix origin/serverfix命令，创建出来的本地分支叫做tracking brach，而这个本地分支track的远程分支叫做upstream branch。此时本地分支就会track那个远程分支，跟远程分支之间会建立关联关系。 

如果我们在tracking分支上，执行git pull命令，git就会自动将对应的远程分支在远程版本库上的代码拉取下来跟本地的tracking分支做合并，如果有冲突的话，还需要解决冲突。 

如果我们是使用clone命令克隆的远程版本库，那么默认就会将本地的master分支创建为追踪origin/master远程分支的。 

使用git branch -u origin/serverfix，可以让当前本地分支track某个指定的远程分支。 

使用git branch -vv，可以查看每个本地分支track的远程分支。 

git fetch origin命令，仅仅就是将远程版本库的commit树拉取下来，同时拉取下来所有最新的远程分支，给我们使用。 

如果我们已经建立起来了本地分支和远程分支的track关系，那么就可以直接使用git pull命令，将远程分支的代码拉取下来合并到本地分支。但是通常来说，我们也可以先git fetch origin，然后git merge 远程分支，一样可以将远程分支代码合并到本地分支上去。 

删除远程分支，比如一个远程分支，已经结束了工作了，可以删除一个远程分支，使用git push origin --delete serverfix即可。同时可以使用git branch -d serverfix删除本地分支。

###  12.功能分支工作流

工作流，git工作流，简单来说，就是多个人如何同时基于git远程仓库，加上分支，去非常良好的协作开发，包括去执行各种集成测试，QA测试，预发布测试，生产环境上线。

在git中，整个所有的版本和文件只存储一次，只存储一份，提交历史，commit树，只有一份。 

每个commit object就代表了这个项目在当前时刻一个完整的快照版本，代表了一份完整的代码了。

所以创建分支，就是创建一个轻量级的指针而已，指向了某个commit object，你的当前所处的分支指向哪个commit object，就是指向了哪个版本的代码，你的工作区中的代码就是那个版本的代码。

如果你要git checkout master，首先需要将当前分支上修改的代码全部git add然后git commit，才能切换分支。

合并分支：切换到master分支，然后执行git merge bugfix/001。

这里执行的是一种merge，叫做fast-forward，快进式merge。

### 13.分支管理命令

git branch命令，直接执行，就会显示出当前所有分支列表，以及你在哪个分支上工作。 

git branch -v命令，可以显示出每个分支当前指向的commit object。 

git branch --merged，可以看到哪些分支被merge进了当前分支；git branch --no-merged，可以看到哪些分支还没有被merge进当前分支。

如果git branch -d命令删除一个分支，可能会提示你那个分支还没merge到当前分支来，不让你删除该分支，此时可以使用git branch -D命令，强制删除一个分支。

如果我们从master分支创建一个其他的分支出来，那么此时git会创建一个指针，叫做dev，这个dev是指向master分支最新一次commit的，然后HEAD指针会指向dev指针。

git checkout -b dev，这个命令就是创建dev分支，dev指针指向最新一个commit，同时HEAD指针指向dev指针。

git checkout -b，相当于两个命令，git branch dev，git checkout dev，先创建分支，再切换到分支。

git branch -d dev，可以删除dev分支，此时就会删除dev这个指针了。

### 14.集中式工作流

1、不管是谁，不管在哪里，不管是哪个分支，你做提交，其实就是在自己本地的提交历史树上加一个新的commit，然后你当前的分支就指向最新的那个commit而已。 

2、你切换分支，新建分支，只不过是在切换指向的commit而已；因为不同的分支是指向不同的commit的，对应的是不同版本的代码；新建分支只不过是多一个指针而已，指向当前的这个commit。

3、在本地基于各种分支完成了代码开发，每个分支指向对应的一个commit之后，完成了自己本地的提交历史树的修改了，就可以将本地的提交历史树，推送到远程仓库，跟远程仓库的提交历史树进行合并。

4、此时远程仓库会和你的本地的提交历史树保持一致。 

5、其他研发人员跟你一样，同时可以从远程仓库拉取代码，也就是拉取提交历史树，跟自己本地的提交历史树进行合并，可能会出现代码冲突。你和其他人都对同一个分支进行修改，此时你们俩的提交历史是不一样的。对你来说，一个分支是指向一个commit，对他来说，同一个分支是指向了不同的commit。此时就需要对这个分支指向的两个commit进行代码合并，可能需要解决冲突。合并之后会出现一个新的commit，就是合并了你们两个人的代码的一个commit，然后分支指向那个commit。

### 15.Git高阶实战技巧：比较不同分支之间差了哪些代码改动

版本的回退和前进，git reset --hard commit标识符。

1、短SHA-1值

我们不一定就是要用40位完整的SHA-1 hash值来选择一个commit，也可以只是提供hash值的前几位就可以了，至少要4位以上，同时在git仓库中就这前几位可以唯一定位一个commit就ok。

就是说，我们如果提供hash值的前7位，git会自动唯一定位出来这个前7位对应的是哪个commit。

2、使用分支的指针指代commit

（1）三个区域（三棵树）：工作区，暂存区，仓库。

（2）commit树+分支指针：无论在哪个分支上做开发和提交，都是在这个项目公共的一个commit树上在添加不同的commit，形成一颗越来越长的树。分支只是指向某个commit的一个指针而已。然后每个commit就代表了那个时刻下，项目的一个完整的历史快照版本。 

因为分支实际上就是一些指针，所以分支是指向commit的，也可以直接用分支名称来指代它目前指向的那个commit。 

比如使用命令：git show master，就可以查看master分支指向的那个commit。

3、git reflog 

git log和git reflog的区别是啥？？？？

git log，是你当前的分支指向的commit之前的所有commit会显示出来，你的分支指向的那个commit之后的commit是不会显示的。 

git reflog，是会显示最近几个月，完整的一个commit历史的。 

所以说，在当前分支上，如果要回退到过去的某个历史版本的话，那么此时应该是用git log就可以。 

但是如果要回到未来的某个版本，是用git reflog。 

git会自动记录和追踪过去几个月内，你的HEAD指针和分支指针移动过的每个commit。 

用git reflog命令就能查看完整的一份commit记录，而使用git log就只能查看当前分支指向的那个commit之前的commit记录。 

reflog是每个研发人员自己本地的电脑上的记录，跟git log显示出来的提交历史不同，不会同步到远程仓库上去的。

4、选择历史版本的特殊语法 

git show HEAD^

git show HEAD~2 

HEAD，指针，指向的是当前的那个分支，当前的那个分支又是指向某个commit。

HEAD，就可以指代当前你所处的那个commit对应的版本。 

HEAD^，就是说，HEAD对应的commit的上一个commit，HEAD^^，上两个commit，HEAD^^^，上三个commit。 

HEAD~5，就是说HEAD之前的第5个commit。

### 16.Git高阶实战技巧：查看不同版本之间的代码差异具体是什么

1、本地查看

git diff：工作区和暂存区

git diff --cached：暂存区和仓库

git diff HEAD：工作区和仓库 

git diff feature/001 master：两个分支之间的差异 

git diff HEAD HEAD^：最近两次提交之间的差异

今天和昨天写的代码的差异看一下

（1）今天写的代码还没有做commit，此时的话，代码还停留在工作区中，此时用一个命令：git diff HEAD，将工作区中的代码和仓库中最近一次commit的代码做比较，给你显示出来差异。 

（2）今天写的代码已经做额commit，此时可以用：git diff HEAD^ HEAD，比较commit到仓库中的代码，最近两个commit之间的代码差异。

### 17.Git高阶实战技巧：将暂存区中的多个功能代码分成多次提交

假设你不小心将多天的代码都提交到了暂存区里面去，此时可以将暂存区里的内容分成多个commit来提交。

此时需要先执行一个命令，进入交互式模式：git add -i。

这个时候会给你列出来当前暂存区里所有的文件和内容，以及没加入暂存区的文件和内容，还有一堆可以执行命令。

此时根据各种指令以及选择文件，将暂存区中的员工管理功能对应的文件和代码，先unstage出来，从暂存区里挪出来。

update是放入暂存区。

revert是从暂存区里挪出来。

通过这个技巧，就可以将暂存区里部分文件给挪出来，然后再执行commit操作，将代码分成多次来提交，每天写好的功能是一个commit，跟公司的规范保持统一。

### 18.Git高阶实战技巧：feature分支开发到一半时切换到bugfix分支

GitFlow工作流，修复线上bug，需要从master分支拉一个bugfix分支出来的。

你手头正在开发feature分支，代码写到一半，没弄完呢。 

如果你此时要切换分支，在git中，是会让你先提交当前分支的代码，然后再切换分支的。

写到一半儿的代码当然是立即从master分支拉一个bugfix分支出来了，但是问题是，如果要从feature分支切换到bugfix分支，你就必须先提交feature的代码，做一次commit，然后才能切换到bugfix分支。

此时，实际上就是要用到stash功能，将feature分支开发的代码暂存起来。

stash功能，其实就是将当前feature分支上对应的工作区的修改过的代码，全部暂存起来，然后将工作区的代码恢复到干净的状态，好像什么都没修改过一样。 

接着此时你就可以切换到master分支，拉一个bugfix分支出来，接着的话修复bug，上线，bugfix分支合并到master分支和develop分支。

都搞完了之后，删除bugfix分支，同时在本地再次切换到feature分支，将之前stash起来的修改好的到吗恢复到现场，继续开发。 

假设我们现在在feature分支上做了一些开发，修改了一些代码，此时需要立即切换到一个bugfix分支去修复bug。 

此时执行一个命令：git stash。 

现在执行git status看一下，会告诉你工作区没有任何修改的文件。

可以使用git stash apply stash@{0}，恢复到指定的一次stash。

此外，如果执行git stash的时候，有放入了暂存区的内容，在恢复的时候需要使用git stash apply --index。

也可以用git stash drop stash名称，手动删除掉某个stash。

此外还有一个有用的命令，就是默认情况下，git仅仅stash那些tracked bug modified文件，但是如果要同时stash那些untracked文件，需要使用： 

默认情况下，它只会将工作区中的修改的文件进行stash，modified状态。如果要将untracked和暂存区中的文件都进行stash。git stash --index --include-untracked，就是将暂存区中的内容，tracked和untracked的内容，全部进行stash暂存。 

git stash apply --index，就是同时将工作区和暂存区中的内容进行恢复。

### 19.Git高阶实战技巧：对本地不规范的提交历史进行修改和调整

1、对最近一次commit的备注或者代码进行修改 

（1）修复上一个提交的不规范的备注 

如果你刚刚执行了一个commit，结果发现，不好，我其实应该补充更多的备注，才能符合公司对commit备注这块的规范的，刚才写的太简单了。

git commit --amend。 

直接就可以修改上一个commit的备注信息，补充更多的提交说明 。

相当于是将上一个commit删除掉，然后基于上一个commit对应的代码重新构建一个commit object。

（2）对上一次commit加入几行遗漏的代码 

或者说，你刚执行了一个commit，结果扫了一眼代码，发现今天忘记修改几行代码了，需要修改之后补充到上一个commit中去，没关系 

先修改你的代码，同时加入暂存区，然后再次执行：

git commit --amend 

就会将你最新修改的代码跟上一次合并到一个commit中去。 

但是这里要注意，git commit --amend是会修改commit的SHA-1 hash值的，所以如果一个commit已经push到了远程仓库，就不要再去修改了，否则的话会造成提交历史的一个混乱。

2、对历史上的多个commit进行修改

你可能在本地连续几天都提交了几个commit了，但是一直都忘了push到远程仓库。此时突然有一天，你发现你需要将这个几天的commit都是一次性push到远程仓库，但是你看了一下，发现你3天前的某个commit的备注不规范，需要调整一下，此时你需要。。。

git commit --amend，只能针对最近一次commit去修复和调整。 

使用git rebase -i HEAD~3，就是调整最近3个commit。

同时要注意，如果你已经推送到了远程版本库的commit，不要修改和调整。

然后需要将你需要调整的commit之前的pick修改为edit，比如选择其中一个commit。 

此时你需要对你要修改的commit，一个一个的进行修改，可以修改3天前的那个commit的备注，执行git commit --amend即可。

3、删除commit以及调整commit的顺序

（1）删除commit是需要的，比如说你偶尔，不小心手贱，或者是一个应届生，不小心就加了一个空行，结果就搞了一个提交，此时需要将那个commit给删除掉。 

（2）调整commit的顺序，这个就不太靠谱了。

接着上面的命令，可以对多个commit，删除你不想要的那个commit，同时也可以调整顺序，然后保存。 

接着git直接会自动追溯到那几个commit的最近一个parent commit，重新按照你的顺序应用代码改动。 

这个场景常见于说，你不小心进行了某次无意义的提交，举个例子，不小心加了一个测试代码，结果还提交了，其实那个commit是不需要的，此时可以用这个办法删除那个commit。

4、将多个commit合并为一个commit 

如果恰巧你就是今天不小心做了多次commit，干脆破罐子破摔吧，就做多次commit。 

但是最后今天要push到远程仓库之前，可以将今天的多个commit合并在一起的。 

可以将多个commit前面的命令修改为squash，就会自动将这些commit合并为一个commit。 

提示一下，如果要合并多个commit，那么一定要往前多选择一个commit。

5、将一个commit切分为多个commit 

还有一种情况，就是你不小心将3天修改的代码都合并成了一个commit了，此时木已成舟，没法用git add -i来多次commit了，咋办？ 

没事的，将这个commit拆分为多个commit就好了。

将要拆分开来的commit前面的指令，修改为edit。 

然后你就会回到那个commit对应的版本，此时执行git reset HEAD^，此时会直接回退到之前的状态，然后此时可以执行多次git add和git commit，形成多个commit。 

接着最后执行git rebase --continue结束这次commit拆分。

### 20.Git高阶实战技巧：对本地刚做出的修改、暂存和提交进行撤回

1、reset撤回操作

（1）执行git reset --soft HEAD^，会有下面的影响

1）在仓库中，将master分支指向上一个commit。

2）暂存区和工作区还是停留在第三个版本，没有变化。

因此这个命令，相当于是撤回了git commit操作，因为git commit操作，就是将暂存区中的第三个版本放入仓库，同时创建一个新的commit object指向第三个版本，master指针也指向最新的commit object。

此时就相当于取消了刚执行完的git commit操作。

我们可以再次对暂存区加入更多的内容，然后再次执行git commit操作，此时再看一下效果，你会发现跟git commit --amend类似。 

（2）执行git reset HEAD^，默认等同于git reset --mixed HEAD^ 

回到三个区域都是第三个版本的状态，执行git reset HEAD^ 

1）将master指针指向上一个commit

2）将master指针指向的commit对应的版本恢复到暂存区中 

此时暂存区中的文件版本会变成第二个版本。 

这个命令，相当于是撤回了git commit和git add操作。 

这里我们可以想象一下，如果用git reset HEAD，就可以实现将刚git add到暂存区中的文件版本回退到仓库中的上一个版本，相当于就是回退了git add操作 

（3）执行git reset --hard HEAD^ 

相当于是彻底撤回刚才的git commit，git add，以及工作区中的代码修改。 

因为这个命令，会将指针指向上一个commit，同时暂存区里的内容变成上一个commit的版本，同时将上一个commit的内容恢复到工作区。

如果说此时你修改的代码比较少的话，那么就用git checkout -- 文件名，就可以将这个文件的内容恢复到上一次提交的状态，完全抹掉你最近的修改。 

但是如果你已经修改了很多代码了，此时你需要来个一次性的，强有力的恢复：git reset --hard HEAD。 

git reset --hard HEAD 或者是 git checkout -- 文件名。 

意思就是保持仓库和暂存区里的文件版本为HEAD最新版本，同时将上一次提交的HEAD版本写回工作区中，覆盖掉这次修改。

 2）如果修改了工作区中的内容，同时放入了暂存区，但是还没提交，此时要抛弃这些修改，怎么办？ 

（1）你希望将暂存区里加入进去的内容撤销掉，然后在工作区里面重新修改代码 

git reset HEAD = git reset --mixed HEAD，一样的命令，就是将仓库中HEAD指向的上一个commit，恢复到暂存区，同时恢复到工作区 

（2）你希望将暂存区和工作区里面的内容一次性抹掉：git reset --hard HEAD 

3）如果修改了工作区中的内容，放入了暂存区，还提交了，怎么办？ 

本来今天的代码你都提交了，但是你的leader说，这块代码不要了，此时怎么办？回退版本，直接回退到没做任何代码修改的那个版本

git reset --hard HEAD^，git reset --hard commit标识符 

git reset --hard HEAD^，分支指针指向上一个commit，同时上一个commit的内容恢复到暂存区，也恢复到工作区 

git reset --hard v1_commit_标识符

git reset --hard HEAD^^

### 21.Git高阶实战技巧：远程和本地同时撤回分支合并操作

1、在出现冲突的时候 

如果两个分支merge出现了冲突，临时打算取消注意，不想merge了，那么此时可以执行git merge --abort命令即可。 

2、如果仅仅是在本地完成了merge后想要撤回 

很简单，直接执行git reset --hard HEAD^，将分支挪回merge之前的那个commit即可，就相当于没执行过merge。

3、如果是在推送了merge后的commit到远程仓库之后想要撤回

你的代码都合并到master分支了。。。但是说不上线了，此时你是不能将不准备上线的代码留在master分支里的。 

如果你有其他需求在最近一两天也要上线，也会合并到master分支，然后如果那个需求上线，master分支会把你的那个不准备上线的功能的代码，给带上线。

所以这种情况，是不能接受的。

如果碰到这样的情况，你是需要将本次之前的merge给撤回的，无论是本地还是远程，都要撤回。

但是如果push到了远程仓库以后，你是不能再本地reset一下，再push的，因为那样会搞乱你的提交历史的。

不上线了。。。撤回release/v1.2.0到master分支的合并。 

需要在本地执行一个命令：git revert -m 1 HEAD。 

git revert实现的效果，不是说指针往回挪动，而是说创建一个新的commit，对应的代码版本跟合并之前的那个commit是一模一样的。 

-m 1的意思就是，恢复到当前这个分支合并之前的那个状态，此时会创建一个新的commit，这个新的commit对应的版本跟合并之前的commit是一模一样的。 

这就完美撤回了merge操作，同时此时如果执行git push origin master，会让远程仓库的master分支也回退到merge之前的状态，但是也多了一个新的commit出来。 

但是此时有一个问题，就是如果再次执行git merge feature/001，你会发现，git不让你merge了，因为之前已经merge过了，很尴尬。 

这个时候，过了几天了，然后呢，已经准备好可以上线了，你需要重新将release分支的代码合并到master分支上来，此时该怎么办呢？ 

而且此时一般是说feature分支需要继续修改代码，比如又加入了一些代码，但是如果这个时候你胡乱再次合并，会发现master分支仅仅merge进去了feature分支后来修改的那一点代码而已。。。。 

此时，要做的事情是，继续对feature分支进行修改，完成最终版本的代码。

然后，再次执行git revert HEAD，将master分支再次进行revert，回退到之前merge过后的那个状态，这个状态是包含了feature分支merge进来的部分代码的，然后再次执行git merge feature/001，将feature分支最新的修改也merge进来。

ok了，完美解决，再次git push origin master，推送到远程分支去。

#### 总结

（1）如果是要撤回已经push到远程仓库的merge操作。 

（2）在本地执行git revert -m 1 HEAD，再执行git push origin master，此时本地和远程的提交历史都会多一个commit出来，该commit的内容和合并之前的master指向的那个commit是一样的，同时master此时指向最新的那个commit。 

（3）但是后面，如果要再次将release分支和master分支进行合并，此时是需要特殊处理的，再次在本地执行：git revert HEAD，git push origin master。就是再次将master还原到指向一个新的commit，该commit的内容与上一次merge后的那个commit一样，包含merge的内容。 

（4）最后再次将release分支与master分支进行合并，此时可以保证release分支所有的内容，都是合并到master分支的。

5.如果在本地做了commit，同时还推送到了远程仓库，此时要撤回，怎么办？

简单了，比撤回merge简单多了，直接git revert HEAD，不就回到了上一个commit的状态，然后再次git push origin master，完美同步撤回远程仓库的commit操作了。

### 22.Git高阶实战技巧：二分查找哪一次提交引入了线上bug

如果你们线上发现了bug，然后定位到bug是某个代码文件的某几行代码导致的，此时需要看一下是谁在哪一次commit引入了这几行导致bug的代码。 

谁在哪一次commit具体的引入了这个bug，用git提供的二分查找。

git bisect start：开始二分查找

git bisect bad：标注当前这个commit是有bug的

git biset good v1.0.0：用tag来标注说从上一次哪个上线为止，是没有bug的，就是那个tag对应的commit之前都是好的

此时git会做一个二分查找，比如说两次commit中间差了12个commit，此时会给你定位到第6个commit。

然后你可以对这个版本的代码，运行个测试用例，看看bug能否复现出来。 

如果发现这个版本的代码没有bug，那么说明是之后引入的bug，输入git bisect good，会继续往后面二分查找；如果这个版本就有bug，那么说明是之前引入的bug，输入git bisect bad，会继续往前二分查找。 

以此类推，直到最后找到最后一个commit，认为就是这个commit开始引入的bug。

git会自动推断出来是哪个commit是实际引入bug的commit，最后会给你打印出来。

最后你需要执行git bisect reset，就是从二分查找状态中恢复到之前的状态。

### 23.Git高阶实战技巧：让多个项目共享一个子项目的代码修改权利

如果两个项目在两个仓库中，要共享一个子项目的代码，两个项目的人都可以去修改他，该怎么办呢？ 

但是实际上，在实际工作场景中，我坚决不建议出现这样的情况，大家就是如果有某一个子项目的话，单独交给某个人，然后单独拉一个仓库就可以了。

1、Submodules

一般来说，submodule使用的场景，就是要在一个父项目下，再建立一个子项目。

（1）我们先建立一个DbConnector子项目

（2）将所有工程师的父项目中都注册DbConnector这个子项目 

进入某一个父项目，然后在其下创建一个submodule：

git submodule add git@192.168.31.80:OA/DbConnector.git

这个时候，会出现一个新的目录，就是DbConnector。而且此时用git status命令，会发现说有.gitmodules和DbConnector两个东西等待提交。 

.gitmodules里面保存了你的子模块和其对应的git url之间的映射关系 。

然后执行下面的命令，将子模块提交： 

git add --all .

git commit -m ‘added DbConnector module’

git push origin master 

接着别的研发人员，可以将这个包含了一个子模块的项目克隆到自己本地，但是此时默认是只有一个子模块的目录，里面是空的，没有文件的。 

git clone git@192.168.31.80:OA/oa-parent.git 

或者是 

git pull 

此时需要运行两个命令： 

git submodule init

git submodule update 

才能初始化子模块的目录，同时拉取子模块的文件 。

或者也可以简化一点，通过给下面的命令，一次性把上述工作都干了： 

git clone --recursive git@192.168.31.80:OA/oa-parent.git

（3）对子项目进行代码修改

1）无论谁都可以跟平时一样，在DbConnector里面，当做是一个完全独立的项目，在里面可以修改源代码，包括说修改之后提交，分支操作，在DbConnector里面去执行就可以了。 

2）以后每次如果某个人要更新子模块中的数据，那么执行以下命令即可： 

进入子模块的目录

git fetch

git merge origin/master 

2、Subtree

git以前是推荐使用submodule来管理子项目，但是git 1.5.2之后就推荐使用subtree了。subtree的优点，就是子项目的维护对我们来说几乎就是透明的，不需要引入太多的维护成本。 

cd P1项目的路径

git subtree add --prefix=用来放S项目的相对路径 S项目git地址 xxx分支 

上面的代码，就会把s项目的代码（指定分支）下载到本地放入指定的目录中，同时提交到本地p1项目中。 

对于P2项目也做一样的事情。 

直接在p1中各种修改代码和提交，包括s项目的代码，都没有关系。 

接着某一天，p1项目负责人想把对s项目的代码更改提交到远程仓库，此时执行下面的命令：

git subtree push --prefix=S项目的路径 S项目git地址 xxx分支。

这个时候，git会找到所有对s目录的commit，然后一起提交到其对应的远程版本库中。 

接着p2项目的负责人知道了这个事情，需要将s项目最新的代码更新到自己本地，此时就执行下面的命令即可： 

git subtree pull --prefix=S项目的路径 S项目git地址 xxx分支。

### 24.Git高阶实战技巧：团队拆分迁移项目代码时简化提交历史

将原来的一个仓库拆分成两个仓库。 

echo 'get history from blah blah blah' | git commit-tree 9c68fdc^{tree} 

我们从要截断的那个commit的上一个commit，创建一个基准commit出来，比如用倒数第二个commit。 

git rebase --onto 622e88 9c68fdc 

接着，执行git rebase --onto命令，将倒数第二个commit之后的commit重新基于那个commit基础之上来构建提交历史。

此时，master分支代码就会只保留了2个commit 。

然后可以将这个master分支的代码和commit历史提交到另外一个新的代码仓库上去，那个代码仓库就会仅仅保留两个commit了。

### 25.Git高阶实战技巧：将新版本的功能放到上一个版本提前上线

假设我们同时在开发两个版本，v1.0和v1.1，然后pm突然决定，将v1.1的部分功能提前放到v1.0版本先上线？此时该怎么办呢？1627098499

 希望v1.1版本的部分核心功能，提前放到v1.0先上线，v1.1的版本剩余的功能可以慢慢上线。

v1.1版本里面，提交历史，这个时候就来了，为什么之前告诉大家说，一定要按照规范，每天开发好的那一块儿完整的功能，就作为一个commit，后续基于这条清晰的脉络可以做很多事情。 

比如说现在就可以从V1.1的提交历史中，找到v1.0需要提前上线的那些功能对应的commit。 

如果你没有清晰而且定义良好的commit历史的话，试问，你怎么从一团乱麻的v1.1版本的代码中，找出来指定的那几个功能对应的代码呢？？

很简单，用git cherry-pick功能。

如果我们要做到部分功能提前挪到v1.0先上线，那么就需要将v1.1中那些功能对应的git commit放到v1.0分支上去，然后将那些commit对应的代码都应用到v1.0分支上去，就可以了。 

git checkout feature/v1.0

git cherry-pick feature/v1.1分支对应的多个commit标识。

### 26.Git高阶实战技巧：基于GitLab的代码权限控制以及强制代码审查

代码权限控制。

（1）第一件事情，有些分支是需要被保护起来的，就是说不能直接被push，尤其是master分支。

在settings->repository，有一个protected branch，就是master分支默认是被保护起来的，只有这个项目的master角色的开发人员，才能直接对master分支进行push操作。

只有拥有master权限的用户才可以合并。

（2）仓库全都是private私有的，如果在一个部门中建立了一个项目，那么默认这个部门下的研发人员对这个项目都是有权限的，但是有的人是master，有的人是developer。 

developer是只有权限对普通的分支进行各种操作的，feature，develop，release。

但是最后developer是没有权限直接push代码到master分支的。 

而且，developer最后是需要提交merge request，将release分支合并到master分支的，此时只有master权限的人才可以去review merge request，将其他分支merge到master分支。 

其他团队的人，其他group的人，默认是没有权限来访问我们部门的项目的。 

每个部门的人，默认就只能访问自己部门的项目和代码。 

每个项目的话呢，又只有master负责人有权限操作master分支。 

其他团队的人可以作为guest来访问我们团队的项目，因为经常会遇到什么呢？有其他团队的人说，让我看一下你们的代码行不行啊，那此时可以啊，给一个reporter权限就可以了。 

guest，你可以给一些大领导，有些大领导是不需要修改代码的，可能也不看代码，然后就是来看看你的项目里的一些issue，gitlab管理一些缺陷，大领导，可能会关注一些bug的情况，会进来看看bug相关的一些issue。

（3）强制code review

强制code review，每个工程师在feature分支中开发好和单元测试好之后，他们本来是需要将feature合并到develop去做集成的。 

其实这个环节是需要进行code review，要求他们必须提交merge request到develop分支，让项目里的master权限的高工去review他们的代码。 

但是如果默认情况下，没有将develop分支设置为protected，那么普通人是可以绕过高工，直接在本地跟develop分支进行合并，然后push到远程的develop分支上去的。 

如果将develop分支保护起来之后，就是说，每个研发人员第一次写好feature分支的代码，都必须通过merge request往develop分支合并代码，此时项目里的高工会负责review他们的代码。 

review通过之后，可以合并到develop分支，做持续集成。 

后续是要在develop分支做持续集成测试的，可能需要修改bug，修复集成测试的bug，还是在feature分支上进行。每次修复好一批bug之后，push到feature分支，然后提交merge request合并到develop分支，高工继续review代码。 

就是说卡在持续集成这个环节，将代码质量给严格把关，每次提交的代码，都必须高工过目，看一眼，不能写的太烂。 

通过develop持续集成之后，会从develop拉那个release分支，接下来，每个研发人员就可以在release分支直接进行bug的修复了

### 27..git目录

平时我们如果执行git init之后，就会生成一个.git目录，其中就是git存放所有数据的地方。如果我们拷贝这个.git目录，那么就相当于备份了一个完整的git项目的数据。

config

description

HEAD

hooks/

info/

objects/

refs/ 

description文件仅仅是由Git WebUI程序去使用的，我们一般不用去管这个文件。 

config文件包含了这个git项目的所有配置项。 

info目录中包含了我们在.gitignore文件中定义的不需要git追踪的那些被排除掉的文件。 

hooks目录中包含了我们的client-side或者是server-side的钩子脚本。 

其实上面那些都不是最重要的，最重要的是这几个东西：HEAD文件，indexes文件，objects目录，以及refs目录。 

objects目录存储了git中所有的数据，refs目录中存储了所有的指针（包括了branch，tag，remote，等等，指向了objects目录中的数据），HEAD指向了当前我们所处的分支指针，indexes文件存储了暂存区中的内容。

### 28.git数据存储机制

1）git hash-object初体验 

git的数据存储格式实际上是一种非常简单的key-value存储格式。就是说，git支持将任何东西存储到其数据库中，一般就是存储文件的内容，然后用一个key值来引用它。 

首先随便找一个目录，初始化为git项目： 

git init 

此时去.git/objects目录中检查一下，发现只有两个子目录，info和pack，没有任何文件，此时数据库是空的。 

用下面命令，可以手动将一个字符串作为value存储到git的数据库（objects，git的数据库，用了比较简单的方式去存储）中去，会看到返回了一个hash值来引用那个字符串value： 

echo 'hello world' | git hash-object -w --stdin 

解释一下上面的命令： 

git hash-object就是git的一个底层命令，一般我们是不会直接用的，一般都是git add，git commit之类的命令在调用这种底层命令。 

git hash-object命令，默认的表现是这样的，接受一个value，然后针对这个value返回一个hash值，默认是不将value写入自己的数据库的。 

但是使用了-w之后，git就会将value写入数据库之中，同时用hash值作为key来引用那个value对应的文件。同时--stdin指的是从命令行接收value，也就是echo ‘test content’中的test content。如果不使用--stdin，那么要求是在hash-object之后跟一个文件名，它就会将一个文件作为value存储到数据库中去，然后返回一个hash值。 

git返回的是SHA-1 hash值，是40位的字符串，是根据文件内容计算出来的一个checksum，校验和。

2）git的文件存储机制（轻量级索引机制） 

你往git里存储一个文件，git一定会根据文件的内容计算一个40位的hash值出来，作为那个文件的名称，也是指针，40位hash值就可以来引用这个文件。存储的时候，40位hash的头2位，会作为目录名称放在objects里面，然后那个存储的文件会放在那个目录中，文件名是40位hash值的后38位。 

大家来思考一下，git作为一个高性能的版本控制系统，一大特点就是性能超高，在切换分支的时候，都需要进行大量的磁盘读写，磁盘存储机制是比较值得我们注意的。 

将40位hash值的头2位作为目录，其实你可以认为是一种轻量级的索引机制。

这样的话，有一个好处，就是每次你要根据一个hash值定位一个数据的时候，直接根据头2位先定位到一个目录，然后再在这个目录下去查找，linus写git的时候，就是用了一种非常轻量级的文件索引机制。

3）git cat-file体验 

使用下面的命令可以查看刚存储进去的文件值，git cat-file是另外一个底层命令，就是专门用来查看存储到git中的文件的内容的： 

git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4

4）一个文件的多个版本存储机制

git会将一个文件的多个不同版本，以完整快照的方式存储在objects数据库中，每个版本都用一个单独的文件来存储，每个版本的文件都有一个不同的hash值。

使用下面的命令可以查看.git/objects中所有的文件：find .git/objects -type f。

实际的文件的内容，每个文件的版本快照，会作为一个blob object存储在git数据库中。

git数据库，git仓库，database，repository。

5）梳理一下

实际上，就是一个文件的不同的版本，都会用一个完整的快照的形式，作为一个单独的文件存储在git数据库中，所谓的完整快照，每个版本，都是用一个文件存储这个版本的完整的内容的。

每个版本的文件内容，都是通过blob object的类型存储在git数据库中的。

### 29.tree object

git有另外一种object叫做tree object，每次将一堆文件的一个版本存储到git仓库中，都是用blob object来存储那些文件的版本，然后用一个tree object来引用多个blob object。tree object通常会包含指向其他多个blob的指针，或者是其他tree object的指针。用下面的命令，可以查看master分支指针指向的那个tree object： 

git cat-file -p master^{tree}

git通常是基于暂存区中的内容来创建一个tree object的，我们首先需要在暂存区中加入一些内容，然后可以试着手动创建一个tree object。可以将仓库中存储的某个commit object的内容放入暂存区中： 

git update-index --add --cacheinfo 100644 83baae61804e65cc73a7201a7252750c76066a30 test.txt 

git update-index：底层命令，一般是git add命令去调用的，将数据库中存储的某个blob数据的引用放到暂存区中。 

--add：因为暂存区中还没有这个文件，你可以认为这个文件是第一次进入暂存区，所以需要这个选项。 

--cacheinfo：不是从工作区中加入文件放到暂存区，是从git仓库（git数据库）中加入文件。 

100644 mode：这种模式的意思是，这个是一个普通的文件。

所谓的将仓库里的东西读出来放到暂存区，实际上只是将一些引用放到暂存区，但是blob，tree，实际还是存储在objects中的。

### 30.commit object

每次如果执行了一些相关的操作，可以将当前这个时刻，有变化的文件的版本都存储到git仓库中，同时放入暂存区中。 

此时，暂存区中，没有变化的文件，还是保持着之前的版本；有变化的文件，暂存区中会存放最新版本的blob对应的引用 

然后，如果此时创建一个tree object，就是基于暂存区中当前所有的文件的版本的blob，创建一个tree 

此时，这个tree object就代表了这个项目的所有代码的此时此刻的一个完整的快照

现在我们实际上是有3个tree object，分别代表了项目的不同时刻的快照。但是此时此刻，我们有个文件，就是不知道那些tree object是谁添加的，为什么添加，什么时候添加的，什么都不知道。 

但是，光有tree object是不够的 

此时就需要commit object了。 

用下面的命令，基于已有的tree object创建一个commit object，同时给这个commit object一个提交备注： 

echo 'first commit' | git commit-tree d8329f 

然后查看一下这个commit object，会发现包含了一个tree object，同时又作者，提交日期，以及提交备注： 

git cat-file -p fdf4fc3 

commit object的含义很简单，就是指向了一个tree object，而那个tree object指向了多个blob，其实这个tree object就代表了某一时刻项目中所有代码文件的快照版本，同时那个commit object包含了作者、提交时间、提交备注，此外还包含了指向上一次commit object的指针。 

同样的，我们可以为另外两颗tree object分别创建一个commit object，代表了另外两次提交： 

echo 'second commit' | git commit-tree 0155eb -p fdf4fc3

echo 'third commit' | git commit-tree 3c4e9c -p cac0cab 

此时相当于我们就有了三次跟真实一样的提交了，可以用git log来查看一下提交历史： 

git log --stat 

就会看到最近的3次提交。

git add：相当于是将你的有修改的文件作为一个新的版本，采用单独的blob文件存储到仓库中去，同时将仓库中的文件更新到暂存区中。 

git commit：相当于就是将暂存区中的文件快照，也就是对应的blob指针创建一个tree object，写入仓库中，同时再创建一个commit object包含了提交人/提交时间/提交备注，来指向那个tree object，代表了一次提交对应的仓库快照。

### 31.SHA-1 hash值如何计算

首先blob的内容分为两块，一个是header，一个是content。header就是类型+空格+长度+\u0000。然后会用header+content来计算出一个SHA-1校验和，作为hash值。

### 32.提交代码原理

git update-index --add 文件名：其实就是，我们在工作区里修改了文件以后，会用这个底层命令，将工作区中有修改的文件的一个版本，放入git仓库中作为一个blob去存储这个版本，同时将这个blob的引用放入暂存区中。 

多次有git update-index之后，git仓库中会存储项目中每个文件的所有版本，同时在暂存区中保留了每个文件的最新版本，没有变化的就保留值钱的版本。 

执行git write-tree，就可以针对暂存区中当前项目的最新版本，创建一个tree object，引用项目中所有的文件当前此时此刻的一个版本对应的blob，tree object，就代表了项目当前的一个完整快照版本。

接着执行git commit-tree，就可以基于这个tree object创建一个commit object，包含了tree object的创建人，创建时间，备注信息，等等，作为一次提交。

1）我们首先在工作里各种新增、修改文件。 

2）我们执行git add --all .这个命令：此时会将工作区中有变化的文件，作为一个新的版本，直接存储到git仓库中去，作为一个blob来存储；同时在暂存区中加入这些最新版本的blob的引用，替换同一个文件之前在暂存区中的版本。 

git update-index

3）我们执行git commit，同时给出一个提交备注：此时会针对暂存区中现在的所有的版本，创建一个tree object，作为项目此时此刻的一个快照版本，放入仓库；另外再创建一个commit object，包含了提交人，提交时间，提交备注，来引用这个tree object。 

4）到此为止，完成一次git add和git commit。

### 33.指针

（1）分支指针 

如果我们要查看某个commit，那么还是要知道它的hash值，然后用git log hash_value来查看这个commit。但是这还是很麻烦，所以最好是可以有个文件存储那个hash值，然后我们直接用一个较为简单的名称来引用。 

.git/refs目录中，有几个子目录，包括了heads和tags两个子目录，在这里就有各种指向objects中的指针。 

.git/refs/heads目录下，有一个master文件，实际上是git自动创建一个初始的分支，分支其实就是一个指针而已，指向了某个commit object的，在master文件中就包含了一个40位的hash值，当前就是指向了最近一次commit object。 

.git/refs/heads目录下，会包含很多个文件，就是每次创建一个分支，就会在这个里面对应新建一个文件，文件名称就是分支的名称，文件里的内容就是分支指针当前指向的那个commit object的hash值，代表着这个分支当前指向了哪个commit object。

新建一个分支，实际上就是在.git/refs/heads下面新建对应的目录和文件，在文件里面维护一个指针，一个hash值，指向了.git/objects中某个commit object的hash值 

通常不建议直接自己手动修改refs文件的内容，可以用下面的命令更新某个refs文件指针指向的commit object：

git update-ref refs/heads/feature/001 1a410efbd13591db07496601ebc7a059dd55cfe9

实际上我们如果执行git branch命令，就是执行update-ref命令，创建那个分支对应的文件，然后文件内容就是对应的commit object的SHA-1 hash值。 

删除一个分支很简单的，就是删除.git/refs/heads下面的分支对应的目录和文件即可。

（2）HEAD指针 

HEAD文件中，保存了对某个分支指针的引用，其实就是某个分支对应的文件。 

HEAD其实也是一个文件，但是也是一个指针，这个HEAD呢，就是指向了当前你所处的那个分支而已，保存的是refs/heads下面的分支对应的目录和文件名。

比如当前你在master分支，那么HEAD文件中的内容就是refs/heads/master，代表了HEAD当前指向了master分支。

因此如果我们比如在master分支上，创建并且切换到了另外一个分支，那么此时会根据HEAD找到当前分支文件，再找到当前分支指向的commit object，那么新建的分支就会同样指向那个commit object。

比如master分支切换到feature/001分支，实际上就是一个非常轻量级的操作 。

（1）HEAD文件里的内容，从refs/heads/master，变为refs/heads/feature/001，代表着HEAD指针指向了feature/001分支。 

（2）将feature/001分支指向的那个commit对应的tree，对应的那些blob对应的文件版本的内容，一次性从仓库里恢复到工作区中，工作区中所有文件的版本和内容，会变成那个分支指向的commit当时的那个快照版本。

每次切换分支之后，HEAD都会指向那个分支的文件。

可以通过git checkout命令+cat .git/HEAD命令结合起来，然后就可以看到每次切换后的HEAD值。

每次我们执行一次git commit操作，都会新建一个commit object出来，然后会让当前分支指向最新的commit object。

分支就是一个文件，保存了指向的commit的40位hash置。

HEAD也是一个文件，保存了指向的那个分支对应的文件名称。

（3）tag object

tag object是指向某个commit object的，包含了标签时间，标签名称，等等。而且tag object永远就指向创建时指定的那个commit object，不能修改。

我们可以通过下面的命令创建一个轻量级的tag object。 

就只是一个指针而已，在refs/tags下面，会有一个tag名称对应的文件，然后里面就是那个tag指向的commit的40位hash值。 

git tag v1.0：轻量级的tag，直接就指向了当前分支指向的那个commit object，就是在refs/tags下面搞一个问加你，作为一个指针而已。 

git update-ref refs/tags/v1.0 cac0cab538b970a37ea1e769cbbde608743bc96d

（4）remote 

最后一种引用类型，就是remote。 

如果我们添加了一个remote，同时push了一些东西到那个remote，git会在refs/remotes中保留对每个分支，最近一次push到远程服务器对应的commit。

###  34.文件合并

哪怕是很大的文件，比如说20kb的一个代码文件，每次改动一点代码，git也是存储改动后的版本对应的一个完整的文件；如果这个20kb的代码文件修改了100次，会怎么样？存储100个版本对应的文件，也就是100个文件，2000kb ~ 2MB。所以这样的话对存储的压力还是蛮大的。。。。 

git实际上是可以更加智能的处理这种文件改动的。默认情况下，git会使用loose格式存储文件（loose，松散格式，采取这个文件的每个版本，就存储一个完整的单独的文件），但是一段时间过后，git会将多个loose格式的松散文件打包到一个packfile二进制文件中，来节省磁盘空间。

什么时候git会进行packfile打包的操作呢？两个时机：我们手动执行git gc操作，或者是将文件push到远程服务器的时候，git会看一下loose格式文件是不是太多了，如果是，则进行打包合并。 

git gc：garbage collector，jvm gc是不一样的，git用来处理自己的文件的。

如果我们将本地的commit数据push到了远程服务器后，就代表远程服务器包含了我们的数据了，此时git就可能会执行一次packfile打包。

find .git/objects -type f，之前应该是一堆hash值组成的目录和文件，包含完了十几个object（blob，tree，commit），会发现出现了objects/info/packs和objects/pack等新的目录。

我们是将之前所有的hash值组成的文件打包成了一个packfile文件，进行了压缩以及合并。 

pack包含了两个文件，一个是packfile包含了打包的所有数据，一个是index文件，包含了每个blob object在packfile中的offset，通过.idx文件标识出每个object在.pack文件里是哪一个部分。

git做文件存储的优化，两块：多个文件打包成给一个，压缩，二进制格式，packfile，节省空间；在打packfile的时候，就会对我们刚才说的那种情况，大ruby文件，仅仅保留第一个版本对应的完整内容，然后之后的版本都是直接保留的是它的那些delta增量内容，后面的版本就不是全部保留全量内容了，进一步节约空间。

git，git gc；git push，自动会做打packfile。

实际上，在git进行打包packfile的时候，就会对一个文件的多个版本，仅仅保留其每次增量修改数据，避免对一个文件保留多个全量的版本快照。

#### 总结

（1）git gc / git push，会执行packfile的压缩优化。

（2）将.git/objects下的各种object对应的文件，压缩成一个packfile。

（3）每个packfile都对应一个.pack文件和一个.idx文件，.pack文件包含了多个object的内容，.idx文件包含了每个object在那个.pack里面的offset偏移量。

（4）在打packfile的意义：多个文件合并一个，节省空间：对大文件进行增量处理，仅仅是最新的版本保留全量内容，之前的版本保留增量内容。

（5）用git verify-pack -v命令，可以查看.pack文件里包含的具体内容。

### 35.提交代码

（1）fork出来一份，在自己的账号里有一个拷贝的项目，同时将自己的ssh key加入github，将fork出来的项目克隆到本地。 

（2）从fork出来的项目的master分支创建一个自己的分支出来。 

（3）在那个分支上提交代码。 

（4）push分支和代码到自己fork出来的项目。 

（5）在github上提交一个pull request，对开源项目提交。

 