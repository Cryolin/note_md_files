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

## 1.2 #define宏定义

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

## 2.2 布尔型

C++中，布尔型的关键字是bool

## 2.3 数据输入输出

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

注意C++中数组是不可以赋值的，如下代码报错

```C++
	int arr1[1] = {};
	int arr2[1] = {};
	//arr1 = arr2;	// 报错，表达式必须是可修改的左值
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

## 4.3 数组作为函数参数

有三种方法可以将数组传递给函数，但实质上都是传一个指针

```C++
#include <iostream>
using namespace std;

// 1、指针接收
void func1(int* arr)
{
	cout << arr << endl;
}

// 2、未定义大小的数组接收
void func2(int arr[])
{
	cout << arr << endl;
}

// 3、定义大小的数组接收
// 就函数而言，数组的长度是无关紧要的，因为 C++ 不会对形式参数执行边界检查。
void func3(int arr[10])
{
	cout << arr << endl;
}

int main() {

	int arr[2] = {};

	func1(arr);	// 000000972C92F758
	func2(arr);	// 000000972C92F758
	func3(arr);	// 000000972C92F758
	
	system("pause");
	return 0;
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

## 6.6 函数指针

Android中zygote进程加载JNI的代码示例：

```C++
#include <iostream>
using namespace std;

// 函数指针01，android zogote进程注册JNI的代码分析
// 具体位置：frameworks/base/core/jni/AndroidRuntime.cpp

// REG_JNI宏定义，用大括号包起来，用于给下面的结构体初始化赋值
// name前面加井号，代表取宏参数的字符串
#define REG_JNI(name) { name, #name } 

// 这个结构体包含一个函数指针和一个字符串
// 可以通过  结构体变量.mProc()  的方式，使用结构体中的函数指针
struct myType {
    void(*mProc)();
    const char* mName;
};

void print() {
    printf("hello,world\n");
}

// 结构体数组
const myType gRegJNI[] = {
    // 与宏定义替换之后，可以给myType初始化
    // 初始化后，把  print  函数赋值给函数指针
    REG_JNI(print),
};

void register_jni_procs(const myType arrays[], size_t count)
{
    // 打印结果：
    // hello world
    // print
    for (size_t i = 0; i < count; i++)
    {
        arrays[i].mProc();
        cout << arrays[i].mName << endl;
    }
}

int main() {
    register_jni_procs(gRegJNI, 1);
    system("pause");
    return 0;
}
```

函数指针的定义：

```
函数返回值类型 (* 指针变量名) (函数参数列表);
```

“函数返回值类型”表示该指针变量可以指向具有什么返回值类型的函数；“函数参数列表”表示该指针变量可以指向具有什么参数列表的函数。这个参数列表中只需要写函数的参数类型即可。

```C++
#include <iostream>
using namespace std;

int Max(int, int);
int main(void)
{
	// 定义一个函数指针
	int(*p)(int, int);  
	int a = 10;
	int b = 20;

	// 把函数Max赋给指针变量p, 使p指向Max函数
	p = Max;

	//通过函数指针调用Max函数
	int c = (*p)(a, b);

	cout << "c = (*p)(a, b)  : " << c << endl;
	system("pause");
	return 0;
}
int Max(int x, int y)  //定义Max函数
{
	return x > y ? x : y;
}
```

访问结构体、结构体指针、结构体引用中的函数指针，对象、对象指针、对象引用中的函数指针同理

```c++
#include <iostream>
using namespace std;

struct student
{
	int (*max)(int, int);
	string name;
};

int Max(int x, int y)
{
	return x > y ? x : y;
}

// 访问结构体中的函数指针
void test01()
{
	student s1 = {};
	s1.name = "str";
	s1.max = Max;
	int max = s1.max(10, 20);

	cout << "s1.max(10,20) : " << max << endl;
}

// 访问结构体指针中的函数指针
void test02()
{
	student s = {};
	student* sp = &s;
	sp->name = "zhangsan";
	sp->max = Max;
	int max = sp->max(10, 20);

	cout << "s.max(10,20) : " << max << endl;
}

// 访问结构体引用中的函数指针
void test03()
{
	student s = {};
	student& s1 = s;
	s1.name = "lisi";
	s1.max = Max;
	int max = s1.max(10, 20);

	cout << "s1.max(10,20) : " << max << endl;
}

int main(void)
{
	//test01();

	//test02();

	test03();

	system("pause");
	return 0;
}
```

函数指针可以作为函数参数，可以用于回调

```C++
#include <iostream>
using namespace std;

void add(int num1, int num2) {
	printf("num1 + num2 = %d\n", (num1 + num2));
}

void mins(int num1, int num2) {
	printf("num1 - num2 = %d\n", (num1 - num2));
}
// 函数指针作为函数传参
void opreate(void(*method)(int, int), int num1, int num2) {
	// method(num1, num2);也是可以的
	(*method)(num1, num2);
}
void main() {

	// add(1,2);
	opreate(add, 1, 2);
	opreate(mins, 1, 2);

	system("pause");
}
```

## 6.7 void指针

void 指针是一种特殊的指针，表示为“无类型指针”，任何指针都可以直接赋值给void指针

http://c.biancheng.net/view/365.html

```C++
#include <iostream>
using namespace std;

// 如果函数的参数可以是任意类型指针，应该将其参数声明为 void*
// 比较典型的函数有内存操作函数 memcpy 和 memset，如下面的代码所示：
void* memset(void* buffer, int b, size_t size)
{
	char p[] = "abc";
	return p;
}

int main(void)
{
	// 其他指针赋值给void指针无需强转
	void* p1;
	int a = 10;
	int* p2 = &a;
	p1 = p2;

	// 报错,void指针赋值给别的指针需要强转
	//p2 = p1;
	p2 = (int*) p1;

	// 避免对void指针做算数操作，如下代码报错
	//p1++;

	system("pause");
	return 0;
}
```



# 7. 结构体&联合体&枚举

## 7.1 结构体的定义和使用

```c++
#include <iostream>

using namespace std;

// 结构体定义
struct Student
{
	string name;
	int age;
	int score;
}s6; // 创建结构体变量方法3

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

	// 可以只初始化部分内容，但必须按定义的顺序初始化，如下s5的方式报错
	Student s3 = { "12" };
	Student s4 = { "sf", 20 };
	//Student s5 = { 30 };

	// 3. 在定义结构体时顺便创建变量
	s6.name = "王五";
	s6.age = 20;
	s6.score = 60;

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

## 7.4 联合体

联合体与结构体的区别是，联合体只能给一个成员赋值：

```C++
#include <iostream>
using namespace std;

union student
{
	char name[10];
	int age;
};

int main()
{
	student stu;
	strcpy(stu.name, "abc");
	// 只有name
	cout << stu.name << stu.age << endl;

	stu.age = 10;
	// name被覆盖，只有age
	cout << stu.name << stu.age << endl;

	// sizeof联合体，选取最大的成员的长度，如果不能整除基本数据类型，向上取整
	// 例如student最大成员长度为10,但另一个成员是int，10不能整除4，需要与int对齐
	// 故sizeof(stu) 为12
	cout << sizeof(stu) << endl;

	system("pause");
}
```

## 7.5 枚举

```C++
#include <iostream>
using namespace std;

enum COLOR_A
{
	RED,YELLOW,BLUE
};

enum COLOR_B
{
	RED_B = 10, YELLOW_B, BLUE_B
};

enum COLOR_C
{
	RED_C = 10, YELLOW_C = 13, BLUE_C
};

void test01()
{
	COLOR_A color1 = RED;
	COLOR_A color2 = YELLOW;
	COLOR_A color3 = BLUE;

	// 打印0,1,2
	cout << color1 << endl;
	cout << color2 << endl;
	cout << color3 << endl;
	cout << endl;
}

void test02()
{
	COLOR_B color1 = RED_B;
	COLOR_B color2 = YELLOW_B;
	COLOR_B color3 = BLUE_B;

	// 打印10,11,12
	cout << color1 << endl;
	cout << color2 << endl;
	cout << color3 << endl;
	cout << endl;
}

void test03()
{
	COLOR_C color1 = RED_C;
	COLOR_C color2 = YELLOW_C;
	COLOR_C color3 = BLUE_C;

	// 打印10,13,14
	cout << color1 << endl;
	cout << color2 << endl;
	cout << color3 << endl;
	cout << endl;
}

int main()
{
	test01();
	test02();
	test03();

	system("pause");
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

## 8.4 malloc和free

C语言中，可以通过malloc开辟内存，free释放内存

```C
#include <iostream>
using namespace std;

void test01()
{
	// 直接在栈区开辟大数组，报Stack Overflow，如下代码报错
	//int arr[10 * 1024 * 1024];

	// 通过malloc函数堆空间开辟内存，返回内存的首地址
	// 返回值类型是void*，可以强转为任意指针
	int* arr = (int*) malloc(10 * 1024 * 1024 * sizeof(int));

	// 开辟完内存后，可以直接作为int数组写入
	// 可能开辟失败，要判空
	if (arr)	// 等效于 if (arr != NULL)
	{
		arr[0] = 1;
		arr[1] = 2;
	}

	// 使用完后记得调用free释放内存空间
	if (arr)
	{
		free(arr);
		arr = NULL;
	}

	// 请注意，如下方式定义的int指针是不能作为数组使用的，未进行数组初始化
	//int* arr2;
	//arr2[1] = 10;

	// 这样是可以的
	int* arr3 = new int[10];
	arr3[1] = 1;
	delete[] arr3;
}

// 
void test02()
{
	int a = 1;

	// 变量不能直接用作数组初始化，如下代码报错
	//int arr[a];

	// 如果想通过变量初始化数组大小，可以使用new关键字在堆区创建
	int* arr = new int[a];
	arr[0] = 1;
	delete[] arr;

	// 当然，malloc也是可以的
	int* arr2 = (int*)malloc(sizeof(int)*a);
	if (arr2)
	{
		arr2[0] = 1;
	}
	// 使用完后记得调用free释放内存空间
	if (arr2)
	{
		free(arr2);
	}
}

int main(void)
{
	test01();

	test02();

	system("pause");
	return 0;
}
```

## 8.5 realloc

realloc用于重新开辟内存。默认情况下，新内存和老内存反馈同一个指针地址。当新开辟内存空间不连续时，会返回一个新的指针地址，把老内存的内容copy过去，并free老内存。realloc可能存在开辟失败的情况，返回NULL。

```c
#include <iostream>
using namespace std;

void test01()
{
	int num_old = 2;
	int* arr_old = (int*)malloc(sizeof(int) * num_old);
	if (arr_old)
	{
		for (int i = 0; i < num_old; i++)
		{
			arr_old[i] = i;
		}
	}
	cout << "old 首地址： " << arr_old << endl;

	int num_new = 3;
	int* arr_new = (int*)realloc(arr_old, sizeof(int) * num_new);
	if (arr_new)
	{
		// old的部分会被直接copy过来，没有必要从0重新赋值了
		for (int i = num_old; i < num_new; i++)
		{
			arr_new[i] = i;
		}
	}
	cout << "new 首地址： " << arr_new << endl;
	// old首地址与new首地址可能相同，也可能不同，也可能因开辟失败为NULL

	for (int i = 0; i < num_new; i++)
	{
		cout << "arr_new[i] = " << arr_new[i] << endl;
	}

	// 仅释放新的就可以了
	if (arr_new)
	{
		free(arr_new);
	}
}

void test02()
{
	int num_old = 2;
	int* arr_old = (int*)malloc(sizeof(int) * num_old);
	arr_old[0] = 0;
	arr_old[1] = 1;

	// realloc的长度可以小于旧的，会把后面的内存释放掉
	int* arr_new = (int*)realloc(arr_old, sizeof(int) * 1);
	cout << arr_new[0] << endl;
	cout << arr_new[1] << endl;

	// 虽然arr_new[1] 不报错，但是该内存已经被释放了
	// 0
	// - 33686019

	free(arr_new);
}

int main(void)
{
	//test01();

	test02();

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

## 10.6 内联函数（inline）

http://c.biancheng.net/view/199.html

https://www.runoob.com/w3cnote/cpp-inline-usage.html

C++函数调用需要耗费时间，对于需要大量被调用，且执行的代码比较简单的函数，建议使用inline标识为内联函数。

```C++
#include <iostream>
using namespace std;

inline const char* num_check(int v)
{
    return (v % 2 > 0) ? "奇" : "偶";
}

int main()
{
    int i;
    for (i = 0; i < 100; i++)
    {
        cout << i << " 是 " << num_check(i) << " 数" << endl;
    }

	system("pause");
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

## 11.17 =delete和=default

```C++
#include <iostream>
using namespace std;

class Person
{
public:
	// 在重载了有参构造函数后，一般情况下编译器会删除默认构造函数
	// 可通过 = default 告诉编译器需要生成默认构造函数
	Person() = default;

	// 不想让对应的函数被调用时，可以通过 = delete 实现
	Person(int age) = delete;

	void speak() = delete;
};

int main()
{
	Person p;

	// 报错
	//Person p(10);

	// 报错
	//p.speak();

	system("pause");
}
```



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

# 14. 继承

## 14.1 继承的基本语法

```C++
#include <iostream>

using namespace std;

class Father
{
public:
	void speak()
	{
		cout << "hello" << endl;
	}
};

// 继承的基本语法：class 子类类名 : 继承方式 父类类名
class Son : public Father
{
public:
	void study()
	{
		cout << "I'm studying" << endl;
	}
};

int main() {
	Son son;
	son.speak();
	son.study();

	system("pause");
	return 0;
}
```

## 14.2 继承方式

| 继承方式\父类成员权限 | public    | protected | private  |
| --------------------- | --------- | --------- | -------- |
| public                | public    | protected | 无法继承 |
| protected             | protected | protected | 无法继承 |
| private               | private   | private   | 无法继承 |

## 14.3 继承的对象模型

如下代码的类Son，占用多少字节？

```C++
#include <iostream>

using namespace std;

class Father
{
public:
	int a;
protected:
	int b;
private:
	int c;
};

class Son : public Father
{
private:
	int d;
};

int main() {
	// 打印结果：16
	cout << "sizeof(son)" << sizeof(Son) << endl;

	system("pause");
	return 0;
}
```

说明父类private的成员也会被子类继承下来。

如何查看对象模型？

1. 开始菜单，找到"Developer PowerShell for VS 2019"并打开
2. 定位到当前cpp对应位置
3. 输入如下命令

```
cl /d1 reportSingleClassLayout类名 所属的文件名
```

结果如下，证明了刚才的观点

```
PS D:\git\C_plus_demo\2. C++核心编程> cl /d1 reportSingleClassLayoutSon '.\48. 继承中的对象模型.cpp'
用于 x86 的 Microsoft (R) C/C++ 优化编译器 19.29.30037 版
版权所有(C) Microsoft Corporation。保留所有权利。

48. 继承中的对象模型.cpp

class Son       size(16):
        +---
 0      | +--- (base class Father)
 0      | | a
 4      | | b
 8      | | c
        | +---
12      | d
        +---
```

## 14.4 继承中构造与析构的顺序

父类先构造，子类先析构

```C++
#include <iostream>

using namespace std;

class Father
{
public:
	Father()
	{
		cout << "父类构造函数" << endl;
	}

	~Father()
	{
		cout << "父类析构函数" << endl;
	}
};

class Son : public Father
{
public:
	Son()
	{
		cout << "子类构造函数" << endl;
	}

	~Son()
	{
		cout << "子类析构函数" << endl;
	}
};

void test01()
{
	Son son;
}

int main() {
	test01();

	system("pause");
	return 0;
}
```

```
父类构造函数
子类构造函数
子类析构函数
父类析构函数
```

## 14.5 继承中同名成员的访问

子类中的同名成员，直接访问即可

父类中的同名成员，通过父类类名::访问

```C++
#include <iostream>

using namespace std;

class Father
{
public:
	Father()
	{
		age = 30;
	}
	void speak()
	{
		cout << "I'm father" << endl;
	}

	int age;
};

class Son : public Father
{
public:
	Son()
	{
		age = 10;
	}
	void speak()
	{
		cout << "I'm son" << endl;
	}
	int age;
};

void test01()
{
	Son son;

	son.age;
	// 父类成员变量，通过类名::访问
	son.Father::age;

	son.speak();
	// 父类成员函数，通过类名::访问
	son.Father::speak();
}

int main() {
	test01();

	system("pause");
	return 0;
}
```

## 14.6 继承同名静态成员的访问

分为通过对象访问和通过类名访问两种方式：

```C++
#include <iostream>

using namespace std;

class Father
{
public:
	static void speak()
	{
		cout << "I'm father" << endl;
	}

	static int age;
};

class Son : public Father
{
public:
	static void speak()
	{
		cout << "I'm son" << endl;
	}
	static int age;
};

int Father::age = 30;
int Son::age = 10;

void test01()
{
	Son son;

	son.age;
	// 访问父类静态成员变量
	// 通过对象访问
	son.Father::age;
	// 通过类名访问
	Son::Father::age;

	son.speak();
	// 访问父类静态成员函数
	// 通过对象访问
	son.Father::speak();
	// 通过类名访问
	Son::Father::speak();
}

int main() {
	test01();

	system("pause");
	return 0;
}
```

## 14.7 多继承

C++可以多继承，实际开发中不推荐使用。

```C++
#include <iostream>

using namespace std;

class Father1
{
public:
	int a;
	int b;
};

class Father2
{
public:
	int a;
	int c;
};

// 多继承语法
class Son : public Father1, public Father2
{
};

void test()
{
	Son son;

	// 多继承时，多个父类如果有重名成员，需要通过作用域指明
	// son.a;

	son.Father1::a;
	son.Father2::a;
}

int main() {
	test();

	system("pause");
	return 0;
}
```

## 14.8 菱形继承

菱形继承的问题：

```C++
#include <iostream>

using namespace std;

class Father
{
public:
    int a;
};

class Son1 : public Father
{
};

class Son2 : public Father
{
};

class GrandSon : public Son1, public Son2
{
};

void test()
{
	GrandSon gs;
	// 菱形继承，子类继承了两份数据，如下代码报错
	//gs.a = 10;

	// 必须显示地指明是哪个父类的数据，但实际上不需要两份
	gs.Son1::a = 10;
	gs.Son2::a = 20;
}

int main() {
	test();

	system("pause");
	return 0;
}
```

打印GrandSon的结构：

```C++
PS D:\git\C_plus_demo\2. C++核心编程> cl /d1 reportSingleClassLayoutGrandSon '.\53. 菱形继承01.cpp'
用于 x86 的 Microsoft (R) C/C++ 优化编译器 19.29.30037 版
版权所有(C) Microsoft Corporation。保留所有权利。

53. 菱形继承01.cpp

class GrandSon  size(8):
        +---
 0      | +--- (base class Son1)
 0      | | +--- (base class Father)
 0      | | | a
        | | +---
        | +---
 4      | +--- (base class Son2)
 4      | | +--- (base class Father)
 4      | | | a
        | | +---
        | +---
        +---
```

可通过虚继承解决菱形继承的问题

```C++
#include <iostream>

using namespace std;

class Father
{
public:
	int a;
};

class Son1 : virtual public Father
{
};

class Son2 : virtual public Father
{
};

class GrandSon : public Son1, public Son2
{
};

void test()
{
	GrandSon gs;
	// 采用虚继承后，不存在不明确的问题
	gs.a = 10;
}

int main() {
	test();

	system("pause");
	return 0;
}
```

打印GrandSon，Son1中保存4字节的vbptr（virtual base pointer虚基类指针），指向下面Son1的虚基类表（vbtable），vbtable有8个字节的偏移量，正好指向GrandSon中的a。Son2同理。

```
PS D:\git\C_plus_demo\2. C++核心编程> cl /d1 reportSingleClassLayoutGrandSon '.\54. 菱形继承02.cpp'
用于 x86 的 Microsoft (R) C/C++ 优化编译器 19.29.30037 版
版权所有(C) Microsoft Corporation。保留所有权利。

54. 菱形继承02.cpp

class GrandSon  size(12):
        +---
 0      | +--- (base class Son1)
 0      | | {vbptr}
        | +---
 4      | +--- (base class Son2)
 4      | | {vbptr}
        | +---
        +---
        +--- (virtual base Father)
 8      | a
        +---

GrandSon::$vbtable@Son1@:
 0      | 0
 1      | 8 (GrandSond(Son1+0)Father)

GrandSon::$vbtable@Son2@:
 0      | 0
 1      | 4 (GrandSond(Son2+0)Father)
vbi:       class  offset o.vbptr  o.vbte fVtorDisp
          Father       8       0       4 0
```

## 14.9 子类调用父类构造函数

子类可以继承父类所有的成员变量和成员方法，但不继承父类的构造方法）。因此，在创建子类对象时，为了初始化从父类继承来的数据成员，系统需要调用其父类的构造方法。

1. 如果子类没有定义构造函数，则调用父类的默认构造方法。
2. 在创建子类对象时候，如果子类的构造函数没有显示调用父类的构造函数，则会调用父类的默认构造函数。
3. 在创建子类对象时候，如果子类的构造函数没有显示调用父类的构造函数 且父类只定义了自己的有参构造函数，则会出错（如果父类只有有参数的构造方法，则子类必须显示调用此带参构造方法）。
4. 如果子类调用父类带参数的构造方法，需要用初始化列表的方式。

```C++
#include <iostream>
using namespace std;

class Father
{
public:
	Father()
	{
		cout << "Father 无参构造函数" << endl;
	}

	Father(string name)
	{
		cout << "Father 有参构造函数" << endl;
		this->name = name;
	}

	string name;
};

class Son : public Father
{
public:
	// 调用父类构造函数
	Son() : Father(), age(10)
	{
		cout << "Son 无参构造函数" << endl;
	}

	// 调用父类有参构造函数
	Son(string name) : Father(name), age(10)
	{
		cout << "Son 有参构造函数" << endl;
	}
	int age;
};

void test()
{
	Son son;
	Son son2("Tom");
}

int main()
{
	test();

	system("pause");
}
```

```
Father 无参构造函数
Son 无参构造函数
Father 有参构造函数
Son 有参构造函数
```



# 15. 多态

C++通过派生类与虚函数实现多态，对象运行时绑定

多态出现的几个条件：

1. 有继承关系
2. 子类重写父类的虚函数
3. 父类指针或父类引用接受子类对象

## 15.1 多态举例

```C++
#include <iostream>

using namespace std;

class Animal
{
public:
	virtual void speak()
	{
		cout << "动物叫" << endl;
	}
};

class Dog : public Animal
{
public:
	void speak()
	{
		cout << "汪汪汪" << endl;
	}
};

// 多态条件：父类引用接收子类对象
void test(Animal& animal)
{
	animal.speak();
}

// 多态条件：父类指针接收子类对象
void test(Animal* animal)
{
	animal->speak();
}

int main() {
	Dog dog;
	test(dog);
	test(&dog);

	system("pause");
	return 0;
}
```

```
汪汪汪
汪汪汪
```

打印下Dog的对象结构，可以看到父类中保存的是一个指向vftable（虚函数表）的指针（vfptr），该表中保存了实际要执行的函数。例如如果子类重写了，就是Dog::speak()，否则是Animal::speak()

```
PS D:\git\C_plus_demo\2. C++核心编程> cl /d1 reportSingleClassLayoutDog '.\55. 多态举例.cpp'
用于 x86 的 Microsoft (R) C/C++ 优化编译器 19.29.30037 版
版权所有(C) Microsoft Corporation。保留所有权利。

55. 多态举例.cpp

class Dog       size(4):
        +---
 0      | +--- (base class Animal)
 0      | | {vfptr}
        | +---
        +---

Dog::$vftable@:
        | &Dog_meta
        |  0
 0      | &Dog::speak
```



## 15.2 纯虚函数和抽象类

在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容

因此可以将虚函数改为**纯虚函数**

纯虚函数语法：`virtual 返回值类型 函数名 （参数列表）= 0 ;`

当类中有了纯虚函数，这个类也称为抽象类

**抽象类特点**：

- 无法实例化对象
- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```C++
#include <iostream>

using namespace std;

class Animal
{
public:
	// 纯虚函数语法，有纯虚函数的类是抽象类，无法实例化对象
	virtual void speak() = 0;
};

class Dog : public Animal
{
public:
	// 父类中存在纯虚函数，子类需重写，否则子类也是抽象类
	void speak()
	{
		cout << "汪汪汪" << endl;
	}
};

int main() {
	// Animal是抽象类，无法实例化对象，如下代码报错
	//Animal animal;

	system("pause");
	return 0;
}
```

## 15.3 虚析构和纯虚析构

什么时候需要虚析构/纯虚析构？

同时满足：

1. 对象是创在在堆区的（new），需要释放
2. 对象涉及多态

上面两条满足时，只会调用父类的析构函数，不会调用子类的，例如：

```C++
#include <iostream>

using namespace std;

class Animal
{
public:
	Animal()
	{
		cout << "父类构造函数" << endl;
	}

	~Animal()
	{
		cout << "父类析构函数" << endl;
	}

	virtual void speak() = 0;
};

class Dog : public Animal
{
public:
	Dog()
	{
		cout << "子类构造函数" << endl;
		name = new string("Jack");
	}

	// 父类中存在纯虚函数，子类需重写，否则子类也是抽象类
	void speak()
	{
		cout << "汪汪汪" << endl;
	}

	~Dog()
	{
		cout << "子类析构函数" << endl;
		if (name != NULL)
		{
			delete name;
			name = NULL;
		}
	}
	string* name;
};

void test()
{
	Animal* animal = new Dog();
	animal->speak();
	delete animal;
}

int main() {
	test();

	system("pause");
	return 0;
}
```

打印（可以看到子类析构函数并未调用，导致name无法释放）：

```
父类构造函数
子类构造函数
汪汪汪
父类析构函数
```

可以通过虚析构函数解决该问题，在父类的虚析构函数前加上virtual

```C++
#include <iostream>

using namespace std;

class Animal
{
public:
	Animal()
	{
        cout << "父类构造函数" << endl;
	}

	virtual ~Animal()
	{
        cout << "父类析构函数" << endl;
	}
};

class Dog : public Animal
{
public:
	Dog()
	{
		cout << "子类构造函数" << endl;
		name = new string("Jack");
	}

	~Dog()
	{
		cout << "子类析构函数" << endl;
		if (name != NULL)
		{
			delete name;
			name = NULL;
		}
	}
	string* name;
};

void test()
{
	Animal* animal = new Dog();
	delete animal;
}

int main() {
	test();

	system("pause");
	return 0;
}
```

```
父类构造函数
子类构造函数
子类析构函数
父类析构函数
```

同样的，虚析构也有纯虚析构，有纯虚析构的类也是抽象类。但与普通的纯虚函数不同的是，纯虚析构函数必须在类外定义，否则执行报错。

```C++
#include <iostream>

using namespace std;

class Animal
{
public:
	Animal()
	{
		cout << "父类构造函数" << endl;
	}

	// 纯虚析构
	virtual ~Animal() = 0;
};

// 纯虚析构函数必须在外面定义，不能等到子类再定义，如下代码是必要的
Animal::~Animal() {}

class Dog : public Animal
{
public:
	Dog()
	{
		cout << "子类构造函数" << endl;
		name = new string("Jack");
	}

	~Dog()
	{
		cout << "子类析构函数" << endl;
		if (name != NULL)
		{
			delete name;
			name = NULL;
		}
	}
	string* name;
};

void test()
{
	Animal* animal = new Dog();
	delete animal;
}

int main() {
	test();

	system("pause");
	return 0;
}
```

# 16. 文件操作

C++中进行文件操作需要包含头文件 < fstream >

文件类型分为两种：

1. **文本文件** - 文件以文本的**ASCII码**形式存储在计算机中
2. **二进制文件** - 文件以文本的**二进制**形式存储在计算机中，用户一般不能直接读懂它们

操作文件的三大类:

1. ofstream：写操作
2. ifstream： 读操作
3. fstream ： 读写操作

文件打开方式：

| 打开方式    | 解释                       |
| ----------- | -------------------------- |
| ios::in     | 为读文件而打开文件         |
| ios::out    | 为写文件而打开文件         |
| ios::ate    | 初始位置：文件尾           |
| ios::app    | 追加方式写文件             |
| ios::trunc  | 如果文件存在先删除，再创建 |
| ios::binary | 二进制方式                 |

**注意：** 文件打开方式可以配合使用，利用|操作符

**例如：**用二进制方式写文件 `ios::binary | ios:: out`

## 16.1 写文件（文本文件）

```C++
#include <iostream>
// 1、包含头文件
#include <fstream>

using namespace std;

int main() {
	// 2、创建流对象
	ofstream ofs;
	// 3、打开文件
	ofs.open("test.txt", ios::out);
	// 4、写数据
	ofs << "hello world";
	// 5、关闭文件
	ofs.close();

	system("pause");
	return 0;
}
```

## 16.2 读文件（文本文件）

```C++
#include <iostream>
#include <fstream>

using namespace std;

int main() {
	ifstream ifs;
	ifs.open("test.txt", ios::in);
	if (!ifs.is_open())
	{
		cout << "文件打开失败" << endl;
		system("pause");
		return 0;
	}

	//第一种方式
	//char buf[1024] = { 0 };
	//while (ifs >> buf)
	//{
	//	cout << buf << endl;
	//}

	//第二种
	//char buf[1024] = { 0 };
	//while (ifs.getline(buf,sizeof(buf)))
	//{
	//	cout << buf << endl;
	//}

	//第三种
	//string buf;
	//while (getline(ifs, buf))
	//{
	//	cout << buf << endl;
	//}

	char c;
	while ((c = ifs.get()) != EOF)
	{
		cout << c;
	}
	
	ifs.close();

	system("pause");
	return 0;
}
```

## 16.3 写文件（二进制文件）

```C++
#include <iostream>
#include <fstream>

using namespace std;

class Person
{
public:
	int age;
	char name[64];
};

void test()
{
	ofstream ofs;
	ofs.open("testBinary.txt", ios::out | ios::binary);
	
	Person person = {10, "张三"};

	ofs.write((const char*)&person, sizeof(person));

	ofs.close();
}

int main() {
	test();

	system("pause");
	return 0;
}
```

## 16.4 读文件（二进制文件）

```C++
#include <iostream>
#include <fstream>

using namespace std;

class Person
{
public:
	int age;
	char name[64];
};

void test()
{
	ifstream ifs("testBinary.txt", ios::in | ios::binary);
	if (!ifs.is_open())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	Person p;
	ifs.read((char*)&p, sizeof(p));
	cout << p.name << p.age << endl;
	ifs.close();
}

int main() {
	test();

	system("pause");
	return 0;
}
```

# 17. 模板

模板即C++中的泛型，分为函数模板与类模板两种

## 17.1 函数模板语法

```
template<typename T>
函数声明或定义

或

template<class T>
函数声明或定义
```

**解释：**

template — 声明创建模板

typename — 表明其后面的符号是一种数据类型，可以用class代替

T — 通用的数据类型，名称可以替换，通常为大写字母

**使用函数模板的两种方式**

1. 自动类型推导
2. 显示指定类型

注意事项：

1. 使用自动类型推导时，必须是可以确定的数据类型
2. 函数模板在不确定时，无法执行

```C++
#include <iostream>

using namespace std;

// 函数模板01
template<typename T>
void func()
{
	cout << sizeof(T) << endl;
}

// 函数模板02
template<class T>
void mySwap(T& a, T& b)
{
	T temp = a;
	a = b;
	b = temp;
}

void test01()
{
	int a = 10;
	int b = 20;
	char c = 'c';
	// 函数模板的使用方式一：自动类型推导
	mySwap(a, b);
	// 函数模板的使用方式二：显示指定类型
	mySwap<int>(a, b);

	// 函数模板必须可以推导出T的数据类型，如下代码报错
	//mySwap(a, c);
	// 同样报错
	//mySwap<char>(a, c);
}

void test02()
{
	func<char>();

	// 函数模板在不确定时，无法执行，如下代码报错
	//func();
}

int main() {
	test01();
	test02();

	system("pause");
	return 0;
}
```



## 17.2 函数模板与隐式类型转换

先了解下C++中的隐式类型转换：

```C++
#include <iostream>

using namespace std;

// C++隐式类型转换的说明
class Animal
{
public:
	virtual void speak() = 0;
};

class Cat : public Animal
{
public:
	void speak()
	{
		cout << "miao miao" << endl;
	}
};

// 隐式类型转换01：多态
void test01(Animal& animal)
{
	animal.speak();
}

// 隐式类型转换02：编译器自动将数据进行了转换
void test02(int i)
{
	cout << i << endl;

	int a = 10;
	double b = 1.0;
	a + b; // int自动转化为double
}

int main() {

	Cat cat;
	test01(cat);

	// 传参是char类型，自动转换为int类型
	test02('c');

	system("pause");
	return 0;
}
```



```C++
#include <iostream>

using namespace std;

// 普通函数
int add01(int a, int b)
{
	return a + b;
}

// 函数模板
template<typename T>
T add02(T a, T b)
{
	return a + b;
}

int main() {
	int a = 10;
	int b = 20;
	char c = 'c';
	
	// 普通函数可发生隐式类型转换，char自动转换为int
	add01(a, b);
	add01(a, c);

	add02(a, b);

	// 使用函数模板时，如果使用自动类型推导，不会发生隐式类型转换
	// 如下代码报错，因为编译器不知道T到底是int还是char
	//add02(a, c);

	// 使用函数模板时，如果显示指定类型，可以发生隐式类型转换
	add02<int>(a, c);
	add02<char>(a, c);

	system("pause");
	return 0;
}
```

多态相关的隐式类型转换同理：

```C++
#include <iostream>

using namespace std;

class Animal
{
};

class Cat : public Animal
{
};

void test01(Animal& a, Animal& b)
{
}

template<typename T>
void test02(T& a, T& b)
{
}

int main() {
	Cat cat1;
	Cat cat2;
	Animal animal;

	// 普通函数可以发生多态（隐式类型转换）
	test01(cat1, cat2);
	test01(cat1, animal);

	test02(cat1, cat2);

	// 使用函数模板时，如果使用自动类型推导，不会发生多态（隐式类型转换）
	// 如下代码报错，编译器不知道T是Cat还是Animal
	//test02(cat1, animal);

	// 使用函数模板时，如果使用显示指定类型，可以发生多态（隐式类型转换）
	test02<Animal>(cat1, animal);

	system("pause");
	return 0;
}
```

## 17.3 普通函数与函数模板的调用规则

```C++
#include <iostream>

using namespace std;

//普通函数与函数模板调用规则
void myPrint(int a, int b)
{
	cout << "调用的普通函数" << endl;
}

template<typename T>
void myPrint(T a, T b)
{
	cout << "调用的模板" << endl;
}

template<typename T>
void myPrint(T a, T b, T c)
{
	cout << "调用重载的模板" << endl;
}

void test01()
{
	//1、如果函数模板和普通函数都可以实现，优先调用普通函数
	int a = 10;
	int b = 20;
	myPrint(a, b); //调用普通函数

	//2、可以通过空模板参数列表来强制调用函数模板
	myPrint<>(a, b); //调用函数模板

	//3、函数模板也可以发生重载
	int c = 30;
	myPrint(a, b, c); //调用重载的函数模板

	//4、 如果函数模板可以产生更好的匹配,优先调用函数模板
	char c1 = 'a';
	char c2 = 'b';
	myPrint(c1, c2); //调用函数模板
}

int main() {
	test01();

	system("pause");
	return 0;
}
```

## 17.4 模板的局限性

通过函数模板具体化实现函数模板对类型T的重载

```C++
#include <iostream>

using namespace std;

class Person
{
public:
	Person(int age)
	{
		this->age = age;
	}
	int age;
};

// 模板局限性示例：传入Person对象时，是无法进行大小判断比较的
template<typename T>
const T& getMax(const T& a, const T& b)
{
	return a > b ? a : b;
}

// 为进行＞判断，可以通过运算符重载实现
//bool operator>(const Person& p1, const Person& p2)
//{
//	return p1.age > p2.age;
//}

// 此处介绍另一种方法，通过函数模板具体化实现
// 相应的，还有类模板具体化，会在下面章节介绍
// 具体语法：template<>实现函数模板针对T具体类型的重载
template<> const Person& getMax(const Person& a, const Person& b)
{
	return a.age > b.age ? a : b;
}

ostream& operator<< (ostream& os, const Person& person)
{
	os << person.age;
	return os;
}

int main() {

	Person p = Person(10);
	Person p2 = Person(5);
	cout << getMax(p, p2) << endl;

	system("pause");
	return 0;
}
```

## 17.5 类模板语法

类模板作用：

- 建立一个通用类，类中的成员 数据类型可以不具体制定，用一个**虚拟的类型**来代表。

**语法：**

```c++
template<typename T>
类
```

**解释：**

template — 声明创建模板

typename — 表面其后面的符号是一种数据类型，可以用class代替

T — 通用的数据类型，名称可以替换，通常为大写字母

**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

void test01()
{
	// 指定NameType 为string类型，AgeType 为 int类型
	Person<string, int>P1("孙悟空", 999);
	P1.showPerson();
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：类模板和函数模板语法相似，在声明模板template后面加类，此类称为类模板

## 17.6 类模板与函数模板区别

类模板与函数模板区别主要有两点：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数

**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、类模板没有自动类型推导的使用方式
void test01()
{
	// Person p("孙悟空", 1000); // 错误 类模板使用时候，不可以用自动类型推导
	Person <string ,int>p("孙悟空", 1000); //必须使用显示指定类型的方式，使用类模板
	p.showPerson();
}

//2、类模板在模板参数列表中可以有默认参数
void test02()
{
	Person <string> p("猪八戒", 999); //类模板中的模板参数列表 可以指定默认参数
	p.showPerson();
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

- 类模板使用只能用显示指定类型方式
- 类模板中的模板参数列表可以有默认参数

## 17.7 类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机是有区别的：

- 普通类中的成员函数一开始就可以创建
- 类模板中的成员函数在调用时才创建

**示例：**

```C++
class Person1
{
public:
	void showPerson1()
	{
		cout << "Person1 show" << endl;
	}
};

class Person2
{
public:
	void showPerson2()
	{
		cout << "Person2 show" << endl;
	}
};

template<class T>
class MyClass
{
public:
	T obj;

	//类模板中的成员函数，并不是一开始就创建的，而是在模板调用时再生成

	void fun1() { obj.showPerson1(); }
	void fun2() { obj.showPerson2(); }

};

void test01()
{
	MyClass<Person1> m;
	
	m.fun1();

	//m.fun2();//编译会出错，说明函数调用才会去创建成员函数
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：类模板中的成员函数并不是一开始就创建的，在调用时才去创建

## 17.8 类模板对象做函数参数

学习目标：

- 类模板实例化出的对象，向函数传参的方式

一共有三种传入方式：

1. 指定传入的类型 — 直接显示对象的数据类型
2. 参数模板化 — 将对象中的参数变为模板进行传递
3. 整个类模板化 — 将这个对象类型 模板化进行传递

**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int> 
class Person
{
public:
	Person(NameType name, AgeType age)
	{
		this->mName = name;
		this->mAge = age;
	}
	void showPerson()
	{
		cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};

//1、指定传入的类型
void printPerson1(Person<string, int> &p) 
{
	p.showPerson();
}
void test01()
{
	Person <string, int >p("孙悟空", 100);
	printPerson1(p);
}

//2、参数模板化
template <class T1, class T2>
void printPerson2(Person<T1, T2>&p)
{
	p.showPerson();
	cout << "T1的类型为： " << typeid(T1).name() << endl;
	cout << "T2的类型为： " << typeid(T2).name() << endl;
}
void test02()
{
	Person <string, int >p("猪八戒", 90);
	printPerson2(p);
}

//3、整个类模板化
template<class T>
void printPerson3(T & p)
{
	cout << "T的类型为： " << typeid(T).name() << endl;
	p.showPerson();

}
void test03()
{
	Person <string, int >p("唐僧", 30);
	printPerson3(p);
}

int main() {

	test01();
	test02();
	test03();

	system("pause");

	return 0;
}
```

总结：

- 通过类模板创建的对象，可以有三种方式向函数中进行传参
- 使用比较广泛是第一种：指定传入的类型

## 17.9 类模板与继承

当类模板碰到继承时，需要注意一下几点：

- 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活指定出父类中T的类型，子类也需变为类模板

**示例：**

```C++
template<class T>
class Base
{
	T m;
};

//class Son:public Base  //错误，c++编译需要给子类分配内存，必须知道父类中T的类型才可以向下继承
class Son :public Base<int> //必须指定一个类型
{
};
void test01()
{
	Son c;
}

//类模板继承类模板 ,可以用T2指定父类中的T类型
template<class T1, class T2>
class Son2 :public Base<T2>
{
public:
	Son2()
	{
		cout << typeid(T1).name() << endl;
		cout << typeid(T2).name() << endl;
	}
};

void test02()
{
	Son2<int, char> child1;
}


int main() {

	test01();

	test02();

	system("pause");

	return 0;
}
```

总结：如果父类是类模板，子类需要指定出父类中T的数据类型

## 17.10 类模板成员函数类外实现

学习目标：能够掌握类模板中的成员函数类外实现

**示例：**

```C++
#include <string>

//类模板中成员函数类外实现
template<class T1, class T2>
class Person {
public:
	//成员函数类内声明
	Person(T1 name, T2 age);
	void showPerson();

public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}

void test01()
{
	Person<string, int> p("Tom", 20);
	p.showPerson();
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：类模板中成员函数类外实现时，需要加上模板参数列表

## 17.11 类模板分文件编写

学习目标：

- 掌握类模板成员函数分文件编写产生的问题以及解决方式

问题：

- 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

解决：

- 解决方式1：直接包含.cpp源文件
- 解决方式2：将声明和实现写到同一个文件中，并更改后缀名为.hpp，hpp是约定的名称，并不是强制

**示例：**

person.hpp中代码：

```C++
#pragma once
#include <iostream>
using namespace std;
#include <string>

template<class T1, class T2>
class Person {
public:
	Person(T1 name, T2 age);
	void showPerson();
public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
```

类模板分文件编写.cpp中代码

```C++
#include<iostream>
using namespace std;

//#include "person.h"
#include "person.cpp" //解决方式1，包含cpp源文件

//解决方式2，将声明和实现写到一起，文件后缀名改为.hpp
#include "person.hpp"
void test01()
{
	Person<string, int> p("Tom", 10);
	p.showPerson();
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

总结：主流的解决方式是第二种，将类模板成员函数写到一起，并将后缀名改为.hpp

## 17.12 类模板与友元

学习目标：

- 掌握类模板配合友元函数的类内和类外实现

全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要提前让编译器知道全局函数的存在

**示例：**

```C++
#include <string>

//2、全局函数配合友元  类外实现 - 先做函数模板声明，下方在做函数模板定义，在做友元
template<class T1, class T2> class Person;

//如果声明了函数模板，可以将实现写到后面，否则需要将实现体写到类的前面让编译器提前看到
//template<class T1, class T2> void printPerson2(Person<T1, T2> & p); 

template<class T1, class T2>
void printPerson2(Person<T1, T2> & p)
{
	cout << "类外实现 ---- 姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
}

template<class T1, class T2>
class Person
{
	//1、全局函数配合友元   类内实现
	friend void printPerson(Person<T1, T2> & p)
	{
		cout << "姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
	}


	//全局函数配合友元  类外实现
	friend void printPerson2<>(Person<T1, T2> & p);

public:

	Person(T1 name, T2 age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}


private:
	T1 m_Name;
	T2 m_Age;

};

//1、全局函数在类内实现
void test01()
{
	Person <string, int >p("Tom", 20);
	printPerson(p);
}


//2、全局函数在类外实现
void test02()
{
	Person <string, int >p("Jerry", 30);
	printPerson2(p);
}

int main() {

	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：建议全局函数做类内实现，用法简单，而且编译器可以直接识别

## 17.13 类模板案例

案例描述: 实现一个通用的数组类，要求如下：

- 可以对内置数据类型以及自定义数据类型的数据进行存储
- 将数组中的数据存储到堆区
- 构造函数中可以传入数组的容量
- 提供对应的拷贝构造函数以及operator=防止浅拷贝问题
- 提供尾插法和尾删法对数组中的数据进行增加和删除
- 可以通过下标的方式访问数组中的元素
- 可以获取数组中当前元素个数和数组的容量

**示例：**

myArray.hpp中代码

```C++
#pragma once
#include <iostream>
using namespace std;

template<class T>
class MyArray
{
public:
    
	//构造函数
	MyArray(int capacity)
	{
		this->m_Capacity = capacity;
		this->m_Size = 0;
		pAddress = new T[this->m_Capacity];
	}

	//拷贝构造
	MyArray(const MyArray & arr)
	{
		this->m_Capacity = arr.m_Capacity;
		this->m_Size = arr.m_Size;
		this->pAddress = new T[this->m_Capacity];
		for (int i = 0; i < this->m_Size; i++)
		{
			//如果T为对象，而且还包含指针，必须需要重载 = 操作符，因为这个等号不是 构造 而是赋值，
			// 普通类型可以直接= 但是指针类型需要深拷贝
			this->pAddress[i] = arr.pAddress[i];
		}
	}

	//重载= 操作符  防止浅拷贝问题
	MyArray& operator=(const MyArray& myarray) {

		if (this->pAddress != NULL) {
			delete[] this->pAddress;
			this->m_Capacity = 0;
			this->m_Size = 0;
		}

		this->m_Capacity = myarray.m_Capacity;
		this->m_Size = myarray.m_Size;
		this->pAddress = new T[this->m_Capacity];
		for (int i = 0; i < this->m_Size; i++) {
			this->pAddress[i] = myarray[i];
		}
		return *this;
	}

	//重载[] 操作符  arr[0]
	T& operator [](int index)
	{
		return this->pAddress[index]; //不考虑越界，用户自己去处理
	}

	//尾插法
	void Push_back(const T & val)
	{
		if (this->m_Capacity == this->m_Size)
		{
			return;
		}
		this->pAddress[this->m_Size] = val;
		this->m_Size++;
	}

	//尾删法
	void Pop_back()
	{
		if (this->m_Size == 0)
		{
			return;
		}
		this->m_Size--;
	}

	//获取数组容量
	int getCapacity()
	{
		return this->m_Capacity;
	}

	//获取数组大小
	int	getSize()
	{
		return this->m_Size;
	}


	//析构
	~MyArray()
	{
		if (this->pAddress != NULL)
		{
			delete[] this->pAddress;
			this->pAddress = NULL;
			this->m_Capacity = 0;
			this->m_Size = 0;
		}
	}

private:
	T * pAddress;  //指向一个堆空间，这个空间存储真正的数据
	int m_Capacity; //容量
	int m_Size;   // 大小
};
```

类模板案例—数组类封装.cpp中

```C++
#include "myArray.hpp"
#include <string>

void printIntArray(MyArray<int>& arr) {
	for (int i = 0; i < arr.getSize(); i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
}

//测试内置数据类型
void test01()
{
	MyArray<int> array1(10);
	for (int i = 0; i < 10; i++)
	{
		array1.Push_back(i);
	}
	cout << "array1打印输出：" << endl;
	printIntArray(array1);
	cout << "array1的大小：" << array1.getSize() << endl;
	cout << "array1的容量：" << array1.getCapacity() << endl;

	cout << "--------------------------" << endl;

	MyArray<int> array2(array1);
	array2.Pop_back();
	cout << "array2打印输出：" << endl;
	printIntArray(array2);
	cout << "array2的大小：" << array2.getSize() << endl;
	cout << "array2的容量：" << array2.getCapacity() << endl;
}

//测试自定义数据类型
class Person {
public:
	Person() {} 
		Person(string name, int age) {
		this->m_Name = name;
		this->m_Age = age;
	}
public:
	string m_Name;
	int m_Age;
};

void printPersonArray(MyArray<Person>& personArr)
{
	for (int i = 0; i < personArr.getSize(); i++) {
		cout << "姓名：" << personArr[i].m_Name << " 年龄： " << personArr[i].m_Age << endl;
	}

}

void test02()
{
	//创建数组
	MyArray<Person> pArray(10);
	Person p1("孙悟空", 30);
	Person p2("韩信", 20);
	Person p3("妲己", 18);
	Person p4("王昭君", 15);
	Person p5("赵云", 24);

	//插入数据
	pArray.Push_back(p1);
	pArray.Push_back(p2);
	pArray.Push_back(p3);
	pArray.Push_back(p4);
	pArray.Push_back(p5);

	printPersonArray(pArray);

	cout << "pArray的大小：" << pArray.getSize() << endl;
	cout << "pArray的容量：" << pArray.getCapacity() << endl;

}

int main() {

	//test01();

	test02();

	system("pause");

	return 0;
}
```

总结：

能够利用所学知识点实现通用的数组

# 18. main函数传参

C++可以没有传参，如果有传参，格式如下

```C++
#include <iostream>

using namespace std;

// argc代表参数个数
// char** argv,等同于 char* argv[],是一个字符串数组
// argv[0] 为exe文件的路径
// argv[1] 为第1个参数，以此类推
int main(int argc, char** argv)
{
	for (int i = 0; i < argc; i++)
	{
		cout << "第" << i << "个参数是：" << argv[i] << endl;
	}
	system("pause");
}
```

可以通过在cmd窗口，在exe可执行文件后，紧跟参数的方式传入参数，例如：

```
D:\git\C_plus_demo\4. C++实用编程\Debug>"4. C++实用编程.exe" a b c
第0个参数是：4. C++实用编程.exe
第1个参数是：a
第2个参数是：b
第3个参数是：c
请按任意键继续. . .
```

# 19. 预处理命令

预处理命令，以#开头，代表**编译之前**进行的处理。C/C++的预处理命令包含三种

1. 宏定义
2. 文件包含
3. 条件编译

## 19.1 宏定义

针对1.2节进行拓展，#define分为无参数宏和带参数宏

```C++
#include <iostream>
using namespace std;

// 宏定义
// 无参数宏
#define PI 3.14
#define M "abc"

// 有参数宏，注意不等同于函数
#define f(x) x*x + 3*x
// 可以通过分号执行多条语句，但不建议这么做
#define Sum(a,b,c) a=b*c; b=c*a; a=b*c

void testDefine()
{
	//PI：3.14
	//MyStr：abc
	//3 * f(2)18
	//a = 54
	//b = 18
	//c = 3
	cout << "PI：" << PI << endl;
	cout << "MyStr：" << M << endl; 

	// 如下代码只是做了替换，并不是值传递：3*2*2 + 3*2
	cout << "3 * f(2)" << 3 * f(2) << endl;

	int a = 1;
	int b = 2;
	int c = 3;
	Sum(a, b, c);
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
	cout << "c = " << c << endl;
}

int main()
{
	testDefine();


	system("pause");
}
```

​	宏定义中#的作用：把宏参数转化为字符串

```C++
#define STR(s)		#s
STR(vck);	// 输出字符串"vck"
STR(123);  	//输出为字符串“123”
```

## 19.2 文件包含

#include有两种形式：

```C++
// 到配置路径找
#include <stdio.h>

// 从当前目录开始找，没有找到则从配置路径开始找
#include "stdio.h"
```

文件包含允许嵌套，例如文件A包含了文件B，文件B又包含了文件C，最终文件B和文件C都会被引入A文件。

## 19.3 条件编译

分为#if系列和#ifdef系列，后者见1.3.2节

```C++
#include <iostream>
using namespace std;

#define debug 0
#define beta 1
#define status 0

int main()
{
// if,elif,else后的判断条件必须是define的宏定义，或者直接指定清楚值
// 如果是变量，或者const开头的常量，预处理的时候是不知道执行时的值得，只能使用默认值
#if (b == debug)
	cout << "程序调试中" << endl;
#elif (b == beta)
	cout << "程序测试中" << endl;
#else
	cout << "欢迎使用正式版本" << endl;
#endif
	system("pause");
}
```

# 20. 字符串

学习下字符串以及string.h的常用函数

## 20.1 字符串

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

## 20.2 获取字符串长度

```C++
#include <iostream>
using namespace std;

int getLength(char* str)
{
	// 如下方式不对，sizeof(str)的值不准确
	//return sizeof(str) / sizeof(str[0]);

	// 正确的返回字符串长度的方法：循环读取char，直到读到\0
	int length = 0;
	while (*str != '\0')
	{
		length++;
		str++;
	}
	return length;
}

int main()
{
	char str1[] = "abc";
	char str2[] = { 'a','b','c','\0' };

	// 字符串以\0标识结尾，str1等价于str2，strlen均返回3
	cout << "str1的长度 ： " << strlen(str1) << endl;
	cout << "str2的长度 ： " << strlen(str2) << endl;

	// 但如果采用数组长度的计算方法，计算出来的结果为4,
	// 所以针对char数组形式的字符串，不要用此方法计算长度
	cout << "str1的长度 ： " << sizeof(str1) / sizeof(str1[0]) << endl;
	cout << "str2的长度 ： " << sizeof(str1) / sizeof(str1[0]) << endl;

	// 如果把char数组指针传到函数，是计算不出
	cout << "字符串长度： " << getLength(str1) << endl;

	system("pause");
}
```

## 20.3 字符串比较

语法/原型：

```C
int strcmp(const char* stri1，const char* str2);
```

参数 str1 和 str2 是参与比较的两个字符串，不能是string，必须是char*。

strcmp() 会根据 ASCII 编码依次比较 str1 和 str2 的每一个字符，直到出现不到的字符，或者到达字符串末尾（遇见`\0`）。

返回值：

- 如果返回值 < 0，则表示 str1 小于 str2。
- 如果返回值 > 0，则表示 str2 小于 str1。
- 如果返回值 = 0，则表示 str1 等于 str2。

```C++
#include <iostream>
using namespace std;

void testStrcmp()
{
	char str1[] = "abc";
	char str2[] = "abc";
	// 合法，打印 0
	cout << strcmp(str1, str2) << endl;

	// 合法，打印 -1
	cout << strcmp("abc", "abcd") << endl;

	// 合法，打印 1
	cout << strcmp("ab", "aab") << endl;

	string str3 = "abc";
	// 不合法，报错，传参不能是string
	//cout << strcmp(str3, str2) << endl;

	// 可通过strcmp判断字符串是否相等
	if (!strcmp(str1, str2))
	{
		cout << "字符串相等" << endl;
	}
	else
	{
		cout << "字符串不相等" << endl;
	}
}

int main()
{
	testStrcmp();

	system("pause");
}
```

## 20.4 字符串转换

```C++
#include <iostream>
using namespace std;

int main()
{
	// 1. 通过aoti将字符串强转为int
	char str1[] = "12";
	cout << atoi(str1) << endl;	//12

	char str2[] = "12xxx";
	cout << atoi(str2) << endl;	//12

	char str3[] = "xxx12xxx";
	cout << atoi(str3) << endl;	//0		无法转换，返回默认值0

	// 2. 通过aotf将字符串强转为float
	char str4[] = "12.5";
	cout << atof(str4) << endl;	//12.5

	char str5[] = "12.5xxx";
	cout << atof(str5) << endl;	//12.5

	char str6[] = "xxx12.5xxx";
	cout << atof(str6) << endl;	//0		无法转换，返回默认值0

	system("pause");
}
```

## 20.5 字符串查找

```c++
#include <iostream>
using namespace std;

char str1[] = "I'm Chinese";
char str2[] = "nes";
char str3[] = "abc";

// 字符串查找
void test01()
{
	char* pos = strstr(str1, str2);

	// 返回子串在父串的第一次出现的指针位置
	// 如下打印会从父串的指针位置打印到结尾'\0'
	// nese
	cout << pos << endl;

	// 不包含时，返回NULL
	char* pos2 = strstr(str1, str3);
}

// 返回子串在父串中第一次出现的index，通过指针相减实现
int getSubStrIndex()
{
	char* pos = strstr(str1, str2);
	return pos - str1;
}

int main()
{
	test01();

	cout << getSubStrIndex() << endl;;

	system("pause");
}
```

## 20.6 字符串拼接

```C++
#include <iostream>
using namespace std;

char str1[] = "I'm ";
char str2[] = "Chinese";

// 字符串拼接
void test01()
{
	char* str3 = strcat(str1, str2);

	// I'm Chinese
	cout << str3 << endl;
}

int main()
{
	test01();

	system("pause");
}
```

## 20.7 字符串复制

```C++
#include <iostream>
using namespace std;

// 字符串复制
void test01()
{
	// 把str2中的内容复制到str1中，str1的长度至少要是4,小于4会执行报错
	char str1[4];
	char str2[] = "abc";
	strcpy(str1, str2);
	cout << "str1 : " << str1 << endl;
	cout << "str2 : " << str2 << endl;
}

int main()
{
	test01();

	system("pause");
}
```

## 20.8 字符串截取

```C++
#include <iostream>
using namespace std;

// 字符串截取
char* subStr(char* str, int start, int end)
{
	int len = strlen(str);
	if (start < 0 || start >= len)
	{
		return NULL;
	}
	// 含头不含尾
	if (end < 0 || end > len)
	{
		return NULL;
	}
	if (start > end)
	{
		return NULL;
	}
	char* result = (char*)malloc(sizeof(char) * (end - start + 1));
	if (!result)
	{
		return NULL;
	}
	char* temp = result;
	for (int i = 0; i < end - start; i++)
	{
		*result = str[start + i];
		result++;
	}

	// 标记字符串结尾
	temp[end - start] = '\0';
	return temp;
}

int main()
{
	char str[] = "Hello World";

	char* result = subStr(str, 3, 5);
	cout << result << endl;

	// 截取的长度为2
	cout << strlen(result) << endl;

	free(result);

	system("pause");
}
```



# 21. 命名空间

命名空间可以确定变量、类、枚举等的范围，可以避免命名冲突。

语法：

```C++
// 定义命名空间
namespace 命名空间名称
{
	变量、类、枚举、结构体等
}

// 使用命名空间
using namespace 命名空间名称;
```

使用示例：

```C++
#include <iostream>
using namespace std;

namespace A
{
	// 变量
	int num;
	// 结构体
	struct student
	{
		string name;
		int age;
	};
	// 类
	class Teacher
	{
	public :
		Teacher(string name, int age)
		{
			this->name = name;
			this->age = age;
		}
	private:
		string name;
		int age;
	};
	void func()
	{
		cout << "method func in namespace A" << endl;
	}

	enum SEASON
	{
		SPRING, SUMMER, AUTUMN, WINTER
	};

	// namespace可以嵌套
	namespace A_inner
	{
		int level;
	}
}

namespace B
{
	// 变量
	int num;
	// 结构体
	struct student
	{
		string name;
		int age;
	};
	// 类
	class Teacher
	{
	public:
		Teacher(string name, int age)
		{
			this->name = name;
			this->age = age;
		}
	private:
		string name;
		int age;
	};
	void func()
	{
		cout << "method func in namespace B" << endl;
	}

	// namespace可以嵌套
	namespace A_inner
	{
		int level;
	}
}

// 使用命名空间，可以使用多个
using namespace A;
using namespace B;

int main()
{
	// 报错，不清楚是A的还是B的
	//func();

	// 不报错，SEASON是命名空间A特有的
	SEASON;

	// 通过作用域运算符找到指定命名空间的变量
	A::A_inner::level = 10;

	system("pause");
}
```

# 22. 智能指针

## 22.1 智能指针及RAII

**问题**

C++中最令人头疼的问题是强迫程序员对申请的资源（文件，内存等）进行管理，一不小心就会出现泄露（忘记对申请的资源进行释放）的问题。

```C++
// C++
auto ptr = new std::vector<int>();
```

```java
//使用了垃圾回收技术,不在需要人为管理，相关的虚拟机会自动释放不需要使用的资源。
// Java
ArrayList<int> list = new ArrayList<int>();
```

```python
# Python
lst = list()
```

**C++的解决办法：RAII**

在传统 C++ 里我们只好使用 `new` 和 `delete` 去『记得』对资源进行释放。而 C++11 引入了智能指针的概念，使用了引用计数的想法，让程序员不再需要关心手动释放内存。

**解决思路：**

- 利用C++中一个对象出了其作用域会被自动析构，因此我们只需要在构造函数的时候申请空间，而在析构函数（在离开作用域时调用）的时候释放空间，这样，就减轻了程序员在编码过程中，考虑资源释放的问题，这就是**RAII**。

**RAII**，完整的英文是 Resource Acquisition Is Initialization，是 C++ 所特有的资源管理方式。

- 有少量其他语言，如 D、Ada 和 Rust 也采纳了 RAII，但主流的编程语言中， C++ 是唯一一个依赖 RAII 来做资源管理的。
- RAII 依托栈和析构函数，来对所有的资源——包括堆内存在内——进行管理。对 RAII 的使用，使得 C++ 不需要类似于 Java 那样的垃圾收集方法，也能有效地对内存进行管理。

具体而言，C11的stl中为大家带来了3种智能指针，正确合理的使用可以有效的帮助大家管理资源，当然，在C++的使用智能指针没有像Java,python这种具备垃圾回收机制的语言那么舒适，毕竟，程序员还需要做一些额外的事情，但是，这远比传统的C或者C++更加优雅。

3种智能指针分别是：

- `std::shared_ptr` 强指针
- `std::unique_ptr`
- `std::weak_ptr ` 弱指针

在早期有一个auto_ptr，这四种指针在使用上有区别：

-  auto_ptr有缺陷是过时的产物。

-  unique_ptr对auto_ptr的问题进行了修正。

-  shared_ptr使用了引用计数，但是会出现循环引用的问题需要配合后面的weak_ptr一起使用。

## 22.2 引用计数

要正确的理解智能指针，首先必须理解引用计数技术。

**深拷贝、浅拷贝的概念**

- 深拷贝优缺点：
  - 优点：每一个的对象（哪怕是通过拷贝构造函数实例化的对象）的指针都有指向的内存空间，而不是共享，所以在对象析构的时候就不存在重复释放或内存泄露的问题了。
  - 缺点：内存开销大

- 浅拷贝优缺点：
  - 优点：通过拷贝构造函数实例化的对象的指针数据变量指向的共享的内存空间，因此内存开销较小。
  - 缺点：对象析构的时候就可能会重复释放或造成内存泄露。

​    鉴于深拷贝和浅拷贝的优缺点，可采用**引用计数**技术，既减小了内存开销，又避免了堆的重复释放或内存泄露问题。

**举例说明**

在深拷贝的情况下，通过拷贝构造或者赋值构造的对象均各自包含自己的指针指向“Hello”。

![image-20210813002033591](.\images\image-20210813002033591.png)

但是，显然这样比较浪费内存，存在冗余，那么下面的版本更好：

![image-20210813002145887](.\images\image-20210813002145887.png)

我们使用代码来描述这一过程：

```c++
#include "stdafx.h"
#include <iostream.h>
#include <cstring>

class CStudent
{
public:
	CStudent(const char* pszName);
	CStudent(CStudent& obj);

	CStudent& operator=(CStudent& obj);

	void release();

	void Show()
	{
		cout << hex << (int&)m_pszName << m_pszName <<endl;
	}

private:
    char* m_pszName;	
};


CStudent::CStudent(const char* pszName)
{
	m_pszName = new char[256];

	strcpy(m_pszName, pszName);
}


CStudent::CStudent(CStudent& obj)
{
	m_pszName = obj.m_pszName;
	
	//strcpy(m_pszName, obj.m_pszName);
}


CStudent& CStudent::operator=(CStudent& obj)
{
	m_pszName = obj.m_pszName;

	return *this;
}

void CStudent::release()
{
	if (m_pszName != NULL)
	{
		delete m_pszName;
		m_pszName = NULL;
	}
}

int main(int argc, char* argv[])
{
	CStudent stu1("zhang san");

	CStudent stu2("li si");

	CStudent stu3 = stu2;


	stu1.Show();
	stu2.Show();
	stu3.Show();

	stu2.release();

	stu3.Show();

	return 0;
}
```

**这样做带来的问题**：

 但是这样同样存在问题，一旦其中一个对象释放了资源，那么所有的其他对象的资源也被释放了。

 **解决方法**：增加一个变量，记录**资源使用的次数**。

![image-20210813002306583](.\images\image-20210813002306583.png)

```C++
#include "stdafx.h"
#include <iostream.h>
#include <cstring>

class CStudent
{
public:
	CStudent(const char* pszName);
	CStudent(CStudent& obj);

	~CStudent();

	CStudent& operator=(CStudent& obj);

	void release();

	void Show()
	{
		if (*m_pCount > 0)
		{
			cout << hex << (int&)m_pszName << m_pszName <<endl;
		}
	}

private:
    char* m_pszName;
	int * m_pCount;
};


CStudent::CStudent(const char* pszName)
{
	m_pszName = new char[256];
	m_pCount  = new int;

	strcpy(m_pszName, pszName);

	*m_pCount = 1;
}


CStudent::CStudent(CStudent& obj)
{
	m_pszName = obj.m_pszName;
	
	m_pCount = obj.m_pCount;
	(*m_pCount)++;

}

CStudent::~CStudent()
{
	release();
}


CStudent& CStudent::operator=(CStudent& obj)
{
	if (obj.m_pszName == m_pszName)
	{
		return *this;
	}

	if (--(*m_pCount) == 0)
	{
		delete m_pszName;
		m_pszName = NULL;
		delete m_pCount;

	}

	m_pszName = obj.m_pszName;
	m_pCount = obj.m_pCount;

	(*m_pCount)++;

	return *this;
}

void CStudent::release()
{
	if (m_pszName != NULL && --*m_pCount == 0)
	{
		delete m_pszName;
		m_pszName = NULL;

		delete m_pCount;
	}
}

int main(int argc, char* argv[])
{
	CStudent stu1("zhang san");

	CStudent stu2("li si");

	CStudent stu3 = stu2;


	stu1.Show();
	stu2.Show();
	stu3.Show();

	stu2.release();
	stu3.release();

	stu3.Show();

	return 0;
}
```

最后，我们将该引用计数做一个简易的封装，也就是把引用计数作为一个新的类来使用：

```c++
#include "stdafx.h"
#include <iostream.h>
#include <cstring>

struct RefValue 
{

	RefValue(const char* pszName);
	~RefValue();

	void AddRef();
	void Release();

	char* m_pszName;
	int   m_nCount;
};

RefValue::RefValue(const char* pszName)
{
	m_pszName = new char[strlen(pszName)+1];
	
	m_nCount = 1;

}

RefValue::~RefValue()
{

	if (m_pszName != NULL)
	{
		delete m_pszName;
		m_pszName = NULL;
	}
}

void RefValue::AddRef()
{
	m_nCount++;
}

void RefValue::Release()
{
	if (--m_nCount == 0)
	{
		delete this;
	}
	
}


class CStudent
{
public:
	CStudent(const char* pszName);
	CStudent(CStudent& obj);

	~CStudent();

	CStudent& operator=(CStudent& obj);

	void release();

	void Show()
	{
		if (m_pValue->m_nCount > 0)
		{
			cout << hex << (int&)m_pValue->m_pszName << m_pValue->m_nCount <<endl;
		}
	}

private:
	RefValue* m_pValue;
};


CStudent::CStudent(const char* pszName)
{
	m_pValue = new RefValue(pszName);
}


CStudent::CStudent(CStudent& obj)
{
	m_pValue = obj.m_pValue;
	m_pValue->AddRef();
}

CStudent::~CStudent()
{
	release();
}


CStudent& CStudent::operator=(CStudent& obj)
{
	if (obj.m_pValue == m_pValue)
	{
		return *this;
	}

	m_pValue->Release();

	m_pValue = obj.m_pValue;
	m_pValue->AddRef();

	return *this;
}

void CStudent::release()
{
	m_pValue->Release();
}

int main(int argc, char* argv[])
{
	CStudent stu1("zhang san");

	CStudent stu2("li si");

	CStudent stu3 = stu2;


	stu1.Show();
	stu2.Show();
	stu3.Show();

	stu2.release();
	//stu3.release();

	stu3.Show();

	stu3.release();

	return 0;
}
```



**问题**：

上面的做法能在一定程度上解决资源多次重复申请的浪费，但是仍然存在两个核心的问题。

- 如果对其中某一个类对象中的资源进行了修改，那么所有引用该资源的对象全部会被修改，这显然是错误的。
- 当前的计数器作用于Student类，在使用时候，需要强行加上引用计数类，这样复用性不好，使用不方便。

## 22.3 写时拷贝 

**问题**：如果共享资源中的值发生了变化，那么其他使用该共享资源的值如何保持不变？

![image-20210813002433588](.\images\image-20210813002433588.png)

**解决思路**：使用引用计数时，当发生共享资源值改变的时候，需要对其资源进行重新的拷贝，这样改变的时拷贝的值，而不影响原有的对象中的共享资源。

![image-20210813002527128](.\images\image-20210813002527128.png)

**写时拷贝**(COW copy on write)，在所有需要改变值的地方，重新分配内存。

```c++
// Student.cpp : Defines the entry point for the console application.
//

#include "stdafx.h"
#include <iostream.h>
#include <cstring>

struct RefValue 
{

	RefValue(const char* pszName);
	~RefValue();

	void AddRef();
	void Release();

	char* m_pszName;
	int   m_nCount;
};

RefValue::RefValue(const char* pszName)
{
	m_pszName = new char[256];
	
	strcpy(m_pszName, pszName);
	m_nCount = 1;

}

RefValue::~RefValue()
{
	if (m_pszName != NULL)
	{
		delete m_pszName;
		m_pszName = NULL;
	}
}

void RefValue::AddRef()
{
	m_nCount++;
}

void RefValue::Release()
{
	if (--m_nCount == 0)
	{
		delete this;
	}
	
}


class CStudent
{
public:
	CStudent(const char* pszName);
	CStudent(CStudent& obj);

	void SetName(const char* pszName);

	~CStudent();

	CStudent& operator=(CStudent& obj);

	void release();

	void Show()
	{
		if (m_pValue->m_nCount > 0)
		{
			cout << hex << (int&)m_pValue->m_pszName << m_pValue->m_pszName <<endl;
		}
	}

private:
	RefValue* m_pValue;
};

void CStudent::SetName(const char* pszName)
{
	m_pValue->Release();
	m_pValue = new RefValue(pszName);
}

CStudent::CStudent(const char* pszName)
{
	m_pValue = new RefValue(pszName);
}

CStudent::CStudent(CStudent& obj)
{
	m_pValue = obj.m_pValue;
	m_pValue->AddRef();
}

CStudent::~CStudent()
{
	release();
}


CStudent& CStudent::operator=(CStudent& obj)
{
	if (obj.m_pValue == m_pValue)
	{
		return *this;
	}

	m_pValue->Release();

	m_pValue = obj.m_pValue;
	m_pValue->AddRef();

	return *this;
}

void CStudent::release()
{
	m_pValue->Release();
}

int main(int argc, char* argv[])
{
	CStudent stu1("zhang san");

	CStudent stu2("li si");

	CStudent stu3 = stu2;


	stu2.Show();
	stu3.Show();

	stu2.SetName("li si2");

	stu2.Show();
	stu3.Show();

	return 0;
}
```

## 22.4 智能指针的简易实现

前面，我们学会了如何使用引用计数及写时拷贝，这是理解智能指针必不可少的方法。但是，在实际写代码中，我们其实更倾向于让程序员对于资源的管理没有任何的感知，也就是说，最好让程序员只需要考虑资源的何时申请，对于何时释放以及资源内部如何计数等问题，统统交给编译器内部自己处理。

智能指针另外一点就是在使用上要像真正的指针一样可以支持取内容*, 指针访问成员->等操作，因此，就需要对这些运算符进行重载。



```c++
#include "stdafx.h"
#include <iostream.h>

//智能指针

class CStudent
{
public:
  CStudent()
  {
  
  }

  void test()
  {
    cout << "CStudent" << endl;
  }
  
private:
  char* m_pszBuf;
  int   m_nSex;
};

class CRefCount
{
  friend class CSmartPtr;

  CRefCount(CStudent* pStu)
  {
    m_pObj = pStu;
    m_nCount = 1;
  }

  ~CRefCount()
  {
    delete m_pObj;
    m_pObj = NULL;
  }

  void AddRef()
  {
    m_nCount++;
  }

  void Release()
  {
    if (--m_nCount == 0)
    {
      delete this;
    }
  }

private:
  CStudent* m_pObj;
  int       m_nCount;
};


class CSmartPtr
{
public:

  CSmartPtr()
  {
    m_pRef = NULL;
  }

  CSmartPtr(CStudent* pStu)
  {
    m_pRef = new CRefCount(pStu);
  }

  ~CSmartPtr()
  {
    m_pRef->Release();
  }

  CSmartPtr(CSmartPtr& obj)
  {
//     if (m_pRef != NULL)
//     {
//       m_pRef->Release();
//     }

    m_pRef = obj.m_pRef;
    m_pRef->AddRef();

  }

  CSmartPtr& operator=(CSmartPtr& obj)
  {
    if (m_pRef == obj.m_pRef)
    {
      return *this;
    }
      
    if (m_pRef != NULL)
    {
      m_pRef->Release();
    }
    
    m_pRef = obj.m_pRef;
    m_pRef->AddRef();
    

    return *this;
  }

  void test2()
  {
    cout << "test2" << endl;
  }

  CStudent* operator->()
  {
    return m_pRef->m_pObj;
  }

  CStudent** operator&()
  {
    return &m_pRef->m_pObj;
  }

  CStudent& operator*()
  {
    return *m_pRef->m_pObj;
  }

  operator CStudent*()
  {
    return m_pRef->m_pObj;
  }


//   operator CStudent()
//   {
//     return *m_pStu;
//   }

private:
  CRefCount* m_pRef;
};

int main(int argc, char* argv[])
{
  CSmartPtr obj = new CStudent;
  CSmartPtr obj4 = new CStudent;
  
  CSmartPtr obj2 = obj;
  
  {
    CSmartPtr obj3;

    obj3 = obj;
    obj3 = obj4;
  }

  //CStudent* pStu = new CStudent;
  //obj.test();

  //pStu ==> obj->
  //pStu->test();

  //obj->->test();
  //pStu->test();
  

  
//   obj->test(); //==> pStu->test();
// 
//   CStudent** ppStu = &obj;
//   
//   //(obj).test();
//   (*obj).test();
// 
//   CSmartPtr obj2 = obj;
// 
//   obj2->test();

	return 0;
}
```

## 22.5 为智能指针添加模板

我们在前面自己做一个智能指针，让其帮助我们管理资源。但是这并不通用，因此，我们可以为其添加上模板的技术，这样让其更加通用。

```c++
// TestC11.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include <cstring>

using namespace std;

//智能指针：
// 1. 用起来像指针
// 2. 会自己对资源进行释放

class CStudent
{
public:
    CStudent(){}

    void test(){
        cout << "CStudent" << endl;
        m_nSex = 1;
    }

private:
    char* m_pszBuf;
    int   m_nSex;
};


template<typename T>
class CSmartPtr;

template<typename T>
class CRefCount
{
    friend class CSmartPtr<T>;
public:
    CRefCount(T* pStu){
        m_pObj = pStu;
        m_nCount = 1;
    }

    ~CRefCount(){
        delete m_pObj;
        m_pObj = NULL;
    }

    void AddRef(){
        m_nCount++;
    }

    void Release(){
        if (--m_nCount == 0){
            //这么写就表示自己一定要是一个堆对象
            delete this;
        }
    }

private:
    T* m_pObj;
    int       m_nCount;
};

//致命问题， CSmartPtr中表示的类型是固定的，是CStudent, 需要添加模板
template<typename T>
class CSmartPtr
{
public:

    CSmartPtr()
    {
        m_pRef = NULL;
    }

    CSmartPtr(T* pStu)
    {
        m_pRef = new CRefCount<T>(pStu);
    }

    ~CSmartPtr()
    {
        m_pRef->Release();
    }

    CSmartPtr(CSmartPtr& obj)
    {
        m_pRef = obj.m_pRef;
        m_pRef->AddRef();
    }

    CSmartPtr& operator=(CSmartPtr& obj)
    {
        if (m_pRef == obj.m_pRef){
            return *this;
        }

        if (m_pRef != NULL)
        {
            m_pRef->Release();
        }

        m_pRef = obj.m_pRef;
        m_pRef->AddRef();

        return *this;
    }

    void test2()
    {
        cout << "test2" << endl;
    }

    T* operator->()
    {
        return m_pRef->m_pObj;
    }

    T** operator&()
    {
        return &m_pRef->m_pObj;
    }

    T& operator*()
    {
        return *m_pRef->m_pObj;
    }

    operator T*()
    {
        return m_pRef->m_pObj;
    }

private:
    CRefCount<T>* m_pRef;
};

class CTest{
public:
    CTest(){}

};




int main(int argc, char* argv[])
{
    CStudent* pStu = new CStudent();

    //CRefCount<CStudent> ref(pStu);

    CSmartPtr<CStudent> sp1(pStu);
    CSmartPtr<CStudent> sp2(new CStudent()); //拷贝构造
    //sp2 = sp1; //运算符重载

    //
    CSmartPtr<CTest> sp3(new CTest);

   
    return 0;
}
```



另外一种写法：

```c++
    template<typename FriendClass, typename DataType>  
    class PtrCount  
    {  
        friend FriendClass;  
        PtrCount(DataType* _p):p(_p),use(1){}  
        ~PtrCount(){delete p;}  
        DataType* p;  
        size_t use;  
    };  
      
    template<typename DataType>  
    class SmartPtr  
    {  
    public:  
        SmartPtr(DataType *p)  
            :m_ref(new PtrCount<SmartPtr, DataType>(p))  
        {  
        }  
      
        SmartPtr(const SmartPtr &orig)  
            :m_ref(orig.m_ref)  
        {  
            ++m_ref->use;  
        }  
      
        SmartPtr& operator=(const SmartPtr &rhs)  
        {  
            ++rhs.m_ref->use;  
            if (--m_ref->use == 0)  
                delete m_ref;  
            m_ref = rhs.m_ref;  
            return *this;  
        }  
      
        ~SmartPtr()  
        {  
            if (--m_ref->use == 0)  
                delete m_ref;  
        }  
      
    private:  
        PtrCount<SmartPtr, DataType>* m_ref;  
    };  
```

```c++
//另一种实现方式
#include <utility>  // std::swap

class shared_count {
public:
  shared_count() noexcept
    : count_(1) {}
  void add_count() noexcept
  {
    ++count_;
  }
  long reduce_count() noexcept
  {
    return --count_;
  }
  long get_count() const noexcept
  {
    return count_;
  }

private:
  long count_;
};

template <typename T>
class smart_ptr {
public:
  template <typename U>
  friend class smart_ptr;

  explicit smart_ptr(T* ptr = nullptr)
    : ptr_(ptr)
  {
    if (ptr) {
      shared_count_ =
        new shared_count();
    }
  }
  ~smart_ptr()
  {
    if (ptr_ &&
      !shared_count_
         ->reduce_count()) {
      delete ptr_;
      delete shared_count_;
    }
  }

  smart_ptr(const smart_ptr& other)
  {
    ptr_ = other.ptr_;
    if (ptr_) {
      other.shared_count_
        ->add_count();
      shared_count_ =
        other.shared_count_;
    }
  }
  template <typename U>
  smart_ptr(const smart_ptr<U>& other) noexcept
  {
    ptr_ = other.ptr_;
    if (ptr_) {
      other.shared_count_->add_count();
      shared_count_ = other.shared_count_;
    }
  }
  template <typename U>
  smart_ptr(smart_ptr<U>&& other) noexcept
  {
    ptr_ = other.ptr_;
    if (ptr_) {
      shared_count_ =
        other.shared_count_;
      other.ptr_ = nullptr;
    }
  }
  template <typename U>
  smart_ptr(const smart_ptr<U>& other,
            T* ptr) noexcept
  {
    ptr_ = ptr;
    if (ptr_) {
      other.shared_count_
        ->add_count();
      shared_count_ =
        other.shared_count_;
    }
  }
  smart_ptr&
  operator=(smart_ptr rhs) noexcept
  {
    rhs.swap(*this);
    return *this;
  }

  T* get() const noexcept
  {
    return ptr_;
  }
  long use_count() const noexcept
  {
    if (ptr_) {
      return shared_count_
        ->get_count();
    } else {
      return 0;
    }
  }
  void swap(smart_ptr& rhs) noexcept
  {
    using std::swap;
    swap(ptr_, rhs.ptr_);
    swap(shared_count_,
         rhs.shared_count_);
  }

  T& operator*() const noexcept
  {
    return *ptr_;
  }
  T* operator->() const noexcept
  {
    return ptr_;
  }
  operator bool() const noexcept
  {
    return ptr_;
  }

private:
  T* ptr_;
  shared_count* shared_count_;
};

template <typename T>
void swap(smart_ptr<T>& lhs,
          smart_ptr<T>& rhs) noexcept
{
  lhs.swap(rhs);
}

template <typename T, typename U>
smart_ptr<T> static_pointer_cast(
  const smart_ptr<U>& other) noexcept
{
  T* ptr = static_cast<T*>(other.get());
  return smart_ptr<T>(other, ptr);
}

template <typename T, typename U>
smart_ptr<T> reinterpret_pointer_cast(
  const smart_ptr<U>& other) noexcept
{
  T* ptr = reinterpret_cast<T*>(other.get());
  return smart_ptr<T>(other, ptr);
}

template <typename T, typename U>
smart_ptr<T> const_pointer_cast(
  const smart_ptr<U>& other) noexcept
{
  T* ptr = const_cast<T*>(other.get());
  return smart_ptr<T>(other, ptr);
}

template <typename T, typename U>
smart_ptr<T> dynamic_pointer_cast(
  const smart_ptr<U>& other) noexcept
{
  T* ptr = dynamic_cast<T*>(other.get());
  return smart_ptr<T>(other, ptr);
}
```

## 22.6 auto_ptr

> class template
>
> <memory>
>
> # std::auto_ptr
>
> ```
> template <class X> class auto_ptr;
> ```
>
> Automatic Pointer [deprecated]

 从官网的文档上就可以看出，这个auto_ptr指针不推荐使用(deprecated)，原因这里也有说明：

> **Note:** This class template is deprecated as of C++11. [unique_ptr](http://www.cplusplus.com/unique_ptr) is a new facility with a similar functionality, but with improved security (no fake copy assignments), added features (*deleters*) and support for arrays. See [unique_ptr](http://www.cplusplus.com/unique_ptr) for additional information.

 **解释**：auto_ptr指针在c++11标准中就被废除了，可以使用unique_ptr来替代，功能上是相同的，unique_ptr相比较auto_ptr而言，提升了安全性（没有浅拷贝），增加了特性（delete析构）和对数组的支持。



>  This class template provides a limited *garbage collection* facility for pointers, by allowing pointers to have the elements they point to automatically destroyed when the *auto_ptr* object is itself destroyed.

 **解释**：这个类模板提供了有限度的垃圾回收机制，通过将一个指针保存在auto_ptr对象中，当auto_ptr对象析构时，这个对象所保存的指针也会被析构掉。



> `auto_ptr` objects have the peculiarity of *taking ownership* of the pointers assigned to them: An `auto_ptr` object that has ownership over one element is in charge of destroying the element it points to and to deallocate the memory allocated to it when itself is destroyed. The destructor does this by calling `operator delete` automatically.

**解释**：  auto_ptr 对象拥有其内部指针的所有权。这意味着auto_ptr对其内部指针的释放负责，即当自身被释放时，会在析构函数中自动的调用delete，从而释放内部指针的内存。



> Therefore, no two `auto_ptr` objects should *own* the same element, since both would try to destruct them at some point. When an assignment operation takes place between two `auto_ptr` objects, *ownership* is transferred, which means that the object losing ownership is set to no longer point to the element (it is set to the *null pointer*).

解释：  

- 正因如此，不能有两个auto_ptr 对象拥有同一个内部指针的所有权，因为有可能在某个时机，两者均会尝试析构这个内部指针。

- 当**两个auto_ptr对象**之间发生**赋值**操作时，内部指针被拥有的所有权会发生转移，这意味着这个赋值的右者对象会丧失该所有权，不在指向这个内部指针（其会被设置成null指针）。



到这里，我们来看一下auto_ptr的提供的接口和使用方法：

![image-20210813002832004](.\images\image-20210813002832004.png)

其中构造值得说一下：

![image-20210813002854864](.\images\image-20210813002854864.png)

> Constructs an `auto_ptr` object either from a pointer or from another `auto_ptr` object.
> Since `auto_ptr` objects take ownership of the pointer they *point to*, when a new `auto_ptr` is constructed from another `auto_ptr`, the former owner *releases* it.

解释：  auto_ptr的构造的参数可以是一个指针，或者是另外一个auto_ptr对象。

- 当一个新的auto_ptr获取了内部指针的所有权后，之前的拥有者会释放其所有权。

**1.auto_ptr的构造及所有权的转移**

```c++
#include "stdafx.h"
#include <iostream>
#include <memory>
using namespace std;

int _tmain(int argc, _TCHAR* argv[])
{
	//通过指针进行构造
	std::auto_ptr<int> aptr(new int(3)); 
	
	printf("aptr %p : %d\r\n", aptr.get(), *aptr);
    
	//这样会编译出错，因为auto_ptr的构造有关键字explicit
	//explicit关键字表示调用构造函数时不能使用隐式赋值，而必须是显示调用
	//std::auto_ptr<int> aptr2 = new int(3); 

	//可以用其他的auto_ptr指针进行初始化
	std::auto_ptr<int> aptr2 = aptr;
	printf("aptr2 %p : %d\r\n", aptr2.get(), *aptr2);

	//但是这么内存访问出错，直接0xc05,因为aptr已经释放了其所有权。
	//*aptr = 4;
	printf("aptr %p\r\n", aptr.get());
	
	return 0;
}
```

**2.auto_ptr析构及资源的自动释放**

```C++
void foo_release()
{
	//释放
	int* pNew = new int(3);
	{
		std::auto_ptr<int> aptr(pNew);
	}

}
```

- 这里显然，当出了块作用域之后，aptr对象会自动调用析构，然后在析构中会自动的delete其内部指针，也就是出了这个作用域后，其内部指针就被释放了。

- 当然上面这种写法是不推荐的，因为我们这里本质上就是希望不去管理指针的释放工作，上面的写法就又需要程序员自己操心指针的问题，也就是使用**智能指针要避免出现指针的直接使用**！



在这里可以在使用前调用release，从而放弃其内部指针的使用权，但是同样这么做违背了智能指针的初衷。

```C++
void foo_release()
{
	//释放
	int* pNew = new int(3);
	{
		std::auto_ptr<int> aptr(pNew);
		int* p = aptr.release();
	}

}
```

**3.分配新的指针所有权**

  可以调用reset来重新分配指针的所有权，reset中会先释放原来的内部指针的内存，然后分配新的内部指针。

 

```
void foo_reset()
{
	//释放
	int* pNew = new int(3);
	int*p = new int(5);
	{
		std::auto_ptr<int> aptr(pNew);
		aptr.reset(p);

	}
}
```

**4.=运算符的使用**

```C++
void foo_assign()
{
	std::auto_ptr<int> p1;
	std::auto_ptr<int> p2;

	p1 = std::auto_ptr<int>(new int(3));
	*p1 = 4;
	p2 = p1;
}
```



**auto_ptr存在的问题**

为什么11标准会不让使用auto_ptr，原因是其使用有问题。

**1. 作为参数传递会存在问题。**

因为有拷贝构造和赋值的情况下，会释放原有的对象的内部指针，所以当有函数使用的是auto_ptr时，调用后会导致原来的内部指针释放。

```c++
void foo_test(std::auto_ptr<int> p)
{
	printf("%d\r\n", *p);
}

int _tmain(int argc, _TCHAR* argv[])
{
	std::auto_ptr<int> p1 = std::auto_ptr<int>(new int(3));
	foo_test(p1);

	//这里的调用就会出错，因为拷贝构造函数的存在，p1实际上已经释放了其内部指针的所有权了
	printf("%d\r\n", *p1);
	
	return 0;
}
```

**2. 不能使用vector数组**

因为数组的实现，所以这么定义会出错：

```c++
void foo_ary()
{
	std::vector<std::auto_ptr<int>> Ary;
	std::auto_ptr<int> p(new int(3));
	Ary.push_back(p);

	printf("%d\r\n", *p);

}
```

## 22.7 unique_ptr

前面我们讲解了auto_ptr的使用及为什么会被C++11标准抛弃，接下来，我们来学习unique_ptr的使用：

unique_ptr提供了以下操作：

![image-20210813003020772](.\images\image-20210813003020772.png)

看起来似乎与auto_ptr相似，但是其实有区别。

**1. 构造函数**

![image-20210813003119312](.\images\image-20210813003119312.png)

 虽然这里的构造函数比较多，但是可以发现，实际上是没有类似auto_ptr的那种拷贝构造：

 

```c++
void foo_constuct()
{
    //这样构造是可以的
    std::unique_ptr<int> p(new int(3));

    //空构造
    std::unique_ptr<int> p4;

    //下面三种写法会报错
    std::unique_ptr<int> p2 = p;
    std::unique_ptr<int> p3(p);
    p4 = p;

}
```

因此，这就从根源上杜绝了auto_ptr作为参数传递的写法了。

**2. reset**

 reset的用法和auto_ptr是一致的：

 

```c++
void foo_reset()
{
    //释放
    int* pNew = new int(3);
    int*p = new int(5);
    {
        std::unique_ptr<int> uptr(pNew);
        uptr.reset(p);

    }
}
```

**3.release**

release与reset一样，也不会释放原来的内部指针，只是简单的将自身置空。

 

```c++
void foo_release()
{
    //释放
    int* pNew = new int(3);
    int* p = NULL;
    {
        std::unique_ptr<int> uptr(pNew);
        p = uptr.release();
    }
}
```

![image-20210813003241971](.\images\image-20210813003241971.png)

**4.move**

但是多了个move的用法：

 

```c++
void foo_move()
{
    int* p = new int(3);
    
    std::unique_ptr<int> uptr(p);
    std::unique_ptr<int> uptr2 = std::move(uptr);
    
}
```

因为unique_ptr不能将自身对象内部指针直接赋值给其他unique_ptr，所以这里可以使用std::move()函数，让unique_ptr交出其内部指针的所有权，而自身置空，内部指针不会释放。

![image-20210813003332135](.\images\image-20210813003332135.png)

**5.数组**

可以采用move的方法来使用数组。

直接使用仍然会报错：

 

```c++
void foo_ary()
{
    std::vector<std::unique_ptr<int>> Ary;
    std::unique_ptr<int> p(new int(3));
    Ary.push_back(p);

    printf("%d\r\n", *p);

}
```

![image-20210813003526750](.\images\image-20210813003526750.png)

但是可以采用move的办法，这样就编译通过了：

 

```c++
void foo_ary()
{
    std::vector<std::unique_ptr<int>> Ary;
    std::unique_ptr<int> uptr(new int(3));
    Ary.push_back(std::move(uptr));

    printf("%d\r\n", *uptr);

}
```

但是因为uptr的语义，所以作为参数传递了， 转移了内部指针的所有权，原来的uptr就不能使用了。

所以综上，unique_ptr指的是只有一个对象拥有指针的所有权，可以转移，但是不能直接赋值或者拷贝构造。

所有示例代码如下：

 

```c++
// testUniqueptr.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include <memory>
#include <vector>

void foo_constuct()
{
    //这样构造是可以的
    std::unique_ptr<int> p(new int(3));

    //空构造
    std::unique_ptr<int> p4;

    //下面三种写法会报错
//  std::unique_ptr<int> p2 = p;
//  std::unique_ptr<int> p3(p);
//  p4 = p;

}

void foo_reset()
{
    //释放
    int* pNew = new int(3);
    int*p = new int(5);
    {
        std::unique_ptr<int> uptr(pNew);
        uptr.reset(p);

    }
}

void foo_release()
{
    //释放
    int* pNew = new int(3);
    int* p = NULL;
    {
        std::auto_ptr<int> uptr(pNew);
        p = uptr.release();
    }
}



void foo_move()
{
    int* p = new int(3);
    std::unique_ptr<int> uptr(p);
    std::unique_ptr<int> uptr2 = std::move(uptr);
}

void foo_ary()
{
    std::vector<std::unique_ptr<int>> Ary;
    std::unique_ptr<int> uptr(new int(3));
    Ary.push_back(std::move(uptr));

    printf("%d\r\n", *uptr);

}


int _tmain(int argc, _TCHAR* argv[])
{
    foo_ary();




    return 0;
}
```

## 22.8 智能指针的循环引用

```c++
// TestC11.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include <iostream>
#include <cstring>

using namespace std;

//智能指针：
// 1. 用起来像指针
// 2. 会自己对资源进行释放

class CStudent
{
public:
    CStudent() {}

    void test() {
        cout << "CStudent" << endl;
        m_nSex = 1;
    }

private:
    char* m_pszBuf;
    int   m_nSex;
};


template<typename T>
class CSmartPtr;

template<typename T>
class CRefCount
{
    friend class CSmartPtr<T>;
public:
    CRefCount(T* pStu) {
        m_pObj = pStu;
        m_nCount = 1;
    }

    ~CRefCount() {
        delete m_pObj;
        m_pObj = NULL;
    }

    void AddRef() {
        m_nCount++;
    }

    void Release() {
        if (--m_nCount == 0) {
            //这么写就表示自己一定要是一个堆对象
            delete this;
        }
    }

private:
    T* m_pObj;
    int       m_nCount;
};

template<typename T>
class CSmartPtr
{
public:

    CSmartPtr()
    {
        m_pRef = NULL;
    }

    CSmartPtr(T* pStu)
    {
        m_pRef = new CRefCount<T>(pStu);
    }

    ~CSmartPtr()
    {
        if (m_pRef != NULL){
            m_pRef->Release();
        } 
    }

    CSmartPtr(CSmartPtr& obj)
    {
        m_pRef = obj.m_pRef;
        m_pRef->AddRef();
    }

    CSmartPtr& operator=(CSmartPtr& obj)
    {
        if (m_pRef == obj.m_pRef) {
            return *this;
        }

        if (m_pRef != NULL)
        {
            m_pRef->Release();
        }

        m_pRef = obj.m_pRef;
        m_pRef->AddRef();

        return *this;
    }

    void test2()
    {
        cout << "test2" << endl;
    }

    T* operator->()
    {
        return m_pRef->m_pObj;
    }

    T** operator&()
    {
        return &m_pRef->m_pObj;
    }

    T& operator*()
    {
        return *m_pRef->m_pObj;
    }

    operator T*()
    {
        return m_pRef->m_pObj;
    }

private:
    CRefCount<T>* m_pRef;
};

class B;

class A
{

public:
    A() {}
    CSmartPtr<B> m_b;
};

class B
{

public:
    B() {}
    CSmartPtr<A> m_a;
};


int main(int argc, char* argv[])
{
    {
        CSmartPtr<A> a = new A;
        CSmartPtr<B> b = new B;
        a->m_b = b;
        b->m_a = a;
    }

    return 0;
}
```

## 22.9 shared_ptr与weak_ptr

shared_ptr是带引用计数的智能指针：

**1. 构造**

其初始化多了一种写法：std::make_shared<int>

 

```c++
void foo_construct()
{
    int* p = new int(3);

    std::shared_ptr<int> sptr(p);
    std::shared_ptr<int> sptr2(new int(4));
    std::shared_ptr<int> sptr3 = sptr2;
    std::shared_ptr<int> sptr4 = std::make_shared<int>(5);
}
```

![image-20210814111945082](.\images\image-20210814111945082.png)

这里显然可以看到有引用计数的存在。

通过修改上面例子种的sptr3的作用域，可以发现，出了块作用域之后，shared_ptr对应的引用计数的值减少了。

 

```c++
void foo_construct()
{
    int* p = new int(3);

    std::shared_ptr<int> sptr(p);
    std::shared_ptr<int> sptr2(new int(4));
    {
        std::shared_ptr<int> sptr3 = sptr2;
    }
    
    std::shared_ptr<int> sptr4 = std::make_shared<int>(5);

}

```

![image-20210814112822333](.\images\image-20210814112822333.png)

**2. 注意事项：**

1. 如果用同一个指针去初始化两个shared_ptr时，则引用计数仍然会出错：

 

```c++
void foo_test()
{
    int* p = new int(3);

    {
        std::shared_ptr<int> sptr(p);

        {
            std::shared_ptr<int> sptr2(p);
        }
    }
}
```

显然出了最里面的作用域之后，sptr2对象就已经释放了，此时，对于sptr2来说，p的引用计数为0，所有p被释放，但是实际上sptr还存在，所以再释放sptr时，就会0xc0000005.

2. shared_ptr最大的问题是存在循环引用的问题：

  如果两个类的原始指针的循环使用，那么会出现重复释放的问题：

 

 

```c++
class CPerson;
class CSon;

class Cperson
{
public:
    Cperson(){
        
    }

    void Set(CSon* pSon){
        m_pSon = pSon;
    }
    
    ~Cperson(){
        if (m_pSon != nullptr)
        {
            delete m_pSon;
            m_pSon = nullptr;
        }
    }

    CSon* m_pSon;
};

class CSon
{
public:
    CSon(){

    }

    void Set(Cperson* pParent){
        m_pParent = pParent;
    }

    ~CSon(){
        if (m_pParent != nullptr)
        {
            delete m_pParent;
            m_pParent = nullptr;
        }
    }

    Cperson* m_pParent;
};


int _tmain(int argc, _TCHAR* argv[])
{
    Cperson* pPer = new Cperson();
    CSon* pSon = new CSon();

    pPer->Set(pSon);
    pSon->Set(pPer);

    delete pSon;

    return 0;
}
```

  

这里，delete pSon会出现循环的调用父子类的析构函数，问题很大。

因此，这里考虑使用引用计数的shared_ptr来实现。

 

```c++
#pragma once

#include <memory>
class CPerson;
class CSon;

class Cperson
{
public:
    Cperson(){

    }

    void Set(std::shared_ptr<CSon> pSon){
        m_pSon = pSon;
    }

    ~Cperson(){
    }

    std::shared_ptr<CSon> m_pSon;
};

class CSon
{
public:
    CSon(){

    }

    void Set(std::shared_ptr<Cperson> pParent){
        m_pParent = pParent;
    }

    ~CSon(){
    }

    std::shared_ptr<Cperson> m_pParent;
};
```

 

```c++
void testShared()
{
    CSon* pSon = new CSon();
    Cperson* pPer = new Cperson();

    {
        std::shared_ptr<Cperson> shared_Parent(pPer);
        std::shared_ptr<CSon> shared_Son(pSon);

        shared_Parent->Set(shared_Son);
        shared_Son->Set(shared_Parent);

        printf("pSon : use_count = %d\r\n", shared_Son.use_count());
        printf("pPer : use_count = %d\r\n", shared_Parent.use_count());
    }


}
```

这里在出作用域后发现，实际上两个对象均未被销毁：

最后两者的引用计数均为1，原因是出了块作用域之后，两个shared_parent和shared_son均会析构，在这两个智能指针的内部，均会先去判断对应的内部指针是否-1是否为0，显然这里相互引用的情况下，引用计数初值为2，减1后值为1，所以两个指针均不会被释放。

这里，其实只需要一个释放了，另外一个也能跟着释放，可以采用弱指针，即人为的迫使其中一个引用计数为1，从而打破闭环。

这里只需要将上例子中的任意一个强指针改为弱指针即可。

举例：

![image-20210814113642535](.\images\image-20210814113642535.png)

最后的结果：

此时，两个内部指针均会得到释放。

原因是，弱指针的引用不会增加原来的引用计数，那么就使得引用不再是闭环，所以在出作用域之后，全部得到释放。

**weak_ptr的使用**

1. weak_ptr本身并不具有普通内部指针的功能，而只是用来观察其对应的强指针的使用次数。

2. 因此，这里弱指针的在使用上，实际上是一个特例，即不增加引用计数也能获取对象，因此，实际上在使用弱指针时，不能通过弱指针，直接访问内部指针的数据，而应该是先判断该弱指针所观察的强指针是否存在（调用expired()函数），如果存在，那么则使用lock()函数来获取一个新的shared_ptr来使用对应的内部指针。

3. 实际上，如果不存在循环引用，就不需要使用weak_ptr了，这种做法仍然增加了程序员的负担，所以不如java c#等语言垃圾回收机制省心。

 

```c++
void testWeak()
{
    std::shared_ptr<int> sharedPtr(new int(3));
    std::weak_ptr<int> weakPtr(sharedPtr);


    printf("sharedPtr_Count = %d, weakPtr_Count = %d, Value = %d \r\n", sharedPtr.use_count(), weakPtr.use_count(), *sharedPtr);
    //当weakPtr为空或者对应的shared_ptr不再有内部指针时，expired返回为true.
    if (!weakPtr.expired())
    {
        std::shared_ptr<int> sharedPtr2 = weakPtr.lock();
        printf("sharedPtr_Count = %d, weakPtr_Count = %d, Value = %d \r\n", sharedPtr.use_count(), weakPtr.use_count(), *sharedPtr);
        *sharedPtr2 = 5;
    }

    printf("sharedPtr_Count = %d, weakPtr_Count = %d, Value = %d \r\n", sharedPtr.use_count(), weakPtr.use_count(), *sharedPtr);
}
```

执行结果如下：

## 22.10 深入分析shared_ptr与weak_ptr的实现

  stl中使用了shared_ptr来管理一个对象的内部指针，并且使用了weak_ptr来防止前面所提到的shared_ptr循环引用的问题。

  接下来简单的分析shared_ptr和weak_ptr的实现，最后通过自己写代码来模拟shared_ptr和weak_ptr，达到深入学习的目的：

  测试代码如下：

```c++
#include "stdafx.h"
#include <memory>

int _tmain(int argc, _TCHAR* argv[])
{   
    std::shared_ptr<int> sptr(new int(3));
    std::shared_ptr<int> sptr2 = sptr;

    std::weak_ptr<int> wptr = sptr;

    if (!wptr.expired()){
        
        std::shared_ptr<int> sptr3 = wptr.lock();
    }

    return 0;
}
```

1. **首先**看直接看继承关系和类成员：

   shared_ptr与weak_ptr均继承自同一个父类  _Ptr_base

 

```c++
template<class _Ty>
    class shared_ptr
        : public _Ptr_base<_Ty>
    {   // class for reference counted resource management
public:
    typedef shared_ptr<_Ty> _Myt;
    typedef _Ptr_base<_Ty> _Mybase;

    shared_ptr() _NOEXCEPT
        {   // construct empty shared_ptr
        }

    template<class _Ux>
        explicit shared_ptr(_Ux *_Px)
        {   // construct shared_ptr object that owns _Px
        _Resetp(_Px);
        }

    template<class _Ux,
        class _Dx>
        shared_ptr(_Ux *_Px, _Dx _Dt)
        {   // construct with _Px, deleter
        _Resetp(_Px, _Dt);
        }

    shared_ptr(nullptr_t)
        {   // construct empty shared_ptr
        }

    _Myt& operator=(_Myt&& _Right) _NOEXCEPT
        {   // construct shared_ptr object that takes resource from _Right
        shared_ptr(_STD move(_Right)).swap(*this);
        return (*this);
        }

    template<class _Ty2>
        _Myt& operator=(shared_ptr<_Ty2>&& _Right) _NOEXCEPT
        {   // construct shared_ptr object that takes resource from _Right
        shared_ptr(_STD move(_Right)).swap(*this);
        return (*this);
        }

    ~shared_ptr() _NOEXCEPT
        {   // release resource
        this->_Decref();
        }

    _Myt& operator=(const _Myt& _Right) _NOEXCEPT
        {   // assign shared ownership of resource owned by _Right
        shared_ptr(_Right).swap(*this);
        return (*this);
        }

    template<class _Ty2>
        _Myt& operator=(const shared_ptr<_Ty2>& _Right) _NOEXCEPT
        {   // assign shared ownership of resource owned by _Right
        shared_ptr(_Right).swap(*this);
        return (*this);
        }


    void reset() _NOEXCEPT
        {   // release resource and convert to empty shared_ptr object
        shared_ptr().swap(*this);
        }

    template<class _Ux>
        void reset(_Ux *_Px)
        {   // release, take ownership of _Px
        shared_ptr(_Px).swap(*this);
        }

    template<class _Ux,
        class _Dx>
        void reset(_Ux *_Px, _Dx _Dt)
        {   // release, take ownership of _Px, with deleter _Dt
        shared_ptr(_Px, _Dt).swap(*this);
        }

    void swap(_Myt& _Other) _NOEXCEPT
        {   // swap pointers
        this->_Swap(_Other);
        }

    _Ty *get() const _NOEXCEPT
        {   // return pointer to resource
        return (this->_Get());
        }

    typename add_reference<_Ty>::type operator*() const _NOEXCEPT
        {   // return reference to resource
        return (*this->_Get());
        }

    _Ty *operator->() const _NOEXCEPT
        {   // return pointer to resource
        return (this->_Get());
        }

    bool unique() const _NOEXCEPT
        {   // return true if no other shared_ptr object owns this resource
        return (this->use_count() == 1);
        }

    explicit operator bool() const _NOEXCEPT
        {   // test if shared_ptr object owns no resource
        return (this->_Get() != 0);
        }
    };
```

 

```c++
   
            
template<class _Ty>
    class weak_ptr
        : public _Ptr_base<_Ty>
    {   // class for pointer to reference counted resource
public:
    weak_ptr() _NOEXCEPT
        {   // construct empty weak_ptr object
        }

    weak_ptr(const weak_ptr& _Other) _NOEXCEPT
        {   // construct weak_ptr object for resource pointed to by _Other
        this->_Resetw(_Other);
        }

    template<class _Ty2,
        class = typename enable_if<is_convertible<_Ty2 *, _Ty *>::value,
            void>::type>
        weak_ptr(const shared_ptr<_Ty2>& _Other) _NOEXCEPT
        {   // construct weak_ptr object for resource owned by _Other
        this->_Resetw(_Other);
        }

    template<class _Ty2,
        class = typename enable_if<is_convertible<_Ty2 *, _Ty *>::value,
            void>::type>
        weak_ptr(const weak_ptr<_Ty2>& _Other) _NOEXCEPT
        {   // construct weak_ptr object for resource pointed to by _Other
        this->_Resetw(_Other.lock());
        }

    ~weak_ptr() _NOEXCEPT
        {   // release resource
        this->_Decwref();
        }

    weak_ptr& operator=(const weak_ptr& _Right) _NOEXCEPT
        {   // assign from _Right
        this->_Resetw(_Right);
        return (*this);
        }

    template<class _Ty2>
        weak_ptr& operator=(const weak_ptr<_Ty2>& _Right) _NOEXCEPT
        {   // assign from _Right
        this->_Resetw(_Right.lock());
        return (*this);
        }

    template<class _Ty2>
        weak_ptr& operator=(const shared_ptr<_Ty2>& _Right) _NOEXCEPT
        {   // assign from _Right
        this->_Resetw(_Right);
        return (*this);
        }

    void reset() _NOEXCEPT
        {   // release resource, convert to null weak_ptr object
        this->_Resetw();
        }

    void swap(weak_ptr& _Other) _NOEXCEPT
        {   // swap pointers
        this->_Swap(_Other);
        }

    bool expired() const _NOEXCEPT
        {   // return true if resource no longer exists
        return (this->_Expired());
        }

    shared_ptr<_Ty> lock() const _NOEXCEPT
        {   // convert to shared_ptr
        return (shared_ptr<_Ty>(*this, false));
        }
    };
```

  从这里可以看出来shared_ptr和weak_ptr里面本身并没有成员变量，提供的是对外的接口。shared_ptr可以对外提供模拟内部指针的操作，而weak_ptr是用来提供获取shared_ptr的接口。

  

  具体用来记录保存内部指针和使用次数是他们的共同父类_Ptr_base：

```c++
template<class _Ty>
    class _Ptr_base
    {   // base class for shared_ptr and weak_ptr
public:
    typedef _Ptr_base<_Ty> _Myt;
    typedef _Ty element_type;

    _Ptr_base()
        : _Ptr(0), _Rep(0)
        {   // construct
        }

    _Ptr_base(_Myt&& _Right)
        : _Ptr(0), _Rep(0)
        {   // construct _Ptr_base object that takes resource from _Right
        _Assign_rv(_STD forward<_Myt>(_Right));
        }

    template<class _Ty2>
        _Ptr_base(_Ptr_base<_Ty2>&& _Right)
        : _Ptr(_Right._Ptr), _Rep(_Right._Rep)
        {   // construct _Ptr_base object that takes resource from _Right
        _Right._Ptr = 0;
        _Right._Rep = 0;
        }

    _Myt& operator=(_Myt&& _Right)
        {   // construct _Ptr_base object that takes resource from _Right
        _Assign_rv(_STD forward<_Myt>(_Right));
        return (*this);
        }

    void _Assign_rv(_Myt&& _Right)
        {   // assign by moving _Right
        if (this != &_Right)
            _Swap(_Right);
        }

    long use_count() const _NOEXCEPT
        {   // return use count
        return (_Rep ? _Rep->_Use_count() : 0);
        }

    void _Swap(_Ptr_base& _Right)
        {   // swap pointers
        _STD swap(_Rep, _Right._Rep);
        _STD swap(_Ptr, _Right._Ptr);
        }

    template<class _Ty2>
        bool owner_before(const _Ptr_base<_Ty2>& _Right) const
        {   // compare addresses of manager objects
        return (_Rep < _Right._Rep);
        }

    void *_Get_deleter(const _XSTD2 type_info& _Typeid) const
        {   // return pointer to deleter object if its type is _Typeid
        return (_Rep ? _Rep->_Get_deleter(_Typeid) : 0);
        }

    _Ty *_Get() const
        {   // return pointer to resource
        return (_Ptr);
        }

    bool _Expired() const
        {   // test if expired
        return (!_Rep || _Rep->_Expired());
        }

    void _Decref()
        {   // decrement reference count
        if (_Rep != 0)
            _Rep->_Decref();
        }

    void _Reset()
        {   // release resource
        _Reset(0, 0);
        }

    template<class _Ty2>
        void _Reset(const _Ptr_base<_Ty2>& _Other)
        {   // release resource and take ownership of _Other._Ptr
        _Reset(_Other._Ptr, _Other._Rep);
        }

    template<class _Ty2>
        void _Reset(const _Ptr_base<_Ty2>& _Other, bool _Throw)
        {   // release resource and take ownership from weak_ptr _Other._Ptr
        _Reset(_Other._Ptr, _Other._Rep, _Throw);
        }

    template<class _Ty2>
        void _Reset(const _Ptr_base<_Ty2>& _Other, const _Static_tag&)
        {   // release resource and take ownership of _Other._Ptr
        _Reset(static_cast<_Ty *>(_Other._Ptr), _Other._Rep);
        }

    template<class _Ty2>
        void _Reset(const _Ptr_base<_Ty2>& _Other, const _Const_tag&)
        {   // release resource and take ownership of _Other._Ptr
        _Reset(const_cast<_Ty *>(_Other._Ptr), _Other._Rep);
        }

    template<class _Ty2>
        void _Reset(const _Ptr_base<_Ty2>& _Other, const _Dynamic_tag&)
        {   // release resource and take ownership of _Other._Ptr
        _Ty *_Ptr = dynamic_cast<_Ty *>(_Other._Ptr);
        if (_Ptr)
            _Reset(_Ptr, _Other._Rep);
        else
            _Reset();
        }

    template<class _Ty2>
        void _Reset(auto_ptr<_Ty2>&& _Other)
        {   // release resource and take _Other.get()
        _Ty2 *_Px = _Other.get();
        _Reset0(_Px, new _Ref_count<_Ty>(_Px));
        _Other.release();
        _Enable_shared(_Px, _Rep);
        }

    template<class _Ty2>
        void _Reset(_Ty *_Ptr, const _Ptr_base<_Ty2>& _Other)
        {   // release resource and alias _Ptr with _Other_rep
        _Reset(_Ptr, _Other._Rep);
        }

    void _Reset(_Ty *_Other_ptr, _Ref_count_base *_Other_rep)
        {   // release resource and take _Other_ptr through _Other_rep
        if (_Other_rep)
            _Other_rep->_Incref();
        _Reset0(_Other_ptr, _Other_rep);
        }

    void _Reset(_Ty *_Other_ptr, _Ref_count_base *_Other_rep, bool _Throw)
        {   // take _Other_ptr through _Other_rep from weak_ptr if not expired
            // otherwise, leave in default state if !_Throw,
            // otherwise throw exception
        if (_Other_rep && _Other_rep->_Incref_nz())
            _Reset0(_Other_ptr, _Other_rep);
        else if (_Throw)
            _THROW_NCEE(bad_weak_ptr, 0);
        }

    void _Reset0(_Ty *_Other_ptr, _Ref_count_base *_Other_rep)
        {   // release resource and take new resource
        if (_Rep != 0)
            _Rep->_Decref();
        _Rep = _Other_rep;
        _Ptr = _Other_ptr;
        }

    void _Decwref()
        {   // decrement weak reference count
        if (_Rep != 0)
            _Rep->_Decwref();
        }

    void _Resetw()
        {   // release weak reference to resource
        _Resetw((_Ty *)0, 0);
        }

    template<class _Ty2>
        void _Resetw(const _Ptr_base<_Ty2>& _Other)
        {   // release weak reference to resource and take _Other._Ptr
        _Resetw(_Other._Ptr, _Other._Rep);
        }

    template<class _Ty2>
        void _Resetw(const _Ty2 *_Other_ptr, _Ref_count_base *_Other_rep)
        {   // point to _Other_ptr through _Other_rep
        _Resetw(const_cast<_Ty2*>(_Other_ptr), _Other_rep);
        }

    template<class _Ty2>
        void _Resetw(_Ty2 *_Other_ptr, _Ref_count_base *_Other_rep)
        {   // point to _Other_ptr through _Other_rep
        if (_Other_rep)
            _Other_rep->_Incwref();
        if (_Rep != 0)
            _Rep->_Decwref();
        _Rep = _Other_rep;
        _Ptr = _Other_ptr;
        }

private:
    _Ty *_Ptr;
    _Ref_count_base *_Rep;
    template<class _Ty0>
        friend class _Ptr_base;
    };
```

可以看到这个类里面主要提供了两个成员

-   成员_Ty *_Ptr主要用来记录内部指针。 

-   成员_Ref_count_base *_Rep用来记录使用次数和弱指针使用次数。

​      实际上_Ref_count_base *_Rep 这个指针也是new出来的，当weak_count为0时就可以删除，而使用次数是用来记录内部指针的，当使用次数为0时，就可以释放内部指针了。

一些重要的成员函数：

   Reset  _Decref _Decwref use_count等。

  

 再来看看类_Ref_count_base的实现：

```c++
class _Ref_count_base
    {   // common code for reference counting
private:
    virtual void _Destroy() = 0;
    virtual void _Delete_this() = 0;

private:
    _Atomic_counter_t _Uses;
    _Atomic_counter_t _Weaks;

protected:
    _Ref_count_base()
        {   // construct
        _Init_atomic_counter(_Uses, 1);
        _Init_atomic_counter(_Weaks, 1);
        }

public:
    virtual ~_Ref_count_base() _NOEXCEPT
        {   // ensure that derived classes can be destroyed properly
        }

    bool _Incref_nz()
        {   // increment use count if not zero, return true if successful
        for (; ; )
            {   // loop until state is known
 #if defined(_M_IX86) || defined(_M_X64) || defined(_M_CEE_PURE)
            _Atomic_integral_t _Count =
                static_cast<volatile _Atomic_counter_t&>(_Uses);

            if (_Count == 0)
                return (false);

            if (static_cast<_Atomic_integral_t>(_InterlockedCompareExchange(
                    reinterpret_cast<volatile long *>(&_Uses),
                    _Count + 1, _Count)) == _Count)
                return (true);

 #else /* defined(_M_IX86) || defined(_M_X64) || defined(_M_CEE_PURE) */
            _Atomic_integral_t _Count =
                _Load_atomic_counter(_Uses);

            if (_Count == 0)
                return (false);

            if (_Compare_increment_atomic_counter(_Uses, _Count))
                return (true);
 #endif /* defined(_M_IX86) || defined(_M_X64) || defined(_M_CEE_PURE) */
            }
        }

    unsigned int _Get_uses() const
        {   // return use count
        return (_Get_atomic_count(_Uses));
        }

    void _Incref()
        {   // increment use count
        _MT_INCR(_Mtx, _Uses);
        }

    void _Incwref()
        {   // increment weak reference count
        _MT_INCR(_Mtx, _Weaks);
        }

    void _Decref()
        {   // decrement use count
        if (_MT_DECR(_Mtx, _Uses) == 0)
            {   // destroy managed resource, decrement weak reference count
            _Destroy();
            _Decwref();
            }
        }

    void _Decwref()
        {   // decrement weak reference count
        if (_MT_DECR(_Mtx, _Weaks) == 0)
            _Delete_this();
        }

    long _Use_count() const
        {   // return use count
        return (_Get_uses());
        }

    bool _Expired() const
        {   // return true if _Uses == 0
        return (_Get_uses() == 0);
        }

    virtual void *_Get_deleter(const _XSTD2 type_info&) const
        {   // return address of deleter object
        return (0);
        }
    };
```

 

在这个_Ref_count_base类中提供了

   _Atomic_counter_t _Uses;

​    _Atomic_counter_t _Weaks;

 实际上就是记录的内部指针使用次数和_Ref_count_base使用次数。

在这里有一个简单的继承关系：_Ref_count继承自_Ref_count_base

```c++
template<class _Ty>
    class _Ref_count
    : public _Ref_count_base
    {   // handle reference counting for object without deleter
public:
    _Ref_count(_Ty *_Px)
        : _Ref_count_base(), _Ptr(_Px)
        {   // construct
        }

private:
    virtual void _Destroy()
        {   // destroy managed resource
        delete _Ptr;
        }

    virtual void _Delete_this()
        {   // destroy self
        delete this;
        }

    _Ty * _Ptr;
    };
```

这里_Ptr_base中的_Ref_count_base*  _Rep成员是使用的new  _Ref_count。

 

```c++
void _Resetp(_Ux *_Px)
{   // release, take ownership of _Px
    _TRY_BEGIN  // allocate control block and reset
        _Resetp0(_Px, new _Ref_count<_Ux>(_Px));
    _CATCH_ALL  // allocation failed, delete resource
        delete _Px;
    _RERAISE;
    _CATCH_END
}
```

然后使用子类转父类，而这个类实现了_Ref_count_base*的两个接口函数：

  virtual void _Destroy() = 0;

  virtual void _Delete_this() = 0;

这个引用的次数：

 

```
    _Ref_count_base()
        {   // construct
        _Init_atomic_counter(_Uses, 1);
        _Init_atomic_counter(_Weaks, 1);
        }
```

接下来分析内存结构：

这里有两个成员，分别是

 _Ptr(内部指针) ：0x71b8d0

 _Rep(引用base)：0x0071b910

 而引用base这里实际上是_Ref_count对象，因为有虚函数，所以这里存在虚表指针：

 

前4个字节是虚表指针

中间两个4字节分别是内部对象计数器和自身的计数器。

最后4个字节是内部对象指针。

到这里就shared_ptr与weak_ptr的代码就分析的差不多了



最后说一下**计数器增减**的规则：

初始化及增加的情形：

- 当创建一个新的shared_ptr时，内部对象计数器和自身的计数器均置1.

- 当将另外一个shared_ptr赋值给新的shared_ptr时，内部对象计数器+1,自身计数器不变。

- 当将另外一个shared_ptr赋值给新的weak_ptr时,内部对象计数器不变,自身计数器+1。

- 当从weak_ptr获取一个shared_ptr时，内部对象计数器+1,自身计数器不变。

减少的情形：

- 当一个shared_ptr析构时，内部对象计数器-1。当内部对象计数器减为0时，则释放内部对象，并将自身计数器-1。

- 当一个weak_ptr析构时，自身计数器-1。当自身计数器减为0时，则释放自身_Ref_count*对象。

那么就可以自己来模拟强弱指针，并修改成模板。

 

```C++
#include "stdafx.h"
#include <memory>


/*
    问题1：
     为什么会存在强弱指针的计数？
     A{

        B对象弱智能指针（引用次数  1） weak_ptr_uses_count
     }

     B{
     
        A对象智能指针（引用次数  2）   shared_ptr_uses_count
     }




    问题2：
     强弱指针计数的用途是什么，具体的代码实现是什么？

     shared_ptr :  对外提供接口，并无成员变量 表示强指针
               父类：_Ptr_base

     weak_ptr   :  对外提供接口，并无成员变量 表示弱指针
               父类：_Ptr_base

     _Ptr_base{
 
        两个成员变量：
            _Ty *_Ptr;    //表示智能指针关联的原始的指针， 内部指针
            _Ref_count_base *_Rep; //用于管理智能指针的次数
     }

     基类 纯虚类
     _Ref_count_base{
            virtual void _Destroy() _NOEXCEPT = 0;
            virtual void _Delete_this() _NOEXCEPT = 0;

            //实际上表达的是当前有多少个强指针在引用内部指针
            _Atomic_counter_t _Uses;  //表示强指针使用次数 

            //实际上表达的是当前_Ref_count_base类型的使用次数
            _Atomic_counter_t _Weaks; //表示弱指针使用次数
     }

     有一个派生类：
     _Ref_count： //真正的计数器对象，使用时，需要将指针强转为父类指针，仅仅使用接口
                _Ref_count_base
     {
        //派生类多了一个成员
        _Ty * _Ptr; //表达的是内部指针
     }

     //强指针构造，析构，=赋值 拷贝构造等情况下，计数器的变化
     //弱指针构造，析构，=赋值 拷贝构造等情况下，计数器的变化
     //弱指针提升为强指针时，计数器的变化

     //强指针直接构造（拿原始指针构造）时：
     //1. 初始化_Ty * _Ptr
     //2. 创建_Ref_count对象
     //3. _Ref_count_base对象构造时，会分别为_Uses = 1 并且 _Weaks = 1

*/

int _tmain(int argc, _TCHAR* argv[])
{
    std::shared_ptr<int> sptr(new int(3));
    std::shared_ptr<int> sptr2 = sptr;

    std::weak_ptr<int> wptr = sptr;

    if (!wptr.expired()) {
        std::shared_ptr<int> sptr3 = wptr.lock();
    }

    return 0;
}
```

## 22.11 再次编写智能指针

经过前面的分析，我们彻底理解了智能指针的使用，因此，这里我们根据stl中的智能指针的写法，对自己版本的智能指针进行修改，从而到底彻底理解智能指针的目的。

```c++
#pragma once

class CRefCount
{
public:
    CRefCount(){
        m_nUsedCount = 1;
        m_nWeakCount = 1; 
    }

    void incUsed(){
        m_nUsedCount++;
    }

    int decUsed(){
        m_nUsedCount--;
        return m_nUsedCount;
    }

    void incWeak(){
        m_nWeakCount++;
    }

    int decWeak(){
        m_nWeakCount--;

        return m_nWeakCount;
    }

    int getUsed(){
        return m_nUsedCount;
    }

private:
    int m_nUsedCount; //强指针引用次数
    int m_nWeakCount; //弱指针引用次数
};

template<typename T>
class CMySmartPtrBase
{
public:
    CMySmartPtrBase(){};
    ~CMySmartPtrBase(){};

    void destroy(){
        delete m_Ptr;
    }

    void release(){
        if (m_pRef != nullptr && m_pRef->decWeak() == 0){
            delete m_pRef;
        }
    }

protected:
    T* m_Ptr;
    CRefCount* m_pRef;
};

//强指针类型
template<typename T>
class CMyWeakPtr;

template<typename T>
class CStrongPtr : public CMySmartPtrBase<T>
{
    friend class CMyWeakPtr<T>;
public:
    CStrongPtr(){
        m_Ptr = nullptr;
        m_pRef = nullptr;
    }

    explicit CStrongPtr(T* p){
        m_Ptr = p;
        m_pRef = new CRefCount;
    }

    CStrongPtr(CStrongPtr<T>& obj){

        m_Ptr = obj.m_Ptr;
        obj.m_pRef->incUsed();
        m_pRef = obj.m_pRef;
    }

    CStrongPtr<T>& operator=(CStrongPtr<T>& obj){

        if (m_pRef != nullptr && m_pRef->decUsed() == 0){
            destroy();
            release();
        }

        m_Ptr = obj.m_Ptr;
        obj.m_pRef->incUsed();
        m_pRef = obj.m_pRef;

        return *this;
    }

    CStrongPtr(CMyWeakPtr<T>& obj){
        m_Ptr = obj.m_Ptr;
        obj.m_pRef->incUsed();
        m_pRef = obj.m_pRef;
    }

    ~CStrongPtr(){
        if (m_pRef != nullptr && m_pRef->decUsed() == 0){
            destroy();
            release();
        }
    }

    T& operator*(){
        return *m_Ptr;
    }

    T* operator->(){
        return m_Ptr;
    }

    T* get(){
        return m_Ptr;
    }
};


//弱指针类型
template<typename T>
class CMyWeakPtr : public CMySmartPtrBase<T>
{
public:
    friend class CStrongPtr<T>;

    CMyWeakPtr(){
        m_Ptr = nullptr;
        m_pRef = nullptr;
    }

    CMyWeakPtr(CStrongPtr<T>& obj){
        //release();

        m_Ptr = obj.m_Ptr;
        obj.m_pRef->incWeak();
        m_pRef = obj.m_pRef;
    }

    CMyWeakPtr<T>& operator = (CStrongPtr<T>& obj){
        release();

        m_Ptr = obj.m_Ptr;
        obj.m_pRef->incWeak();
        m_pRef = obj.m_pRef;

        return *this;
    }

    CMyWeakPtr(CMyWeakPtr<T>& obj){

        m_Ptr = obj.m_Ptr;
        obj.m_pRef->incWeak();
        m_pRef = obj.m_pRef;
    }

    ~CMyWeakPtr(){
        release();
    }

    CStrongPtr<T>& lock(){
        if (m_pRef == nullptr){
            return CStrongPtr<T>();
        }

        return CStrongPtr<T>(*this);
    }

    bool IsExpried(){
        if (m_pRef == nullptr){
            return true;
        }

        return m_pRef->getUsed() == 0;
    }
};

```

main:

```c++
// MySharedPtr.cpp : 定义控制台应用程序的入口点。
//

#include "stdafx.h"
#include "MySmartPtr.h"

class CSon;

class CTest{
public:

    void set(CStrongPtr<CSon> p2){
        m_p1 = p2;
    }

    CStrongPtr<CSon> m_p1;
};

class CSon{
public:

    void set(CStrongPtr<CTest> p2){
        m_p1 = p2;
    }

    CMyWeakPtr<CTest> m_p1;
};

void foo(){
    CTest* father = new CTest();
    CSon* son = new CSon();


    CStrongPtr<CTest> ptrFather(father);
    CStrongPtr<CSon> ptrSon(son);

    father->set(ptrSon);
    son->set(ptrFather);

}

int _tmain(int argc, _TCHAR* argv[])
{
    foo();

    return 0;
}
```

## 22.12 Android中的智能指针

详细可以看《深入理解Android 内核设计思想》6.1节的内容

Android中的智能指针设计思路与shared_ptr和weak_ptr的一致，但继承关系略有区别。

`shared_ptr&weak_ptr类图：`

![image-20210814173441954](.\images\image-20210814173441954.png)

`android中的sp&wp类图：`

![image-20210814174626655](.\images\image-20210814174626655.png)

对比可知android中要求原始指针需继承计数类RefBase，而shared_ptr中对计数类进行了封装。