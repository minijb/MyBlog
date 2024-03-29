# cherry-pick

这时分两种情况。一种情况是，你需要另一个分支的所有代码变动，那么就采用合并（`git merge`）。另一种情况是，你只需要部分代码变动（某几个提交），这时可以采用 Cherry pick。

![image.png](https://s2.loli.net/2022/12/19/kGlSnMiArDNH3mO.png)

```sh
#git cherry-pick ea0ee38                
[master f8c477c] type in m1 fix
 Date: Mon Dec 19 22:02:34 2022 +0800
 1 file changed, 1 insertion(+), 1 deletion(-)

D:\Project\Learn\branches>git reset --hard HEAD~1
HEAD is now at d3dcc61 add in m1
```

此时我们复制的一个commit到master branch中，注意两者id不同！！！！

![image.png](https://s2.loli.net/2022/12/19/IjRtKAVlWZq9Dr4.png)

Cherry pick 支持一次转移多个提交。

```sh
git cherry-pick <HashA> <HashB>
```

上面的命令将 A 和 B 两个提交应用到当前分支。这会在当前分支生成两个对应的新提交。

如果想要转移一系列的连续提交，可以使用下面的简便语法。

```sh
git cherry-pick A..B 
```

上面的命令可以转移从 A 到 B 的所有提交。它们必须按照正确的顺序放置：提交 A 必须早于提交 B，否则命令将失败，但不会报错。

注意，使用上面的命令，提交 A 将不会包含在 Cherry pick 中。如果要包含提交 A，可以使用下面的语法。

```sh
git cherry-pick A^..B 
```

如果有冲突，我们就需要自己解决之后进行提交