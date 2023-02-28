## 理解多出来的分支

多出来的分支是远程分支的副本！！！，代表远程仓库中对用的分支的位置

当我们push的时候，我们就知道了此时远程仓库中master的位置！！这对用着多出来的分支

![image.png](https://s2.loli.net/2022/12/20/4Lprxalem7TIGEy.png)

使用`git remote -r`查看远程分支,**远程分支是不可以编辑的，我们checkout进去也是分离HEAD**

我们创建并push一个分支

```sh
#git push origin feature
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (4/4), 300 bytes | 300.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'feature' on GitHub by visiting:
remote:      https://github.com/xxx/learn/pull/new/feature
remote:
To https://github.com/minijb/learn.git
 * [new branch]      feature -> feature
 
 
#git branch -a
* feature
  master
  remotes/origin/feature
  remotes/origin/master
```

我们同样可以在远程仓库中创建branch，在远处的操作不会立即同步到本地

我们可以通过`git ls-remote`查看远程的branch

```sh
#git ls-remote
From https://github.com/xxx/learn.git
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        HEAD
efdab37d81c986409348ac5fca97e8a8df30e1ec        refs/heads/feature
946d1460245e4502f294751a63bdc8e2d27b75c6        refs/heads/feature-remote
32a4a450dbd1dcfc79e861f6cef7832ea1accf72        refs/heads/master
```

改变的远程分支---两种方法`git pull/fetch`

如果我们仅仅想要更新远程分支的状态而不希望改变当前工作目录的状态，我们使用fetch

```sh
#git fetch origin 

#git branch -a
* feature
  master
  remotes/origin/feature
  remotes/origin/feature-remote
  remotes/origin/master
```

git pull是fetch和merge的合体

```sh
#git pull origin master                                 
From https://github.com/minijb/learn
 * branch            master     -> FETCH_HEAD
Already up to date.
```

fetch之后，我们可以进入远程HEAD确认是否正确,注：远程头是只读的

