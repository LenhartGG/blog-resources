**Git改写提交**

[toc]

## 1. commit --amend
我们将修改最近一次的提交。

首先进入stepup-tutorial/tutorial1目录。

用log命令确认历史记录。

```javascript
$ git log
commit 326fc9f70d022afdd31b0072dbbae003783d77ed
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:17:56 2012 +0900

    添加add的说明

commit 48eec1ddf73a7fb508ef664efd6b3d873631742f
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:16:14 2012 +0900

    first commit
```

首先打开sample.txt档案，并添加commit的注释。

```javascript
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
```

添加 --amend 选项，然后提交。

```javascript
$ git add sample.txt
$ git commit --amend
```

编辑工具会显示最近一次提交的提交消息，把消息修改为「添加add和commit的讲解」并进行保存。

现在已经修改了提交的内容，然后用log命令确认历史记录和提交消息。

```javascript
$ git log
commit e9d75a02e62814541ee0410d9c1d1bf47ab1c057
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:17:56 2012 +0900

    添加add和commit的讲解

commit 48eec1ddf73a7fb508ef664efd6b3d873631742f
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:16:14 2012 +0900

    first commit
```

## 2. revert

我们将用revert命令来取消「添加pull的讲解」提交。

首先进入stepup-tutorial/tutorial2目录。

用log命令确认历史记录

```javascript
$ git log
commit 0d4a808c26908cd5fe4b6294a00150342d1a58be
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:19:26 2012 +0900

    添加pull的说明

commit 9a54fd4dd22dbe22dd966581bc78e83f16cee1d7
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:19:01 2012 +0900

    添加commit的说明

commit 326fc9f70d022afdd31b0072dbbae003783d77ed
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:17:56 2012 +0900

    添加add的说明

commit 48eec1ddf73a7fb508ef664efd6b3d873631742f
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:16:14 2012 +0900

    first commit
```

打开sample.txt档案，确认内容。

```javascript
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端数据库的内容
```

用revert取消「添加pull的讲解」提交。

```javascript
$ git revert HEAD
[master d47bb1d] Revert "添加pull的说明"
 1 files changed, 1 insertions(+), 2 deletions(-)
```
打开sample.txt档案看看，如果pull的说明消失了，就表明取消提交成功了。

用log命令确认历史记录

```javascript
$ git log
commit 7bcf5e3b6fc47e875ec226ce2b13a53df73cf626
Author: yourname <yourname@yourmail.com>
Date:   Wed Jul 18 15:46:28 2012 +0900

    Revert "添加pull的说明"

    This reverts commit 0d4a808c26908cd5fe4b6294a00150342d1a58be.

commit 0d4a808c26908cd5fe4b6294a00150342d1a58be
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:19:26 2012 +0900

    添加pull的说明

commit 9a54fd4dd22dbe22dd966581bc78e83f16cee1d7
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:19:01 2012 +0900

    添加commit的说明

commit 326fc9f70d022afdd31b0072dbbae003783d77ed
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:17:56 2012 +0900

    添加add的说明

commit 48eec1ddf73a7fb508ef664efd6b3d873631742f
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:16:14 2012 +0900

    first commit
```

## 3. reset
我们将用reset来删除master分支最前面的两个提交。

首先进入stepup-tutorial/tutorial3目录。

用log命令确认历史记录。

```javascript
$ git log
commit 0d4a808c26908cd5fe4b6294a00150342d1a58be
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:19:26 2012 +0900

    添加pull的说明

commit 9a54fd4dd22dbe22dd966581bc78e83f16cee1d7
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:19:01 2012 +0900

    添加commit的说明

commit 326fc9f70d022afdd31b0072dbbae003783d77ed
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:17:56 2012 +0900

    添加add的说明

commit 48eec1ddf73a7fb508ef664efd6b3d873631742f
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:16:14 2012 +0900

    first commit
```

打开sample.txt档案，确认内容。

```javascript
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端数据库的内容
```

用reset删除提交。

```javascript
$ git reset --hard HEAD~~
HEAD is now at 326fc9f 添加add的说明
```

打开sample.txt，看看「添加commit的讲解」和「添加pull的讲解」是否消失了。或者用log命令确认历史记录。

```javascript
$ git log
commit 326fc9f70d022afdd31b0072dbbae003783d77ed
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:17:56 2012 +0900

    添加add的说明

commit 48eec1ddf73a7fb508ef664efd6b3d873631742f
Author: yourname <yourname@yourmail.com>
Date:   Mon Jul 16 23:16:14 2012 +0900

    first commit
                        
```

在reset之前的提交可以参照ORIG_HEAD。Reset错误的时候，在ORIG_HEAD上reset 就可以还原到reset前的状态。

```javascript
$ git reset --hard ORIG_HEAD
HEAD is now at 0d4a808 添加pull的说明
```

## 4. cherry-pick
我们进入stepup-tutorial/tutorial4目录。仅把在其他分支执行的「添加commit的讲解」的修改导入到master分支。

把修改移动到master分支后，用cherry-pick 取出「添加commit的讲解」提交，然后将其添加到master。（文档里的提交"99daed2"和下载到数据库里的提交有可能不相同。在下载的数据库里执行git log，确认是正确的提交之后再使用。）

```javascript
$ git checkout master
Switched to branch 'master'
$ git cherry-pick 99daed2
error: could not apply 99daed2... commit
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'
```

如果发生冲突，就打开sample.txt，修改冲突的部分之后再提交。

```javascript
$ git add sample.txt
$ git commit
```

## 5. 用rebase -i 汇合提交
我们进入stepup-tutorial/tutorial5目录。在这里汇合「添加commit的讲解」和「添加pull的讲解」的修改，然后合并到一个提交。

若要汇合过去的提交，请用rebase -i。

```javascript
$ git rebase -i HEAD~~
```

打开文本编辑器，将看到从HEAD到HEAD~~的提交如下图显示。

```
pick 9a54fd4 添加commit的说明
pick 0d4a808 添加pull的说明

# Rebase 326fc9f..0d4a808 onto d286baa
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
```

将第二行的“pick”改成“squash”，然后保存并退出。由于合并后要提交，所以接着会显示提交信息的编辑器，请编辑信息后保存并退出。

这样，两个提交就合并成一个提交了。请用log命令确认历史记录。

## 6. 用rebase -i 修改提交
我们进入stepup-tutorial/tutorial6目录。我们在这里修改「添加commit的讲解」的内容。

用rebase -i ，首先选择要修改的提交。

```javascript
$ git rebase -i HEAD~~
```

打开文本编辑器，将看到从HEAD到HEAD~~的提交如下图显示。

```
pick 9a54fd4 添加commit的说明
pick 0d4a808 添加pull的说明

# Rebase 326fc9f..0d4a808 onto d286baa
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
```

将第一行的“pick”改成“edit”，然后保存并退出。将会显示以下内容，修改过的提交呈现退出状态。

```
Stopped at d286baa... 添加commit的说明
You can amend the commit now, with

        git commit --amend

Once you are satisfied with your changes, run

        git rebase --continue
```

打开sample.txt，适当地修改“commit的讲解”部分。

```
连猴子都懂的Git命令
add 把变更录入到索引中
commit 记录索引的状态
pull 取得远端数据库的内容
```

用commit --amend保存修改。

```
$ git add sample.txt
$ git commit --amend
```

现在已经commit，但是rebase操作还没结束。若要通知这个提交的操作已经结束，请指定 --continue选项执行rebase。

```
$ git rebase --continue
```

>这时，有可能其他提交会发生冲突, 请修改冲突部分后再执行add和rebase --continue。这时不需要提交。如果在中途要停止rebase操作，请在rebase指定--abort选项执行，这样就可以抹去并停止在rebase的操作。

提交的修改完成了。如果要把多个提交修改成edit，下一个要修改的提交会退出，请执行同样的修改。

>实际上，在rebase之前的提交会以ORIG_HEAD之名存留。如果rebase之后无法复原到原先的状态，可以用git reset --hard ORIG_HEAD复原到rebase之前的状态。

## 7. merge --squash
我们移动到stepup-tutorial/tutorial7目录。把issue1分支的所有提交合并成一个提交，并导入到master分支。

切换到master分支后，指定 --squash选项执行merge。

```javascript
$ git checkout master
Switched to branch 'master'
$ git merge --squash issue1
Auto-merging sample.txt
CONFLICT (content): Merge conflict in sample.txt
Squash commit -- not updating HEAD
Automatic merge failed; fix conflicts and then commit the result.
```

看来发生冲突了。请打开sample.txt，修改冲突的部分，然后提交。

```javascript
$ git add sample.txt
$ git commit
[master 0d744a7] Conflicts:   sample.txt
 1 files changed, 4 insertions(+), 0 deletions(-)
issue1分支上所有的提交都汇合并添加到master分支了。请用log命令确认历史记录。
```