# STL--标注库

```c++
#include <iostream>
#include <string>
#include <memory>
#include <vector>

// using std::string;
using std::cout;
using std::endl;


struct Vertex
{
	float x, y, z;
};

std::ostream& operator<<(std::ostream& stream,const Vertex& vertex)
{
	stream << vertex.x << "," << vertex.y << "," << vertex.z;
	return stream;
}

int main()
{
	Vertex* vertives1 = new Vertex[5];//固定大小的值
	std::vector<Vertex> vertices2;//是存储指向堆的指针还是存储行内变量是不同的
	//当我们添加值的时候我们会重新分配内存，那此时使用指针就更快
	vertices2.push_back({ 1,2,3 });
	vertices2.push_back({ 2,2,3 });
	vertices2.push_back({ 3,2,3 });
	for ( int i = 0 ; i < vertices2.size() ; ++i)
	{
		cout << vertices2[i] << endl;
	}

	for ( Vertex& v : vertices2)//尽量避免复制
	{
		cout << v << endl;
	}

	vertices2.erase(vertices2.begin() + 1);//删除第二个
	for (Vertex& v : vertices2)//尽量避免复制
	{
		cout << v << endl;
	}
	vertices2.clear();//清0
	std::cin.get();
}

```

## 优化std的使用

```c++
#include <iostream>
#include <vector>

using std::cout;
using std::endl;

struct Vertex
{
	float x, y, z;
	Vertex(float x, float y, float z) :x(x), y(y), z(z) {}
	Vertex(const Vertex& vertex):x(vertex.x),y(vertex.y),z(vertex.z)
	{
		cout << "copied!" << endl;
	}
};

int main()
{
	std::vector<Vertex> vectices;//在此处（3）不是设置空位，而是调用构造函数

	vectices.reserve(4);//事先将空位设置为3---现在总共3次
	vectices.push_back(Vertex(1, 2, 3));//1次----在main的栈上创建了Vertex,然后将这个放到vector中----此时复制了一次
	vectices.push_back(Vertex(4, 5, 6));//3次 自己本身复制了一次，因为要扩大---再复制之前的元素一次----总共两次
	vectices.push_back(Vertex(7, 8, 9));//6次

	// vectices.reserve(100);
	//另一个优化：使用emplace_back代替push----push是在行内进行初始化然后再复制到vector中，而example是直接在vector中进行初始化

	vectices.emplace_back(1, 2, 3);//没有copy
	std::cin.get();
}
```

