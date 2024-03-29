# 使用其他库

## 二进制文件

是一个经过编译的文件，我们不使用源文件，而是使用编译后的文件

 https://www.glfw.org/download.html

现在win32位文件

c++普遍的文件系统----include(头文件),library(二进制文件---动态/静态)

静态链接：库被放入可执行文件中，也就是.exe文件中--更快

动态链接：在运行的时候链接。dll文件

我们需要将链接器指向库文件

库里由三个文件`.dll , .lib , dll.lib`

dll是我们运行时实际使用的动态链接库

dii.lib是我们与dll一起使用的静态库，他包含dll中所有指向functin，以及变量的指针，方便我们在编译的时候链接他们,没有这个文件也可以，但是需要询问每个函数的dll文件

.lib 就是静态链接库

- 修改vs的设置

在c/c++中的常规，添加附加包含目录

例子`$(SolutionDir)\Depandencies\GLFW\include`

- 导入文件

```c++
#include "GLFW/glfw3.h"
```

- 此时我们还没有连接到实际的库上面！！！

我们只是找到了头文件

我们需要在设置中链接器--- input中添加我们的库---静态链接

两种方案

- 在input中添加完整的路径
- 在常规---附加库目录中添加我们之前的链接文件夹---在input中添加需要的库文件(不需要完整目录)

******

我们可以自己定义，但是因为原本的文件是c写的，需要转换名字，我们可以使用

```c++
extern "C" int glfwInit();
```

结果和之前一样。

## 动态链接

一般来说静态链接比动态链接快，有更多优化。

动态链接实在运行的时候才会去找动态链接库，在编写代码的时候没有什么不同

这次在input中添加`glfw3dll.lib`并将glfw3.dll放入执行文件中如果只放入一个就会报错。

## 在vs中创建多个库

- 直接在解决方案中，添加一个空项目，将这个项目的General---coniguration type --- static lib
- 此时我们有两个项目，那么我们如何链接两个项目呢

1. 使用相对路径导入

```c++
#inlcude "../../[xxx]/xxx.h"
```

2. 使用编译器路径

像之前添加静态/动态头文件一样，我们在c/c++中的常规，添加附加包含目录，添加我们需要的头文件目录，之后我们就可以直接include了

```c++
#include "Engine.h"
```

当然可以使用尖括号，但是没必要

****

我们的子项目在build的时候会生成一个lib文件

我们需要做的是在主文件中添加reference(引用)---选中子项目---将lib文件连接到我们主项目中

> 我们build主项目，会自动帮我们编译子项目的
>
> - 因为是静态链接的因此我们可以直接将exe文件单独使用



