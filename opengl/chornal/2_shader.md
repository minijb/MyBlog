# shader

opengl有一个默认的shader，因此之前的buffer能够正常加载

### 什么是shader

shader是运行在GPU上的代码，GPU在处理图形上臂CPU快很多

两种常用shader -- 顶点着色器 ， 片段着色器(90%都用这两种)。还有其他shader。

顶点着色器---在渲染顶点的时候用到-----这里我们有三个顶点 ---- 主要目的就是定位

之后我们会进入片段/像素着色器 --- 光栅化---就是在一片位置画像素。这里就是在三角形中的每一个像素画上颜色。(这里会调用很多很多次----根据图形的大小变化)

> 有一个很有意思的优化就是，将数据从顶点着色器中传递到片段着色器，因为片段着色器中的代价很高。

## 创造一个shader

```c++
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>

static unsigned int compileShader(const std::string& source, unsigned int type)
{
    unsigned int id = glCreateShader(type);
    const char* src = source.c_str();
    //注意如果source中字符串超过上限，那么此时src就是一个垃圾指针。
    glShaderSource(id, 1, &src, nullptr);
    glCompileShader(id);

    int result;
    glGetShaderiv(id, GL_COMPILE_STATUS, &result);

    if (result == GL_FALSE)
    {
        int length;
        glGetShaderiv(id, GL_INFO_LOG_LENGTH, &length);
        // char message[length];
        char* message = (char*)alloca(length * sizeof(char));
        //alloca ---  栈上分配空间
        glGetShaderInfoLog(id, length, &length, message);
        std::cout << "Failed to compile " <<
            (type == GL_VERTEX_SHADER ? "vertex" : "fragment")
            << "shader" << std::endl;
        std::cout << message << std::endl;
        glDeleteShader(id);
        return 0;
    }


    return id;
}

//参数是源代码
static int creatShader(const std::string& vertexShader, const std::string& fragmentShader)
{
    unsigned int programe = glCreateProgram();
    unsigned int vs = compileShader(vertexShader, GL_VERTEX_SHADER);
    unsigned int fs = compileShader(fragmentShader, GL_FRAGMENT_SHADER);
    glAttachShader(programe, vs);
    glAttachShader(programe, fs);
    glLinkProgram(programe);
    glValidateProgram(programe);

    glDeleteShader(vs);//加载上去之久就删除中间文件
    glDeleteShader(fs);

    //detach 可以分离源码，但是没必要
    return programe;
}

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

    if (glewInit() != GLEW_OK)
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

    //启用顶点数组属性
    glEnableVertexAttribArray(0);
    //我们并没有具体告诉gl数据是什么
    //我们需要告诉gl我们的数据是如何布局的
    glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, sizeof(float) * 2, 0);

    std::string vertexShader =
        R"(#version 330 core
layout(location =0 ) in vec4 position;
void main()
{
	gl_Position=position;
})";

    std::string fragmentShader =
        R"(#version 330 core
layout(location =0 ) out vec4 color;
void main()
{
	color = vec4(1.0, 0.0, 0.0, 1.0);
})";
    unsigned int shader = creatShader(vertexShader, fragmentShader);
    glUseProgram(shader);
    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window))
    {
        /* Render here */
        glClear(GL_COLOR_BUFFER_BIT);


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

## 处理着色器

我们可以专门使用一个文件来存储之前的字符串----不是分为多个文件

```c++
struct  ShaderProgramSource
{
    std::string vertexShader;
    std::string FragmentShader;
};

static ShaderProgramSource parseShader(const std::string& filePath)
{
    std::ifstream stream(filePath);

    enum class ShaderType
    {
        NONE = -1, VERTEX = 0, FRAGMENT = 1
    };

    std::string line;
    std::stringstream ss[2];
    ShaderType type = ShaderType::NONE;
    while(std::getline(stream,line))
    {
        if (line.find("#shader") != std::string::npos)
        {
            if (line.find("vertex") != std::string::npos)
                type = ShaderType::VERTEX;
            else if (line.find("fragment") != std::string::npos)
                type = ShaderType::FRAGMENT;
        }
        else
        {
            ss[(int)type] << line << '\n';
        }
    }
    return { ss[0].str() , ss[1].str() };
}
```

