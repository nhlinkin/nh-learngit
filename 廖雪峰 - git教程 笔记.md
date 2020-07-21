# 廖雪峰 - git教程



#### 2. 时光机穿梭



##### 2.4 撤销修改

```
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作，用命令git checkout -- file。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
```

##### 2.5 删除文件

```
创建一个test.txt

查看当前git状态
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.txt

nothing added to commit but untracked files present (use "git add" to track)

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$

提交代码
$ git add test.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   test.txt


Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$

$ git commit -m "add test.txt"
[master 3cb23d2] add test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git status
On branch master
nothing to commit, working tree clean

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$
```

```
现在删除test.txt
$ rm test.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ ls
LICENSE  readme.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$
```

```
现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

```
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

或
$ git restore test.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ ls
LICENSE  readme.txt  test.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$
```



### 3. 远程仓库

```
在windows中创建ssh秘钥
$ ssh-keygen -t rsa -C "nhlinkin@163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator.WINMICR-4SP7KKJ/.ssh/id_rsa):
Created directory '/c/Users/Administrator.WINMICR-4SP7KKJ/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator.WINMICR-4SP7KKJ/.ssh/id_rsa
Your public key has been saved in /c/Users/Administrator.WINMICR-4SP7KKJ/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:Bb7NrUbShFrEiHOOexKFSOZ0rcJ7DifE6pgO2EtSQ28 nhlinkin@163.com
The key's randomart image is:
+---[RSA 3072]----+
| .+..+ oo        |
| +..+ =o.o       |
| oo  B  + o      |
| .=.+ .o B .     |
| oooEo. S = .    |
|oo+o= .  o .     |
|*.o* o    o      |
|++ ..    .       |
|...              |
+----[SHA256]-----+

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ ls
LICENSE  readme.txt  test.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$
```

在C:\Users\Administrator\ .ssh目录下就会生成2个文件：id_rsa和id_rsa.pub

然后登录自已的github.com，添加id_rsa.pub公钥的内容到SSH Keys中。



##### 3.1 添加远程库

在自已的github上创建远程库 learngit

```
与自已的git目录建立关联，添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ pwd
/d/git/learngit
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git remote add origin git@github.com:nhlinkin/learngit.git

下一步，就可以把本地库的所有内容推送到远程库上：
$ git push -u origin master
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
Enumerating objects: 18, done.
Counting objects: 100% (18/18), done.
Delta compression using up to 2 threads
Compressing objects: 100% (13/13), done.
Writing objects: 100% (18/18), 1.50 KiB | 139.00 KiB/s, done.
Total 18 (delta 3), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (3/3), done.
To github.com:nhlinkin/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$

把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
```

```
小结

要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！
```



##### 3.2 从远程库克隆

```
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git
$ git clone git@github.com:nhlinkin/gitskills.git
Cloning into 'gitskills'...
Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git
$ cd gitskills/

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/gitskills (master)
$ ls
README.md

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/gitskills (master)
$
```



### 4 分支管理



##### 4.1 创建与合并分支

```
首先，我们创建dev分支，然后切换到dev分支：
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git checkout -b dev
Switched to a new branch 'dev'

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (dev)
$

git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

```
然后，用git branch命令查看当前分支：
$ git branch
* dev
  master
  
git branch命令会列出所有分支，当前分支前面会标一个*号。
```

```
然后，我们就可以在dev分支上正常提交，比如对readme.txt做个修改，加上一行Creating a new branch is quick.：
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Creating a new branch is quick.
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (dev)
$

然后提交：
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (dev)
$ git add readme.txt

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (dev)
$ git commit -m 'branch test'
[dev 44f8a20] branch test
 1 file changed, 2 insertions(+), 1 deletion(-)
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (dev)
$ git status
On branch dev
nothing to commit, working tree clean
```

```
现在，dev分支的工作完成，我们就可以切换回master分支：
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (dev)
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$

切换回master分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$
```

```
现在，我们把dev分支的工作成果合并到master分支上：
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git merge dev
Updating 3cb23d2..44f8a20
Fast-forward
 readme.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$

查看readme.txt,现在可以看到在dev分支上添加的内容了：
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Creating a new branch is quick.
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$
```

```
git merge命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。

注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。

当然，也不是每次合并都能Fast-forward，我们后面会讲其他方式的合并。
```

```
合并完成后，就可以放心地删除dev分支了：
Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git branch
  dev
* master

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$ git branch -d dev
Deleted branch dev (was 44f8a20).

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$

删除后，查看branch，就只剩下master分支了：
$ git branch
* master

Administrator@WINMICR-4SP7KKJ MINGW64 /d/git/learngit (master)
$
```

```
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。
```

> ### switch

我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。

实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：

```
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```
$ git switch master
```

使用新的`git switch`命令，比`git checkout`要更容易理解。

```
小结

Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>或者git switch <name>

创建+切换分支：git checkout -b <name>或者git switch -c <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>
```

































































































































































