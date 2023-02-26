## namespace

为什么不使用`using namespace`

- 明确知道函数来自std

- 使用多个标准库的时候我们可以明确知道来自哪个
- 多个标准库有可能重名----我们进不清楚使用哪一个

不要在头文件中使用`using namespace`

## what is namespace

可以在一个项目中可以出现相同命名，返回值以及参数的函数---(same symbol)

如果我们想要两个一摸一样的函数---我们就需要用到namespace

在c中我们只有通过修改函数名实现，在c++中我们可以使用namespace

同时namespace是可以嵌套的----也就是

```c++
namespace apple
{
	namespace function
	{
		void print(const char* text)
		{
			cout << text << endl;
		}
	}
}
	apple::function::print("hello");
//类本身即是一个命名空间
```

可以单独引入

```c++
using apple::function::print;
```

可以重新命名namespace

```c++
namespace  a = apple;
```

重新命名对于嵌套namespace很有用，大大减少了打字

> 在一些小作用域中，用namespace是可以的
>
> 前往不要在顶层或者头文件中使用using namespace