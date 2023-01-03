# tracking branch

tracking branch---追踪分支---用来交换信息的

分为两种 1：remote tracking branch 2: local tracking branch

remote tracking branch ---远程分支的副本，只读

local tracking branch--- 本地索引，索引的对象是remote tracking branch(并非一定的)---是可以被编译的---可以使用push pull

**基本的逻辑**

- remote tracking branch 从远程仓库中得到信息
- local tracking branch 引用 remote tracking branch，得到最新的变化
- 我们修改local tracking branch
- push local tracking branch  到远程仓库

![image.png](https://s2.loli.net/2022/12/20/JpI3YynHmCTZMsw.png)

<img src="https://s2.loli.net/2022/12/20/Oq9mXNGIPEJKwaY.png" alt="image.png" style="zoom:50%;" />

## 创建本地跟踪分支

```sh
#git branch --track feature-remote-local origin/feature-remote
branch 'feature-remote-local' set up to track 'origin/feature-remote'.

#git branch -a
  feature
  feature-remote-local
* master
  remotes/origin/feature
  remotes/origin/feature-remote
  remotes/origin/master


#git switch feature-remote-local
Switched to branch 'feature-remote-local'
Your branch is up to date with 'origin/feature-remote'.

#git add .

#git commit -m "added in local tracking"
[feature-remote-local d725a3d] added in local tracking
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 local-tracking.txt

D:\Project\Learn\remote>git push               
fatal: The upstream branch of your current branch does not match
the name of your current branch.  To push to the upstream branch
on the remote, use

    git push origin HEAD:feature-remote

To push to the branch of the same name on the remote, use

    git push origin HEAD

To choose either option permanently, see push.default in 'git help config'.

To avoid automatically configuring upstream branches when their name
doesn't match the local branch, see option 'simple' of branch.autoSetupMerge
in 'git help config'.
```

此时我们可以直接`git push`因为我们在一个封闭的环境中，他知道remote是谁，但是我们会失败，因为当前branch的名称和远程不同。

```sh
#git branch -D feature-remote-local
Deleted branch feature-remote-local (was d725a3d).

#git branch --track feature-remote origin/feature-remote
branch 'feature-remote' set up to track 'origin/feature-remote'.

#git branch -a
* feature
  feature-remote
  master
  remotes/origin/feature
  remotes/origin/feature-remote
  remotes/origin/master
  
#add , commit

#git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 328 bytes | 328.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/minijb/learn.git
   3230822..68ea961  feature-remote -> feature-remote
```

此时我们可以直接git push，当然在local-tracking上我们可以直接pull

```sh
#git pull
Updating 68ea961..a9e2063
Fast-forward
 file3.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 file3.txt

```

我们可以通过branch命令查看branch分支，

```sh
#普通的git branch无法通过颜色区分，白色为普通分支，绿色选中的branch ,红色为remote-tracking

#使用git branch -vv可以看看更多信息 -r查看远程分支
#git branch -vv
  feature        efdab37 f1
* feature-remote a9e2063 [origin/feature-remote] file3
  master         32a4a45 [origin/master] m2
```

![image.png](https://s2.loli.net/2022/12/21/3Qk1Vd2Dl5GZwsM.png)

