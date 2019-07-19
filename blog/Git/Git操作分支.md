**Git操作分支**

## 0. 事前预备

首先建立一个新目录，并在里面建立一个空数据库。这里我们创建一个名为tutorial的目录。

```
$ mkdir tutorial
$ cd tutorial
$ git init
Initialized empty Git repository in /Users/eguchi/Desktop/tutorial/.git/
```

在tutorial目录创建一个名为myfile.txt的档案，然后提交。

```
连猴子都懂的Git命令
```

```
$ git add myfile.txt
$ git commit -m "first commit"
[master (root-commit) a73ae49] first commit
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 myfile.txt
```

## 1. 建立分支
创建名为issue1的分支。

您可以通过branch命令来创建分支。

```
$ git branch <branchname>
```

创建名为issue1的分支。

```
$ git branch issue1
```

不指定参数直接执行branch命令的话，可以显示分支列表。 前面有*的就是现在的分支。

```
$ git branch
  issue1
* master
```

## 2. 切换分支
若要在新建的issue1分支进行提交，需要切换到issue1分支。

要执行checkout命令以退出分支。

```
$ git checkout <branch>
```

切换到issue1分支。

```
$ git checkout issue1
Switched to branch 'issue1'
```

>在checkout命令指定 -b选项执行，可以创建分支并进行切换。
    
```
$ git checkout -b <branch>
```

在切换到issue1分支的状态下提交，历史记录会被记录到issue1分支。在myfile.txt添加add命令的说明后再提交。

```
连猴子都懂的Git命令
add 把变更录入到索引中
```

```
$ git add myfile.txt
$ git commit -m "添加add的说明"
[issue1 b2b23c4] 添加add的说明
 1 files changed, 1 insertions(+), 0 deletions(-)
```

## 3. 合并分支
向master分支合并issue1分支的修改。

执行merge命令以合并分支。

```
$ git merge <commit>
```

该命令将指定分支导入到HEAD指定的分支。先切换master分支，然后把issue1分支导入到master分支。
```
$ git checkout master
Switched to branch 'master'
```

打开myfile.txt档案以确认内容，然后提交。

```
连猴子都懂的Git命令
```

已经在issue1分支进行了编辑上一页的档案，所以master分支的myfile.txt的内容没有更改。

```
$ git merge issue1
Updating 1257027..b2b23c4
Fast-forward
 myfile.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```

master分支指向的提交移动到和issue1同样的位置。这个是fast-forward（快进）合并。

打开myfile.txt档案，确认内容。

```
连猴子都懂的Git命令
add 把变更录入到索引中
```

已添加「add 把变更录入到索引中」。

## 4. 删除分支

既然issue1分支的内容已经顺利地合并到master分支了，现在可以将其删除了。

在branch命令指定-d选项执行，以删除分支。

```
$ git branch -d <branchname>
```
执行以下的命令以删除issue1分支。

```
$ git branch -d issue1
Deleted branch issue1 (was b2b23c4).
```
issue1分支被删除了。您可以用branch命令来确认分支是否已被删除。

```
$ git branch
* master
```

## 5. 并行操作
接下来，创建2个分支来尝试并行操作吧。

首先创建issue2分支和issue3分支，并切换到issue2分支。

```
$ git branch issue2
$ git branch issue3
$ git checkout issue2
Switched to branch 'issue2'
$ git branch
* issue2
  issue3
  master
```

在issue2分支的myfile.txt添加commit命令的说明后提交。

```
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
```

```
$ git add myfile.txt
$ git commit -m "添加commit的说明"
[issue2 8f7aa27] 添加commit的说明
 1 files changed, 2 insertions(+), 0 deletions(-)
```

接着，切换到issue3分支。

```
$ git checkout issue3
Switched to branch 'issue3'
```

打开myfile.txt档案。由于在issue2分支添加了commit命令的说明，所以issue3分支的myfile.txt里只有add命令的说明。

添加pull命令的说明后提交。

```
连猴子都懂的Git命令
add 把变更录入到索引中
pull 取得远端数据库的内容
```

```
$ git add myfile.txt
$ git commit -m "添加pull的说明"
[issue3 e5f91ac] 添加pull的说明
 1 files changed, 2 insertions(+), 0 deletions(-)
```

这样，添加commit的说明的操作，和添加pull的说明的操作就并行进行了。

## 6. 解决合并的冲突
把issue2分支和issue3分支的修改合并到master。

切换master分支后，与issue2分支合并。

```
$ git checkout master
Switched to branch 'master'
$ git merge issue2
Updating b2b23c4..8f7aa27
Fast-forward
 myfile.txt |    2 ++
 1 files changed, 2 insertions(+), 0 deletions(-)
```

执行fast-forward（快进）合并。

接着合并issue3分支。

```
$ git merge issue3
Auto-merging myfile.txt
CONFLICT (content): Merge conflict in myfile.txt
Automatic merge failed; fix conflicts and then commit the result.
```

自动合并失败。由于在同一行进行了修改，所以产生了冲突。这时myfile.txt的内容如下：

```
连猴子都懂的Git命令
add 把变更录入到索引中
<<<<<< HEAD
commit 记录索引的状态
=======
pull 取得远端数据库的内容
>>>>>> issue3
```

在发生冲突的地方，Git生成了内容的差异。请做以下修改：
```
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端数据库的内容
```

修改冲突的部分，重新提交。
```
$ git add myfile.txt
$ git commit -m "合并issue3分支"
# On branch master
nothing to commit (working directory clean)
```

历史记录如下图所示。因为在这次合并中修改了冲突部分，所以会重新创建合并修改的提交记录。这样，master的HEAD就移动到这里了。这种合并不是fast-forward合并，而是non fast-forward合并。

## 7. 用rebase合并
合并issue3分支的时候，使用rebase可以使提交的历史记录显得更简洁。

现在暂时取消刚才的合并。

```
$ git reset --hard HEAD~
```

切换到issue3分支后，对master执行rebase。

```
$ git checkout issue3
Switched to branch 'issue3'
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: 添加pull的说明
Using index info to reconstruct a base tree...
<stdin>:13: new blank line at EOF.
+
warning: 1 line adds whitespace errors.
Falling back to patching base and 3-way merge...
Auto-merging myfile.txt
CONFLICT (content): Merge conflict in myfile.txt
Failed to merge in the changes.
Patch failed at 0001 添加pull的说明

When you have resolved this problem run "git rebase --continue".
If you would prefer to skip this patch, instead run "git rebase --skip".
To check out the original branch and stop rebasing run "git rebase --abort".
```

和merge时的操作相同，修改在myfile.txt发生冲突的部分。

```
连猴子都懂的Git命令
add 把变更录入到索引中
<<<<<< HEAD
commit 记录索引的状态
=======
pull 取得远端数据库的内容
>>>>>> issue3
```

rebase的时候，修改冲突后的提交不是使用commit命令，而是执行rebase命令指定 --continue选项。若要取消rebase，指定 --abort选项。

```
$ git add myfile.txt
$ git rebase --continue
Applying: 添加pull的说明
```

这样，在master分支的issue3分支就可以fast-forward合并了。切换到master分支后执行合并。

```
$ git checkout master
Switched to branch 'master'
$ git merge issue3
Updating 8f7aa27..96a0ff0
Fast-forward
 myfile.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```

myfile.txt的最终内容和merge是一样的。