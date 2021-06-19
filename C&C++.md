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

## 1.2 #define重定义

C++中，多次define选择最后一次

```C++
// define01.h
#include <iostream>;

using namespace std;

#define MAX 10
#define MAX 20
#define MAX 30
```

```C++
// define01.cpp
#include "define01.h"

#define MAX 40

// 最近的一次是40,故打印40

int main() {
	cout << "MAX = " << MAX << endl;
	system("pause");
	return 0;
}
```

如果调整下顺序：

```C++
// define01.cpp
#define MAX 40
#include "define01.h"

// 最近的一次是30,故打印30

int main() {
	cout << "MAX = " << MAX << endl;
	system("pause");
	return 0;
}
```

重复定义，会有如下编译告警：

------

*1>D:\git\C_plus_demo\1. C++基础编程\define01.h(7,1): warning C4005: “MAX”: 宏重定义
1>D:\git\C_plus_demo\1. C++基础编程\define01.h(6): message : 参见“MAX”的前一个定义*

------

重定义给编译器制造了额外的工作，开发中要尽可能避免。

## 1.3 避免重复包含

有些头文件不可避免的会在多个文件中include，在程序预处理阶段，如果多次include同一个头文件，会增加预处理器的检查负担。

为避免上述问题，有两种方式：

### 1.3.1 使用#pragma once

在头文件开头添加#pragma once，编译器会保证同一个头文件只include一次

但这种写法不兼容一些老版本

```c++
// define02.h
#pragma once
#define MIN = 0;
```

### 1.3.2 使用#ifndef

兼容性更好的写法，但是缺点是每次都得想一个宏的名字作为开关

```C++
#ifndef SWITCH
#define SWITCH "on"
	// 执行要执行的声明、定义语句
#endif
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

## 6.3 const修饰指针

```C++
	int a = 10;
	int b = 20;

	// const修饰int*，代表这个指针指向的值不能修改
	// 可以理解为*p不能修改
	const int* p1 = &a;
	//*p1 = 20;	// 报错，*p1不能修改
	a = 20;		// a本身还是可以修改的
	p1 = &b;	// p1的指针也是可以修改的

	// const修饰p，代表这个指针不能修改
	// 可以理解为p不能修改
	int* const p2 = &a;
	*p2 = 20;	// *p2可以修改
	//p2 = &b;	// 报错，p2不可修改

	// const同时修饰int*和p，代表指针以及指针指向的值都不可修改
	const int* const p3 = &a;
	//*p3 = 30;	// 报错，*p3不可修改
	//p3 = &b;	// 报错，p3不可修改

```

## 6.4 指针和数组

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

## 6.5 函数传递地址

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

# 9. 引用

## 9.1 引用基本用法

```C++
	int a = 10;
	int b = 20;

	// 数据类型& 引用变量名 = 变量名
	// 创建a的引用，命名为c，相当于给变量a创建了个别名c
	int& c = a;
	//int& c;	// 引用必须在定义时赋值，本行报错
	
	// 对引用的修改，会影响引用和原变量
	c = 100;
	cout << "a = " << a << endl;	// a = 100
	cout << "c = " << c << endl;	// c = 100

	// 引用被创建为a的别名后，就不能修改为b的别名
	// 下面的代码是赋值，并非将c修改为b的引用
	c = b;
```

## 9.2 引用作为函数参数

```c++
// 直接传入，无法修改
void swap01(int a, int b)
{
	int temp = a;
	a = b;
	b = temp;
}

// 通过指针，可以修改
void swap02(int* a, int* b)
{
	int temp = *a;
	*a = *b;
	*b = temp;
}

// 通过引用，可以修改
void swap03(int& a, int& b)
{
	int temp = a;
	a = b;
	b = temp;
}


int main()
{
	int a = 10;
	int b = 20;

	//swap01(a, b);
	//swap02(&a, &b);
	swap03(a, b);
	
	cout << "a = " << a << endl;	// a = 100
	cout << "b = " << b << endl;	// c = 100


	system("pause");
	return 0;
}
```

## 9.3 引用做函数返回值

```C++
// 不要直接返回局部变量的引用
// 编译器会清理其内存
int& test01()
{
	int a = 10;
	return a;
}

int& test02()
{
	static int a = 10;
	return a;
}

int test03()
{
	return 10;
}

int main()
{
	int& a = test01();
	cout << "a = " << a << endl;	// a = 10
	cout << "a = " << a << endl;	// 内存被清理

	int& b = test02();
	cout << "b = " << b << endl;	// a = 10
	cout << "b = " << b << endl;	// a = 10

	// 当函数的返回值是引用时，可以作为左值
	test02() = 1000;
	cout << "test02() = " << test02() << endl;
	cout << "b = " << b << endl;

	// 返回值不是引用的函数无法作为左值
	// test03() = 1000;

	system("pause");
	return 0;
}
```

## 9.4 引用的本质

```C++
// 发现是引用，转换为int* const ref = &a
void func(int& ref)
{
	// ref是引用，转换为*ref = 100
	ref = 100;
}

int main()
{
	int a = 10;

	// 发现是引用，自动转换为int* const ref = &a
	// ref指向的地址不可修改，也说明为什么引用不可修改
	int& ref = a;

	// ref是引用，自动转换为*ref = 20;
	ref = 20;

	system("pause");
	return 0;
}
```

## 9.5 const修饰引用

```C++
void printValue(const int& a)
{
	// 与const修饰指针一样，对于不想被函数修改的引用，作为传参时
	// 可以通过const修饰避免修改，如下代码报错
	//a = 1000;

	cout << "a = " << a << endl;
}

int main()
{
	int a = 10;

	// 引用无法直接指向数值，如下代码报错
	// int& b = 10;

	// 但被const修饰的引用可以直接指向数值
	// 如下代码实际上相当于指向了一个临时变量：
	// int temp = 10;
	// const int& b = temp;
	const int& b = 10;

	printValue(a);

	system("pause");
	return 0;
}
```

# 10. 函数高级

## 10.1 函数的默认参数

```C++
int addNum01(int a, int b, int c)
{
	return a + b + c;
}

// 可以给函数默认值，语法：
// 函数返回值 函数名(参数 = 参数值)
// 有默认值的传参需位于右方
int addNum02(int a, int b = 30, int c = 30)
{
	return a + b + c;
}

// 如下代码报错，有默认值得传参需位于右方
//int addNum03(int a, int b = 30, int c)
//{
//	return a + b + c;
//}


// 如下代码报错，函数的声明和定义只能有一处有默认值
// 否则编译器不知道应选择哪个
//int addNum04(int a, int b = 30, int c = 30);
//int addNum04(int a, int b = 30, int c = 30)
//{
//	return a + b + c;
//}

int main()
{
	int a = 10;
	int b = 20;
	int c = 30;

	// 60
	cout << "addNum01(a, b, c) = " << addNum01(a, b, c) << endl;

	// 有默认值的部分可以缺省，如下返回70
	cout << "addNum02(a) = " << addNum02(a) << endl;	

	// 返回40
	cout << "addNum02(a, 0) = " << addNum02(a, 0) << endl;

	// 优先传入的值而非默认值，返回60
	cout << "addNum02(a, b, c) = " << addNum02(a, b, c) << endl;

	system("pause");
	return 0;
}
```

## 10.2 函数的占位参数

```c++
// 占位参数，只有数据类型，无变量名
void printNum(int a, int)
{
	cout << "a = " << a  << " in printNum() " << endl;
}

// 占位参数也可以有默认参数
void printNum02(int a, int = 10)
{
	cout << "a = " << a << " in printNum02" << endl;
}

int main()
{
	int a = 10;

	// 传参时，占位参数也必须传值
	printNum(a, 10);
	// 如下代码，不给占位参数传值，报错
	//printNum(a);

	// 对于有默认参数的占位参数，可以不给占位参数传值
	printNum02(a);

	system("pause");
	return 0;
}
```

## 10.3 函数重载

```C++
// C++函数重载方式与java基本一致，通过函数参数重载，不能通过返回值重载
void func();
void func(int a);
void func(double a);
void func(int a, double b);
void func(double a, int b);
//int func();	// 不能通过返回值重载

int main()
{
	system("pause");
	return 0;
}
```

## 10.4 重载与引用

```C++
// 如下两个函数是可以重载的
void func(int& a)
{
	cout << "func(int& a)" << endl;
}

void func(const int& a)
{
	cout << "func(const int& a)" << endl;
}

int main()
{
	int a = 10;

	// 传入变量时，执行func(int& a)
	func(a);

	// 传入常量时，执行func(const int& a)
	// 假如执行func(int& a)，int& a = 10是非法的
	func(10);

	// const常量，执行func(const int& a)
	const int b = 20;
	func(b);

	system("pause");
	return 0;
}
```

## 10.5 重载与默认参数

```C++
void func(int a)
{
	cout << "func(int a)" << endl;
}

void func(int a, int b = 10)
{
	cout << "func(int a, int b)" << endl;
}

int main()
{
	int a = 10;

	// 如下代码报错，因为编译器不知道选择哪个方法执行
	// 所以当有函数重载时，尽可能不要设置默认参数
	//func(10);

	system("pause");
	return 0;
}
```

# 11. 类和对象



## 11.1 简单示例

与java类似，注意差别：

1. 类的定义需要有访问权限
2. 类的定义末尾需要有分号
3. 对象的创建无需new关键字

```C++
// 定义一个类
// 语法： class 类名{访问权限: 属性/行为};
class Student
{
// 访问权限
public:

	// C++中叫成员变量
	string name;

	// C++中叫成员函数
	void introduceSelf()
	{
		cout << "我叫： " << name << endl;
	}
};

int main()
{
	// 实例化类无需new关键字
	Student john;
	john.name = "john";
	john.introduceSelf();

	system("pause");
	return 0;
}
```

## 11.2 访问权限

```c++
class Student
{
// 访问权限
// public权限，类内部类外部均可访问
public:
	string name;

	void introduceSelf()
	{
		cout << "我叫： " << name << endl;
	}

// protected访问权限，本类和子类可以访问
protected:
	string car;

// private访问权限，仅本类可以访问
private:
	string password;
};

int main()
{
	Student john;
	john.name = "john";
	john.introduceSelf();

	//john.car = "benz"; // car是protected的成员，外部无法访问
	//john.password = "123456";	// password是private的成员，外部无法访问

	system("pause");
	return 0;
}
```

## 11.3 struct与class的区别

```C++
class Clazz
{
	// class中的成员默认是private的
	int a;
};

struct struct_test
{
	// struct中的成员默认是public的
	int a;

	void func()
	{
		cout << "struct中可以定义函数" << endl;
	}

protected:
	int b;

private:
	int c;

	void func02()
	{
		cout << "struct中，可以给成员函数或者成员变量设置访问权限" << endl;
	}
};

int main()
{
	struct_test s1;
	s1.a = 10;
	s1.func();

	// struct也可以跟class一样，配置protected和private的成员
	// 如下两个成员变量或成员函数，因为不是public的，所以无法访问，报错
	//s1.b = 20;
	//s1.c = 30;
	//s1.func02();

	Clazz c1;
	// class中的成员默认是private的，如下代码报错
	//c1.a = 10;

	system("pause");
	return 0;
}
```

## 11.4 工程中的常用写法

在C++项目中，一个类通常拆分到.h和.cpp两个文件中，前者用于声明，后者用于定义，语法如下：

```c++
// Point.h
// 防止被多次include(多次include会导致多次编译头文件)
#pragma once
#include <iostream>

using namespace std;

class Point
{
public:
	void setX(int x);
	void setY(int y);

private:
	int m_x;
	int m_y;
};
```

```C++
#include "Point.h"

// Point::表示这是Point类的成员函数的定义
void Point::setX(int x)
{
	m_x = x;
}

void Point::setY(int y)
{
	m_y = y;
}
```

