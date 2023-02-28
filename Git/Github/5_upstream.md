# upstream

默认的push不会为我们创建local tracking，但是使用upstream则会将当前分支变为local tracking！！！

```sh
#git push -u origin feature-upstream 
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote: 
remote: Create a pull request for 'feature-upstream' on GitHub by visiting:
remote:      https://github.com/minijb/learn/pull/new/feature-upstream
remote:
To https://github.com/minijb/learn.git
 * [new branch]      feature-upstream -> feature-upstream
 #***************************************
branch 'feature-upstream' set up to track 'origin/feature-upstream'.
 #***************************************
 
#git branch -vv
  feature-local    7a7b582 [origin/feature-local] file1
* feature-upstream 32a4a45 [origin/feature-upstream] m2
  master           32a4a45 [origin/master] m2
  origin/feature   efdab37 [remotes/origin/feature] f1
```

