# hand conflict

当我们两个branch上都有commit此时我们可以并可以会遇到冲突的问题。

此时

```sh
#git merge feature   
Auto-merging feature/f1.txt
CONFLICT (content): Merge conflict in feature/f1.txt
Automatic merge failed; fix conflicts and then commit the result.
```

```txt
<<<<<<< HEAD
changed in master
=======
change on feature branch
>>>>>>> feature
```

可以通过status查看当前状态

```sh
#git status  
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   feature/f1.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以使用`git merge --abort`来放弃这次合并，回到之前的状态

使用`git diff`可以查看我们需要修改冲突

**修改好冲突之后**我们需 要add,commit创建一个新的节点接受本次修改！！！

![image.png](https://s2.loli.net/2022/12/19/n5i4duJqFREaosl.png)