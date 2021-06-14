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

# 6. 指针

## 6.1 指针定义和使用

指针变量定义：数据类型* 指针变量名

```C++
int a = 10;

// 指针变量的定义和赋值
int* p = &a;

cout << "&a = " << &a << endl;	// 打印a的地址
cout << "p= " << p << endl;		// 打印指针变量p，其值等于a的地址

// 指针变量的使用
cout << "*p = " << *p << endl;	// 对p进行解引用，其值为10

cout << "修改*p的值为100" << endl;
*p = 100;
cout << "*p = " << *p << endl;	// 对p解引用，相当于对该地址保存的值赋值
cout << "a = " << a << endl;	// 变量a和*p的值也随之改变
```

指针占用内存大小

```C++
// 指针保存内存地址
// 32位系统占用4个字节，64位系统占用8个字节
cout << "指针占用内存大小： " << sizeof(p) << endl;
cout << "指针占用内存大小： " << sizeof(int*) << endl;
cout << "指针占用内存大小： " << sizeof(float*) << endl;
```

## 6.2 空指针&野指针

空指针

```C++
// 空指针:指针变量指向内存中编号为0~255的空间
// 用途：初始化指针变量
// 注意：空指针指向的内存是不可以访问的
int* p1 = NULL;
//*p1 = 100;	// 不可以修改空指针地址的内容，报错
//cout << *p1 << endl;	// 同样也报错
```

野指针

```C++
// 野指针：指向非法内存空间
int* p2 = (int*)0x11010120;
cout << *p2 << endl;	// 不允许访问，程序报错
```

## 6.3 指针和数组

```C++
int arr[5] = { 1,2,3,4,5 };
int* p = arr;
cout << *p << endl;		// arr[0]

// 注意指针的+1，是增加sizeof(指针类型)
// 例如这里增加了sizeof(4)，相当于把指针从arr[0]的首地址移动到尾地址
// 也就是arr[1]的首地址，通过p++,就可以访问到arr[1]了
p++;
cout << *p << endl;		// 再次解引用时，p已经指向了arr[1]

double arr2[5] = { 1.1,2.2,3.3,4.4,5.5 };
double* p2 = arr2;
cout << *p2 << endl;	// arr2[0]

// 同样的，对于double类型的指针，++操作相当于地址增加了sizeof(double)
// 相当于指针从arr2[0]的首地址移动到arr2[0]的尾地址
// 也就是arr2[1]的首地址，通过p2++，就可以访问到arr2[1]了
p2++;
cout << *p2 << endl;	// arr2[1]
```

## 6.4 函数传递地址

通过传递地址，可以实现地址解引用内容的更改

```C++
void swap(int* p1, int* p2)
{
	int temp = *p1;
	*p1 = *p2;
	*p2 = temp;
}

int main() {
	int a = 10;
	int b = 20;
	swap(&a, &b);
	cout << "a = " << a << endl;	// a = 20
	cout << "b = " << b << endl;	// b = 10

	system("pause");
	return 0;
}
```

# 7. 结构体

## 7.1 结构体的定义和使用

```c++
// 结构体定义
struct Student
{
	string name;
	int age;
	int score;
}s3; // 创建结构体变量方法3

int main() {
	// 创建结构体变量的三种方法
	
	// 1. struct 结构体名 变量名
	struct Student s1;
	// Student s1; 创建时，struct是可以省略的
	s1.name = "张三";	// 结构体变量通过"."访问成员
	s1.age = 18;
	s1.score = 100;


	// 2. struct 结构体名 变量名 = {成员1值，成员2值...}
	Student s2 = { "李四", 19, 80 };

	// 3. 在定义结构体时顺便创建变量
	s3.name = "王五";
	s3.age = 20;
	s3.score = 60;

	system("pause");
	return 0;
}
```

## 7.2 结构体指针

```C++
struct Student
{
	string name;
	int age;
	int score;
};

void printStu1(Student stu)
{
	stu.name = "cryolin";
	cout << "printStu1 " << stu.name << endl;
}

void printStu2(Student* stu)
{
	stu->name = "cryolin";
	// 结构体指针，通过"->"访问成员
	cout << "printStu2 " << stu->name << endl;
}

int main() {
	Student stu = { "colin", 18, 750 };
	Student* p = &stu;

	// 直接传入结构体对象到函数，是无法修改结构体的内容的
	printStu1(stu);
	cout << "stu.name : " << stu.name << endl;	// 打印"colin"

	// 传入stu地址，可以修改结构体内容
	printStu2(p);
	cout << "stu.name : " << stu.name << endl;	// 打印"cryolin

	system("pause");
	return 0;
}
```

## 7.3 结构体使用const的场景

```C++
// 结构体重使用const的场景：
// 1. 直接传入结构体，会复制全部结构体内容到栈，增大内存占用
// 2. 所以一般使用结构体指针传入
// 3. 但结构体指针传入后，向函数暴露了结构体写入的能力
// 4. 对于不想让函数修改结构体的场景，可以通过在结构体指针前加const的方法
void printStu3(const Student* stu)
{
	// 加入const之后，对stu传参内容的修改会报错
	// stu->name = "cryolin";
	cout << "printStu3 " << stu->name << endl;
}
```

# 8. C++内存分配

## 8.1 内存的静态分配与动态分配

**静态内存分配**：静态内存分配是在程序编译或者运行过程中，按事先规定的大小分配内存空间的分配方式，他的前提的必须事先知道所需内存空间的大小

**动态内存分配**：按输入信息的大小分配所需要的内存单元，特点是按需分配。

程序内存空间分为三个部分：

1. 静态存储区：全局变量，静态变量
2. 动态存储区：栈、堆
3. 程序区：存放程序语句

```C++
int g_a = 10;
static int s_g_a = 10;
const int c_g_a = 10;

int main()
{
	int l_a = 10;
	static int s_l_a = 10;
	const int c_l_a = 10;

	/*
	普通局部变量的地址0078FE5C
	普通全局变量的地址00DBC000
	静态局部变量的地址00DBC008
	静态全局变量的地址00DBC004
	const全局常量的地址00DB9B44
	const局部常量的地址0078FE50
	*/
	cout << "普通局部变量的地址" << &l_a << endl;
	cout << "普通全局变量的地址" << &g_a << endl;
	cout << "静态局部变量的地址" << &s_l_a << endl;
	cout << "静态全局变量的地址" << &s_g_a << endl;
	cout << "const全局常量的地址" << &c_g_a << endl;
	cout << "const局部常量的地址" << &c_l_a << endl;

	// 静态分配的有：全局变量和静态变量

	system("pause");
	return 0;
}
```

## 8.2 栈

方法入参、局部变量和返回地址会保存在栈中，程序员无需关注其声明周期。

不要将局部变量的地址作为返回值返回，因为程序会在函数返回后回收栈中的内存。

例如：如下打印：

```C++
int* getInt()
{
	int a = 10;
	return &a;
}

int main()
{
	int* a = getInt();

	// 10			第一行打印10
	// 2084673928	第二行打印时，内存已被回收
	cout << *a << endl;
	cout << *a << endl;

	system("pause");
	return 0;
}
```

## 8.3 堆与new关键字

堆中的对象程序员需关注其声明周期并手动释放，可通过new关键字主动在堆内存中创建对象，delete关键字释放其内存。

```C++
int* getInt()
{
	// new关键字创建int对象，调用int对象的构造函数
	// 返回指向new的对象类型的指针
	int* a = new int(10);
	return a;
}

int* getIntArray()
{
	// 通过new关键字创建int数组，返回指向数组首地址的指针
	int* array = new int[10];
	for (int i = 0; i < 10; i++)
	{
		array[i] = 100 + i;
	}
	return array;
}

int main()
{
	int* a = getInt();

	cout << *a << endl;
	cout << *a << endl;

	// 通过delete函数，释放a的内存，相当于调用了int的析构函数
	// 执行完delete之后，无法再通过*a解引用
	delete a;

	int* array = getIntArray();
	cout << array[0] << endl;
	cout << array[1] << endl;

	// 对于数组，要通过delete[]释放内存，否则只能释放第0个位置的内存
	delete[] array;

	system("pause");
	return 0;
}
```

