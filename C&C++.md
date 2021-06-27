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

![image-20210606200714945](.\images\image-20210606200714945.png)

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
// C++中如下写法不会报错，根据就近原则，i是内部for循环的值。
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
cout << arr4[0] << endl; // 未初始化，打印当前内存的内容（不可预估）

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
} s3; // 创建结构体变量方法3

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
// 结构体中使用const的场景：
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
	// a = 1000;

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

# 11. 类和对象基础



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
// Point.cpp
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

## 11.5 构造函数和析构函数

C++中，对象的创建会执行构造函数，销毁会执行析构函数，C++会默认创建空参、空实现的构造函数与析构函数，定义类时，也可以自行实现。

简单示例如下：

```C++
class Person
{
public:
	// 语法： 类名(){}
	// 空参构造函数（默认构造函数）
	// 可以重载
	// 访问修饰符改为protected或private后，无法通过空参构造创建
	Person()
	{
		cout << "Person的构造函数打印" << endl;
	}

	// 语法： ~类名(){}
	// 空参析构函数
	// 不可以重载
	// 必须配置成public的，否则无法访问，无法销毁
	~Person()
	{
		cout << "Person的析构函数打印" << endl;
	}
};

void test()
{
	// 局部变量，在创建完成之后，会调用析构函数
	Person p1;
}

int main()
{
	test();
	
	// return 0;执行之前，调用析构函数
	Person p2;

	system("pause");
	return 0;
}
```

## 11.6 构造函数的分类及调用

构造函数分类：

按参数分为：无参构造和有参构造

按类型分为：普通构造和拷贝构造

构造函数的调用：

分为括号法、显示法、隐式转换法

```C++
#include <iostream>

using namespace std;

// 1、构造函数的分类
class Person
{
public:
	// 无参构造（默认构造）
	Person()
	{
		cout << "Person的无参构造函数打印" << endl;

	}

	// 有参构造
	Person(int age)
	{
		cout << "Person的有参构造函数打印" << endl;
		this->age = age;
	}

	Person(string name, int age)
	{
		cout << "Person的多参数构造函数打印" << endl;
		this->name = name;
		this->age = age;
	}

	// 拷贝构造函数
	// 使用const引用，一般用于复制某个已有对象
	Person(const Person& p)
	{
		cout << "Person的拷贝构造函数打印" << endl;
		name = p.name;
		age = p.age;
	}

	~Person()
	{
		cout << "Person的析构函数打印" << endl;
	}

private:
	string name;
	int age;
};

// 2、构造函数的调用
void test()
{
	// 1、括号法，常用
	Person p1;
	Person p2(10);
	Person p3("lilei", 18);
	Person p4(p2);	// 括号法调用拷贝构造函数

	// 注意1：调用无参构造函数不能加括号，编译器会认为是函数声明
	// 下面写法会被认为是声明，并非无参构造函数
	Person p5();

	// 2、显示法
	Person p6 = Person();
	Person p7 = Person(10);
	Person p8 = Person("hanmeimei", 16);
	Person p9 = Person(p8);

	// Person(10)单独写就是匿名对象，当前行结束之后，马上析构
	// 例如下面两行的例子：
	Person(19);
	cout << "Person(19)匿名对象的析构函数会先于本行打印" << endl;

	// 注意2：不能利用拷贝构造函数初始化匿名对象
	// 如下代码会报错：Person p10重定义，原因在于Person(p10)编译器会认为是 Person p10;
	Person p10 = Person(10);
	Person(p10);

	// 3、隐式转换法
	Person p11 = {};
	Person p12 = {10};
	Person p13 = 10;	// 只有一个参数时，大括号可以省略
	Person p14 = { "banana", 19 };
	Person p15 = { p13 };
	Person p16 = p13;
}

int main()
{
	test();

	// return 0;执行之前，调用析构函数

	system("pause");
	return 0;
}
```

## 11.7 拷贝构造函数的调用时机

先说明下C++中函数返回的时候做的事情：

先在新的地址上创建一个返回值得拷贝，然后回收函数里的局部返回值变量，最后退出函数。

例如，对于返回结构体与普通int类型的函数：

```C++
#include <iostream>

using namespace std;

struct structTest 
{
	int a;
};

structTest test01()
{
	structTest s1 = { 10 };
	cout << "struct s1 的地址： " << (int)&s1 << endl;

	return s1;
}

int test03()
{
	int a = 1;
	cout << "a 的地址： " << (int)&a << endl;
	return a;
}

int main()
{
	structTest s2 = test01();
	cout << "struct s2 的地址： " << (int)&s2 << endl;

	int b = test03();
	cout << "b 的地址： " << (int)&b << endl;

	system("pause");
	return 0;
}
```

打印结果：（可以看到地址不同）

```
struct s1 的地址： -1003489340
struct s2 的地址： -1003489052
a 的地址： -1003489340
b 的地址： -1003489020
```

下面说明下拷贝构造函数的三个调用时机：

1、 用于对象复制，可以参考11.6节 中拷贝构造函数的调用，用于将指定对象中的属性复制到新创建的对象中，不再举例。

2、 函数传参通过类接收的场景

3、 函数返回值是类对象的场景

场景2举例：

```C++
#include <iostream>

using namespace std;

class Person
{
public:
	Person()
	{
		cout << "Person 默认构造函数" << endl;
	}

	Person(const Person& p)
	{
		cout << "Person 拷贝构造函数" << endl;
	}

	~Person()
	{
		cout << "Person 析构函数" << endl;
	}
};

void test01(Person p)
{
}

int main()
{
	Person p;
	// 注意这里test01()是直接接收Person对象的（而不是通过引用）
	// 相当于执行了Person p = p，这是之前讲过的隐式转换法调用构造函数
	// 故在执行test01(p)时，调用了一次拷贝构造函数
	test01(p);

	system("pause");
	return 0;
}
```

场景2打印：可以看到执行了一次拷贝构造函数

```
Person 默认构造函数
Person 拷贝构造函数
Person 析构函数
```

场景3举例：

```C++
#include <iostream>

using namespace std;

class Person
{
public:
	Person()
	{
		cout << "Person 默认构造函数" << endl;
	}

	Person(const Person& p)
	{
		cout << "Person 拷贝构造函数" << endl;
	}

	~Person()
	{
		cout << "Person 析构函数" << endl;
	}
};

Person test02()
{
	Person p1;
	cout << "Person p1 的地址： " << (int)&p1 << endl;

	return p1;
}

int main()
{
	Person p2 = test02();
	cout << "Person p2 的地址： " << (int)&p2 << endl;

	system("pause");
	return 0;
}
```

场景3打印：可以看到执行了拷贝构造函数

```C++
Person 默认构造函数
Person p1 的地址： 1771764356
Person 拷贝构造函数
Person 析构函数
Person p2 的地址： 1771764676
```

## 11.8 构造函数的调用规则

默认情况下，C++编译器至少给一个类三个构造函数：

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造函数，对属性值进行浅拷贝

构造函数 调用规则如下：

- 如果用户定义有参构造函数，c++不在提供默认无参构造，但是会提供默认拷贝构造
- 如果用户定义拷贝构造函数，c++不再提供默认无参构造

```C++
#include <iostream>

using namespace std;

class Person
{
public:
	int age;
};

class Book
{
public:
	Book(string bookName)
	{
		this->bookName = bookName;
	}
private:
	string bookName;
};

class Student
{
public:
	Student(const Student& student)
	{
		name = student.name;
	}
private:
	string name;
};

int main()
{
	int age = 10;

	Person p1;	// 编译器默认创建无参构造函数
	p1.age = 10;
	Person p2(p1);	// 编译器会默认创建拷贝构造函数

	string bookName = "Harry Potter";
	Book book1 = bookName;
	// Book book2;	// 如果主动定义了有参构造函数，那么编译器就不会帮我们定义默认的构造函数，左侧报错
	Book book3 = {book1}; // 编译器还是会帮我们创建默认的拷贝构造函数的

	//Student john;	// 同样的，如果主动定义了拷贝构造函数，那么编译器不会帮我们定义默认的构造函数，左侧报错

	system("pause");
	return 0;
}
```

## 11.9 深拷贝与浅拷贝

先看一个类的成员变量是指针的例子，思考下会打印什么：

```c++
#include <iostream>

using namespace std;

class Person
{
public:
	Person(string name, int age)
	{
		this->name = name;
		this->age = &age;
	}

	~Person()
	{
	}

	string name;
	int* age;
};

void test()
{
	Person p1 = { "john", 18 };
	Person p2 = p1;
	p1.name = "xixi";
	*p1.age = 20;

	cout << "p1的名字：" << p1.name << " p1的年龄：" << *p1.age << endl;
	cout << "p2的名字：" << p2.name << " p2的年龄：" << *p2.age << endl;
}

int main()
{
	test();
	system("pause");
	return 0;
}
```

打印结果：

```
p1的名字：xixi p1的年龄：20
p2的名字：john p2的年龄：20
```

**如果该指针对应的成员变量，是指向堆空间的，由于编译器不会帮我们主动施放，所以需要在析构函数中施放该内存空间***

但是，写了析构函数后，如下代码执行过程中报错：*C++核心编程.exe 已触发了一个断点。*

```C++
#include <iostream>

using namespace std;

// 浅拷贝执行报错的例子
class Person
{
public:
	Person(string name, int age)
	{
		this->name = name;

		// 指向堆空间
		this->age = new int(age);
	}

	~Person()
	{
		// 通过析构函数回收内存
		if (age != NULL)
		{
			delete age;

			// 置为NULL，避免野指针
			age = NULL;
		}
	}

	string name;
	int* age;
};

void test()
{
	Person p1 = { "john", 18 };
	Person p2(p1);

	cout << "p1的名字：" << p1.name << " p1的年龄：" << *p1.age << endl;
	cout << "p2的名字：" << p2.name << " p2的年龄：" << *p2.age << endl;
}

int main()
{
	test();
	system("pause");
	return 0;
}
```

上述代码执行报错的原因在于，C++默认创建的拷贝构造函数，只是简单地进行了复制。当两个对象有指向同一块堆空间的成员时，如果一个对象先析构回收了该空间，另一个对象析构时就会报错。

例如对于上面的代码，编译器默认给实现的拷贝构造函数如下：

```C++
	Person(const Person& p)
	{
		name = p.name;
		age = p.age;
	}
```

可以通过**深拷贝**解决上述问题，**当有开辟在堆区的成员变量时，一定要自己提供拷贝构造函数，避免浅拷贝的问题**

```C++
	Person(const Person& p)
	{
		cout << "执行拷贝构造函数" << endl;
		name = p.name;
		age = new int(*p.age);
	}
```

最终代码&执行结果：

```C++
#include <iostream>

using namespace std;

// 浅拷贝执行报错的例子
class Person
{
public:
	Person(string name, int age)
	{
		cout << "执行有参构造函数" << endl;
		this->name = name;

		// 指向堆空间
		this->age = new int(age);
	}

	Person(const Person& p)
	{
		cout << "执行拷贝构造函数" << endl;
		name = p.name;
		age = new int(*p.age);
	}

	~Person()
	{
		cout << "执行析构" << endl;
		// 通过析构函数回收内存
		if (age != NULL)
		{
			delete age;

			// 置为NULL，避免野指针
			age = NULL;
		}
	}

	string name;
	int* age;
};

void test()
{
	Person p1 = { "john", 18 };
	Person p2(p1);

	cout << "p1的名字：" << p1.name << " p1的年龄：" << *p1.age << endl;
	cout << "p2的名字：" << p2.name << " p2的年龄：" << *p2.age << endl;
}

int main()
{
	test();
	system("pause");
	return 0;
}
```

```
执行有参构造函数
执行拷贝构造函数
p1的名字：john p1的年龄：18
p2的名字：john p2的年龄：18
执行析构
执行析构
```

*注：关于析构的顺序，后创建的会先析构*

## 11.10 初始化列表

C++提供了初始化列表语法，用来初始化属性

**语法：**`构造函数()：属性1(值1),属性2（值2）... {}`

```C++
#include <iostream>

using namespace std;

class Person
{
public:
	//// 传统初始化方法
	//Person(int a, int b, int c)
	//{
	//	this->a = a;
	//	this->b = b;
	//	this->c = c;
	//}

	// 通过初始化列表
	Person(int a, int b, int c) : a(a), b(b), c(c)
	{
		cout << "执行其他逻辑" << endl;
		// this->a = 30;	// 这里可以覆盖前面的初始化结果
	}
	
	// 可以只针对部分变量进行初始化
	Person(int a, int b) : a(a), b(b) {}

	// 也可以这么初始化，但只能传固定值
	Person() :a(10), b(20), c(30) {};
	int a;
	int b;
	int c;
};

int main()
{
	Person p1 = Person(10, 20, 30);

	Person p2 = Person(10, 20);
	// c未初始化，打印结果：p2.c-858993460
	cout << "p2.c" << p2.c << endl;	

	system("pause");
	return 0;
}
```

从概念上来讲，构造函数的执行可以分为两个阶段：**初始化阶段**和**计算阶段**

初始化阶段优先执行初始化列表里的代码，对于不在初始化列表中的代码，会采用默认赋值，string默认为空，int等默认不赋值。

**对于类中的对象，如果没有在初始化列表，会使用默认的空参构造函数初始化。**

例如下面的代码，会执行Phone类默认的空参构造函数

```C++
#include <iostream>

using namespace std;

// 类对象作为类成员时，初始化阶段默认调用空参构造函数
class Phone
{
public:
	Phone() 
	{
		cout << "Phone默认构造函数" << endl;
	}
	string phoneName;
};

class Person
{
public:
	string name;
	Phone phone;
};

int main()
{
	Person p1;
	cout << "p1.name " << p1.name << "  ." << endl;
	cout << "p1.phone.phoneName " << p1.phone.phoneName << "  ." << endl;

	system("pause");
	return 0;
}
```

打印结果：

```
Phone默认构造函数
p1.name   .
p1.phone.phoneName   .
```

假如Phone类没有定义无参构造函数，代码会报错：

```c++
#include <iostream>

using namespace std;

// 没有默认空参构造函数，本文件编译报错
class Phone
{
public:
	Phone(string phoneName)
	{
		cout << "Phone有参构造函数" << endl;
	}
	string phoneName;
};

class Person
{
public:
	Person(string name, Phone phone)
	{
		// 报错：“类Phone不存在默认构造函数”
	}
	string name;
	Phone phone;
};

int main()
{
	system("pause");
	return 0;
}
```



所以得出结论：**类对象作为类成员时，如果没有默认构造函数，一定要通过初始化列表的方式，初始化该成员**

具体做法参考11.11节

## 11.11 类对象作为类成员

A类作为B类的成员，且通过初始化列表的方式初始化时，A类和B类的构造函数执行顺序如何？

```C++
#include <iostream>

using namespace std;

class Phone
{
public:
	Phone(string phoneName)
	{
		cout << "Phone构造函数" << endl;
		this->phoneName = phoneName;
	}

	Phone(string phoneName, int phoneAge)
	{
		cout << "Phone构造函数" << endl;
		this->phoneName = phoneName;
		this->phoneAge = phoneAge;
	}

	~Phone()
	{
		cout << "Phone析构函数" << endl;
	}

	string phoneName;
	int phoneAge;
};

class Person
{
public:
	// 普通的写法，直接把Phone对象传过来
	// 但这种写法必须手动创建一个Phone对象传进来才行
	Person(string name, Phone phone) :name(name), phone(phone) {}

	// 下面的写法可以不手动创建一个Phone对象
	// 利用初始化列表，初始化Phone对象
	// phone(phoneName)相当于执行了如下代码：即是一次隐式转换法创建对象的过程
	// phone = phoneName
	Person(string name, string phoneName) :name(name), phone(phoneName) 
	{
		cout << "Person构造函数" << endl;
	}

	// 初始化块也可以传入多个参数，相当于：
	// phone = {phoneName, phoneAge}
	Person(string name, string phoneName, int phoneAge) :name(name), phone(phoneName, phoneAge) 
	{
		cout << "Person构造函数" << endl;
	}

	~Person()
	{
		cout << "Person析构函数" << endl;
	}

	Phone phone;
	string name;
};

void test()
{
	Person p1("john", "HuaWei");
	cout << p1.name << "\t购买了\t" << p1.phone.phoneName << "手机" << endl;

	Person p2("Tom", "XiaoMi", 2);
	cout << p2.name << "\t购买了\t" << p2.phone.phoneName << "手机, 已经使用了\t" << p2.phone.phoneAge << "年了" << endl;
}

int main()
{
	test();
	system("pause");
	return 0;
}
```

打印结果：可以看到Phone作为Person的成员，先于Person创建。然后根据C++的规则，先创建的后析构。

```
Phone构造函数
Person构造函数
john    购买了  HuaWei手机
Phone构造函数
Person构造函数
Tom     购买了  XiaoMi手机, 已经使用了  2年了
Person析构函数
Phone析构函数
Person析构函数
Phone析构函数
```

## 11.12 静态成员

静态成员就是在成员变量和成员函数前加上关键字static，称为静态成员

静态成员分为：

- 静态成员变量
  - 所有对象共享同一份数据
  - 在编译阶段分配内存
  - 类内声明，类外初始化
- 静态成员函数
  - 所有对象共享同一个函数
  - 静态成员函数只能访问静态成员变量

静态成员变量示例：

```C++
#include <iostream>

using namespace std;

class Person
{

public:

	static int m_A; //静态成员变量

	//静态成员变量特点：
	//1 在编译阶段分配内存
	//2 类内声明，类外初始化
	//3 所有对象共享同一份数据

private:
	static int m_B; //静态成员变量也是有访问权限的
};
int Person::m_A = 10;
int Person::m_B = 10;

void test01()
{
	//静态成员变量两种访问方式

	//1、通过对象
	Person p1;
	p1.m_A = 100;
	cout << "p1.m_A = " << p1.m_A << endl;

	Person p2;
	p2.m_A = 200;
	cout << "p1.m_A = " << p1.m_A << endl; //共享同一份数据
	cout << "p2.m_A = " << p2.m_A << endl;

	//2、通过类名
	cout << "m_A = " << Person::m_A << endl;

	//cout << "m_B = " << Person::m_B << endl; //私有权限访问不到
}

int main() {
	test01();

	system("pause");
	return 0;
}

```

静态成员函数举例

```C++
#include <iostream>

using namespace std;

class Person
{
public:
	//静态成员函数特点：
	//1 程序共享一个函数
	//2 静态成员函数只能访问静态成员变量

	static void func()
	{
		cout << "func调用" << endl;
		m_A = 100;
		//m_B = 100; //错误，不可以访问非静态成员变量
	}

	static int m_A; //静态成员变量
	int m_B;
private:

	//静态成员函数也是有访问权限的
	static void func2()
	{
		cout << "func2调用" << endl;
	}
};
int Person::m_A = 10;

void test01()
{
	//静态成员变量两种访问方式

	//1、通过对象
	Person p1;
	p1.func();

	//2、通过类名
	Person::func();

	//Person::func2(); //私有权限访问不到
}

int main() {
	test01();

	system("pause");
	return 0;
}
```

## 11.13 sizeof对象

```C++
class Person1
{
};

class Person2
{
public:
	int age;
	static string name;
	void setAge()
	{
	}
	static void setName()
	{
	}
};

int main() {

	// 空的类对象，sizeof返回1
	Person1 p1;
	cout << "sizeof(p1): " << sizeof(p1) << endl;
	
	// 静态成员变量、静态成员函数、普通成员函数，都不在对象中
	// sizeof仅包含普通成员变量，此处为int，sizeof(p2)返回4
	Person2 p2;
	cout << "sizeof(p2): " << sizeof(p2) << endl;

	system("pause");
	return 0;
}
```

## 11.14 this关键字

this默认创建，是一个指向本对象的指针，主要用途有：

1. 用于区分传参
2. 用于实现链式调用

```C++
class Person
{
public:
	Person(int age)
	{
		// this用法01,用于区分重名的形参与成员变量
		this->age = age;
	}

	Person& addAge(int num)
	{
		age += num;
		// this的用法02，用于链式调用
		return *this;
	}

	int age;
};

int main() {
	Person p(10);
	cout << "p.age: " << p.age << endl;

	p.addAge(2).addAge(2).addAge(2);
	cout << "p.age: " << p.age << endl;

	system("pause");
	return 0;
}
```

## 11.15 空指针访问成员函数

C++中，指向对象的空指针是可以直接访问成员函数的，是否报空指针需要看该成员函数中是否用到了this。

```C++
class Person
{
public:
	void showPerson()
	{
		cout << "show Person" << endl;
	}

	void showPersonAge()
	{
		// age相当于this->age,当this为NULL时报错
		cout << "show Person age" << age << endl;
	}

	int age;
};

int main() {
	// 创建一个空指针，指向类型为Person
	Person* p = NULL;

	// showPerson()不涉及this，可正常执行
	p->showPerson();

	// showPersonAge()用到了Person的成员变量,报异常
	// p->showPersonAge();

	system("pause");
	return 0;
}
```

## 11.16 常函数与常对象

**常函数：**

- 成员函数后加const后我们称这个函数为常函数
- 常函数内不可以修改成员属性
- 成员属性声明时加关键字mutable后，在常函数中依然可以修改

**常对象：**

- 声明对象前加const称该对象为常对象
- 常对象只能调用常函数

```
class Person
{
public:
	// 常函数，注意const的位置
	void showName() const
	{
		// 常函数不可修改成员变量，如下代码报错
		// age = 10;

		// mutable关键字修饰的成员变量可以修改
		name = "lilei";
		cout << "name is " << name << endl;
	}

	void changeAge()
	{
		// 普通成员函数可以修改成员变量
		// 一个类中，常函数与普通成员函数可以同时存在
		age = 10;
	}

	mutable string name;
	int age;
};

int main() {
	Person p1 = Person();
	p1.showName();
	p1.changeAge();

	// 创建对象时，前面加上const，表示这是一个常对象
	const Person p2;
	// 常对象只能调用常函数
	p2.showName();
	// 常对象不能调用非常函数，如下代码报错
	// p2.changeAge();
	// 常变量的内容不可以修改，如下代码报错
	// p2.age = 10;
	// 常对象可以修改mutable修饰的成员变量
	p2.name = "hanmeimei";

	system("pause");
	return 0;
}
```

常函数常对象的本质：

this实际上是一个不可变的指向对象的指针，例如对于Person类的对象：

**this 等效于 Person* const this**

	// 常函数和常对象的本质：
	// 普通对象的this：Person* const this
	// 常对象的this：const Person* const this   作用范围，整个对象
	// 常函数的this：const Person* const this   作用范围，仅常函数

# 12. 友元

友元的目的就是让一个函数或者类 访问另一个类中私有成员

友元的关键字为 friend

友元的三种实现

- 全局函数做友元
- 类做友元
- 成员函数做友元

## 12.1 全局函数做友元

```C++
#include <iostream>

using namespace std;

class Person
{
	// 如下两个函数可以访问Person的非public内容
	friend void getAge(Person& person);
	friend void getAge2(Person* person);
public:
	Person(int age)
	{
		this->age = age;
	}
private:
	int age;
};

void getAge(Person& person)
{
	cout << "person.age: " << person.age << endl;
}

void getAge2(Person* person)
{
	cout << "person.age: " << person->age << endl;
}

int main() {
	Person p(10);
	getAge(p);
	getAge2(&p);

	system("pause");
	return 0;
}
```

## 12.2 类做友元

```C++
#include <iostream>

using namespace std;

// 由于Student需要使用Teacher类作为友元，所以Teacher提前声明
class Teacher;
class Student
{
	// Teacher类是Student类的友元，可以访问Student类的非public内容
	friend class Teacher;
public:
	Student(int age)
	{
		this->age = age;
	}
private:
	int age;
};

class Teacher
{
public:
	void getAge(Student& student)
	{
		cout << "student的年龄： " << student.age << endl;
	}

	void getAge2(Student* student)
	{
		cout << "student的年龄： " << student->age << endl;
	}
};


int main() {
	Teacher t;
	Student s(10);
	t.getAge(s);
	t.getAge2(&s);

	system("pause");
	return 0;
}
```

## 12.3 成员函数做友元

说明本节之前，先看个可能犯的错误：**“使用了未定义的类型”**

当某个函数定义时，用到了某个尚未定义的类，会报此错误，例如：

```C++
// 仅做了个空声明
class Teacher;
class Student
{
public:
	// 这里不会报错，没有用到Teacher的构造函数或者成员函数
	Student(Teacher& teacher)
	{
	}

	Teacher& showMyTeacher(Teacher& teacher)
	{
		// 传参和返回值使用引用，不会调用构造函数，不报错
	}

	//void showMyTeacher()
	//{
	//	// 这里显示调用了Teacher的构造函数，但尚未定义，报错
	//	Teacher t;
	//}

	//Teacher getTeacher(Teacher& teacher)
	//{
	//	// 直接返回Teacher，会调用Teacher的构造函数，报错
	//	return teacher;
	//}

	//void showMyTeacher(Teacher teacher)
	//{
	//	// 直接传teacher，同样会调用Teacher的构造函数，报错
	//}
};

class Teacher
{
};

int main() {
	system("pause");
	return 0;
}
```

改成如下写法，补充下相关的声明后，不再报错：

```C++
#include <iostream>

using namespace std;

// 声明下空参构造函数
class Teacher
{
public:
	Teacher();
};
class Student
{
public:
	Student(Teacher& teacher)
	{
	}

	Teacher& showMyTeacher(Teacher& teacher)
	{
	}

	void showMyTeacher()
	{
		Teacher t;
	}

	Teacher getTeacher(Teacher& teacher)
	{
		return teacher;
	}

	void showMyTeacher(Teacher teacher)
	{
	}
};

Teacher::Teacher()
{
}

int main() {
	system("pause");
	return 0;
}
```

总结下：

**尽可能使用引用或指针用于传参或返回值**

**注意声明的顺序，注意隐藏的构造函数调用**

下面看一个成员函数做友元的例子：

```C++
#include <iostream>

using namespace std;

class Student;
class Teacher
{
public:
	int getAge(Student& student);
	int getAge2(Student* student);
};

class Student
{
	// Teacher的getAge成员函数作为Student的友元，可以访问非public的内容
	friend int Teacher::getAge(Student& student);
private:
	int age;
};

int Teacher::getAge(Student& student)
{
	return student.age;
}

//int Teacher::getAge2(Student* student)
//{
//	// 无法获取到age
//	return student->age;
//}

int main() {
	system("pause");
	return 0;
}
```

# 13. 运算符重载

运算符重载概念：对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

> 总结1：对于内置的数据类型的表达式的的运算符是不可能改变的

> 总结2：不要滥用运算符重载

a，b分别代表两个对象，如下三列是等效的。

| a+b       | a.operator+(b)    | operator+(a, b)     |
| --------- | ----------------- | ------------------- |
| cout << a | 不推荐            | operator<<(cout, a) |
| ++a       | a.operator++()    | operator++(a)       |
| a++       | a.operator++(int) | operator++(a, int)  |
| a = b     | a.operator=(b)    | 不支持              |
|           |                   |                     |
|           |                   |                     |



## 13.1 加号运算符重载

```C++
#include <iostream>

using namespace std;

// 加号运算符重载
class WeightCal
{
	friend void test();
	friend WeightCal operator+(WeightCal w1, WeightCal w2);
public:
	WeightCal() {}

	WeightCal(int weight)
	{
		this->weight = weight;
	}

	// 加号运算符重载方法01：使用成员函数
	// 需要链式调用，返回值不能是void
	// 返回一个新的副本，返回值不能是引用或指针
	WeightCal operator+(const WeightCal& weightCal)
	{
		WeightCal temp;
		temp.weight = weightCal.weight + weight;
		return temp;
	}
private:
	int weight;
};

// 加号运算符重载方法02：使用全局函数
WeightCal operator+(const WeightCal w1, const WeightCal w2)
{
	WeightCal temp;
	temp.weight = w1.weight + w2.weight;
	return temp;
}

void test()
{
	WeightCal w1(100);
	WeightCal w2(200);
	WeightCal w3 = w1 + w2;
	cout << "w3.weight ：" << w3.weight << endl;
}

int main() {
	test();

	system("pause");
	return 0;
}
```

## 13.2 左移运算符重载

```C++
#include <iostream>

using namespace std;

// 左移运算符重载
// 目标：通过cout << 将Person类的name成员变量打印到控制台
class Person
{
	friend ostream& operator<<(ostream& out, Person p);
public:
	Person(string name)
	{
		this->name = name;
	}

	// 成员函数的方式，只能实现 p << cout，无法实现 cout << p
	//ostream& operator<<(ostream& out)
	//{
	//	out << name;
	//	return out;
	//}

private:
	string name;
};

// 注意返回值，这里返回同一个out对象，使用引用
ostream& operator<<(ostream& out, Person p)
{
	out << p.name;
	return out;
}

int main() {
	Person p("lilei");
	cout << p << endl;

	system("pause");
	return 0;
}
```

## 13.3 递增运算符重载

前置递增运算符重载：

```C++
#include <iostream>

using namespace std;

// 递增运算符重载（前置递增）
// 目标：实现一个自定义的类封装int，每次递增2，并实现前置递增与后置递增
class MyInteger
{
	friend ostream& operator<<(ostream& cout, MyInteger integer);
	friend MyInteger& operator++(MyInteger& integer);
public:
	MyInteger(int integer)
	{
		this->integer = integer;
	}

	// 前置递增运算符重载01：使用成员函数
	//MyInteger& operator++()
	//{
	//	integer += 2;
	//	return *this;
	//}
private:
	int integer;
};

// 前置递增运算符重载02：使用全局函数
MyInteger& operator++(MyInteger& integer)
{
	integer.integer += 2;
	return integer;
}

ostream& operator<<(ostream& cout, MyInteger integer)
{
	cout << integer.integer;
	return cout;
}

int main() {
	MyInteger integer(10);
	++(++integer);

	// 打印14
	cout << "integer : " << integer << endl;

	system("pause");
	return 0;
}
```

后置递增运算符重载：

```C++
#include <iostream>

using namespace std;

// 递增运算符重载（后置递增）
class MyInteger
{
	friend ostream& operator<<(ostream& cout, MyInteger integer);
	friend MyInteger operator++(MyInteger& integer, int);
public:
	MyInteger(int integer)
	{
		this->integer = integer;
	}

	// 后置递增运算符重载01：使用成员函数
	// 注意返回值，后置递增需要返回一个临时的值，故不能用引用或指针
	// 编译器通过占位参数判断是后置递增，且只能传int
	//MyInteger operator++(int)
	//{
	//	MyInteger temp(integer);
	//	integer += 2;
	//	return temp;
	//}
private:
	int integer;
};

// 后置递增运算符重载02：使用全局函数
MyInteger operator++(MyInteger& integer, int)
{
	MyInteger temp(integer.integer);
	integer.integer += 2;
	return temp;
}

ostream& operator<<(ostream& cout, MyInteger integer)
{
	cout << integer.integer;
	return cout;
}

int main() {
	MyInteger integer(10);

	// 打印10
	cout << "integer : " << integer++ << endl;
	// 打印12
	cout << "integer : " << integer++ << endl;

	system("pause");
	return 0;
}
```



## 13.4 赋值运算符重载

普通的用法：

```C++
#include <iostream>

using namespace std;

// 赋值运算符重载
class Person
{
public:
	Person(int age)
	{
		this->age = age;
	}

	Person& operator=(const Person& person)
	{
		age = person.age;
		return *this;
	}

	int age;
};

int main() {
	Person p1(10);
	Person p2(20);
	Person p3(30);

	p3 = p2 = p1;

	cout << "p1.age : " << p1.age << endl;
	cout << "p2.age : " << p2.age << endl;
	cout << "p3.age : " << p3.age << endl;

	system("pause");
	return 0;
}
```

```
p1.age : 10
p2.age : 10
p3.age : 10
```

当类的成员在堆空间创建时，11.9节讲过，如果调用拷贝构造函数存在浅拷贝的问题，故需要覆盖默认的拷贝构造，采用深拷贝的方式。

同样的问题也存在与赋值运算符的场景，例如如下代码同样存在浅拷贝的问题：

```C++
#include <iostream>

using namespace std;

// 赋值运算符重载
class Person
{
public:
	Person(int age)
	{
		this->age = new int(age);
	}

	~Person()
	{
		if (age != NULL)
		{
			delete age;
			age = NULL;
		}
	}

	int* age;
};

int main() {
	Person p1(10);
	Person p2(20);

	// 赋值操作，执行时报异常
	p2 = p1;

	system("pause");
	return 0;
}
```

通过赋值运算符重载，引入深拷贝解决本问题：

```C++
#include <iostream>

using namespace std;

// 赋值运算符重载：解决浅拷贝问题
class Person
{
public:
	Person(int age)
	{
		this->age = new int(age);
	}

	~Person()
	{
		if (age != NULL)
		{
			delete age;
			age = NULL;
		}
	}

	// 赋值运算符重载01：使用成员函数
	Person& operator=(Person& p)
	{
		// 深拷贝
		age = new int(*p.age);
		return *this;
	}

	int* age;
};

// 赋值运算符重载必须为成员函数，不能是全局函数
// 如下代码报错
//Person& operator=(Person& p1, Person& p2)
//{
//}

int main() {
	Person p1(10);
	Person p2(20);

	// 赋值操作，执行时报异常
	p2 = p1;

	system("pause");
	return 0;
}
```

## 13.5 关系运算符重载

```C++
#include <iostream>

using namespace std;

// 关系运算符重载
class Person
{
public:
	Person(int age)
	{
		this->age = age;
	}

	bool operator==(const Person& p)
	{
		return age == p.age;
	}

	int age;
};

int main() {
	Person p1(10);
	Person p2(20);
	bool flag = p1 == p2;
	if (p1 == p2)
	{
		cout << "p1 == p2" << endl;
	}
	else
	{
		cout << "p1 != p2" << endl;
	}

	system("pause");
	return 0;
}
```

## 13.6 函数调用运算符重载

- 函数调用运算符 () 也可以重载
- 由于重载后使用的方式非常像函数的调用，因此称为**仿函数**
- 仿函数没有固定写法，非常灵活

```C++
#include <iostream>

using namespace std;

// 函数调用运算符重载
class Person
{
public:
	void operator()(string text)
	{
		cout << text << endl;
	}
};

int main() {
	Person p;
	p("Hello World");

	// 直接通过匿名对象调用也是可以的
	Person()("Hello World");

	system("pause");
	return 0;
}
```

