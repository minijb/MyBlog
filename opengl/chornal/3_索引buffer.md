# index buffer

我们画一个正方形----两个三角形。最简单的方式就是使用两个三角形的坐标，画6个顶点。

但是中间有很多顶点是重合的----这个就浪费了内存。

因此引入index buffer进行顶点的重用。--- 此时正方形就只需要4个顶点，而不是6个。

**注意：index buffer存储的类型必须是unsigned int**

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

    if (glewInit() != GLEW_OK)
    {
        std::cout << "Error! --- glewInit failed!" << std::endl;
    }


    float postions[] = {
        -0.5f,-0.5f,
         0.5f,-0.5f,
         0.5f,0.5f,
        -0.5f,0.5f
    };

    unsigned int indices[] = {
        0,1,2,
        2,3,0
    };

    unsigned int buffer;
    glGenBuffers(1, &buffer);
    glBindBuffer(GL_ARRAY_BUFFER, buffer);
    glBufferData(GL_ARRAY_BUFFER, 4 * 2 * sizeof(float), postions, GL_STATIC_DRAW);


    glEnableVertexAttribArray(0);
    glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, sizeof(float) * 2, 0);

    unsigned int iob;
    glGenBuffers(1, &iob);
    glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, iob);
    glBufferData(GL_ELEMENT_ARRAY_BUFFER, 6 * sizeof(unsigned int), indices, GL_STATIC_DRAW);


    ShaderProgramSource source = parseShader("res/shaders/shader.shader");
    // std::cout << source.FragmentShader << std::endl;

    unsigned int shader = creatShader(source.vertexShader, source.FragmentShader);
    glUseProgram(shader);
    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window))
    {
        /* Render here */
        glClear(GL_COLOR_BUFFER_BIT);

        glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, nullptr);

        /* Swap front and back buffers */
        glfwSwapBuffers(window);

        /* Poll for and process events */
        glfwPollEvents();
    }

    glDeleteProgram(shader);

    glfwTerminate();
    return 0;
}
```

