# rebase

当我们两个分支都有commit的时候，我们不仅仅可以使用ort，还可以使用rebase，将分支COMMIT复制一份直接加到主分支上，另一个分支同理

![image.png](https://s2.loli.net/2022/12/19/23ndD6XEzIRypHr.png)

![image.png](https://s2.loli.net/2022/12/19/sOEg639vGh1SCq4.png)

千万别在共有项目上使用rebase

![image.png](https://s2.loli.net/2022/12/18/vaQ8CH4AMoBwjck.png)

`git rebsase master`(on the feature branch)

![image.png](https://s2.loli.net/2022/12/19/Zd75S8qui6B1nvl.png)

注意：这里的f2，f1和之前的f2，f1的ID是不同的，也就是说，他创建了一个新的commit，我们相当于修改了别人的代码因此，rebase只能在本地使用！！！！

> 此时我们默认merge 就是fast-formard，仅仅是移动了head

> 简单来说：就是如果我们master不提交，我们仅仅是在branch上提交，我们只有一个list，如果我们两个branch都提交了相当于我们有了分支！！！此时就不能fast-forward了，rebase相当于强制合并为一个list！！！！

## 使用rebase的时机

![image.png](https://s2.loli.net/2022/12/19/LRmAopk5FrWQ3PN.png)