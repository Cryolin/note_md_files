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

## 3.2 循环结构

【与java的区别】C++中存在就近原则，如下写法不会报错

```C++
// C++中如下写法不会报错，根据就近原则，i是内存for循环的值。
for (int i = 0; i < 5; i++) {
	for (int i = 10; i < 12; i++) {
		cout << i << endl;
	}
}
```

# 4. 数组

## 4.1 一维数组

一维数组的定义方式

```C++
	// 数据类型 数组名[数组长度]
	int arr1[5];

	// 数据类型 数组名[数组长度] = {值1， 值2 ...}
	int arr2[5] = { 0,1,2,3,4 };
	int arr2[5] = { 0,1 };
	int arr2[5] = {}; // 空白的会使用0填充

	// 数据类型 数组名[] = {值1， 值2 ...}
	int arr3[] = { 0,1,2,3,4 };
```

数组名取值，注意char数组的特殊性

```C++
int arr[] = { 1, 2 };
cout << arr << endl; // 输出地址
char str[] = "hello";
cout << str << endl; // 输出 hello
```

获取数组长度

```C++
// 获取数组长度的两种方法
cout << sizeof(arr) / sizeof(arr[0]) << endl;
cout << end(arr) - begin(arr) << endl;
```

初始值

```C++
int arr4[2];
cout << arr4[0] << endl; // 未初始化，打印地址

int arr5[2] = {};
cout << arr5[0] << endl; // 使用0填充，打印0
```

## 4.2 二维数组

二维数组定义方式

```C++
// 数据类型 数组名[数组行数][数组列数]
int arr1[2][3];

// 数据类型 数组名[数组行数][数组列数] = {{值1, 值2, ...}, {值3, 值4, ...}, ... {值5, 值6, ...}}
int arr2[2][3] = { {1,2,3},{4,5,6} };
int arr3[2][3] = { {},{4,5} }; // 不足的部分使用0填充

// 数据类型 数组名[数组行数][数组列数] = {值1, 值2, 值3, ...}
int arr4[2][3] = { 1,2,3,4,5,6 };
int arr5[2][3] = { 1,2 };
int arr6[2][3] = {}; // 不足的部分使用0填充

// 数据类型 数组名[][数组列数] = {值1, 值2, 值3, 值4}
int arr7[][3] = { 1,2,3,4,5,6 };
int arr8[][3] = { 1,2 }; // 自动计算，该数组为1行，不足部分补0
int arr9[][3] = { 1,2,3,4 }; // 自动计算，该数组为2行，不足部分补0
// int arr10[][3] = {}; // 无法编译
// int arr11[2][] = {1,2}; // 无法编译
```

遍历&计算行数列数

```C++
// 二维数组的遍历 & 行数列数的获取
for (int i = 0; i < sizeof(arr8) / sizeof(arr8[0]); i++)
{
	for (int j = 0; j < sizeof(arr8[0]) / sizeof(arr8[0][0]); j++)
	{
		cout << arr8[i][j] << "\t";
	}
	cout << endl;
}
```

# 5. 函数

## 5.1 函数声明

```C++
// 函数声明，可以有多次
// 提前告诉编译器有这个方法
int max(int a, int b);
int max(int a, int b); // 可以重复声明，但没必要

int main() {

	system("pause");
	return 0;
}

// 函数定义，只能有一次
// max定义位于main的下面，如果没有max的声明，执行会报错
int max(int a, int b) 
{
	return a > b ? a : b;
}
```

通过头文件声明

```C++
// test.h
#include <iostream>;

using namespace std;

int test(int a, int b);
```

```C++
// 自定义头文件，通过双引号导入
#include "test.h"

int test(int a, int b)
{
	cout << "test" << endl;
	return 0;
}
```

调用test方法的函数：

```C++
// 导入test.h
#include "test.h";

// test.h中，已经导入了iostream，所以此处无需重复导入
//#include <iostream>

// test.h中，已经使用了std的命名空间，无需重复声明
//using namespace std;

int main() {
	int a = 1;
	int b = 2;
	int c = test(a, b);
	cout << "main" << endl;
	system("pause");
	return 0;
}

// 所有导入test.h头文件的cpp文件，只能有一个test()函数的定义
// 下方如果重复定义会报错

//int test(int a, int b)
//{
//	cout << "main方法入口文件中，调用swap" << endl;
//	return;
//}
```

