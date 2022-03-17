## Git学习

#### 创建版本库

1.在合适的地方创建目录：

```
mkdir learngit(目录名)
```

2.创建仓库：

```
git init
```



#### 将文件添加至仓库

1.首先要在**learngit**目录下创建文件，例如readme.txt

2.将readme.txt文件添加至仓库中

```
git add readme.txt
```

3.将文件提交至仓库中

```
git commit -m "add a file"
```

> -m后面的内容为提交提交文件时的说明，可以为任意内容，但是最后有语义



#### 修改文件内容

1.首先在文件中，修改要修改的内容

2.再次使用add和commit命令，达到修改的目的

**可以使用git status查看仓库状态**

**使用git diff查看修改内容**



#### 版本回退

1.HEAD指向的版本就是当前的版本，我们可以使用git reset --hard  commit_id(版本号的id)回退到指定					    版本

2.可以使用指令回到原来的版本：

```
git reset --hard HEAD^        //回退到上一个版本

git reset --hard HEAD^^      //回退到上上个版本

git reset --hard HEAD~100    //回退到前100个版本
```

>  穿梭前，可以使用**git log**查看提交历史，以便确定要回退到哪个版本
>
> 要重返未来，可以使用**git reflog**产看命令历史，以便确定要回到未来的哪个版本



#### 工作区和暂存区

> 工作区：我们所有在文件上的操作都相当于在仓库的工作区中操作

> 暂存区：当我们完成部分代码后，我们需要将新的版本往Git版本库中添加，这时，我们其实是分两部进行的，首先，使用 **git add** 将文件添加进去，实际上就是把文件添加到暂存区；然后，我们使用**git  commit** 提交修改，实际上就是把暂存区的所有内容提交到当前分支（因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支），所以现在也是在master分支上提交修改



#### 管理修改

一句话概括：如果不用git add将文件添加至暂存区，是不会加入到commit中的，即不会提交到当前分支



#### 撤销修改

1.当文件修改目前是在工作区中时，撤销修改可以使用以下命令：

```
git restore readme.txt
```

2.当文件已经add到暂存区中，则需要使用两次restore命令：

```
首先   git restore --staged readme.txt

然后再次执行一次restore命令     git restore readme.txt
```

3.当文件已经commit到版本库中，则使用版本回退指令



#### 删除文件

1.通常直接在文件管理器中把没用的文件删除，或者使用rm命令删除

2.如果之前已经提交到版本库中，则此时版本库还保存着之前的文件，如果确实要从版本库中删除改文件，就用`git rm test.txt`删除，并且git commit

3.如果是不小心删错了，分为两种情况，第一种是只删除了文件管理器（工作区）中的文件，暂存区中还保存着文件，则使用`git restore test.txt`即可恢复，另外一种情况是暂存区中的文件也没删除，则执行两次restore命令：

```
git restore --staged test.txt

git restore test.txt
```



#### 添加远程库（github）

1.首先在GitHub上面右上角找到“Create a new repo..”，创建一个新的仓库，并未仓库名命名，其他保持默认设置

2.在本地仓库中运行以下命令，使本地仓库与远程GitHub仓库关联：

#### ![63997055497](C:\Users\A\AppData\Local\Temp\1639970554970.png)

> 其中，远程库的名字就是origin，这是Git默认的叫法，其中642342217是我自己的GitHub账号，gitlearn为我的远程仓库名

关联后，如果是第一次，则使用以下命令：

```
git push -u origin master
```

此后，每次本地提交后，可以使用以下命令，将修改后的文件提交至远程仓库：

```
git push origin master
```



#### 远程克隆

只需要一个命令：

```
git clone git@github.com:642342217/gitskills.git    //642342217为GitHub账号，gitskills为仓库名
```



#### 创建与合并分支

在版本回退里，每次提交，Git都把他们串成一条时间线，这条时间线就是一个分支，截止到目前，我们只使用过master分支，每次提交，`master`分支都会向前移动一步，随着不断提交，`master`分支的线也越来越长；

当我们创建新的分支时，例如`dev`，Git会新建一个指针叫`dev`,指向`master`相同的提交（此处我目前的理解是，就是拷贝一份一模一样的文件给dev分支），再把`HEAD`指向`dev`，表示当前分支在`dev`上；（用自己的话来概括就是，HEAD总是指向当前所在的分支）

```
git branch        		    //查看仓库中的分支
git branch <branchName>     //创建分支
git checkout <name>         //切换分支   或者   git switch <name>
git checkout -b <name>      //创建并切换到该分支上
git merge <name>            //合并某分支到当前分支
git branch -d <name>        //删除分支
```



#### 处理冲突

当在新分支（dev）上修改并提交后，返回`master`分支后，如果在`master`分支上继续修改，在合并分支时会造成冲突，此时必须手动解决冲突后在提交。`git status`会告诉我们冲突的文件，我们也可以直接查看`readme.txt`的内容，会标记出不同分支中的内容，修改文件中的冲突内容后，再次提交即可，最后，删除`dev`分支



#### 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息

使用Fast forward模式：

![63998871417](C:\Users\A\AppData\Local\Temp\1639988714173.png)

不使用Fast forward模式：

![63998879035](C:\Users\A\AppData\Local\Temp\1639988790352.png)

合并分支时，加上`--no--ff`参数就可以使用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并



#### Bug分支

当我们正在一个分支上进行开发时，此时街道一个修复bug的任务，但是当前正在`dev`上进行的工作还没有提交，工作时间还需要一天，但是必须在两小时内修复bug，此时，Git提供一个`stash`功能，可以把当前工作现场储藏起来，等 以后恢复现场后继续工作：

```
git stash
```

此时，可以放心的创建分支来修复bug：

1.首先确定要在哪个分支上修复bug，假设`master`分支上，就从master分支上创建临时分支：

```
git checkout master
git checkout -b issue-101
```

2.修复bug后，提交文件，修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支

3.接着回到`dev`分支上，接着干活，通过以下命令查看工作现场：

```
git stash list
```

恢复工作现场：

```
git stash apply       //恢复后，stash内容并不删除，需要使用                       //git stash drop 来删除
git stash pop         //恢复的同时把stash内容也删除了
```

在master分支上修复bug后，其实这个bug在当前dev分支上也是存在的，此时我们只需要把之前提交所做的修改复制到dev分支，为此Git专门提供了一个`cherry-pick`命令

```
git cherry-pick 4c805e2       //每次提交后都有对应的id
```



#### Feature分支

每次开发新功能，都要新建分支，当新的需求不需要此功能时，需要删除该功能，当该分支还没有合并到master分支上时，可以强行删除

```
git branch -D <分支名>    //注意，此处的D为大写
```



#### 多人协作

多人协作工作模式：

1.首先，可以试图用`git push origin <branch -name>推送自己的修改`

2.如果推送失败，则因为远程分支比你的本地更新（也就是小伙伴已经修改并推送过了），需要先用`git pull`尝试合并；

3.如果合并有冲突，则解决冲突，并在本地提交

4.如果没有冲突或者已经解决冲突后，再用`git push origin <branch-name>`推送就能成功。

> 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`；

查看远程库信息：

```
git remote -v
```

在本地创建和远程分支对应的分支：

```
git checkout -b branch-name origin/branch-name
```

建立本地分支和远程分支的关联：

```
git branch --set-upstream branch-name origin/branch-name
```



#### Rebase

rebase操作可以把本地未push的分叉提交历史整理成直线

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比



#### 创建标签

```
git tag <tagname>     用于新建一个标签，默认未HEAD,也可以指                       定一个commit id
git tag -a <tagname> -m "blablabla.."可以指定标签信息
git tag               查看所有标签
```



#### 标签操作

``` 
git push origin <tagname>   推送一个本地标签
git push origin --tags      推送全部未推送过的本地标签
git tag -d <tagname>        删除一个本地标签
git push origin :refs/tags/<tagname> 删除一个远程标签
```

