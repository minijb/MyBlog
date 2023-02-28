# visual studio setup

visual studio中的资源管理器其实就是筛选器！！！不是真实的目录。筛选器是帮助我们分类管理不同文件的，全部删除也没问题

我们可以在资源管理器上方切换视图

主文件目录下我们有两个文件,一个用来存储源代码，一个用来存储输出的文件

![image.png](https://s2.loli.net/2023/01/12/UBY5AMSpDJmqHh6.png)

我们build生成的文件在主文件目录下面的x64文件，这让项目很混乱。

我们可以通过修改"配置属性"--->常规  来修改 (注意我们直接在上方选择所有配置)

输出目录为`$(SolutionDir)\bin\$(Platform)\$(Configuration)\`

bin表示binary

$(Platform) --- 使用的平台 如win43 x64

$(Configuration)--- debug / release

中间目录`$(SolutionDir)\bin\intermediates\$(Platform)\$(Configuration)\`

