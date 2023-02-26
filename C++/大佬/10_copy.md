# copy

尽可能不适用copy因为花费更多的时间

```c++
	
	int a = 2;
	int b = a;

	Vector2 v1 = { 2,3 };
	Vector2 v2 = v1;

	Vector2* p_v = new Vector2();
	Vector2* p_v1 = p_v; //只是复制了 指针的值  *p_v1 = *p_v 才是复制了指向的东西
	p_v1->x++;//P_v 也会改变
```

我们已string类为例

```c++
#include <iostream>
#include <string>
#include <memory>

// using std::string;
using std::cout;
using std::endl;

struct Vector2
{
	float x, y;
};

class String
{
private:
	char* m_Buffer;
	unsigned int m_Size;
public:
	String(const char* string)
	{
		m_Size = strlen(string);
		m_Buffer = new char[m_Size + 1];//将值复制到buffer中
		memcpy(m_Buffer, string, m_Size);
		m_Buffer[m_Size] = 0;
	}

	~String()
	{
		delete[] m_Buffer;
	}
	friend std::ostream& operator<<(std::ostream& stream, const String& string);//使用友元 可以让对应的函数访问private属性

	char& operator[](unsigned int index)
	{
		return m_Buffer[index];
	}

	String(const String& other)//自动会帮我们创建一个---但是只是浅拷贝
		// =delete 禁用
		:m_Size(other.m_Size)
	{
		cout << "copying !!!" << endl;
		m_Buffer = new char[m_Size];
		memcpy(m_Buffer, other.m_Buffer, m_Size+1 );
	}
};

std::ostream& operator<<(std::ostream& stream , const String& string)
{
	stream << string.m_Buffer;
	return stream;
}

// void printString(String string);//此处也会copy构造函数 -- 但是没必要
void printString(const String& string);//直接使用引用
//因此尽量,总是使用常量引用--- 我们可以在函数内决定什么时候复制而不是在调用的时候直接复制


int main()
{
	String string = "zhouhao";
	String second = string;//浅拷贝 --- 复制了string中所有的属性值---有一个是指针---也就是两个指针指向一块内存
	//此时我们其实调用了一个叫做复制构造器的东西


	second[2] = '!';//两个都改变了 --- 因此我们需要做的是深层次copy，second应该指向自己的内存块
	cout << string << endl ;
	cout << second << endl;
	std::cin.get();
}


```

