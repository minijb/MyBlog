#  delete remote branch

```sh
#git branch -vv
  feature        efdab37 f1
* feature-remote a9e2063 [origin/feature-remote] file3
  master         32a4a45 [origin/master] m2

#git ls-remote
From https://github.com/minijb/learn.git
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        HEAD
efdab37d81c986409348ac5fca97e8a8df30e1ec        refs/heads/feature
7a7b5829003964584a5dd84fe6e8ac2e8f0387cc        refs/heads/feature-local
a9e20637e986274186160fcd8cf318e5780f8300        refs/heads/feature-remote
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        refs/heads/feature-upstream
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        refs/heads/master
```

我们通过fetch同步

```sh
#git fetch origin

#git branch -a
  feature
* feature-remote
  master
  remotes/origin/feature
  remotes/origin/feature-local
  remotes/origin/feature-remote
  remotes/origin/feature-upstream
  remotes/origin/master

```

此时我们可以删除feature，但是remote tracking branch 没有删除,使用`git branch -D [remote-branch]`--->error

我们应该使用如下命令

```sh
#git branch --delete --remote origin/feature
Deleted remote-tracking branch origin/feature (was efdab37).
```

但是此时远程仓库中还有次分支

```sh
#git ls-remote
From https://github.com/minijb/learn.git
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        HEAD
efdab37d81c986409348ac5fca97e8a8df30e1ec        refs/heads/feature
7a7b5829003964584a5dd84fe6e8ac2e8f0387cc        refs/heads/feature-local
a9e20637e986274186160fcd8cf318e5780f8300        refs/heads/feature-remote
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        refs/heads/feature-upstream
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        refs/heads/master

```

我们可以通过如下命令删除远程仓库中的对应分支

```sh
#git push origin --delete feature
To https://github.com/minijb/learn.git
 - [deleted]         feature
 
#git ls-remote
From https://github.com/minijb/learn.git
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        HEAD
7a7b5829003964584a5dd84fe6e8ac2e8f0387cc        refs/heads/feature-local
a9e20637e986274186160fcd8cf318e5780f8300        refs/heads/feature-remote
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        refs/heads/feature-upstream
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        refs/heads/master
```

如果我们直接删除远程仓库上的分支，remote tracking branch也会删除

```sh
#git push origin --delete feature-remote
To https://github.com/minijb/learn.git
 - [deleted]         feature-remote

#git branch -r
  origin/feature-local
  origin/feature-upstream
  origin/master
```

## undo commit 然后 push

我们回退了commit然后push，那么对于远程仓库中的其他任，会造成影响，因此会有warning

```sh
#git reset --hard HEAD~1
HEAD is now at 2c34201 m1

#git push origin master
To https://github.com/minijb/learn.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/minijb/learn.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

也就是说，我们提交的东西在远程仓库的后面(树结构的后面！！！)，远程仓库会丢失东西！！！

我们可以使用`--force`强制执行

```sh
#git push --force origin master
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/minijb/learn.git
 + 32a4a45...2c34201 master -> master (forced update)

#git pull origin master
From https://github.com/minijb/learn
 * branch            master     -> FETCH_HEAD
Already up to date.
```

![image.png](https://s2.loli.net/2022/12/21/6pBu8DGFnkiTchS.png)

简单来说就是每次提交的节点推荐在远程节点的后面！！！

![image.png](https://s2.loli.net/2022/12/21/8CiMNYFfD1JnuR9.png)
