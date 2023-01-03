# tag

用来标记重要的阶段(commit)

- light tag轻量级tag---指针指向commit--临时版本
- 注释标签--带有信息的标签

命令

```sh
git tag#查看所有tag
git show [tag name]#查看tag对应的commit信息

git taf -d [tagname]
#light
git tag [name] [hash]#打标签

#有注释的
git tag -a [name] [hash] -m [messge]

#没有hash默认在当前commit上打标签
```

我们可以在checkout中的使用tag代替hash

