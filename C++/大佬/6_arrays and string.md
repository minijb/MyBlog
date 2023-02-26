# arrays and string 

```c++
#include <iostream>

int main()
{
	int example[5];

	example[0] = 1;

	std::cout << example << std::endl;//example其实是一个指针

	int* ptr = example;
	// example[2] == *(ptr+2)---其实就是根据ptr的int字节数进行位移
	*(int*)((char*)ptr + 8) = 100;

	std::cout << example[2] << std::endl;//100
    //我们将ptr强转为char --- 1字节  那么移动两个int就是8字节， 为了修改正确的4字节我们将 指针类型修改为int
	std::cin.get();
}

```

我们可以将数组存储在堆里

```c++
	int* another = new int[5];

	for( int i =0; i < 5 ; ++i)
	{
		another[i] = 2;
	}

	std::cout <<  another[4] << std::endl;
	delete[] another;
```

间接寻址

```c++
class Entity
{
public:
	int* example = new int[5];

	Entity()
	{
		for (int i = 0 ; i < 5 ; i++)
		{
			example[i] = 2;
		}
	}
};
Entity e;
```

此时我们找到e的地址，会发现内部一个内存地址，也就是example，我们通过这个在进行查找才可以找对值



c++11 中有一个 `std::array`的内置数据结构，有边界检查，等很多方法

在传统数组中是不知道数组长度的，但是可以通过编译器知道！！！

```c++
int a[5];
int count = sizeof(a) / sizeof(int);
```

但是这个只限于存储在栈内的，存储在对堆里的sizeof的是一个指针，永远是int也就是4字节

## string

 a group of character(char)

00(`\0`)---字符串终止符。

c++会读取字符串知道`\0`

std::string---本质上array of char

```c++
#include <iostream>
#include <string>


int main()
{

	std::string name = "hello";
	std::cout << name.size() << std::endl;
	std::cin.get();
}

```

c++中用单双引号的值的类型都是const char/char*

> string头文件中将字符串的输入流重写了

string重载了+ ， 但是const char* 没有

因此

```c++
"a"+"b"; //err
std::string name = "ccc";
name+="b"; //ok
std::string name = std::string("a")+"b";//ok

std::string::npos;//非法位置


void print(std::string name){//a copy 可以使用引用
    std::cout << name << std::endl;
}

//这样更快
void print(const std::string& name){//此处const说明我们不能去修改这个参数
    std::cout << name << std::endl;
}
```

## string iterals 字符串字面量

使用用双引号括起来的字符串

```c++
"hello";
const char* name = "hello";//其实就是指向内存中的位置
//必须有const
```

字符串自卖你连是存储在内存中的只读部分的(也就是说字符串字面量是存储在二进制文件的const部分的)

> 在release模式下
>
> ```c++
> char* name = "zhouhao";//未定义行为
> name[2] = 'K';
> ```
>
> 虽然编译通过了，但是修改是不生效的！！！！
>
> debug模式下  会报错

**如果我们想要修改字符串需要把他定义为一个数组而不是一个指针**

```c++
char name[] = "zhouhao";
name[2] = 'K';
//此时字符串还是在const区域
//但是会创建一个变量，经过cpu计算后将结果给这个变量
//简单来说就是 之前指针的时候是尝试修改常量导致出错
//后面是新建了一个变量存储结果常量的值，因此可以修改变量内的值！！！！
```

```c++
#include <iostream>
#include <string>


int main()
{
	char name[] = u8"hello";
	name[2] = 'A';


	const wchar_t* name2 = L"zhouhao";//宽字符组成 1/2bit由编译器决定
	const char16_t* name3 = u"zhouhao";//2bit
	const char32_t* name4 = U"zhouhao";//4bit--utf32


	using  namespace std::string_literals;
	std::string name0 = u8"cherno"s + "hello";//s标志返回一个字符串
	std::wstring n1 = L"cherno"s + L"hello";
	std::u32string n2 = U"cherno"s + U"hello";


	const char* example = R"(Line1
Line2)";//忽略转义符---所见即所得
	const char* example1 = "Line1\n"
		"Line2\n";//和之前效果一样
	std::cout << example1 << std::endl;
	std::cin.get();
}

```

