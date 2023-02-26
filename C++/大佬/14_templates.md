# template

模板比泛型强大很多

模板允许我们定义一个可以根据用途进行编译的模板，让编译器为我们写代码

```c++
#include <iostream>

using std::cout; using std::endl;

// void print(const int value)
// {
// 	cout << value <<endl;
// }
//
// void print(const std::string value)
// {
// 	cout << value << endl;
// }
//如果我们需要输出很多种类型，那么我们就需要写很多函数的重载
//使用模板我们就可以只定义一个模板

//typename---模板参数  T：name
template<typename T>//也可以使用class
void print(T value)//只有当我们实际调用的时候函数才会被创建，也就是说如果我们没有调用那么就没有这个函数
//函数只是一个模板产生的模板函数
{
	cout << value << endl;
}

//模板不仅仅可以用在函数中并表示数据类型

//此处我们需要在使用的时候给定array对应的大小，此时我们可以如此
//这里我们确定我们只需要一个int类型
//如果我们想要类型可变，可以添加多个模板
template<typename T , int N>
class Array
{
private:
	T m_Array[N];
public:
	int getSize() const { return N; }
};



int main()
{
	print(5);//这里类型被隐式的从实际参数中得到
	print<int>(5);//其实模板就是填空将原本T的地方替换为对应的类型
	print("hello");
	print(1.5f);

	Array<int, 100> array;
	Array<std::string, 100> array1;

	cout << array.getSize() << endl;

	std::cin.get();
	
}

```

使用模板的时机

- 很多公司禁止使用模板，因为深度的模板使用会让项目变得复杂。

- 但是我们需要掌握平衡。