## 宏

在编译的时候第一步就是预处理--也就是文本处理阶段---那么此时我们可以直接自定义一些这个过程，这就是宏。宏可以很复杂也可以很简单。

```c++
#include <iostream>

#define WAIT std::cin.get();

#define LOG(x) std::cout << x << std::endl

int main()
{

	LOG("hello");

	WAIT//每次遇到WAIT会用自定义字符串代替
	//这样做其实不是很好因为如果在另一个文件定义，我们仅仅看到WAIT。这并不直观
	//哪怕是括号都可以
	
}

```

有的时候我们在debug模式下需要日志系统，但是在release中不需要，这个可以通过宏来实现

在项目属性中---c++---预处理器----预处理定义中

debug模式中---定义：`PR_DEBUG`

release模式中---定义：`PR_RELEASE`

```c++
#ifdef PR_DEBUG
#define LOG(x) std::cout << x << std::endl
#else
#define LOG(x) //release中直接用空替换---也就是直接移除
#endif // PR_DEBUG
```

还有一点问题

我们需要定义PR_DEBUG一个值，而不是仅仅定义它---这样更加方便

在属性中

```
PR_DEBUG=1
```

```c++
#if PR_DEBUG == 1
#define LOG(x) std::cout << x << std::endl
#else
#define LOG(x) //release中直接用空替换---也就是直接移除
#endif // PR_DEBUG
```

除了else 还有elif

```c++
#if PR_DEBUG == 1
#define LOG(x) std::cout << x << std::endl
#elif defined(PR_RELEASE)
#define LOG(x) //release中直接用空替换---也就是直接移除
#endif // PR_DEBUG
```

还有一些规则

```c++
#if 0 //false
#endif
\ // 如果要换行的回车键---宏默认在一行内
```

还有其他很多作用！！！！---大部分是用来调试 

## auto关键字

让c++自动推导出类型，有好处有坏处

关键

- 我们不关心这里的类型
- 左边是不改变的，但是右边是可变的

导致两个问题

- 我们可以不写类型了？
- 风格问题

什么时候用auto

- 有的时候---我们的函数返回类型可能需要经常修改，客服端可以不用修改代码，但是有些类型可以隐式转换，那这时候用auto就会比较麻烦，比如`string and char*`之后的逻辑就改变了。因此要自己把握一下

```c++
class Device
{
};

class DeviceManager
{
private:
	std::unordered_map<std::string, std::vector<Device*>> m_Devices;

public:
	const std::unordered_map<std::string, std::vector<Device*>> GetDevices() const
	{
		return m_Devices;
	}
};

int main()
{
	std::vector<std::string> strings;
	strings.push_back("Apple");
	strings.push_back("Orange");

	//这里代替了一个巨大的类型，同时巨大的类型在循环过程中不变
	for (auto it = strings.begin(); it != strings.end(); ++it)
	{
		cout << *it << endl;
	}

	using DeviceMap = std::unordered_map<std::string, std::vector<Device*>>;
	typedef std::unordered_map<std::string, std::vector<Device*>> DeviceMap;

	DeviceManager dm;
	const DeviceMap& devices = dm.GetDevices();
	const auto& devices1 = dm.GetDevices();
	//注意 如果我们返回一个引用，此时auto不会处理他，只会当作一个正常类型，我们需要手动加上去。
	//如果不加，他会复制一个，如果我们改动，不会传导
	std::cin.get();
}
```

如果我们使用了模板，那么此时我们必须使用auto因为我们不知道具体是什么类型。

> 11中我们可以后置类型，使用auto作为先前类型
>
> ```c++
> auto fun() -> char*{
>     return "hello";
> }
> ```
>

## 数组---array---标准静态数组

```c++
#include <iostream>
#include <array>
using std::cout;
using std::endl;

template<int n>
void printArray(const std::array<int, n>& data)
{
	for (int i = 0 ;  i < data.size() ; ++i)
	{
		cout << data[i] << endl;
	}
}


int main()
{

	std::array<int, 5> data;//其实没有size变量，而是模板
	data[0] = 2;
	data[4] = 1;//有边界检查,c风格没有


	printArray<data.size()>(data);
	cout << data.size() << endl;
	std::cin.get();
}

```

## 函数指针

### c语言中的函数指针

```c++
void helloWorld(int a)
{
	std::cout << "hello world" << a << std::endl;
}

int main()
{
	auto function = helloWorld;//不仅仅是指令，二十具体的二进制代码
    //此处经过隐式转换了  auto funtion = &helloWorld;
	function(100);//可以直接调用
	void(*function1)(int) = helloWorld;//类型为void(*)() function1是命名
	function1(101);
	typedef void(*helloWorldFunctionName)(int);

	helloWorldFunctionName function2 = helloWorld;
	std::cin.get();
}
```

具体应用

```c++
void printValue(int value)
{
	std::cout << "value : " << value << endl;
}

void forEach(const std::vector<int>& values, void(*func)(int))
{
	for(int value: values)
	{
		func(value);
	}
}


int main()
{
	std::vector<int> values = { 1,2,3,4,5 };

	// forEach(values, printValue);

	forEach(values, [](int value) { std::cout << "value : "<< value << endl; });

	std::cin.get();
}
```

还有很多c++中的应用，之后再说

## lambda

匿名函数----一次性函数----只要我们有一个函数指针的地方，我们就可以用他

```c++
int main()
{
	std::vector<int> values = { 1,2,3,4,5 };

	// forEach(values, printValue);

	forEach(values, [](int value) { std::cout << "value : "<< value << endl; });

	std::cin.get();
}
```

最常见的用法就是我希望将一个函数传递给api

```c++
#include <iostream>
#include <array>
#include <vector>
#include <functional>
#include <algorithm>
using std::cout;
using std::endl;

void printValue(int value)
{
	std::cout << "value : " << value << endl;
}

void forEach(const std::vector<int>& values,const std::function<void(int)>& func)
{
	for(int value: values)
	{
		func(value);
	}
}


int main()
{
	std::vector<int> values = { 1,5,3,4,5 };
	auto it = std::find_if(values.begin(), values.end(), [](int value) {return value > 3; });
	//仅仅返回大于三的值
	cout << *it << endl;

	int a = 5;
	// forEach(values, printValue);
	//注意因为使用了= 每次调用lambda时都会使用之前的所有代码---因此结果看起来有点奇怪---每次多输出一个5
	auto lambda = [=](int value)//&a , = ,& , a 
	{
		std::cout << "value : " << value << endl;
		// cout << a << endl;//without captures , cant find a
	};
	//[]---captures copy or reference 也可以是this  &--all automatic variables by reference = -- xx by copy
	// capture --- 这个函数是稍后会用到的，那他的是下文就是不确定的，我们需要给他一个上下文
	//但是使用了capture就不是原始c函数指针了，需要#include <functional> 此时类型为const std::function<void(int)>& func
	//cppreference.com --- mutable 关键字  允许我们在body中修改外部变量---only copy
	forEach(values, lambda );

	std::cin.get();
}

```



