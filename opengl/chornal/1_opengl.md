# Opengl

本身是一个规范，不是一个库，具体api是由显卡厂商决定的。

opengl是跨平台的

可能底层是opengl，但是规则是direct3d

## opengl安装及下载

opengl会自动帮我们封装一些底层的api如创建窗口----此处我们使用glfw，这个比较轻量化。

链接include文件夹，使用静态链接

> 注意：添加以下库文件`opengl32.lib;`

复制以下代码进行测试

```c++
#include <GLFW/glfw3.h>

int main(void)
{
    GLFWwindow* window;

    /* Initialize the library */
    if (!glfwInit())
        return -1;

    /* Create a windowed mode window and its OpenGL context */
    window = glfwCreateWindow(640, 480, "Hello World", NULL, NULL);
    if (!window)
    {
        glfwTerminate();
        return -1;
    }

    /* Make the window's context current */
    glfwMakeContextCurrent(window);

    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window))
    {
        /* Render here */
        glClear(GL_COLOR_BUFFER_BIT);

        glBegin(GL_TRIANGLES);
        glVertex2f(-0.5f,-0.5f);
        glVertex2f(0.0f,0.5f);
        glVertex2f(0.5f,-0.5f);
        glEnd();

        /* Swap front and back buffers */
        glfwSwapBuffers(window);

        /* Poll for and process events */
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```

## 通过c++操作opengl

获得新的opengl函数，简单来说就是底层是提供对opengl的支持的，我们需要的是连接这些函数。

windows只给了老版本的opengl函数，我们需要找到新的函数。这些库是提供opengl api规范的，比如函数的声明，符号的声明，常量等等等。这些库文件仅仅是访问不是实现。实际实现的库是`glue/glew  opengl extension wrangler  glad`

下载我们去安装glew：https://glew.sourceforge.net/

注意点：

First you need to create a valid OpenGL rendering context and call `glewInit()` to initialize the extension entry points. If `glewInit()` returns `GLEW_OK`, the initialization succeeded and you can use the available extensions as well as core OpenGL functionality. For example:

还有一个注意点--- 因为我们使用的是静态库，因此我们需要定义`GLEW_STATIC`

此时我们调用`glewInit`是失败的，因为没有OpenGL rendering context

```c++
if(glewInit() != GLEW_OK)
{
    std::cout << "Error! --- glewInit failed!" << std::endl;
}
```

`glfwMakeContextCurrent(window);`这一句才是创建glfw上下文的语句

我们将他放到`glewInit`之前就可以了。注意---window还没有定义，定义语句一起放上去

成功！！！！

## 顶点缓冲区 and shader

在现代opengl中，画一个三角形我们需要做两件事

- 创建顶点缓冲区 --- vertex buffer ---- 就是一个buffer --- 在显存里

- 使用着色器---shader

注意---c++是运行在cpu上的。我们需要通过cpu告诉gpu如何读取和解释缓冲区里的数据，并放到屏幕上。

对显卡编程 ---- 这就是shader

opengl的具体操作就是一个状态机 --- 不是对象或数据

简单来说就是一个传输状态的脚本(给gpu发布任务) --- 我让gpu使用这个buffer并使用这个shader来渲染，之后gpu根据任务画图形。

#### 创建buffer

> 文档 docs.gl

```c++
int main(void)
{
    GLFWwindow* window;

    /* Initialize the library */
    if (!glfwInit())
        return -1;
    /* Create a windowed mode window and its OpenGL context */
    window = glfwCreateWindow(640, 480, "Hello World", NULL, NULL);
    if (!window)
    {
        glfwTerminate();
        return -1;
    }


    /* Make the window's context current */
    glfwMakeContextCurrent(window);

    if(glewInit() != GLEW_OK)
    {
        std::cout << "Error! --- glewInit failed!" << std::endl;
    }

    unsigned int buffer;
    float postions[] = {
    	-0.5f,-0.5f,
    	0.0f,0.5f,
    	0.5f,-0.5f
    };

    glGenBuffers(1, &buffer);
    //因为是状态机---因此我们可以使用数字来表示对象---id
    glBindBuffer(GL_ARRAY_BUFFER, buffer);
    //指定数据
    glBufferData(GL_ARRAY_BUFFER, 6 * sizeof(float), postions, GL_STATIC_DRAW);
    //我们并没有具体告诉gl数据是什么
    //我们需要告诉gl我们的数据是如何布局的


    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window))
    {
        /* Render here */
        glClear(GL_COLOR_BUFFER_BIT);

        // glBegin(GL_TRIANGLES);
        // glVertex2f(-0.5f,-0.5f);
        // glVertex2f(0.0f,0.5f);
        // glVertex2f(0.5f,-0.5f);
        // glEnd();

        glDrawArrays(GL_TRIANGLES, 0, 3);
        // glDrawElements(GL_TRIANGLES, 3, );//当有索引缓冲区的时候使用

        /* Swap front and back buffers */
        glfwSwapBuffers(window);

        /* Poll for and process events */
        glfwPollEvents();
    }

    glfwTerminate();
    return 0;
}
```

GPU如何知道使用哪个buffer呢？----- 就是我们bind的数据

有趣的例子： photoshop --- 我们选择了一个画层(bind) ,然后在画层上作画。如果选错了就一团糟了

这个是上下文相关的。----- 状态机

## 顶点的属性和布局

我们之前存储了缓存，但是GPU并不知道具体的缓存布局，我们需要告诉GPU我们的数据布局。

顶点和位置无关，就是几何图形上的一个点。顶点不仅仅包含位置，纹理坐标，法线位置，颜色。。。。

```c++
void glVertexAttribPointer(	GLuint index,
GLint size,
GLenum type,
GLboolean normalized,
GLsizei stride,
const GLvoid * pointer);
/*
index
Specifies the index of the generic vertex attribute to be modified.

size --- 我们是2为坐标，因此为2
Specifies the number of components per generic vertex attribute. Must be 1, 2, 3, 4. Additionally, the symbolic constant GL_BGRA is accepted by glVertexAttribPointer. The initial value is 4.

type
Specifies the data type of each component in the array. The symbolic constants GL_BYTE, GL_UNSIGNED_BYTE, GL_SHORT, GL_UNSIGNED_SHORT, GL_INT, and GL_UNSIGNED_INT are accepted by glVertexAttribPointer and glVertexAttribIPointer. Additionally GL_HALF_FLOAT, GL_FLOAT, GL_DOUBLE, GL_FIXED, GL_INT_2_10_10_10_REV, GL_UNSIGNED_INT_2_10_10_10_REV and GL_UNSIGNED_INT_10F_11F_11F_REV are accepted by glVertexAttribPointer. GL_DOUBLE is also accepted by glVertexAttribLPointer and is the only token accepted by the type parameter for that function. The initial value is GL_FLOAT.

normalized---是否需要显卡帮忙把数据归一化到-1到+1区间,可以自己做可以让opengl来做
stride --- 一个顶点占有的总的字节数 --- 步长，也就是多个属性之间的步长 也就是想数组一样进行计算位置
Specifies the byte offset between consecutive generic vertex attributes. If stride is 0, the generic vertex attributes are understood to be tightly packed in the array. The initial value is 0.

pointer：当前指针指向的vertex内部的偏离字节数，可以唯一的标识顶点某个属性的偏移量
这里是指向第一个属性，顶点坐标，偏移量为0
*/
```

着色器是根据索引来读取的

例子-- 我们有三种属性：位置，纹理坐标，法线

我们需要位置在0下标处，纹理在1，法线在2。显卡/着色器根据索引得到属性

> https://zhuanlan.zhihu.com/p/545498998

pointer 例子

```c++
float postions[] = {
    -0.5f,-0.5f,
    0.0f,0.5f,
    0.5f,-0.5f
};//为0 --- 因为下一个位置就在之后
//如果我们添加了纹理位置---如图
float postions[] = {
    -0.5f,-0.5f,0.5f
    0.0f,0.5f,
    0.5f,-0.5f
};
//此时我们到下一个顶点位置 间隔一个float --- 此时就为8
//const GLvoid * pointer ---- (const void*)8
```

**注意：要启动顶点属性数组**

```c++
    unsigned int buffer;
    float postions[] = {
    	-0.5f,-0.5f,
    	0.0f,0.5f,
    	0.5f,-0.5f
    };

    glGenBuffers(1, &buffer);
    //因为是状态机---因此我们可以使用数字来表示对象---id
    glBindBuffer(GL_ARRAY_BUFFER, buffer);
    //指定数据
    glBufferData(GL_ARRAY_BUFFER, 6 * sizeof(float), postions, GL_STATIC_DRAW);
    
    //启用顶点数组属性
    glEnableVertexAttribArray(0);
    //我们并没有具体告诉gl数据是什么
    //我们需要告诉gl我们的数据是如何布局的
    glVertexAttribPointer(0,2,GL_FLOAT,GL_FALSE,sizeof(float)*2, 0);
    
```

