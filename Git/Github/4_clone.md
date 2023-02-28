# clone

clone之后的分支

```sh
#git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/feature
  remotes/origin/feature-remote
  remotes/origin/master
  
#git branch -vv
* master 32a4a45 [origin/master] m2
```

可以看到一个local-tracking-branch

我们可以为它创建更多的local-tracking-branch

```sh
#git branch --track origin/feature remotes/origin/feature
branch 'origin/feature' set up to track 'origin/feature'.

#git branch -vv
* master         32a4a45 [origin/master] m2
  origin/feature efdab37 [remotes/origin/feature] f1
```

我们创建一个新的分支, 他并不是一个local tracking 

```sh
#git push origin feature-local
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 274 bytes | 274.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'feature-local' on GitHub by visiting:
remote:      https://github.com/minijb/learn/pull/new/feature-local
remote:
To https://github.com/minijb/learn.git
 * [new branch]      feature-local -> feature-local

#git branch -vv
* feature-local  7a7b582 file1
  master         32a4a45 [origin/master] m2
  origin/feature efdab37 [remotes/origin/feature] f1
```

我们可以通过一下方法将他将他变为local tracking 

```sh
#git switch master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

#git branch -D feature-local
Deleted branch feature-local (was 7a7b582).

#git branch --track feature-local origin/feature-local 
branch 'feature-local' set up to track 'origin/feature-local'.

#git branch -vv
  feature-local  7a7b582 [origin/feature-local] file1
* master         32a4a45 [origin/master] m2
  origin/feature efdab37 [remotes/origin/feature] f1

```

