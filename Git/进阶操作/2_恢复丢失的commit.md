# 恢复丢失的commit

有的时候我们可能使用git reset改变了head的位置，此时我们使用`git log`也无法看到之前的node(log指挥显示到head的node信息)。此时我们想要回退可以查看`git reflog`,一个30天回滚的commit操作日志

我们进行如下操作

- create a file
- add and commit
- git reset --hard  to previous
- cheack in reflog
- use git reset to get back

```sh
git reflog


afd8035 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1
891677d HEAD@{1}: commit: two files
afd8035 (HEAD -> master) HEAD@{2}: commit: feature
89c4e72 HEAD@{3}: reset: moving to HEAD
89c4e72 HEAD@{4}: reset: moving to HEAD
89c4e72 HEAD@{5}: reset: moving to HEAD
89c4e72 HEAD@{6}: commit (initial): first file
```

此时我们可以使用前面的hashcode返回到原本的节点

```sh
git reset --hard 891677d  

HEAD is now at 891677d two files

#git log
commit 891677d4dbe04f768a77d2def38c5d348f44995d (HEAD -> master)
Author: minijb <1414309848@qq.com>
Date:   Sat Dec 17 16:42:40 2022 +0800

    two files
```

如果我们删除了branch，此方法同理可以实现恢复

```sh
D:\Project\Learn\git_learn2>git branch -D feature
Deleted branch feature (was 0f4286c).

D:\Project\Learn\git_learn2>git reflog
891677d (HEAD -> master) HEAD@{0}: checkout: moving from feature to master
0f4286c HEAD@{1}: commit: file3.txt
891677d (HEAD -> master) HEAD@{2}: checkout: moving from master to feature
891677d (HEAD -> master) HEAD@{3}: reset: moving to 891677d   
afd8035 HEAD@{4}: reset: moving to HEAD~1
891677d (HEAD -> master) HEAD@{5}: commit: two files
afd8035 HEAD@{6}: commit: feature
89c4e72 HEAD@{7}: reset: moving to HEAD
89c4e72 HEAD@{8}: reset: moving to HEAD
89c4e72 HEAD@{9}: reset: moving to HEAD
89c4e72 HEAD@{10}: commit (initial): first file
```

此时我们使用checkout可以通过分离head的方式回到branch上，同时为它创建head

```sh
#git checkout 0f4286c
Note: switching to '0f4286c'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 0f4286c file3.txt

#git switch -c feature
Switched to a new branch 'feature'

#git branch
* feature
  master
```

