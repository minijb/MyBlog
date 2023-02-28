# Stash

有的时候我们需要离开一段时间，但是我们不想弄脏我们的暂存区和提交区，此时我们可以将文件保存到stash中，他会返回一个hash代码，我们可以随时返回到之前的状态！！！此时我们的head，branch没有改变。

```sh
#git stash
warning: in the working copy of 'file1.txt', LF will be replaced by CRLF the next time Git touches it
Saved working directory and index state WIP on master: 5d5059e “first file
```

此时我们会返回到原本的分支，同时将之前的修改保存到一个列表里面。

```sh
#取出最近的stash
git stash apply

On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file1.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

同理我们可以对此使用stash，我们可以通过`git stash list`来查看存储的stash

```sh
git stash list

stash@{0}: WIP on master: 89c4e72 first file
stash@{1}: WIP on master: 89c4e72 first file
```

0号总是最近存储的数据。使用apply+数字可以切换到指定的stash上

```sh
git stash apply [n]
```

但是如果当前页面的修改没有stash或者提交，那么切换操作会失败。

有的时候stash看起来不太直观，我们可以在添加stash的时候使用message

```sh
git stach push -m "third feature"


#git stash list
stash@{0}: On master: third feature
stash@{1}: WIP on master: 89c4e72 first file
stash@{2}: WIP on master: 89c4e72 first file
```

可以使用pop+n(默认为0)来取出stash,同时删除stash中的对应的node，常用于需要提交对应的stash

```sh
git stash pop 0


On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)  
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file1.txt

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (f03f93ab2cd3343ff8245f2a9fb67792fa59a6e0)

#git stash list
stash@{0}: WIP on master: 89c4e72 first file
stash@{1}: WIP on master: 89c4e72 first file
```

删除stash

```sh
git stash drop [n]
#删除所有stsh
git stash clear
```



