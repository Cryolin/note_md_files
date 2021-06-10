# 1. 初识C++

## 1.1 C++常量

```c++
#include<iostream>
using namespace std;

// 文件开头，通过#define 常量名 常量值，定义常量
#define Day 7

int main() {
	cout << "一周有" << Day << "天" << endl;

	// const 常量类型 常量名 常量值
	const int month = 12;
	cout << "There is " << month << " months in one year." << endl;

	system("pause");
	return 0;
}
```

# 2. 数据类型

## 2.1 sizeof关键字

**作用**：利用sizeof关键字可以统计数据类型所占内存的大小，单位：字节

**语法**：sizeof(数据类型/变量)

```c++
#include <iostream>
using namespace std;

int main() {
	short num1 = 10;
	cout << "short类型的大小" << sizeof(num1) << "bytes" << endl;

	int num2 = 10;
	cout << "int类型的大小" << sizeof(num2) << "bytes" << endl;

	long num3 = 10;
	cout << "long类型的大小" << sizeof(long) << "bytes" << endl;

	long long num4 = 10;
	cout << "long long类型的大小" << sizeof(long long) << "bytes" << endl;

	system("pause");
	return 0;
}
```

![image-20210606200714945](D:\文档\md文件\images\image-20210606200714945.png)

## 2.2 字符串

```C++
#include <iostream>
using namespace std;

// C++支持两种字符串
int main() {
	// 1、C语言风格字符串
	char str1[] = "Hello World";
	cout << str1 << endl;

	// 2、C++风格字符串
	string str2 = "Hello World";
	cout << str2 << endl;

	system("pause");
	return 0;
}
```

## 2.3 布尔型

C++中，布尔型的关键字是bool

## 2.4 数据输入输出

通过cin和cout进行数据输入输出

```c++
#include <iostream>
using namespace std;

int main() {
	int a = 1;
	cout << "请输入a的值：" << endl;
	cin >> a;
	cout << "a的值为：" << a << endl;

	system("pause");
	return 0;
}
```

# 3. 程序流程结构

## 3.1 选择结构

【与java的区别01】C++中，if()后可以直接跟分号

```C++
// 下面的写法是正确的
if (a > 0);
```

【与java的区别02】三目运算符可以作为左值被赋值

```C++
int a = 2;
int b = 3;
a > b ? a : b = 4;
```
