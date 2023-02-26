有的时候我们需要在一个函数中返回两个string，有的时候多个返回值的类型不同，此时我们不能返回vector

- 有个很简单的方式就是自己创建一个struct。

我们返回自己创建的值就好了

```c++
//返回的时候直接使用struct的默认构造函数
return {vs , fs};
```

其他适用于所有类型的方法：

- 返回值为void，将需要返回的类型使用引用放入参数中。

效率很高，在父函数的堆栈中构造字符串，传入两个引用不同额外分配内存。(使用指针同理)

> 将需要输出的参数命名为out_xxxx

函数之后我们需要用if判断out_xxx是否存在

> 如果我们不关心输出，我们可以输入nullptr

- 返回值为数组

```c++
std::string* function(xxx){
    return new std::string[] {vs,fs};//注意可以优化
}
```

但是我们不知道有多大

```c++
std::array<std::string,2> function(xxx){
    return std::array<std::string,2>(vs,fs);//注意可以优化
}
```

也可以使用vector(注意：vector实在栈上，但是底层存储在堆里(自动new和delete))因此vector相较于array，vector效率低一点。

- 使用pair，tuple

```c++
#include <utility>
#include <tuple>
std::tuple<std::string,std::string> function(xxx){
    return std::make_pair(vs,fs);
}

//取值
std::string s = std::Get<0>(source);

//pair用.first .second
```

