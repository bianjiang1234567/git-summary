# Git 总结

## git中基本概念
git三个区：工作区，暂存区，git仓库

已跟踪文件，未跟踪文件(git无法控制)

已跟踪文件的状态有：已提交（committed）、已修改（modified） 和 已暂存（staged）

* 已修改表示修改了文件，但还没保存到数据库中。

* 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

* 已提交表示数据已经安全地保存在本地数据库中。

## git配置信息获取
git config --list  使用罗列出的最后信息

git config key： 来检查 Git 的某一项配置 如git config user`.`name

## git帮助命令
git help verb

git verb --help

man git-verb

## 初始化
git init 该命令将创建一个名为 .git 的子目录

## 检查当前文件状态
git status

使用 git status -s 命令或 git status --short 命令，得到一种格式更为紧凑的输出
新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。

## 跟踪新文件
git add filename
这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。

## 忽略文件
可以创建一个名为 .gitignore的文件，列出要忽略的文件的模式

规则如下：
所有空行或者以 # 开头的行都会被 Git 忽略。
可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。
匹配模式可以以（/）开头防止递归。
匹配模式可以以（/）结尾指定目录。
要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 匹配
任何一个列在方括号中的字符 （这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）； 问号（?）只
匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配
（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（\*\*）表示匹配任意中间目录，比如 a/**/z 可以
匹配 a/z 、 a/b/z 或 a/b/c/z 等。

举例如下：

\# 忽略所有的 .a 文件

*.a

\# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件

!lib.a

\# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO

/TODO

\# 忽略任何目录下名为 build 的文件夹

build/

\# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt

doc/*.txt

\# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件

doc/**/*.pdf

## 查看已暂存和未暂存的修改
git diff 此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。

git diff --staged 或 git diff --cached 这条命令将比对已暂存文件与最后一次提交的文件差异。

## 跳过使用暂存区域
在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存
起来一并提交，从而跳过 git add 步骤，例如

git commit -a -m 'added new benchmarks'

## 移除文件
从已跟踪文件清单中移除（确切地说，是从暂存区域移除）

下面两条命令，删除在磁盘中的文件

rm PROJECTS`.`md

git rm PROJECTS`.`md

git rm -f filename 强制从磁盘和git跟踪中删除，等价于上面两条命令

git rm --cached README 仅不让git跟踪，不删除磁盘文件

## 移动文件
对工作区和暂存区都可以修改
例如
git mv README`.`md README

此时相当于执行了三条命令

mv README`.`md README

git rm README`.`md

git add README

## 查看提交历史
git log

git log -p -2
选项 -p 或 --patch ，它会显示每次提交所引入的差异（按补丁的格式输出），使用 -2 选项来只显示最近的两次提交。

git log --stat
看到每次提交的简略统计信息，可以使用 --stat 选项

## 补充已提交的内容
git commit -m 'initial commit'

git add forgotten_file

下面这个命令会将暂存区中的文件提交。
git commit --amend -m 'initial commit'

执行上面三个命令最终只会有一个提交——第二次提交将代替第一次提交的结果

## 取消暂存区暂存的文件
暂存后执行，可以取消暂存，文件将变成未暂存的

 git reset HEAD file

## 撤消对文件的修改
git会用提交的版本覆盖工作区的版本，会对工作区的文件撤销已有的修改

git checkout -\- file

例如
 git checkout -\- CONTRIBUTING`.`md

## 查看远程仓库
显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。会列出全部远程仓库
git remote -v

git remote
若显示origin  这是 Git 给克隆的仓库服务器的默认名字

## 添加远程仓库

运行 git remote add shortname url 添加一个新的远程 Git 仓库

例如git remote add origin https://github.com/username/repository/.git

## 向远程仓库推送
在github上建立新的仓库，并添加远程仓库后
git push -u origin master

## 强制更新远程仓库master分支

git push origin master -f

git push -u origin master -f

不想强制的话，提交前要git pull一下，将远程合并到本地

## 从远程仓库中抓取与拉取
git fetch shortname 例如 git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作，但不会合并当前工作，要手动合并

## 抓取与拉取后自动合并分支
默认情况下，git clone 命令会自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支（或其它名字的默认分支）。 克隆命令会自动设置好master和origin两个名字。
运行 git pull 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

## 推送到远程仓库
当你想要将 master 分支推送到 origin 服务器时

git push origin master

只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，push命令才能生效
因此你必须先抓取别人的工作并将其合并进你的工作后才能推送。

## 查看某个远程仓库
git remote show origin

这个命令列出了当你在特定的分支上执行 git push 会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了， 还有当你执行 git pull 时哪些本地分支可以与它跟踪的远程分支自动合并。

## 远程仓库的重命名与移除
将远程仓库pb修改为paul
git remote rename pb paul

移除远程仓库pb
git remote remove pb或 git remote rm pb

## 列出标签
git tag

按照通配符列出标签需要 -l 或 --list 选项，例如git tag -l "v1.8.5*"
对 1.8.5系列感兴趣。每个标签对应一个提交commit

## 创建标签

Git 支持两种标签：轻量标签（lightweight）与附注标签（annotated）
轻量标签很像一个不会改变的分支——它只是某个特定提交的引用。
附注标签是存储在 Git 数据库中的一个完整对象， 它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间， 此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。 通常会建议创建附注标签，这样你可以拥有以上所有信息。但是如果你只是想用一个临时的标签， 或者因为某些原因不想要保存这些信息，那么也可以用轻量标签。

## 附注标签

 git tag -a v1.4 -m "my version 1.4"
 -m 选项指定了一条将会存储在标签中的信息。

 git show 命令可以看到标签信息和与之对应的提交信息，例如
 git show v1.4

 ## 轻量标签

 只需要提供标签名字：
 git tag v1.4-lw

## 后期打标签
git log --pretty=oneline找到提交历史里面的信息

git tag -a v1.2 9fceb02在命令的末尾指定提交的校验和（或部分校验和） 9fceb02为部分校验和

## 共享标签

在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样——你可以运行 git push origin tagname

如果想要一次性推送很多标签，也可以使用带有 --tags 选项的 git push 命令，例如git push origin --tags

## 删除标签

git tag -d tagname本地删除标签

git push origin --delete tagname远程服务器删除标签

## 分支创建

创建了一个可以移动的新的指针，在当前所在的提交对象上创建一个指针。
git branch testing

有一个名为 HEAD 的特殊指针指向当前所在的本地分支

git log --oneline --decorate 查看各个分支当前所指的对象

## 分支切换

创建一个新分支后立即切换过去，这可以用 git checkout -b newbranchname 一条命令

切换到新创建的 testing 分支，HEAD 就指向 testing 分支了。
git checkout testing

HEAD 随着提交commit操作自动向前移动

checkout out 检出时 HEAD 也随之移动

## 项目分叉历史
git log --oneline --decorate --graph --all ，它会输出你的提交历史、各个分支的指向以及项目的分支分叉情况。

## 合并分支
git checkout master 和git merge hotfix 两条命令将hotfix分支合并到master分支上

试图合并两个分支时， 如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候， 只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）”

只需要检出到你想合并入的分支，然后运行git merge 命令

##  删除分支

git branch -d hotfix 删除hotfix分支

对于没有合并的分支进行删除，会丢失该分支，可以使用 -D 选项强制删除它

## 无法顺着一个分支走下去能够到达另一个分支

Git 将合并的结果做一个新的快照并且自动创建一个新的提交指向它。 这个被称作一次合并提交或者三方合并，它的特别之处在于他有不止一个父提交。


## 遇到冲突时的分支合并

如果你在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们

Git 会做合并，但是没有自动地创建一个新的合并提交。

使用 git status 命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件

查看这些unmerged文件，修改每个文件后，对每个文件使用 git add 命令来将其标记为冲突已解决，一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决。

或者使用图形化工具来解决冲突，可以运行 git mergetool

最后可以输入 git commit 来完成合并提交

## 分支管理
git branch 命令不只是可以创建与删除分支。 如果不加任何参数运行它，会得到当前所有分支的一个列表

查看每一个分支的最后一次提交，可以运行 git branch -v 命令

--merged 与 --no-merged 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。

例如git branch --no-merged master显示尚未合并到 master 分支的分支


## 分支开发工作流

长期分支： 稳定分支的指针总是在提交历史中落后一大截，而前沿分支的指针往往比较靠前。随着你的提交不断右移指针。

主题分支： 主题分支是一种短期分支，它被用来实现单一特性或其相关工作。

## 远程分支

又名远程跟踪分支，它们以 remote/branch 的形式命名，例如origin/master。
如果要与给定的远程仓库同步数据，运行 git fetch remote 命令，例如 git fetch origin。这个命令查找 “origin” 是哪一个服务器， 从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针到更新之后的位置。

## 远程分支推送
本地的分支并不会自动与远程仓库同步——必须显式地推送想要分享的分支
git push remote branch

想要在自己的serverfix 分支上工作，可以将其建立在远程跟踪分支之上git checkout -b serverfix origin/serverfix，也可以运行 git merge origin/serverfix 将这些工作合并到当前所在的分支。

## 跟踪分支

从一个远程跟踪分支检出一个本地分支会自动创建所谓的“跟踪分支”（它跟踪的分支叫做“上游分支”）。如果在一个跟踪分支上输入 git pull，Git 能自动地识别去哪个服务器上抓取、合并到哪个分支。

git checkout -b branch remote/branch

git checkout --track origin/serverfix

git checkout -b sf origin/serverfix 本地分支 sf 会自动从 origin/serverfix 拉取。

查看设置的所有跟踪分支，可以使用 git branch 的 -vv 选项。

如果想要统计最新的领先与落后数字，需要在运行此命令前抓取所有的远程仓库。
git fetch --all; git branch -vv

## 拉取

git pull 在大多数情况下它的含义是一个 git fetch 紧接着一个git merge 命令。
git pull 都会查找当前分支所跟踪的服务器与分支， 从服务器上抓取数据然后尝试合并入那个远程分支。

## 删除远程分支

如果想要从服务器上删除 serverfix 分支，运行下面的命令：
git push origin --delete serverfix

## 变基

在 Git 中整合来自不同分支的修改主要有两种方法：merge 以及 rebase。

## 变基的基本操作

可以使用 rebase 命令将提交到某一分支上的所有修改都移至另一分支上。
可以检出 experiment 分支，然后将它变基到 master 分支上。
例如 git checkout experiment；git rebase master

它的原理是首先找到这两个分支（即当前分支 experiment、变基操作的目标基底分支 master） 的最近共同祖先，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件， 然后将当前分支指向目标基底, 最后以此将之前另存为临时文件的修改依序应用。

回到 master 分支，进行一次快进合并。
git checkout master
；git merge experiment

无论是通过变基，还是通过三方合并，整合的最终结果所指向的快照始终是一样的，只不过提交历史不同罢了。 变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。

## 跳过其他分支变基

git rebase --onto master server client
取出 client 分支，找出它从 server 分支分歧之后的补丁， 然后把这些补丁在
master 分支上重放一遍，让 client 看起来像直接基于 master 修改一样。

然后再
git checkout master； git merge client

git rebase basebranch topicbranch 命令可以直接将主题分支 （即 server）变基到目标分支（即 master）上。 这样做能省去你先切换到server 分支，再对其执行变基命令的多个步骤，例如git rebase master server

## 不要用变基的情况

如果提交存在于你的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基

## 搜索

Git 提供了一个 grep 命令，你可以很方便地从提交历史、工作目录、甚至索引中查找一个字符串或者正则表达式。

 默认情况下 git grep 会查找你工作目录的文件。 一种变体是，你可以传递 -n 或 --line-number 选项数来输出 Git 找到的匹配行的行号。
例如 git grep -n gmtime_r

## 重置
HEAD：上一次提交的快照，下一次提交的父结点
HEAD 是当前分支引用的指针，它总是指向该分支上的最后一次提交。 这表示 HEAD 将是下一次提交的父结点。 通常，理解 HEAD 的最简方式，就是将它看做 该分支上的最后一次提交的快照。

Index： 预期的下一次提交的快照
索引是你预期的下一次提交。 这个概念引用为 Git 的“暂存区”，这就是当你运行 git commit时 Git 看起来的样子。

Working Directory： 沙盒，工作目录会将 HEAD和索引 解包为实际的文件以便编辑。可以把工作目录当做 沙盒。

命令reset做的事情：
reset 做的第一件事是移动 HEAD 的指向。例如，你正在 master 分支上，运行 git
reset 9e5e6a4 将会使 master 指向 9e5e6a4。它本质上是撤销了上一次 git commit 命令。使用 reset --soft，HEAD将仅仅停在那儿。HEAD~表示HEAD 的父结点，还可以加数字~2

更新索引（--mixed）reset 会用 HEAD 指向的当前快照的内容来更新索引。回滚到了git add 和 git commit 的命令执行之前。 这也是默认行为，所以如果没有指定任何选项（例如只是 git reset HEAD~），这就是命令将会停止的地方。

更新工作目录（--hard）reset 要做的的第三件事情就是让工作目录看起来像索引。 --hard 标记是 reset 命令唯一的危险用法，它也是 Git 会真正地销毁数据的仅有的几个操作之一。

## 通过路径来重置

运行 git reset file.txt （这其实是 git reset --mixed HEAD file.txt 的简写形
式，它本质上只是将 file.txt 从 HEAD 复制到索引中，它还有 取消暂存文件 的实际效果。通过具体指定一个提交来拉取该文件的对应版本。 我们只需运行类似于 git reset eb43bf file.txt 的命令即可。

## reset与checkout的区别
不同于 reset --hard，checkout 对工作目录是安全的，它会通过检查来确保不会将已更改的文件弄丢。

reset 会移动 HEAD 分支的指向，而 checkout 只会移动HEAD 自身来指向另一个分支。
我们现在在 develop 上（所以HEAD 指向它）。 如果我们运行 git reset master，那么 develop 自身现在会和 master 指向同一个提交。 而如果我们运行 git checkout master 的话，develop 不会移动，HEAD 自身会移动。 现在 HEAD 将会指向 master。

## 合并冲突
Git 会在有冲突的文件中加入标准的冲突解决标记，这样你可以打开这些包含冲突的文件然后手动解决冲突。 你可能不想处理冲突这种情况，完全可以通过git merge --abort 来简单地退出合并

## 子模块调用

子模块允许你将一个 Git 仓库作为另一个 Git 仓库的子目录。
它能让你将另一个仓库克隆到自己的项目中，同时还保持提交的独立。
你可以通过在 git submodule add 命令后面加上想要跟踪的项目的相对或绝对 URL 来添加新的子模块。
