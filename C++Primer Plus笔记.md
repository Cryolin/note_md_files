本文档是《C++ Primer的学习笔记》，旨在对《C&C++.md》进行补充。

# 第1章 预备知识

## 1.2 C++简史

### 1.2.3 面向对象编程

OOP不像过程性编程那样，试图使问题满足语言的过程性方法，而是试图让语言来满足问题的要求。其理念是设计与问题的本质特性相对应的数据格式。

从低级组织（如类）到高级组织（如程序）的处理过程叫做自下向上（bottom-up）的编程。

## 1.4 程序创建的技巧

假设您编写了一个C++程序。如何让它运行起来呢？具体的步骤取决于计算机环境和使用的C++编译器，但大体如下：

*1．使用文本编辑器编写程序，并将其保存到文件中，这个文件就是程序的源代码。
2．编译源代码。这意味着运行一个程序，将源代码翻译为主机使用的内部语言——机器语言。包含了翻译后的程序的文件就是程序的目标代码（object code）。
3．将目标代码与其他代码链接起来。例如，C++程序通常使用库。C++库包含一系列计算机例程（被称为函数）的目标代码，这些函数可以执行诸如在屏幕上显示信息或计算平方根等任务。链接指的是将目标代码同使用的函数的目标代码以及一些标准的启动代码（startup code）组合起来，生成程序的运行阶段版本。包含该最终产品的文件被称为可执行代码。*

# 第2章 开始学习C++

## 2.1 进入C++

### 2.1.1 main()函数

函数头描述了函数与调用它的函数之间的接口。

然而，通常，main( )被启动代码调用，而启动代码是由编译器添加到程序中的，是程序和操作系统（UNIX、Windows 7或其他操作系统）之间的桥梁。事实上，该函数头描述的是main( )和操作系统之间的接口。

main()可以使用下面的变体:


```C++
int main(void)
```

在括号中使用关键字void明确地指出，函数不接受任何参数。在C++（不是C）中，让括号空着与在括号中使用void等效。

有些程序员使用下面的函数头，并省略返回语句：

```C++
void main()
```

这在逻辑上是一致的，因为void返回类型意味着函数不返回任何值。该变体适用于很多系统，但由于它不是当前标准强制的一个选项，因此在有些系统上不能工作。因此，读者应避免使用这种格式，而应使用C++标准格式，这不需要做太多的工作就能完成。
最后，ANSI/ISO C++标准对那些抱怨必须在main( )函数最后包含一条返回语句过于繁琐的人做出了让步。如果编译器到达main( )函数末尾时没有遇到返回语句，则认为main( )函数以如下语句结尾：

```C++
return 0;
```

这条隐含的返回语句只适用于main( )函数，而不适用于其他函数。

### 2.1.3 C++预处理器和iostream文件

#include编译指令导致iostream文件的内容随源代码文件的内容一起被发送给编译器。实际上，iostream文件的内容将取代程序中的代码行#include <iostream>。原始文件没有被修改，而是将源代码文件和iostream组合成一个复合文
件，编译的下一阶段将使用该文件。

### 2.1.4 头文件名

像iostream这样的文件叫做包含文件（include file）—由于它们被包含在其他文件中；也叫头文件（header file）—由于它们被包含在文件起始处。

C语言的传统是，头文件使用扩展名h，将其作为一种通过名称标识文件类型的简单方式。例如，头文件math.h支持各种C语言数学函数，但C++的用法变了。现在，对老式C的头文件保留了扩展名h（C++程序仍可以使用这种文件），**而C++头文件则没有扩展名。**

### 2.1.7 C++源代码的格式化

C++中，如下写法是合法的：

```C++
return (0);
```

## 2.2 C++语句

### 2.2.2 赋值语句

C++（和C）有一项不寻常的特性—可以连续使用赋值运算符

```C++
int steinway;
int baldwin;
int yamaha;
yamaha = baldwin = steinway = 88;
```

赋值将从右向左进行。

cout相对于printf()有更丰富的功能：

例如，printf()要打印字符串"25"和整数25，需要使用特殊代码(%s和%d)。但cout借助C++面向对象的特性，将插入运算符(<<)根据其后的数据类型相应地调整其行为，这是一个运算符重载的例子。

另外，它是可扩展的（extensible）。也就是说，可以重新定义<<运算符，使cout能够识别和显示所开发的新数据类型。

## 2.4 函数

### 2.4.3 用户定义的函数

main()函数的返回值的含义：

可以将计算机操作系统（如UNIX或Windows）看作调用程序。因此，main( )的返回值并不是返回给程序的其他部分，而是返回给操作系统。很多操作系统都可以使用程序的返回值。例如，UNIX外壳脚本和Windows命令行批处理文件都被设计成运行程序，并测试它们的返回值（通常叫做退出值）。通常的约定是，退出值为0则意味着程序运行成功，为非零则意味着存在问题。因此，如果C++程序无法打开文件，可以将它设计为返回一个非零值。然后，便可以设计一个外壳脚本或批处理文件来运行该程序，如果该程序发出指示失败的消息，则采取其他措施。

# 第3章 处理数据

## 3.1 简单变量

### 3.1.3 整型short、int、long和longlong

sizeof运算符指出，在使用8位字节的系统中，int的长度为4个字节。可对类型名或变量名使用sizeof运算符。对类型名（如int）使用sizeof运算符时，应将名称放在括号中；但对变量名（如n_short）使用该运算符，括号是可选的：

```C++
	short n_short = SHRT_MAX;
	cout << "int is " << sizeof(int) << " bytes." << endl;
	cout << "short is " << sizeof n_short << " bytes." << endl;
```

#define预处理指令：

```C++
#define INT_MAX 32767
```

在C++编译过程中，首先将源代码传递给预处理器。在这里，#define和#include一样，也是一个预处理器编译指令。该编译指令告诉预处理器：在程序中查找INT_MAX，并将所有的INT_MAX都替换为32767。因此#define编译指令的工作方式与文本编辑器或字处理器中的全局搜索并替换命令相似。

然而，#define编译指令是C语言遗留下来的。C++有一种更好的创建符号常量的方法（使用关键字const，将在后面的一节讨论），所以不会经常使用#define。然而，有些头文件，尤其是那些被设计成可用于C和C++中的头文件，必须使用#define。

C++还有另一种C语言没有的初始化语法：

```C++
int wrens(432);
```

此外，C++11引入了大括号初始化器：

```C++
int num = {7};
int ares{12};

// 大括号内可以不包含任何东西。在这种情况下，变量将被初始化为零：
int rocs = {};	// set rocs to 0
int psychics{};	// set psychics to 0

// 大括号初始化不能传变量，如下代码编译报错：
int a = 1;
// int b{a}; error
```

### 3.1.5 选择整型类型

short比较省内存，是否要用short？

C++提供了大量的整型，应使用哪种类型呢？通常，int被设置为对目标计算机而言最为“自然”的长度。自然长度（natural size）指的是计算机处理起来效率最高的长度。如果没有非常有说服力的理由来选择其他类型，则应使用int。

所以要看场景，是节省内存更重要，还是效率更重要。一般情况下，只有在大型的整型数组时，才使用short。

此外，short类型在参与表达式运算时，会被自动类型转换为int，因为int往往被认为是计算机运算速度最快的类型。所以short仅在内存占用上有优势。

## 3.2 const限定符

但const比#define好。首先，它能够明确指定类型。其次，可以使用C++的作用域规则将定义限制在特定的函数或文件中（作用域规则描述了名称在各种模块中的可知程度，将在第9章讨论）。第三，可以将const用于更复杂的类型，如第4章将介绍的数组和结构。

C++中，""框住的内容默认是const char*，并且同一个字符串其地址是固定的，例如：

```C++
	const char* str1 = "abc";
	const char* str2 = "abc";
	// 打印str1和str2地址相同
	cout << (void*)str1 << endl;
	cout << (void*)str2 << endl;
```

再拓展下，如下代码会报错：

```C++
	// 报错
	// const char*的值不能用于初始化char*的实体
	char* str = "abc";
```

表面上看，可以通过强转强制该代码编过：

```C++
	// 强转强制编过
	char* str = (char*) "abc";
```

但是强转后的str指向的地址仍跟"abc"相同，这块内存是不可修改的，当试图修改时，运行时会报错：

```C++
	const char* str1 = "abc";
	char* str2 = (char*)"abc";
	cout << (void*)str1 << endl;
	// str2跟str1的地址仍是相同的
	cout << (void*)str2 << endl;

	// 编译不报错，运行时报错
	*str2 = 'b';
```

所以，在创建C-style数组时，不建议通过char*创建，还是老老实实通过[]创建吧，通过测试可知，[]创建的数组会开辟一块新的内存：

```C++
	const char* str1 = "abc";
	char str2[10] = "abc";
	// str1和str2地址不同
	cout << (void*)str1 << endl;
	cout << (void*)str2 << endl;
```

关于const修饰的位置，再展开讨论下：

首先，我们知道

```C++
	int a = 1;
	// 解引用不可修改
	const int* p1 = &a;
	// p2不可修改
	int* const p2 = &a;
```

将上面例子的int修改为char后，稍微有点不好理解：

```C++
	// const修饰在前，意味着解引用不可修改
	// str1跟字符串常量"abc"指向的地址一致，对该地址解引用不可修改
	const char* str1 = "abc";
	// 但是可以将str1指向其他的地址
	// 将str1指向的地址从"abc"首地址改为"def"首地址
	str1 = "def";
```

当const修饰数组时，相当于修饰了数组的每一个元素，例如：

```c++
	// 相当于数组arr中的每一个元素都是const int类型的
	const int arr[2] = { 1,2 };
	// arr[0] = 2;		// error
	// arr[1] = 1;		// error
```

那么，当const修饰指针数组时，如下操作是可以的：

```C++
	const char* strs[2] = { "a", "b" };
	strs[0] = "xixi";
```

那如果再加个const呢：

```C++
	const char* const strs[2] = { "a", "b" };
```

遇到这个不要慌，实际上，无非是strs中的每一个元素的类型都是const char* const罢了，根据之前总结的，第一个const意味着解引用不可修改，即数组中的每个元素对应的地址保存的字符串不能变；第二个const意味着指针指向的地址不能修改，那也就意味着此时再去执行strs[0] = "xixi"就会报错了。

## 3.3 浮点数

浮点数的内部表示方法：

可以把第一个数表示为0.341245（基准值）和100（缩放因子），而将第二个数表示为0.341245（基准值相同）和10000（缩放因子更大）。缩放因子的作用是移动小数点的位置，术语浮点因此而得名。C++内部表示浮点数的方法与此相同，只不过它基于的是二进制数，因此缩放因子是2的幂，不是10的幂。

## 3.4 C++算数运算符

### 3.4.4 类型转换

强制类型转换的通用格式如下：

```C++
(typename) value
typename (value)

// 例如
int a = 1;
long b = (long) a;
long c = long (a);
```

第一种格式来自C语言，第二种格式是纯粹的C++。新格式的想法是，要让强制类型转换就像是函数调用。这样对内置类型的强制类型转换就像是为用户定义的类设计的类型转换。

static_cast也可以实现强转，例如：

```c++
char ch = 'M';
int a = static_cast<int>(ch);
```

### 3.4.5 C++11中的auto声明

auto关键字让编译器能够根据初始值的类型推断变量的类型。例如：

```C++
auto n = 100;	// int
auto x = 1.5;	// double
auto y = 1.3e12L;	// long double
```

## 3.6 复习题

5. 下面两条C++语句是否等价？

   ```c++
   char grade = 65;
   char grade = 'A';
   ```

   这两条语句并不真正等价，虽然对于某些系统来说，它们是等效的。最重要的是，只有在使用ASCII码的系统上，第一条语句才将得分设置为字母A，而第二条语句还可用于使用其他编码的系统。其次，65是一个int常量，而‘A’是一个char常量。

# 第4章 复合类型

## 4.1 数组

### 4.1.1 程序说明

注意，如果将sizeof运算符用于数组名，得到的将是整个数组中的字节数。

```C++
int yams[3] = { 1, 2, 3 };

// 打印12
cout << "Size of yams array = " << sizeof yams;
```

C++不能将一个数组赋值给另一个数组，但是java可以。（注：C++11新增的array模板类可以赋值）

C++不能用变量去声明数组的长度，但是java可以。

```C++
int a = 20;
int b[a];		// error，非new创建数组，编译时必须明确数组长度

int a = 10;
int* b = new int[a];	// 0K, new创建数组，运行时明确数组长度
```

如果只对数组的一部分进行初始化，则编译器将把其他元素设置为0。因此，将数组中所有的元素都初始化为0非常简单—只要显式地将第一个元素初始化为0，然后让编译器将其他元素都初始化为0即可：

```C++
long totals[100] = {0};
```

如果初始化数组时方括号内（[ ]）为空，C++编译器将计算元素个数。例如，对于下面的声明：编译器将使things数组包含4个元素。

```C++
short things[] = { 1, 5, 3, 8};
```

### 4.1.3 C++11数组初始化方法

C++11将使用大括号的初始化（列表初始化）作为一种通用初始化方式，可用于所有类型。数组以前就可使用列表初始化，但C++11中的列表初始化新增了一些功能。

首先，初始化数组时，可省略等号（=）：

```C++
double earnings[4] {1.2e4, 1.6e4, 1.1e4, 1.7e4};
```

其次，可不在大括号内包含任何东西，这将把所有元素都设置为零：

```C++
unsigned int counts[10] = {};
float balances[100] {};
```

第三，列表初始化禁止缩窄转换，这在第3章介绍过：

```C++
long plifs[] = {25, 92, 3.0};	// not allowed
char slifs[4] = {'h', 'i', 1122011, '\0'};	// not allowed
char tlifs[4] = {'h', 'i', 112, '\0'};	// allowed
```

在上述代码中，第一条语句不能通过编译，因为将浮点数转换为整型是缩窄操作，即使浮点数的小数点后面为零。第二条语句也不能通过编译，因为1122011超出了char变量的取值范围（这里假设char变量的长度为8位）。第三条语句可通过编译，因为虽然112是一个int值，但它在char变量的取值范围内。C++标准模板库（STL）提供了一种数组替代品—模板类vector，而C++11新增了模板类array。这些替代品比内置复合类型数组更复杂、更灵活，本章将简要地讨论它们，而第16章将更详细地讨论它们。

## 4.2 字符串

C++处理字符串的方式有两种。第一种来自C语言，常被称为C-风格字符串（C-style string）。本章将首先介绍它，然后介绍另一种基于string类库的方法。  

C-风格字符串具有一种特殊的性质：以空字符（null character）结尾，空字符被写作\0，其ASCII码为0，用来标记字符串的结尾。例如，请看下面两个声明：

```c++
char dog[3] = {'d', 'o', 'g'};	// not a string
char cat[4] = {'c', 'a', 't', '\0'};	// a string
```

这种方式创建字符串太繁琐，C风格字符串更常见的格式如下：

``` c++
char bird[11] = "Mr. Cheeps";	// 编译器自动补齐'\0'
char fish[] = "Buddles";	// 编译器自动计算长度
```

```C++
// 注意，C-风格字符串只允许在声明处赋值，声明后不允许等号赋值，如下写法报错：
char str[20];
str = "abcd";		// error

// 可以考虑使用strcpy()
strcpy(str, "abcd");
```

如果指定的长度大于赋值的长度，编译器会自动补'\0'，如下图所示。处理字符串的函数（如cout，strlen）一般情况是根据\0的位置，而不是字符数组的长度判断，所以，如果字符串的长度长于赋值的长度，不会带来问题，但会造成空间的浪费。

![image-20220824201640770](D:\git\note_md_files\images\image-20220824201640770.png)

Q：字符常量和字符串常量的区别？

A：注意，字符串常量（使用双引号）不能与字符常量（使用单引号）互换。字符常量（如'S'）是字符串编码的简写表示。在ASCII系统上，'S'只是83的另一种写法，因此，下面的语句将83赋给shirt_size：  

```C++
char shirt_size = 'S';			// 合法
```

但"S"不是字符常量，它表示的是两个字符（字符S和\0）组成的字符串。更糟糕的是，"S"实际上表示的是字符串所在的内存地址。因此下面的语句试图将一个内存地址赋给shirt_size：  

```C++
char shirt_size = "S";			// 非法
```

### 4.2.3 字符串输入

cin进行字符串读取的问题：

如下代码：

```C++
#include <iostream>
int main()
{
	using namespace std;
	const int ArSize = 20;
	char name[ArSize];
	char dessert[ArSize];

	cout << "Enter your name:\n";
	cin >> name;
	cout << "Enter your favorite dessert:\n";
	cin >> dessert;
	cout << "I have some delicous " << dessert;
	cout << " for you, " << name << ".\n";

	system("pause");
	return 0;
}
```

运行结果：

```
Enter your name:
Alistair Dreeb
Enter your favorite dessert:
I have some delicous Dreeb for you, Alistair.
请按任意键继续. . .
```

我们甚至还没有对“输入甜点的提示”作出反应，程序便把它显示出来了，然后立即显示最后一行。  

cin是如何确定已完成字符串输入呢？由于不能通过键盘输入空字符，因此cin需要用别的方法来确定字符串的结尾位置。cin使用空白（空格、制表符和换行符）来确定字符串的结束位置，这意味着cin在获取字符数组输入时只读取一个单词。读取该单词后，cin将该字符串放到数组中，并自动在结尾添加空字符。  

这个例子的实际结果是，cin把Alistair作为第一个字符串，并将它放到name数组中。这把Dreeb留在输入队列中。当cin在输入队列中搜索用户喜欢的甜点时，它发现了Dreeb，因此cin读取Dreeb，并将它放到dessert数组中（参见图4.4）。  

![image-20220824204636210](D:\git\note_md_files\images\image-20220824204636210.png)

### 4.2.4 每次读取一行字符串输入

istream中的类（如cin）提供了一些面向行的类成员函数：getline( )和get( )。这两个函数都读取一行输入，直到到达换行符。然而，随后getline( )将丢弃换行符，而get( )将换行符保留在输入序列中。  

一：getline()

getline( )函数读取整行，它使用通过回车键输入的换行符来确定输入结尾。要调用这种方法，可以使用cin.getline( )。该函数有两个参数。第一个参数是用来存储输入行的数组的名称，第二个参数是要读的字符数。  使用示例如下：

```C++
#include <iostream>
int main()
{
	using namespace std;
	const int ArSize = 20;
	char name[ArSize];
	char dessert[ArSize];

	cout << "Enter your name:\n";
	cin.getline(name, ArSize);
	cout << "Enter your favorite dessert:\n";
	cin.getline(dessert, ArSize);
	cout << "I have some delicous " << dessert;
	cout << " for you, " << name << ".\n";

	system("pause");
	return 0;
}
```

![image-20220824205600134](D:\git\note_md_files\images\image-20220824205600134.png)

二：get()

get()有两种函数重载，一种跟getline()相同，但区别在于，get()不会丢弃换行符，而是将其添加到输入队列中。

```C++
cin.get(name, Arsize);
// 无法读取，因为get(两参数)不会丢弃换行符，这里读到换行符直接返回，且不消化输入队列中的换行符
// 要解决此问题，需要在中间加一个get()（空参）调用
cin.get(dessert, Arsize);	
```

另一种是空参的get()，其作用是处理下一个字符，包括换行符。

此外，无论是getline()还是get()，其返回类型都是cin对象，所以可以按如下方式链式调用：

```C++
cin.get(name, Arsize).get();
cin.getline(name1, Arsize).getline(name2, Arsize);
```

此外，还有单参的get()，可以将单个字符读取到字符变量内：

```C++
cin.get(ch);
```

## 4.3 string类简介

string对象也可以像C风格字符串一样，用数组表示法访问指定位置的字符，例如：

```C++
string str = "cat";
cout << str[2];
```

### 4.3.1 C++11字符串初始化

正如您预期的，C++11也允许将列表初始化用于C-风格字符串和string对象 ：

```C++
char first_date[] = {"Le Chapon Dodu"};
string third_date = {"The Bread Bowl"};
```

### 4.3.2 赋值、拼接和附加

C++允许字符串对象的拼接，但不允许两个字符串常量的拼接：

```C++
string a = "a";
string b = "b";
string c = a + b;
string d = a + "f";

string e = "a" + "b";			// error
```

C++字符串设置可以这么拼接：

```C++
string str = "abc""123";		// 合法
```

cout打印的时候也是可以这么拼接的：

```C++
cout << "1234\n"
	"abcd";
```

但是cout中不能通过+拼接

```C++
cout << "1234" + "abcd" << endl;		// error
```

### 4.3.3 string类的其他操作

在C++新增string类之前，程序员也需要完成诸如给字符串赋值等工作。对于C-风格字符串，程序员使用C语言库中的函数来完成这些任务。头文件cstring（以前为string.h）提供了这些函数。例如，可以使用函数strcpy( )将字符串复制到字符数组中，使用函数strcat( )将字符串附加到字符数组末尾：  

```C++
strcpy(charr1, charr2);		// copy charr2 to charr1
strcat(charr1, charr2);		// append contents of charr2 to charr1
strlen(charr1);				// get length of charr1
```

相应的，string进行上述操作的方式如下：

```C++
str1 = str2;		// 直接赋值，替代strcpy
str1 += str2;		// 替代strcat()
str1.size();		// 替代strlen()
```

### 4.3.4 string类I/O

这一节主要了解两个点：

1、C-风格字符串，如果没有初始化，其内容是未定义的。针对未定义的C-风格字符串执行strlen()，函数strlen( )从数组的第一个元素开始计算字节数，直到遇到空字符。在这个例子中，在数组末尾的几个字节后才遇到空字符。对于未被初始化的数据，第一个空字符的出现位置是随机的，因此您在运行该程序时，得到的数组长度很可能与此不同。  

```C++
	char charr[20];
	cout << strlen(charr) << endl;		// 结果随机
```

2、cin.getline()双参函数，支持读取一行输入到指定C-风格字符串，但如需读取一行输入到string中，需采用如下方法：

```C++
	// 读取到C-风格字符串
	char charr[20];
	cout << "Enter a line of text:\n";
	cin.getline(charr, 20);

	// 读取到string
	string str;
	cout << "Enter another line of text:\n";
	getline(cin, str);
```

## 4.4 结构简介

结构中的每一项被称为结构成员。

如果您熟悉C语言中的结构，则可能已经注意到了，C++允许在声明结构变量时省略关键字struct：

```C++
struct inflatable goose;	// keyword struct required in C
inflatable vincent;			// keyword struct not required in C++
```

### 4.4.2 C++11结构初始化

与数组一样，C++11也支持将列表初始化用于结构，且等号（=）是可选的：

```C++
inflatable duck {"Daphne", 0.12, 9.98};
```

其次，如果大括号内未包含任何东西，各个成员都将被设置为零。例如，下面的声明导致mayor.volume和mayor.price被设置为零，且mayor.name的每个字节都被设置为零：

```C++
inflatable mayor {};
```

### 4.4.4 其他结构属性

可以同时完成定义结构和创建结构变量的工作。为此，只需将变量名放在结束括号的后面即可：

```C++
struct perks
{
	int key_number;
	char car[12];
} mr_smith, ms_jones;	// two perks variables
```

甚至可以初始化以这种方式创建的变量：

```C++
struct perks
{
	int key_number;
	char car[12];
} mr_glitz =
{
	7,				// value for mr_glitz.key_number member
	"Packard"		// value for mr_glitz.car member
}
```

然而，将结构定义和变量声明分开，可以使程序更易于阅读和理解。

还可以声明没有名称的结构类型，方法是省略名称，同时定义一种结构类型和一个这种类型的变量：

```C++
struct				// no tag
{
	int x;			// 2 members
	int y;
} position;			// a structure variable
```

这样将创建一个名为position的结构变量。可以使用成员运算符来访问它的成员（如position.x），但这种类型没有名称，因此以后无法创建这种类型的变量。本书将不使用这种形式的结构。

## 4.5 共用体

共用体（union）是一种数据格式，它能够存储不同的数据类型，但只能同时存储其中的一种类型。

## 4.6 枚举

C++的enum工具提供了另一种创建符号常量的方式，这种方式可以代替const。它还允许定义新类型，但必须按严格的限制进行。使用enum的句法与使用结构相似。例如，请看下面的语句：

```C++
enum spectrum {red, orange, yellow, green, blue, violet, indigo, ultraviolet};
```

这条语句完成两项工作。

- 让spectrum成为新类型的名称；spectrum被称为枚举（enumeration），就像struct变量被称为结构一样。
- 将red、orange、yellow等作为符号常量，它们对应整数值0～7。这些常量叫作枚举量（enumerator）。

可以用枚举名来声明这种类型的变量：

```C++
spectrum band;
band = blue;
```

## 4.7 指针和自由存储空间

### 4.7.1 声明和初始化指针

*运算符两边的空格是可选的。传统上，C程序员使用这种格式：

```C
int *ptr;
```

这强调*ptr是一个int类型的值。而很多C++程序员使用这种格式：

```C++
int* ptr;
```

这强调的是：int*是一种类型—指向int的指针。

但要知道的是，下面的声明创建一个指针（p1）和一个int变量（p2）：

```C++
int* p1, p2;
```

对每个指针变量名，都需要使用一个*。

### 4.7.3 指针和数字

```C++
int* pt;
pt = 0xB8000000;	// type mismatch
```

在C99标准发布之前，C语言允许这样赋值。但C++在类型一致方面的要求更严格，编译器将显示一条错误消息，通告类型不匹配。要将数字值作为地址来使用，应通过强制类型转换将数字转换为适当的地址类型：

```C++
int* pt;
pt = (int*) 0xB8000000;
```

### 4.7.4 使用new来分配内存

指针除了可以通过取址符获取变量的地址进行创建外，还支持在运行阶段访问未命名的内存。C语言中，可以通过malloc()函数来分配内存；在C++中仍然可以这么做，但C++还有更好的方法---new运算符。

```C++
typename* pointer_name = new typename;

// 例如
int* pt = new int;
```

new int告诉程序，需要适合存储int的内存。new运算符根据类型来确定需要多少字节的内存。然后，它找到这样的内存，并返回其地址。

对于指针，需要指出的另一点是，new分配的内存块通常与常规变量声明分配的内存块不同。变量nights和pd的值都存储在被称为栈（stack）的内存区域中，而new从被称为堆（heap）或自由存储区（free store）的内存区域分配内存。

### 4.7.5 使用delete释放内存

当需要内存时，可以使用new来请求，这只是C++内存管理数据包中有魅力的一个方面。另一个方面是delete运算符，它使得在使用完内存后，能够将其归还给内存池，这是通向最有效地使用内存的关键一步。归还或释放（free）的内存可供程序的其他部分使用。使用delete时，后面要加上指向内存块的指针（这些内存块最初是用new分配的）：

```C++
int* ps = new int;		// 通过new分配内存
...						// 使用内存
delete ps;				// 使用delete释放内存
```

这将释放ps指向的内存，但不会删除指针ps本身。例如，可以将ps重新指向另一个新分配的内存块。一定要配对地使用new和delete；否则将发生内存泄漏（memory leak），也就是说，被分配的内存再也无法使用了。如果内存泄漏严重，则程序将由于不断寻找更多内存而终止。

不要尝试释放已经释放的内存块，C++标准指出，这样做的结果将是不确定的，这意味着什么情况都可能发生。另外，只能用delete来释放使用new分配的内存。然而，对空指针使用delete是安全的。

```C++
int* ps = new int;		// OK
delete ps;				// OK
delete ps;				// not OK now
int jugs = 5;			// OK
int* pi = &jugs;		// OK
delete pi;				// not allowed, memory not allocated by new
```

### 4.7.6 使用new来创建动态数组

如果程序只需要一个值，则可能会声明一个简单变量，因为对于管理一个小型数据对象来说，这样做比使用new和指针更简单，尽管给人留下的印象不那么深刻。通常，对于大型数据（如数组、字符串和结构），应使用new，这正是new的用武之地。**例如，假设要编写一个程序，它是否需要数组取决于运行时用户提供的信息。如果通过声明来创建数组，则在程序被编译时将为它分配内存空间。不管程序最终是否使用数组，数组都在那里，它占用了内存。在编译时给数组分配内存被称为静态联编（static binding），意味着数组是在编译时加入到程序中的。但使用new时，如果在运行阶段需要数组，则创建它；如果不需要，则不创建。还可以在程序运行时选择数组的长度。这被称为动态联编（dynamic binding），意味着数组是在程序运行时创建的。这种数组叫作动态数组（dynamic array）。使用静态联编时，必须在编写程序时指定数组的长度；使用动态联编时，程序将在运行时确定数组的长度。**

使用new创建动态数组

```C++
int* psome = new int[10];		// get a block of 10 ints
```

当程序使用完new分配的内存块时，应使用delete释放它们。然而，对于使用new创建的数组，应使用另一种格式的delete来释放：

```C++
delete[] psome;			// free a dynamic array
```

c-style字符串是一个char指针，所以貌似如下写法是ok的：

```C++
	char temp[] = "abc";
	char* str = new char;
	strcpy(str, temp);
	cout << str << endl;
	delete str;
```

但实际上，上述new操作只创建了长度为sizeof(char)的堆空间，当调用strcpy时，试图往不属于heap buffer的内存写数据，运行时会报错：

```
Heap Corruption
CRT detected that the application wrote to memory after end of heap buffer.
```

正确的写法需要在new时指定长度:

```C++
char* str = new char[20];
```

## 4.8 指针、数组和指针算数

### 4.8.2 指针小结

数组可以有指针表示法，和数组表示法：

```C++
// 数组表示法：
int array_one[3] = {1, 2, 3};

// 指针表示法：
int* array_two = new int[3];
int* array_three = &array_one[0];
```

数组表示法的数组名，实际上是一个地址，指向数组第一个元素的首地址：

```C++
// 如下方式可访问array_one的第三个元素
*(array_one + 2)
```

指针表示法仍可通过[]进行元素访问：

```C++
// 访问数组表示法中的元素
int a = array_one[0];

// 访问指针表示法中的元素
int b = array_two[1];
int c = array_three[2];
```

原因是C++中，[]实际等效于解引用：

```
array_one[0]	等效于		*array_one
array_three[2]		等效于		*(array_three + 2)
```

### 4.8.3 指针和字符串

在cout和多数C++表达式中，char数组名、char指针以及用引号括起的字符串常量都被解释为字符串第一个字符的地址。

例如：

```C++
char a[4] = "abc";
cout << a << endl;		// 数组名a是一个指针，cout识别到char*指针后，会按字符串打印，往后寻找'\0'

char a = 'a';
char* b = &a;
cout << b << endl;		// b也是个char*指针，如果直接cout打印，会按字符串打印，直到找到'\0'，故打印结果不可预知，不要这么使用

char c[3] = {'a', 'b', 'c'};
cout << c << endl;		// c是一个单纯地char数组，其结尾没有'\0'，故cout打印结果亦不可预知。
```

一般来说，如果给cout提供一个指针，它将打印地址。但如果指针的类型为char *，则cout将显示指向的字符串。如果要显示的是字符串的地址，则必须将这种指针强制转换为另一种指针类型，如int *。例如如下代码将打印bird的地址：

```C++
const char* bird = "wren";
cout << bird << " at " << (int*)bird << endl;
```

这里再补充下strcpy()的定义，其两个传参都是传的地址，第一个是目标地址，第二个是要复制的字符串的地址：

```C++
strcpy(ps, animal);
```

本节还介绍了C++对字符串字面量的保存，前面说了，字符串字面量也会被解释为第一个字符的地址，有些编译器会把字符串字面量的字面值**唯一的存储**，而有些编译器不允许修改字符串字面量的字面值。**总结来说，某一指定字面量的字符串字面量的地址固定，且不要试图修改其字面量。**

```C++
// const修饰char*,代表解引用的部分不可修改，这里字符串字面量的值不可修改，故需用const修饰
const char* bird = "niao";
const char* bird2 = "niao";

// 在有些编译器中，同一个字面量的字符串字面量，使用唯一的地址，如下两行打印的地址相同
cout << (int*)bird << endl;
cout << (int*)bird2 << endl;
```

### 4.8.4 使用new创建动态结构

```C++
// 使用new创建inflatable结构
inflatable* ps = new inflatable;

// 使用箭头成员运算符访问指向的结构的成员
ps->good;
```

句点成员运算符&箭头成员运算符：

创建动态结构时，不能将成员运算符句点用于结构名，因为这种结构没有名称，只是知道它的地址。C++专门为这种情况提供了一个运算符：箭头成员运算符： ->。该运算符由连字符和大于号组成，可用于指向结构（包括后面要介绍的类）的指针，就像点运算符可用于结构名一样。

### 4.8.5 自动存储、静态存储和动态存储

【自动存储】可以理解为代码块中的局部变量，在跳出代码块后被释放，自动变量存储在栈中。

【静态存储】是整个程序执行期间都存在的存储方式，使变量成为静态的方式有两种：一种是在函数外面定义它；另一种是在声明变量时使用关键字static：

```C++
static double fee = 56.50;
```

自动存储和静态存储的关键在于：这些方法严格地限制了变量的寿命。变量可能存在于程序的整个生命周期（静态变量），也可能只是在特定函数被执行时存在（自动变量）。

【动态存储】new和delete运算符提供了一种比自动变量和静态变量更灵活的方法。它们管理了一个内存池，这在C++中被称为自由存储空间（free store）或堆（heap）。该内存池同用于静态变量和自动变量的内存是分开的。在栈中，自动添加和删除机制使得占用的内存总是连续的，但new和delete的相互影响可能导致占用的自由存储区不连续，这使得跟踪新分配内存的位置更困难。

## 4.10 数组的替代品

### 4.10.1 模板类Vector

一般而言，下面的声明创建一个名为vt的vector对象，它可存储n_elem个类型为typeName的元素：  

```C++
vector<typeName> vt(n_elem);
```

### 4.10.2 模板类array(C++11)

下面的声明创建一个名为arr的array对象，它包含n_elem个类型为typename的元素：

```C++
array<typeName, n_elem> arr;
```

与创建vector对象不同的是，n_elem不能是变量。  

### 4.10.3 比较数组、vector对象和array对象

三者都可以通过中括号访问：

```C++
	double a1[4] = { 1.2, 2.4, 3.6, 4.8 };
	vector<double> a2(4);
	a2[0] = 1.0 / 3.0;
	a2[1] = 1.0 / 5.0;
	a2[2] = 1.0 / 7.0;
	a2[3] = 1.0 / 9.0;
	array<double, 4> a3 = { 3.14, 2.72, 1.62, 1.41 };
	cout << "a3[2]: " << a3[2] << " at " << &a3[2] << endl;
```

三者都可以越界访问（不安全）：

```C++
a2[-2] = 1.2;
a2[200] = 2.4;
```

array和vector可以通过at()函数访问，避免越界访问，但耗时会增长

```C++
a2.at(1) = 2.3;
```

array对象和数组存储在相同的内存区域（即栈）中，而vector对象存储在另一个区域（自由存储区或堆）中。

注意到可以将一个array对象赋给另一个array对象；而对于数组，必须逐元素复制数据。  

补充一点：虽然三者用起来有点类似，都可以通过角标访问，但是只有数组的名字是地址，其他两个指代的是vector和array对象本身。所以在他们三个作为函数传参时，如果采用地址接收数组、vector和array，则数组直接传数组名即可，后两个需要通过&取地址传入，例如：

```c++
int arr1[2];
vector<int> arr2(2);
array<int, 2> arr3;

void method1(int* arr1);
void method2(vector<int>* arr2);
void method3(array<int, 2>* arr3);

method1(arr1);		// 数组名本身就是地址，直接传入即可
method2(&arr2);		// vector需通过&传地址
method3(&arr3);		// array需通过&传地址
```

# 第5章 循环和关系表达式

## 5.1 for循环

### 5.1.1 for循环的组成部分

在C++中，每个表达式都由值。

有时值不这么明显，例如，下面是一个表达式，因为它由两个值和一个赋值运算符组成：

```C++
x = 20;
```

C++将赋值表达式的值定义为左侧成员的值，因此这个表达式的值为20。由于赋值表达式有值，因此可以编写下面这样的语句：

```C++
maids = (cooks = 4) + 3;
```

表达式cooks = 4的值为4，因此maids的值为7。然而，C++虽然允许这样做，但并不意味着应鼓励这种做法。允许存在上述语句存在的原则也允许编写如下的语句：

```C++
x = y = z = 0;
```

这种方法可以快速地将若干个变量设置为相同的值。优先级表（见附录D）表明，赋值运算符是从右向左结合的，因此首先将0赋给z，然后将z = 0赋给y，依此类推。

// 表达式的【副作用】

当判定表达式的值这种操作改变了内存中数据的值时，我们说表达式有副作用（side effect）。

从表达式到语句的转变很容易，只要加分号即可。因此下面是一个表达式：

```C++
age = 100
```

而下面是一条语句：

```C++
age = 100;
```

更准确地说，这是一条表达式语句。只要加上分号，所有的表达式都可以成为语句，但不一定有编程意义。例如，如果rodents是个变量，则下面就是一条有效的C++语句：

```C++
rodents + 6;		// 合法语句，但无意义
```

### 5.1.6 副作用和顺序点

首先，副作用（side effect）指的是在计算表达式时对某些东西（如存储在变量中的值）进行了修改；顺序点（sequence point）是程序执行过程中的一个点，在这里，进入下一步之前将确保对所有的副作用都进行了评估。在C++中，语句中的分号就是一个顺序点，这意味着程序处理下一条语句之前，赋值运算符、递增运算符和递减运算符执行的所有修改都必须完成。

何为完整表达式呢？它是这样一个表达式：不是另一个更大表达式的子表达式。完整表达式的例子有：表达式语句中的表达式部分以及用作while循环中检测条件的表达式。

```c++
while (guests++ < 10)
	cout << guests << endl;
```

while循环将在本章后面讨论，它类似于只有测试表达式的for循环。在这里，C++新手可能认为“使用值，然后递增”意味着先在cout语句中使用guests的值，再将其值加1。然而，表达式guests++ < 10是一个完整表达式，因为它是一个while循环的测试条件，因此该表达式的末尾是一个顺序点。所以，C++确保副作用（将guests加1）在程序进入cout之前完成。然而，通过使用后缀格式，可确保将guests同10进行比较后再将其值加1。现在来看下面的语句：

```C++
y = (4 + x++) + (6 + x++);
```

表达式4 + x++不是一个完整表达式，因此，C++不保证x的值在计算子表达式4 + x++后立刻增加1。在这个例子中，整条赋值语句是一个完整表达式，而分号标示了顺序点，因此C++只保证程序执行到下一条语句之前，x的值将被递增两次。C++没有规定是在计算每个子表达式之后将x的值递增，还是在整个表达式计算完毕后才将x的值递增，有鉴于此，您应避免使用这样的表达式。

### 5.1.7 前缀格式和后缀格式

下面两条语句的作用是否不同？

```C++
for (n = lim; n > 0; --n)
for (n = lim; n > 0; n--)
```

从逻辑上说，在上述两种情形下，使用前缀格式和后缀格式没有任何区别。表达式的值未被使用，因此只存在副作用。在上面的例子中，使用这些运算符的表达式为完整表达式，因此将x加1和n减1的副作用将在程序进入下一步之前完成，前缀格式和后缀格式的最终效果相同。
然而，虽然选择使用前缀格式还是后缀格式对程序的行为没有影响，但执行速度可能有细微的差别。对于内置类型和当代的编译器而言，这看似不是什么问题。然而，C++允许您针对类定义这些运算符，在这种情况下，用户这样定义前缀函数：将值加1，然后返回结果；但后缀版本首先复制一个副本，将其加1，然后将复制的副本返回。因此，对于类而言，前缀版本的效率比后缀版本高。

### 5.1.10 复合语句（语句块）

复合语句还有一种有趣的特性。如果在语句块中定义一个新的变量，则仅当程序执行该语句块中的语句时，该变量才存在。执行完该语句块后，变量将被释放。这表明此变量仅在该语句块中才是可用的：

```C++
#include <iostream>
int main()
{
	using namespace std;
	int x = 20;
	{							// block starts
		int y = 100;
		cout << x << endl;		// OK
		cout << y << endl;		// OK
	}							// block ends
	cout << x << endl;			// OK
	cout << y << endl;			// invalid, won't compile
	return 0;
}
```

如果在一个语句块中声明一个变量，而外部语句块中也有一个这种名称的变量，情况将如何呢？在声明位置到内部语句块结束的范围之内，新变量将隐藏旧变量；然后就变量再次可见，如下例所示：

```C++
#include <iostream>
int main()
{
	using std::cout;
	using std::endl;
	int x = 20;					// original x
	{							// block starts
		cout << x << endl;		// use original x
		int x = 100;			// new x
		cout << x << endl;		// use new x
	}							// block ends
	cout << x << endl;			// use original x
	return 0;
}
```

### 5.1.11 逗号运算符

C++还为逗号运算符提供了另外两个特性。首先，它确保先计算第一个表达式，然后计算第二个表达式（换句话说，逗号运算符是一个顺序点）。如下所示的表达式是安全的：

```c++
i = 20, j = 2 * i				// i set to 20, then j set to 40
```

其次，C++规定，逗号表达式的值是第二部分的值。例如，上述表达式的值为40，因为j = 2 * i的值为40。

在所有运算符中，逗号运算符的优先级是最低的。例如，下面的语句：

```C++
cata = 17,240;
```

被解释为：

```C++
(cats = 17), 240;
```

也就是说，将cats设置为17，240不起作用。然而，由于括号的优先级最高，下面的表达式将把cats设置为240—逗号右侧的表达式值：

```C++
cats = (17, 240);
```

### 5.1.14 C-风格字符串的比较

由于C++将C-风格字符串视为地址，因此如果使用关系运算符来比较它们，将无法得到满意的结果。相反，应使用C-风格字符串库中的strcmp( )函数来比较。该函数接受两个字符串地址作为参数。这意味着参数可以是指针、字符串常量或字符数组名。如果两个字符串相同，该函数将返回零；如果第一个字符串按字母顺序排在第二个字符串之前，则strcmp( )将返回一个负数值；如果第一个字符串按字母顺序排在第二个字符串之后，则strcmp( )将返回一个正数值。

## 5.2 while循环

### 5.2.2 等待一段时间：编写延时循环

类型别名：

C++为类型建立别名的方式有两种。一种是使用预处理器：

```C++
#define BYTE char
```

这样，预处理器将在编译程序时用char替换所有的BYTE，从而使BYTE成为char的别名。
第二种方法是使用C++（和C）的关键字typedef来创建别名。例如，要将byte作为char的别名，可以这样做：

```C++
typedef char byte;
```

下面是通用格式：

```C++
typedef typeName aliasName;
```

换句话说，如果要将aliasName作为某种类型的别名，可以声明aliasName，如同将aliasName声明为这种类型的变量那样，然后在声明的前面加上关键字typedef。例如，要让byte_pointer成为char指针的别名，可将byte_pointer声明为char指针，然后在前面加上typedef：

```C++
typedef char* byte_pointer;
```

也可以使用#define，不过声明一系列变量时，这种方法不适用。例如，请看下面的代码：

```C++
#define FLOAT_POINTER float*
FLOAT_POINTER pa, pb;
```

预处理器置换将该声明转换为这样：

```C++
float* pa, pb;		// pa a pointer to float, pb just a float
```

typedef方法不会有这样的问题。它能够处理更复杂的类型别名，这使得与使用#define相比，使用typedef是一种更佳的选择—有时候，这也是唯一的选择。
注意，typedef不会创建新类型，而只是为已有的类型建立一个新名称。如果将word作为int的别名，则cout将把word类型的值视为int类型。

## 5.5 循环和文件输入

### 5.5.4 文件尾条件

主要对比了两种读取文件结尾的方式：

cin.get(char)成员函数调用通过返回转换为false的bool值来指出已到达EOF，而cin.get( )成员函数调用则通过返回EOF值来指出已到达EOF，EOF是在文件iostream中定义的。

```c++
while (cin.get(ch))					// 这种返回cin类型，如果到EOF，会被转成false

while ((ch = cin.get()) != EOF)				// 这种更复杂，但可以更方便的替换c中的getchar()
```

# 第6章 分支语句和逻辑运算符

## 6.3 字符函数库cctype

C++从C语言继承了一个与字符相关的、非常方便的函数软件包，它可以简化诸如确定字符是否为大写字母、数字、标点符号等工作，这些函数的原型是在头文件cctype（老式的风格中为ctype.h）中定义的。

| 函数名称  | 返回值                                           |
| --------- | ------------------------------------------------ |
| isalnum() | 如果参数是字母数字，即字母或数字，则函数返回true |
| isalpha() | 如果参数是字母，该函数返回true                   |
| isdigit() | 如果参数是数字（0~9），该函数返回true            |
| islower() | 如果参数是小写字母，该函数返回true               |
| ispunct() | 如果参数是标点符号，该函数返回true               |
| isupper() | 如果参数是大写字母，该函数返回true               |
| tolower() | 如果参数是大写字符，则返回其小写，否则返回该参数 |
| toupper() | 如果参数是小写字符，则返回其大写，否则返回其参数 |

## 6.4 ?:运算符

可以这么接受两个int值：

```C++
	int a, b;
	cout << "Enter two integers: ";
	cin >> a >> b;
```

## 6.5 switch语句

### 6.5.2 switch和if else

如果既可以使用if else if语句，也可以使用switch语句，则当选项不少于3个时，应使用switch语句。因为此时就代码长度和执行速度而言，switch语句的效率更高。

## 6.7 读取数字的循环

本节介绍了cin读取内容不符合预期时的操作，首先，不符合预期时，cin为0，作为条件表达式判断时会被强转为false；其次如果在cin为0后，想继续接收输入，需要调用下cin.clear，下面是例子：

```C++
	int golf[MAX];
	cout << "Please enter your golf scores.\n";
	cout << "You must enter " << MAX << " rounds.\n";
	int i;
	for (i = 0; i < MAX; i++)
	{
		cout << "round #" << i + 1 << ": ";
		while (!(cin >> golf[i]))
		{
			cin.clear();
			while (cin.get() != '\n')
				continue;
			cout << "Please enter a number: ";
		}
	}
```

## 6.8 简单文件输入/输出

### 6.8.1 文本I/O和文本文件

这里再介绍一下文本I/O的概念。使用cin进行输入时，程序将输入视为一系列的字节，其中每个字节都被解释为字符编码。不管目标数据类型是什么，输入一开始都是字符数据——文本数据。然后，cin对象负责将文本转换为其他类型。为说明这是如何完成的，来看一些处理同一个输入行的代码。

假设有如下示例输入行：

38.5 19.2

来看一下使用不同数据类型的变量来存储时，cin是如何处理该输入行的。首先，来看使用char数据类型的情况：

```C++
char ch;
cin >> ch;
```

输入行中的第一个字符被赋给ch。在这里，第一个字符是数字3，其字符编码（二进制）被存储在变量ch中。输入和目标变量都是字符，因此不需要进行转换。注意，这里存储的不是数值3，而是字符3的编码。执行上述输入语句后，输入队列中的下一个字符为字符8，下一个输入操作将对其进行处理。

接下来看看int类型：

```C++
int n;
cin >> n;
```

在这种情况下，cin将不断读取，直到遇到非数字字符。也就是说，它将读取3和8，这样句点将成为输入队列中的下一个字符。cin通过计算发现，这两个字符对应数值38，因此将38的二进制编码复制到变量n中。
接下来看看double类型：

```C++
double x;
cin >> x;
```

在这种情况下，cin将不断读取，直到遇到第一个不属于浮点数的字符。也就是说，cin读取3、8、句点和5，使得空格成为输入队列中的下一个字符。cin通过计算发现，这四个字符对应于数值38.5，因此将38.5的二进制编码（浮点格式）复制到变量x中。
接下来看看char数组的情况：

```C++
char word[50];
cin >> word;
```

在这种情况下，cin将不断读取，直到遇到空白字符。也就是说，它读取3、8、句点和5，使得空格成为输入队列中的下一个字符。然后，cin将这4个字符的字符编码存储到数组word中，并在末尾加上一个空字符。这里不需要进行任何转换。

最后，来看一下另一种使用char数组来存储输入的情况：

```C++
char word[50];
cin.getline(word, 50);
```

在这种情况下，cin将不断读取，直到遇到换行符（示例输入行少于50个字符）。所有字符都将被存储到数组word中，并在末尾加上一个空字符。换行符被丢弃，输入队列中的下一个字符是下一行中的第一个字符。这里不需要进行任何转换。

### 6.8.2 写入到文本文件中

文件输入的库<fstream>与I/O库<iostream>的使用极其类似。

文件输出的主要步骤如下：
1．包含头文件fstream。
2．创建一个ofstream对象。
3．将该ofstream对象同一个文件关联起来。
4．就像使用cout那样使用该ofstream对象。

示例：

```C++
	ofstream outFile;
	outFile.open("carinfo.txt");
	
	cout << "Enter the make and model of automobile: ";
	cin.getline(automobile, 50);
	cout << "Enter the model year: ";
	cin >> year;
	cout << "Enter the original asking price: ";
	cin >> a_price;
	d_price = 0.913 * a_price;
	
	outFile << fixed;			// 普通方式输出，不使用科学计数法
	outFile.precision(2);		// 保留2位小数点
	outFile.setf(ios_base::showpoint);		// 显示浮点小数点后面的零
	outFile << "Make and model: " << automobile << endl;
	outFile << "Year: " << year << endl;
	outFile << "Was asking $" << a_price << endl;
	outFile << "Now asking $" << d_price << endl;
```

### 6.8.3 读取文本文件

与ofstream类似，ifstream用于读文件，且使用方法与cin极其类似。

示例：

```C++
	char filename[SIZE];
	ifstream inFile;

	cout << "Enter name of data file: ";
	cin.getline(filename, SIZE);
	inFile.open(filename);
	if (!inFile.is_open())
	{
		cout << "Could not open the file " << filename << endl;
		cout << "Program terminating.\n";
		exit(EXIT_FAILURE);
	}
	double value;
	double sum = 0.0;
	int count = 0;

	inFile >> value;
	while (inFile.good())
	{
		++count;
		sum += value;
		inFile >> value;
	}
	if (inFile.eof())
		cout << "End of file reached.\n";
	else if (inFile.fail())
		cout << "Input terminated by data mismatch.\n";
	else
		cout << "Input terminated for unknown reason.\n";
	if (count == 0)
		cout << "No data processed.\n";
	else
	{
		cout << "Items read: " << count << endl;
		cout << "Sum: " << sum << endl;
		cout << "Average: " << sum / count << endl;
	}
	inFile.close();
```

每读一次，进行一次合法性判断（good，eof，fail，bad等都是判断合法性的函数）。在需要一个bool值的情况下，inFile的结果为inFile.good( )，即true或false。所以上面的写法可以进一步简化为：

```C++
while (inFile >> value)
{
	// loop body
}
```

# 第7章 函数--C++的编程模块

## 7.3 函数和数组

### 7.3.2 将数组作为参数意味着什么

```C++
int main()
{
	int cookies[ArSize] = {1,2,4,8,16,32,64,128};
	cout << sizeof cookies << endl;
	
	int sum = sum_arr(cookies, ArSize);
}

int sum_arr(int arr[], int n)
{
	cout << sizeof arr << end;
}
```

cookies和arr指向同一个地址。但sizeof cookies的值为32，而sizeof arr为4。这是由于sizeof cookies是整个数组的长度，而sizeof arr只是指针变量的长度（上述程序运行结果是从一个使用4字节地址的系统中获得的）。顺便说一句，这也是必须显式传递数组长度，而不能在sum_arr()中使用sizeof arr的原因；指针本身并没有指出数组的长度。

数组传给函数的是其首地址，且函数不知道长度。如下函数原型等价：

```C++
int sum_arr(int arr[], int n);
int sum_arr(int* arr, int n)
```

### 7.3.3 更多数组函数示例

const修饰指针该如何理解？

思路一：看const修饰的是啥

```C++
const int* p;		// const修饰的*p，所以，*p不可修改
int* const p;		// const修饰p，故p无法修改
```

思路二：

```C++
const int* p;		// *p是const int类型，所以*p是常量，无法修改
int* const p;		// p饰const类型，p无法修改
```

数组作为函数传参时，如果不想让数组被修改，可以使用const修饰：

```C++
int sum_array(const int[] arr, int n);
```

上节说了，数组传参的时候，int[] arr 等价于int* arr，所以const int[] arr等价于 const int* arr，即arr指向的部分不可修改。

### 7.3.4 使用数组区间的函数

可以通过传入数组的首指针和尾指针遍历数组：

```C++
int main()
{
	int cookies[ArSize] = {1,2,4,8,16,32,64,128};
	int sum = sum_arr(cookies, cookies + 20);
}

int sum_arr(const int* begin, const int* end)
{
	const int* pt;
	int total = 0;
	for (pt = begin; pt != end; pt++)
		total += *pt;
	return total;
}
```

指针cookies和cookies+20定义了区间。首先，数组名cookies指向第一个元素。表达式cookies+ 19指向最后一个元素（即
cookies[19]），因此，cookies + 20指向数组结尾后面的一个位置。

C++禁止将const地址赋值给非const指针，否则可以通过非const指针修改const数据，const就没意义了，例如：

```C++
const float g_moon = 1.63;
float* pm = &g_moon;		// invalid

const int months[12] = {31,28,31,30,31,30,31,31,30,31,30,31};
int sum(int arr[], int n);
sum(months, 12);			// invalid
```

## 7.4 函数和二维数组

int arr\[row][column]是一个row行，column列的二维数组，如果将其视为一维数组的话，是一个长度为row的，每个元素为int[column]的一维数组。如下两个等效：

```C++
int arr[r][c] == *(*(arr + r) + c)

// 拆分下可以这么理解
arr	// 长度为r的指针，指针指向的的元素都是int[c]的数组
arr + r		// 偏移r个int[c]的位置的新指针，指向第r行的首地址
*(arr + r) 	// 取出第r行的int[c]的数组
*(arr + r) + c 	// 将指针指向第r行的第c个位置
*(*(arr + r) + c)	// 取出第r行第c个位置的元素
```

## 7.5 函数和C-风格字符串

### 7.5.1 将C-风格字符串作为参数的函数

C-风格字符串作为参数传递时，无需传递长度，可以通过判断是否是'\0'判断是否到字符串结尾。

处理函数中字符串的标准方式如下：

```C++
while(*str)				// quit when *str is '\0'
{
	statement;
	str++;
}
```

## 7.6 函数和结构

本节提到了三个传参的方式：直接传递，地址传递和引用传递，其中，直接传递会在函数创建一份拷贝，造成额外的开销。

## 7.10 函数指针

如果未提到函数指针，则对C或C++函数的讨论将是不完整的。我们将大致介绍一下这个主题，将完整的介绍留给更高级的图书。

与数据项相似，函数也有地址。函数的地址是存储其机器语言代码的内存的开始地址。通常，这些地址对用户而言，既不重要，也没有什么用处，但对程序而言，却很有用。例如，可以编写将另一个函数的地址作为参数的函数。这样第一个函数将能够找到第二个函数，并运行它。与直接调用另一个函数相比，这种方法很笨拙，但它允许在不同的时间传递不同函数的地址，这意味着可以在不同的时间使用不同的函数。

### 7.10.1 函数指针的基础知识

首先通过一个例子来阐释这一过程。假设要设计一个名为estimate()的函数，估算编写指定行数的代码所需的时间，并且希望不同的程序员都将使用该函数。对于所有的用户来说，estimate()中的一部分代码都是相同的，但该函数允许每个程序员提供自己的算法来估算时间。为实现这种目标，采用的机制是，将程序员要使用的算法函数的地址传递给estimate()。为此，必须能够完成下面的工作：

- 获取函数的地址；
- 声明一个函数指针；
- 使用函数指针来调用函数。

#### 1．获取函数的地址

获取函数的地址很简单：只要使用函数名（后面不跟参数）即可。也就是说，如果think()是一个函数，则think就是该函数的地址。要将函数作为参数进行传递，必须传递函数名。一定要区分传递的是函数的地址还是函数的返回值：

```c++
process(think); //passes address of think() to process()
thought(think()); //passes return value of think() to thought()
```

process()调用使得process()函数能够在其内部调用think()函数。thought()调用首先调用think()函数，然后将think()的返回值传递给thought()函数。

#### 2．声明函数指针

声明指向某种数据类型的指针时，必须指定指针指向的类型。同样，声明指向函数的指针时，也必须指定指针指向的函数类型。这意味着声明应指定函数的返回类型以及函数的特征标（参数列表）。也就是说，声明应像函数原型那样指出有关函数的信息。例如，假设 Pamle Coder 编写了一个估算时间的函数，其原型如下：

```C++
double pam(int);    // prototype
```

则正确的指针类型声明如下：

```C++
double (*pf)(int);      // pf points to a function that
                        // takes one int argument and that
                        // returns type double
```

这与`pam()`声明类似，这是将`pam`替换为了`(*pf)`。由于`pam`是函数，因此`(*pf)`也是函数。而如果`(*pf)`是函数，则`pf`就是函数指针。

**提示**：通常，要声明指向特定类型的函数指针，可以首先编写这种函数的原型，然后用`(*pf)`替换函数名。这样`pf`就是这类函数的指针。

为提供正确的运算符优先级，必须在声明中使用括号将`*pf`括起。括号的优先级比`*`运算符高，因此`*pf(int)`意味着`pf()`是一个返回指针的函数，而`(*pf)(int)`意味着`pf`是一个指向函数的指针：

```C++
double (*pf)(int);      // pf points to a function that returns double
double *pf(int);        // pf() a function that returns a pointer-to-double
```

正确地声明`pf`后，便可将相应函数的地址赋给它：

```C++
double pam(int);
double (*pf)(int);
pf = pam;           // pf now points to the pam() function
```

注意：`pam()`的特征标和返回类型必须与`pf`相同。
假设要将将要编写的代码行数和估算算法（如`pam()`函数）的地址传递给`estimate()`，则其原型将如下：

```C++
void estimate(int lines, double (*pf)(int));
```

上述声明指出，第二个参数是一个函数指针，它指向的函数接受一个`int`参数，并返回一个`double`值。要让`estimate()`使用`pam()`函数，需要将`pam()`的地址传递给它：

```C++
eatimate(50, pam);      // function call telling estimate() to use pam()
```

显然，使用函数指针时，比较棘手的是编写原型，而传递地址则非常简单。

#### 3．使用指针来调用函数

现在进入最后一步，即使用指针来调用被指向的函数。线索来自指针声明。`(*pf)`扮演的角色与函数名相同，因此使用`(*pf)`时，只需将它看作函数名即可：

```C++
double pam(int);
double (*pf)(int);
pf = pam;               // pf now points to the pam() function
double x = pam(4);      // call pam() using the function name
double y = (*pf)(5)     // call pam() using the pointer pf
double y = pf(5)        // also call pam() using the pointer pf
```

实际上，C++ 也允许像使用函数名那样使用`pf`，第一种格式虽然不太好看，但它给出了强有力的提示——代码正在使用函数指针。

**历史与逻辑** 为何`pf`和`(*pf)`等价呢？一种学派认为，由于`pf`是函数指针，而`*pf`是函数，因此应将`(*pf)()`用作函数调用。另一种党派认为，由于函数名是指向该函数的指针，指向函数的指针的行为应与函数名相似，因此应将`pf()`用作函数调用。C++ 进行了折衷——两种方式都是正确的，或者至少是允许的，虽然它们在逻辑上是互相冲突的。

### 7.10.2 函数指针示例

程序清单7.18演示了如何使用函数指针。它两次调用estimate( )函数，一次传递betsy( )函数的地址，另一次则传递pam( )函数的地址。在第一种情况下，estimate( )使用betsy( )计算所需的小时数；在第二种情况下，estimate( )使用pam( )进行计算。这种设计有助于今后的程序开发。当Ralph为估算时间而开发自己的算法时，将不需要重新编写estimate( )。相反，他只需提供自己的ralph( )函数，并确保该函数的特征标和返回类型正确即可。当然，重新编写estimate( )也并不是一件非常困难的工作，但同样的原则也适用于更复杂的代码。另外，函数指针方式使得Ralph能够修改estimate( )的行为，虽然他接触不到estimate( )的源代码。

```c++
#include <iostream>
using namespace std;
double betsy(int);
double pam(int);

void estimate(int lines, double (*pf)(int));

int main()
{
	int code;
	cout << "How many lines of code do you need? ";
	cin >> code;
	cout << "Here's Besty's estimate:\n";
	estimate(code, betsy);
	cout << "Here's Pam's estimate:\n";
	estimate(code, pam);

	system("pause");
	return 0;
}

double betsy(int lns)
{
	return 0.05 * lns;
}

double pam(int lns)
{
	return 0.03 * lns + 0.0004 * lns * lns;
}

void estimate(int lines, double (*pf)(int))
{
	cout << lines << " lines will take ";
	cout << (*pf)(lines) << " hour(s)\n";

	// 如下写法也是合理的
	//cout << (*pf)(lines) << " hour(s)\n";
}
```

### 7.10.3 深入探讨函数指针

下面是一些函数的原型，它们的特征标和返回类型相同：

```C++
const double * f1(const double ar[], int n);
const double * f2(const double [], int);
const double * f3(const double *, int);
```

接下来，假设要声明一个指针，它可指向这三个函数之一。假定该指针名为`p1`，则只需将目标函数原型中的函数名替换为`(*p1)`：

```C++
const double (*p1)(const double *, int);
```

可在声明的同时进行初始化：

```C++
const double (*p1)(const double *, int) = f1;
```

使用C++11 的自动类型推断功能时，代码要简单得多：

```C++
auto p2 = f2;       // C++11 automatic type deduction
```

现在看下面的语句：

```C++
cout << (*p1)(av,3) << ": " << *(*p1)(av,3) << endl;
cout << p2(av,3) << ": " << *p2(av,3) << endl;
```

`(*p1)(av,3)`和`p2(av,3)`都调用指向的函数（这里是`f1()`和`f2()`），因此，显示的是这两个函数的返回值，返回值的类型是`const double *`（即`double`值的地址），因此前半部分显示的都是一个`double`值的地址，为查看存储在这些地址的实际值，需要将运算符`*`应用于这些地址，如表达式`*(*p1)(av,3`和`*p2(av,3)`所示。
鉴于需要使用三个函数，如果有一个函数指针数组将很方便，这样，就可以使用`for`循环通过指针依次调用每个函数。如何声明这样的数组呢？显然，这种声明应类似于单个函数指针的声明，但必须在某个地方加上`[3]`，以指出这是一个包含三个函数指针的数组。问题是在什么地方加上`[3]`，答案如下（包含初始化）：

```C++
const double * (*pa[3])(const double *, int) = {f1, f2, f3};
```

为什么将`[3]`放在这个地方呢？`pa`是一个包含三个元素的数组，而要声明这样的数组，首先要声明`pa[3]`。该声明的的其他部分指出了数组包含的元素是什么样的。运算符`[]`的优先级高于`*`，因此`*pa[3]`表明`pa`是一个包含三个指针的数组。上述声明的其他部分指出了每个指针指向的是什么：特征标为`const double *, int`，且返回类型为`const double *`的函数。因此，`pa`是一个包含三个指针的数组，其中每个指针都指向这样的函数，即将`const double *, int`作为参数，并返回一个`const double *`。
这里能否使用`auto`呢？不能。自动类型推断只能用于单值初始化，而不能用于初始化列表。但声明`pa`后，声明同样类型的数组就很简单了：

```C++
auto pb = pa;
```

数组名是指向第一个元素的指针，因此`pa`和`pb`都是指向函数指针的指针。
如何使用它们来调用函数呢？`pa[i]`和`pb[i]`都表示数组中的指针，因此可将任何一种函数调用的表示法用于它们：

```C++
const double * px = pa[0](av,3);
const double * py = (*pb[1])(av,3);
```

要获得指向的`double`值，可以使用运算符`*`：

```C++
double x = *pa[0](av,3);
double y = *(*pb[1])(av,3);
```

可做的另一件事是创建指向整个数组的指针。由于数组名`pa`是指向函数指针的指针，因此指向数组的指针将是这样的指针，即它指向指针的指针。由于可使用单个值对其进行初始化，因此可以使用`auto`：

```C++
auto pc = &pa;      // C++11 atuomatic type deduction
```

如果声明该怎么办？显然，这种声明应类似于`pa`的声明，但由于增加了一层间接，因此需要在某个地方添加一个`*`。具体地说，如果这个指针名为`pd`，则需要指出它是一个指针，而不是数组。这意味着声明的核心部分应为`(*pd)[3]`，其中的括号让标识符`pd`和`*`先结合：

```C++
*pd[3]      // an array of 3 pointers
(*pd)[3]    // a pointer to an array of 3 elements
```

换名话说，`pd`是一个指针，它指向一个包含三个元素的数组。这些元素是什么呢？由`pa`的声明的其他部分描述，结果如下：

```C++
const double * (*(*pd)[3])(const double *, int) = &pa;
```

要调用函数，需认识到这样一点：既然`pd`指向数组，那么`*pd`就是数组，而`(*pd)[i]`是数组中的元素，即函数指针。因此，较简单的函数调用是`(*pd)[i](av,3)`，而`*(*pd)[i](av,3)`是返回的指针指向的值。也可以使用第二种使用指针调用函数的语法：使用`(*(*pd)[i])(av,3)`来调用函数，而`*(*(*pd)[i])(av,3)`是指向的`double`值。
请注意`pa`（它是数组名，表示地址）和`&pa`之间的差别。在大多数情况下，`pa`都是数组第一个元素的地址，即`&pa[0]`。因此，它是单个指针的地址。但`&pa`是整个数组（即三个指针块）的地址。从数字上说，`pa`和`&pa`的值相同，但它们的类型不同。一种差别是，`pa+1`为数组中下一个元素的地址，而`&pa+1`为数组`pa`后面一个数组长度内存块的地址。另一个差别是，要得到第一个元素的值，只需对`pa`解除一次引用，但需要对`&pa`解除两次引用：

```C++
**&pa == *pa == pa[0]
```

程序清单7.19 arfupt.cpp

```C++
// arfupt.cpp -- an array of function pointers
#include <iostream>
// various notations, same signatures
const double * f1(const double ar[], int n);
const double * f2(const double [], int);
const double * f3(const double *, int);
 
int main()
{
    using namespace std;
    double av[3] = {1112.3, 1542.6, 2227.9};
    
    // pointer to a function
    const double * (*p1)(const double *, int) = f1;
    auto p2 = f2;
    // pre-C++ can use the following code instead
    // const double *(*p2)(const double *, int) = f2;
    cout << "Using pointers to functions:\n";
    cout << " Address Value\n";
    cout << (*p1)(av,3) << ": " << *(*p1)(av,3) << endl;
    cout << p2(av,3) << ": " << *p2(av,3) << endl;
    
    // pa an array of pointers
    // auto doesn't work with list initialization
    const double * (*pa[3])(const double *, int) = {f1, f2, f3};
    // but it does work for initializing to a single value
    // pb a pointer to first dlement of pa
    auto pb = pa;
    // pre-C++11 can use the following code instead
    // const double *(**pb)(const double *, int) = pa; 
    cout << "\nUsing an array of pointers to functions:\n";
    cout << " Address Value\n";
    for (int i = 0; i < 3; i++)
        cout << pa[i](av,3) << ": " << *pa[i](av,3) << endl;
    cout << "\nUsing a pointer to a pointer to a function:\n";
    cout << " Address Value\n";
    for (int i = 0; i < 3; i++)
        cout << pb[i](av,3) << ": " << *pb[i](av,3) << endl;
        
    // what about a pointer to an array of function pointers
    cout << "\nUsing pointers to an array of pointers:\n";
    cout << " Address Value\n";
    // easy way to declare pc
    auto pc = &pa;
    // pre-C++11 cna use the following code instead
    // const double * (*(*pc)[3])(const double *, int) = &pa;
    cout << (*pc)[0](av,3) << ": " << *(*pc)[0](av,3) << endl;
    // hard way to declard pd
    const double *(*(*pd)[3])(const double *, int) = &pa;
    // store return value in pdb
    const double * pdb = (*pd)[1](av,3);
    cout << pdb << ": " << *pdb << endl;
    // alternative notation
    cout << (*(*pd)[2])(av,3) << ": " << *(*(*pd)[2])(av,3) << endl;
    return 0;
}

// some rather dull functions

const double * f1(const double * ar, int n)
{
    return ar;
}

const double * f2(const double ar[], int n)
{
    return ar+1;
}

const double * f3(const double ar[], int n)
{
    return ar+2;
}
```

显示的地址为数组`av`中`double`值的存储位置。

**感谢 auto**：C++11 的目标之一是让 C++ 更容易使用，从而让程序员将主要精力放在设计而不是细节上。自动类型推断演示了这一点。

```C++
auto pc = &pa;                                  // C++11 automatic type deduction
const double * (*(*pc)[3])(const double *, int) = &pa; // C++98, do it yourself
```

自动类型推断功能表明，编译器的角色发生了改变。在 C++98 中，编译器利用其知识帮助您发现错误，而在 C++11 中，编译器利用其知识帮助您进行正确的声明。
存在一个潜在的缺点。自动类型推断确保变量的类型与赋给它的初值类型一致，但您提供的初值的类型可能不对：

```C++
auto pc = *pc;      //oops! used *pa instead of &pa
```

上述声明导致`pc`的类型与`*pa`一致，后面使用它时假定其类型与`&pa`相同，这将导致编译错误。

### 7.10.4 使用typedef进行简化

除`auto`外，C++ 还提供了其他简化声明的工具。关键字`typedef`可以创建类型别名：

```C++
typedef const real;     // makes real another name for double
```

这里采用的方法是，将别名当做标识符进行声明，并在开关使用关键字`typedef`。因此，可将`p_fun`声明为函数指针类型的别名：

```C++
typedef const double * (*p_fun)(const double *, int);   // p_fun now a type name
p_fun p1 = f1;      // p1 points to the f1() function
```

然后使用这个别名来简化代码：

```C++
p_fun pa[3] = {f1, f2, f3};     // pa an array of 3 funtion pointers
p_fun (*pd)[3] = &pa;           // pd points to an array of 3 function pointers
```

使用`typedef`可减少输入量，让您编写代码时不容易犯错，并让程序更容易理解。

# 第8章 函数探幽

## 8.1 内联函数

编译过程的最终产品是可执行程序——由一组机器语言指令组成。运行程序时，操作系统将这些指令载入到计算机内存中，因此每条指令都有特定的内存地址。计算机随后将逐步执行这些指令。有时（如有循环或分支语句时），将跳过一些指令，向前或向后跳到特定地址。常规函数调用也使程序跳到另一个地址（函数的地址），并在函数结束时返回。下面更详细地介绍这一过程的典型实现。执行到函数调用指令时，程序将在函数调用后立即存储该指令的内存地址，并将函数参数复制到堆栈（为此保留的内存块），跳到标记函数起点的内存单元，执行函数代码（也许还需将返回值放入到寄存器中），然后跳回到地址被保存的指令处（这与阅读文章时停下来看脚注，并在阅读完脚注后返回到以前阅读的地方类似）。来回跳跃并记录跳跃位置意味着以前使用函数时，需要一定的开销。

C++内联函数提供了另一种选择。内联函数的编译代码与其他程序代码“内联”起来了。也就是说，编译器将使用相应的函数代码替换函数调用。对于内联代码，程序无需跳到另一个位置处执行代码，再跳回来。因此，内联函数的运行速度比常规函数稍快，但代价是需要占用更多内存。如果程序在10个不同的地方调用同一个内联函数，则该程序将包含该函数代码的10个副本。

应有选择地使用内联函数。如果执行函数代码的时间比处理函数调用机制的时间长，则节省的时间将只占整个过程的很小一部分。如果代码执行时间很短，则内联调用就可以节省非内联调用使用的大部分时间。另一方面，由于这个过程相当快，因此尽管节省了该过程的大部分时间，但节省的时间绝对值并不大，除非该函数经常被调用。

内联函数不可以递归。

注意：“内联”不是简单的替换。先看C中的#define宏定义：

```c++
#define SQUARE(X) X*X
// 这并不是通过传递参数实现的，而是通过文本替换来实现的——X是“参数”的符号标记。
a = SQUARE(5.0); is replaced by a = 5.0*5.0;
a = SQUARE(4.5 + 7.5);	is replaced by a = 4.5 + 7.5*4.5 + 7.5;
a = SQUARE(c++);	is replaced by a = c++*c++;
// 上述示例只有第一个能正常工作。
```

内联是编译时替换，且保证了函数“值传递”的特性，例如：

```C++
#include <iostream>
using namespace std;

inline void calc(int x);

int main()
{
	int x = 5;
	calc(x);
	cout << "x = " << x << endl;

	system("pause");
	return 0;
}

inline void calc(int x)
{
	x = 10;
}
```

calc(x);做了内联处理，但做了复制

```C++
int x = 5;
{
	// 值传递，操作的仍是副本
	x_copy = 10;
}
```

## 8.2 引用变量

引用变量的主要用途是用作函数的形参。通过将引用变量用作参数，函数将使用原始数据，而不是其副本。

### 8.2.1 创建引用变量

C++给&符号赋予了另一个含义，将其用来声明引用。例如，要将rodents作为rats变量的别名，可以这样做：

```c++
int rats;
int& rodents = rats;
```

引用变量类型是int&，意为int类型的引用。引用变量与原变量的地址相同，值也相同，例如：

```C++
	int rats = 101;
	int& rodents = rats;
	cout << "rats = " << rats;
	cout << ", rodents = " << rodents << endl;

	rodents++;
	cout << "rats = " << rats;
	cout << ", rodents = " << rodents << endl;

	cout << "rats address = " << &rats;
	cout << ", rodents address = " << &rodents << endl;
```

打印：

```
rats = 101, rodents = 101
rats = 102, rodents = 102
rats address = 00000052B6EFF874, rodents address = 00000052B6EFF874
```

引用和指针的差别之一是：。引用必须在声明引用时将其初始化，而不能像指针那样，先声明，再赋值：

```C++
int rat;
int& rodent;
rodent = rat;		// No, you can't do this.
```

引用更接近const指针，必须在创建时进行初始化，一旦与某个变量关联起来，就将一直效忠于它。也就是说：

```C++
int& rodents = rats;
```

实际上是下述代码的伪装表示：

```C++
int* const pr = &rats;
```

引用在被创建时即关联了是谁的别名，后续无法再修改，例如：

```C++
	int rats = 10l;
	int& rodents = rats;

	cout << "rats = " << rats;
	cout << ", rodents = " << rodents << endl;

	cout << "rats address = " << &rats;
	cout << ", rodents address = " << &rodents << endl;

	int bunnies = 50;
	rodents = bunnies;
	cout << "bunnies = " << bunnies;
	cout << ", rats = " << rats;
	cout << ", rodents = " << rodents << endl;

	cout << "bunnies address = " << &bunnies;
	cout << ", rodents address =" << &rodents << endl;
```

打印：

```
rats = 10, rodents = 10
rats address = 000000DD40D8FBE4, rodents address = 000000DD40D8FBE4
bunnies = 50, rats = 50, rodents = 50
bunnies address = 000000DD40D8FC24, rodents address =000000DD40D8FBE4
```

### 8.2.2 将引用作为函数参数

函数按值传递导致被调用函数使用调用程序的值的拷贝，C++引入引用后新增了按引用传递的方式，省略了拷贝，提升了时间和空间效率。一般在传递结构体或类对象时，可以考虑传递引用。

### 8.2.3 引用的属性和特别之处

基本数据类型不建议使用引用，可在传参时增加const修饰：

```C++
double refcube(const double& ra);
```

引用的传参有着严格的要求，必须是类型一致的左值，例如：

```C++
double refcube(double& ra);

double side = 3.0;
refcube(side);		// 合法
double* pd = &side;
refcube(*pd);		// 合法
double& rd = side;
refcube(rd);		// 合法

long edge = 5L;
refcube(edge);		// 非法，类型不匹配
refcube(side + 1.0);	// 非法，非左值
```

但是，如果函数接收的传参是const引用，则在**如下两种情况下**是能通过编译的。例如对于edge和side+1.0的传参，函数会创建一个临时变量，初始化为edge和side+1.0的值，然后，ra将成为该临时变量的引用。

1、 实参的类型正确，但不是左值；
2、 实参的类型不正确，但可以转换为正确的类型。

```C++
double refcube(const double& ra);

double side = 3.0;
refcube(side);		// 合法
double* pd = &side;
refcube(*pd);		// 合法
double& rd = side;
refcube(rd);		// 合法

long edge = 5L;
refcube(edge);		// 类型不匹配，创建临时变量
refcube(side + 1.0);	// 非左值，创建临时变量
```

左值是什么呢？左值参数是可被引用的数据对象，例如，变量、数组元素、结构成员、引用和解除引用的指针都是左值。非左值包括字面常量（用引号括起的字符串除外，它们由其地址表示）和包含多项的表达式。在C语言中，左值最初指的是可出现在赋值语句左边的实体，但这是引入关键字const之前的情况。现在，常规变量和const变量都可视为左值，因为可通过地址访问它们。但常规变量属于可修改的左值，而const变量属于不可修改的左值。

## 8.3 默认参数

如何设置默认值呢？必须通过函数原型。由于编译器通过查看原型来了解函数所使用的参数数目，因此函数原型也必须将可能的默认参数告知程序。方法是将值赋给原型中的参数。例如，left( )的原型如下：

```C++
char* left(const char* str, int n = 1);
```

对于带参数列表的函数，必须从右向左添加默认值。也就是说，要为某个参数设置默认值，则必须为它右边的所有参数提供默认值：

```C++
int harpo(int n, int m = 4, int j = 5);				// valid
int chico(int n, int m = 6, int j);					// invalid
int groucho(int k = 1, int m = 2, int n = 3);		// valid
```

注意默认参数需要在声明时指定默认值，定义时不能再次指定，否则会报错：**重定义默认参数**

```C++
void print_str(const char* str, int flag = 0);	// 声明时指定默认参数
void print_str(const char* str, int flag)		// 定义时不能指定默认参数
{
	cout << str << endl;
}
```

## 8.4 函数重载

与java一样，C++通过函数签名判断是否重载。有一些细节需要注意：

1、 如何判断函数调用匹配哪个重载？

1. 类型匹配，直接匹配
2. 类型不匹配，看是否能强制转换，如果有多个重载都能强转，则无法通过编译。如果只有一个可以强转，则选择该函数。

```C++
#include <iostream>
using namespace std;

void print(double d, int width);
void print(long l, int width);
void print(int i, int width);

int main()
{
	unsigned int year = 3210;
	print(year, 6);		// 报错：有多个print重载实例与参数列表匹配

	return 0;
}
```

2、 引用不算做重载

```C++
#include <iostream>
using namespace std;

double cube(double x);
double cube(double& x);

int main()
{
	double x = 1.0;
	cout << cube(x) << endl;	// 报错，两个都匹配，编译器无法判断用哪个

	return 0;
}
```

3、const构成重载，非const可以赋值const，反之不行。

```C++
#include <iostream>
using namespace std;

void dribble(char* bits);
void dribble(const char* cbits);
void dabble(char* bits);
void drivel(const char* bits);

int main()
{
	const char p1[20] = "How's the weather";
	char p2[20] = "How's business?";

	dribble(p1);		// dribble(const char* cbits)
	dribble(p2);		// dribble(char* bits)
	dabble(p1);			// no match
	dabble(p2);			// dabble(char* bits)
	drivel(p1);			// drivel(const char* bits)
	drivel(p2);			// drivel(const char* bits)

	return 0;
}
```

dribble()有两个重载，一个带const，一个不带。编译器会在调用的时候根据传参决定使用哪个。drivel只有一个接收const的重载，在传入p2时，可以将非const的参数赋值给const。

注：本条同样适用于引用，即将上述代码中所有的*换成&后，可得到相同的结论。但是对于非引用非指针的普通变量，由于是值传递，不会对原值进行修改，所以const变量可以赋值给非const，const不构成重载，此时如果同时有const和非const函数，编译报错：

```C++
void dribble(char bits);
void dribble(const char cbits);
void dabble(char bits);

int main()
{
	const char p1 = 'a';
	char p2 = 'b';

	dribble(p1);	// 对于非指针非引用变量，const不构成重载，编译报错
	dribble(p2);	// 同上
	dabble(p1);	// dabble(char bits)，值传递，操作的是拷贝数据而非原数据，故可以将const的p1赋值给非const的形参
	dabble(p2);	// dabble(char bits)

	return 0;
}
```

4、函数重载与默认参数冲突时，编译器报错

```C++
#include <iostream>
using namespace std;

void dribble(char* bits);
void dribble(char* bits, int n = 1);

int main()
{
	char p[20] = "Hello World!";
	dribble(p);		// 报错，有多个重载	

	return 0;
}

```

## 8.5 函数模板

函数模板的语法：

```C++
// C++98新增typename
template <typename T>
void swap(T& a, T& b);

// C++98前使用class
template <class T>
void swap(T& a, T& b);
```

注意，函数模板不能缩短可执行程序。对于程序清单8.11，最终仍将由两个独立的函数定义，就像以手工方式定义了这些函数一样。最终的代码不包含任何模板，而只包含了为程序生成的实际函数。使用模板的好处是，它使生成多个函数定义更简单、更可靠。

### 8.5.2 模板的局限性

假设有如下模板函数：

```C++
template <typename T>
void f(T a, T b);
```

通常，代码假定可执行哪些操作。例如，下面的代码假定定义了赋值，但如果T为数组，这种假设将不成立：

```C++
a = b;
```

同样，下面的语句假设定义了>，但如果T为结构，该假设便不成立：

```C++
if (a > b)
```

另外，为数组名定义了运算符>，但由于数组名为地址，因此它比较的是数组的地址，而这可能不是您希望的。

有两种方案可以解决这种问题，一是使用运算符重载，另一种是针对特定的类型使用显示具体化。

### 8.5.3 显示具体化

显示具体化是为了解决8.5.2节的问题引入的，其仍属于模板的范畴，只是制定了某个特定的模板类型，例如：

```C++
template <class T>
void Swap(T& a, T& b);

struct job
{
	char name[40];
	double salary;
	int floor;
};

template<> void Swap<job>(job& j1, job& j2);
void Show(job& j);

int main()
{
	cout.precision(2);
	cout.setf(ios::fixed, ios::floatfield);
	int i = 10, j = 20;
	cout << "i, j = " << i << ", " << j << ".\n";
	Swap(i, j);
	cout << "Now i, j = " << i << ", " << j << ".\n";

	job sue = { "Susan Yaffee", 73000.60, 7 };
	job sidney{ "Sidney Taffee", 78060.72,9 };
	cout << "Before job swapping:\n";
	Show(sue);
	Show(sidney);
	Swap(sue, sidney);
	cout << "After job swapping:\n";
	Show(sue);
	Show(sidney);

	return 0;
}

template <class T>
void Swap(T& a, T& b)
{
	T temp;
	temp = a;
	a = b;
	b = temp;
}

template<> void Swap(job& j1, job& j2)
{
	double t1;
	int t2;
	t1 = j1.salary;
	j1.salary = j2.salary;
	j2.salary = t1;
	t2 = j1.floor;
	j1.floor = j2.floor;
	j2.floor = t2;
}

void Show(job& j)
{
	cout << j.name << ": $" << j.salary
		<< " on floor " << j.floor << endl;
}
```

其中void Swap(T& a, T& b);是普通的函数模板，而template<> void Swap<job>(job& j1, job& j2);是显示具体化。

```C++
// 显示具体化可选的语法，简单来说，template后的<>是必须的
template<> void Swap<job>(job& j1, job& j2);
template<> void Swap<>(job& j1, job& j2);
template<> void Swap(job& j1, job& j2);
```

当有多个重载匹配时（包括普通重载、模板重载），编译器匹配的优先级：

非模板函数 > 显示具体化 > 普通模板

有些地方会把具体化specialization翻译为专用化

### 8.5.4 实例化和具体化

前文说过，模板中的函数并不是都会被编译器生成函数定义，编译器会在【所有的支持的模板】中【按需生成指定的函数定义】。

而具体化就是给模板中的某一种特定类型增加了特殊的重载逻辑，供编译器选择。

而【按需生成指定的函数定义】的过程，被称为实例化。

举个简单的例子：

```C++
template <class T>
void Swap(T& a, T& b);

struct job
{
	char name[40];
	double salary;
	int floor;
};

template<> void Swap<job>(job& j1, job& j2);
int main()
{
	int i = 10, j = 20;
	Swap(i, j);

	return 0;
}
```

上面的例子中，有Swap的普通模板格式，没有指定任何具体的类型，但是我们通过显示具体化的方式，增加了job类型的模板重载。但是在main()函数中，调用Swap()时，传递了两个int类型的值，所以编译器只会生成int类型的函数实例化定义，而虽然我们做了job类型的函数具体化，但因为没有job的函数调用，所以编译器不会生成job的实例化。

上面Swap(i, j)生成实例化的过程属于隐式实例化，如果需要通知编译器进行显示实例化，可以通过如下的语法：

```C++
template Swap<job>(job& j1, job& j2);
```

注意显示实例化和显示具体化语法上的区别主要在于<>的位置。进行显示实例化后，即使不进行Swap(j1, j2)的调用，编译器也会生成job的实例化函数定义。

### 8.5.5 编译器选择使用哪个函数版本

重载解析将寻找最匹配的函数。如果只存在一个这样的函数，则选择它；如果存在多个这样的函数，但其中只有一个是非模板函数，则选择该函数；如果存在多个适合的函数，且它们都为模板函数，但其中有一个函数比其他函数更具体，则选择该函数。如果有多个同样合适的非模板函数或模板函数，但没有一个函数比其他函数更具体，则函数调用将是不确定的，因此是错误的；当然，如果不存在匹配的函数，则也是错误。

如何确定哪个函数更具体？这里举三个例子：

1、指向非const数据的指针和引用优先与非const指针和引用参数匹配。也就是说，在recycle( )示例中，如果只定义了函数#3和#4是完全匹配的，则将选择#3，因为ink没有被声明为const。然而，const和非const之间的区别只适用于指针和引用指向的数据。也就是说，如果只定义了#1和#2，则将出现二义性错误。

```C++
struct blot {int a; char b[10]};
blot ink = {25, "spots"};
recycle(ink);

// 1/2/3/4都是匹配的
void recycle(blot);					// #1
void recycle(const blot);			// #2
void recycle(blot&);				// #3
void recycle(const blot&);			// #4
```

2、一个完全匹配优于另一个的另一种情况是，其中一个是非模板函数，而另一个不是。在这种情况下，非模板函数将优先于模板函数（包括显式具体化）。如果两个完全匹配的函数都是模板函数，则较具体的模板函数优先。例如，这意味着显式具体化将优于使用模板隐式生成的具体化：

即上文提到的非模板函数 > 显示具体化 > 普通模板

3、更具体是指编译器推断使用哪种类型时执行的转换最少。例如，请看下面两个模板：

```C++
template <class Type> void recycle(Type t);
template <class Type> void recycle(Type* t);

struct blot {int a; char b[10]};
blot ink = {25, "spots"};
recycle(&ink);
```

recycle(&ink)调用与#1模板匹配，匹配时将Type解释为blot *。recycle（&ink）函数调用也与#2模板匹配，这次Type被解释为ink。因此将两个隐式实例——recycle<blot *>(blot *)和recycle <blot>(blot *)发送到可行函数池中。

在这两个模板函数中，recycle<blot >(blot *)被认为是更具体的，因为在生成过程中，它需要进行的转换更少。也就是说，#2模板已经显式指出，函数参数是指向Type的指针，因此可以直接用blot标识Type；而#1模板将Type作为函数参数，因此Type必须被解释为指向blot的指针。也就是说，在#2模板中，Type已经被具体化为指针，因此说它“更具体”。

此外，除了交给编译器选择外，还可以自己指定模板类型：

```C++
#include <iostream>
using namespace std;

template <typename T>
T lesser(T a, T b)		// #1
{
	return a < b ? a : b;
}

int lesser(int a, int b)	// #2
{
	a = a < 0 ? -a : a;
	b = b < 0 ? -b : b;
	return a < b ? a : b;
}

int main()
{
	int m = 20;
	int n = -30;
	double x = 15.5;
	double y = 25.9;
	cout << lesser(m, n) << endl;		// #2
	cout << lesser(x, y) << endl;		// #1 with double
	cout << lesser<>(m, n) << endl;		// #1 with int
	cout << lesser<int>(x, y) << endl;	// #1 with int

	return 0;
}
```

### 8.5.6 函数模板的发展

C++11新增关键字decltype，解决函数模板中无法确定类型的情况：

```C++
int x;
decltype(x) y;		// y与x的类型相同

template <typename T1, typename T2>
void ft(T1 x, T2 y)
{
	decltype(x + y) xpy = x + y;
}
```

deltype结合C++11新增对auto的用法，明确返回值：

```C++
auto h(int x, float y) -> double

template <class T1, class T2>
auto gt(T1 x, T2 y) -> decltype(x + y)
{
	...
	return x + y;
}
```

# 第9章 内存模型和名称空间

## 9.1 单独编译

和C语言一样，C++也允许甚至鼓励程序员将组件函数放在独立的文件中。第1章介绍过，可以单独编译这些文件，然后将它们链接成可执行的程序。（通常，C++编译器既编译程序，也管理链接器。）如果只修改了一个文件，则可以只重新编译该文件，然后将它与其他文件的编译版本链接。这使得大程序的管理更便捷。另外，大多数C++环境都提供了其他工具来帮助管理。例如，UNIX和Linux系统都具有make程序，可以跟踪程序依赖的文件以及这些文件的最后修改时间。运行make时，如果它检测到上次编译后修改了源文件，make将记住重新构建程序所需的步骤。

基于上述情况，程序往往需要进行一定的分解，以便不同的功能组织在不同的文件分开编译。常见的分解策略如下：

- 头文件：包含结构声明和使用这些结构的函数的原型。
- 源代码文件：包含与结构有关的函数的代码。
- 源代码文件：包含调用与结构相关的函数的代码。

**如何理解头文件**？

我们知道，预处理器会将#include对应的头文件的内容直接替换原来的内容。对于编译器而言，看到的已经是替换后的内容了。所以，对于如下的依赖关系：

![image-20221218091415284](D:\git\note_md_files\images\image-20221218091415284.png)

file1.cpp和file2.cpp都include了coordin.h，预处理器完成coordin.h的内容替换后，file1.cpp和file2.cpp之间实际上没有什么联系了。那么哪些内容可以放到头文件中呢？

首先，声明类的内容，例如函数原型、结构体声明、类声明、模板声明，都可以放到头文件，这是因为：

1. 通过抽取到头文件，可以保证file1和file2用到的函数、结构体、类等是同一个，避免分别硬编码在file1和file2出现可能的不一致问题。
2. 重复的声明不会导致问题，相反，如果file1和file2都使用到了某函数、某类、某结构体，那么在该函数中进行相应的声明是必要的，否则会导致编译报错。

其次，对于常量/变量，#define或const定义的符号常量可以放到头文件中。一般的变量不可以放在头文件。

最后，对于函数定义，内联函数可以放到头文件中，因为对于编译器来讲，其实被内联到可执行文件的，不会产生函数重定义。相对应的，普通的函数定义是不能放在头文件。

**为什么可以重复声明，但是不可以重复定义呢？**

回答这个问题，就要看下编译器链接的时候是怎么做的了。例如file1、file2以及头文件的内容如下：

```C++
// coordin.h
int x = 1;
static int y = 2;
const int z = 3;
void show(){};
```

```C++
// file1.cpp
#include "coordin.h"
int main(){}
```

```C++
// file2.cpp
#include "coordin.h"
void test(){};
```

我们知道，#include是做了替换，那么替换后的file1.cpp和file2.cpp如下：

```C++
// file1.cpp
int x = 1;
static int y = 2;
const int z = 3;
int main(){}
void show(){};
```

```C++
// file2.cpp
int x = 1;
static int y = 2;
const int z = 3;
void test(){};
void show(){};
```

这里就用到了一些9.2节要学的【链接性】的知识了，根据9.2节可知，x是文件间可见的，y是单文件可见的，const修饰的常量也是内部链接的。所以x在file1.cpp和file2.cpp之间形成了重定义，y和z没有。此外，函数也是文件间可见的，同样也构成了重定义。

**再看下#ifndef**

如果只是上面举例中的file1.cpp/file2.cpp/coordin.h的包含关系，是不需要#ifndef的，#ifndef是为了解决下图中的问题：

![image-20221218102152537](D:\git\note_md_files\images\image-20221218102152537.png)

在上图中，file1.cpp同时包含了library.h和coordin.h，而library.h又包含了coordin.h，假设三个文件的内容如下：

```C++
// coordin.h
struct polar
{
	double distance;
	double angle;
}
```

```C++
// library.h
#include "coordin.h"
void show_polar(const polar& dapos);
```

```C++
// file1.cpp
#include "coordin.h"
#include "library.h"
int main(){}
```

在完成include替换后，file1.cpp变为：

```C++
struct polar
{
	double distance;
	double angle;
}
struct polar
{
	double distance;
	double angle;
}
void show_polar(const polar& dapos);
int main(){}
```

可以看到，polar重复声明。如果引入#ifndef呢，三个文件是这样的

```C++
// coordin.h
#ifndef COORDIN_H_
#define COORDIN_H_
struct polar
{
	double distance;
	double angle;
}
#endif
```

```C++
// library.h
#include "coordin.h"
#ifndef LIBRARY_
#define LIBRARY_
void show_polar(const polar& dapos);
#endif
```

```C++
// file1.cpp
#include "coordin.h"
#include "library.h"
int main(){}
```

此时再看下file1.cpp的预处理过程，首先全部替换：

```C++
// file1.cpp 替换后
#ifndef COORDIN_H_
#define COORDIN_H_
struct polar
{
	double distance;
	double angle;
}
#endif

#ifndef COORDIN_H_
#define COORDIN_H_
struct polar
{
	double distance;
	double angle;
}
#endif

#ifndef LIBRARY_
#define LIBRARY_
void show_polar(const polar& dapos);
#endif

int main(){}
```

第二次走到#ifndef COORDIN_H_时，由于COORDIN_H已经定义，所以不会重复定义，最终预处理完成后的代码如下：

```C++
// file1.cpp 替换后
struct polar
{
	double distance;
	double angle;
}
void show_polar(const polar& dapos);
int main(){}
```

注意，在包含头文件时，我们使用“coordin.h”，而不是<coodin.h>。如果文件名包含在尖括号中，则C++编译器将在存储标准头文件的主机系统的文件系统中查找；但如果、文件名包含在双引号中，则编译器将首先查找当前的工作目录或源代码目录（或其他目录，这取决于编译器）。如果没有在那里找到头文件，则将在标准位置查找。因此在包含自己的头文件时，应使用引号而不是尖括号。

## 9.2 存储持续性、作用域和链接性

存储持续性（duration）

1、自动存储持续性：在函数定义中声明的变量（包括函数参数）的存储持续性为自动的。它们在程序开始执行其所属的函数或代码块时被创建，在执行完函数或代码块时，它们使用的内存被释放。C++有两种存储持续性为自动的变量。
2、 静态存储持续性：在函数定义外定义的变量和使用关键字static定义的变量的存储持续性都为静态。它们在程序整个运行过程中都存在。C++有3种存储持续性为静态的变量。
3、 线程存储持续性（C++11）：当前，多核处理器很常见，这些CPU可同时处理多个执行任务。这让程序能够将计算放在可并行处理的不同线程中。如果变量是使用关键字thread_local声明的，则其生命周期与所属的线程一样长。本书不探讨并行编程。
4、 动态存储持续性：用new运算符分配的内存将一直存在，直到使用delete运算符将其释放或程序结束为止。这种内存的存储持续性为动态，有时被称为自由存储（free store）或堆（heap）。

作用域（scope）描述了名称在文件（翻译单元）的多大范围内可见。例如，函数中定义的变量可在该函数中使用，但不能在其他函数中使用；而在文件中的函数定义之前定义的变量则可在所有函数中使用。

链接性（linkage）描述了名称如何在不同单元间共享。链接性为外部的名称可在文件间共享，链接性为内部的名称只能由一个文件中的函数共享。

### 9.2.3 静态持续变量

和C语言一样，C++也为静态存储持续性变量提供了3种链接性：外部链接性（可在其他文件中访问）、内部链接性（只能在当前文件中访问）和无链接性（只能在当前函数或代码块中访问）。这3种链接性都在整个程序执行期间存在，与自动变量相比，它们的寿命更长。由于静态变量的数目在程序运行期间是不变的，因此程序不需要使用特殊的装置（如栈）来管理它们。编译器将分配固定的内存块来存储所有的静态变量，这些变量在整个程序执行期间一直存在。另外，如果没有显式地初始化静态变量，编译器将把它设置为0。在默认情况下，静态数组和结构将每个元素或成员的所有位都设置为0。

下面介绍如何创建这3种静态持续变量，然后介绍它们的特点。要想创建链接性为外部的静态持续变量，必须在代码块的外面声明它；要创建链接性为内部的静态持续变量，必须在代码块的外面声明它，并使用static限定符；要创建没有链接性的静态持续变量，必须在代码块内声明它，并使用static限定符。下面的代码片段说明这3种变量：

```C++
int global = 1000;		// static duration, external linkage.
static int one_file = 50;	// static duration, internal linkage.
int main()
{
	...
}
void func1(int n)
{
	static int count = 0;	// static duration, no linkage.
	int llama = 0;
}
void func2(int q)
{
    ...
}
```

正如前面指出的，所有静态持续变量（上述示例中的global、one_file和count）在整个程序执行期间都存在。在funct1( )中声明的变量count的作用域为局部，没有链接性，这意味着只能在funct1( )函数中使用它，就像自动变量llama一样。然而，与llama不同的是，即使在funct1( )函数没有被执行时，count也留在内存中。global和one_file的作用域都为整个文件，即在从声明位置到文件结尾的范围内都可以被使用。具体地说，可以在main( )、funct1( )和funct2( )中使用它们。由于one_file的链接性为内部，因此只能在包含上述代码的文件中使用它；由于global的链接性为外部，因此可以在程序的其他文件中使用它。

### 9.2.4 静态持续性、外部链接性

一方面，在每个使用外部变量的文件中，都必须声明它；另一方面，C++有“单定义规则”（One Definition Rule，ODR），该规则指出，变量只能有一次定义。为满足这种需求，C++提供了两种变量声明。一种是定义声明（defining declaration）或简称为定义（definition），它给变量分配存储空间；另一种是引用声明（referencing declaration）或简称为声明（declaration），它不给变量分配存储空间，因为它引用已有的变量。

引用声明使用关键字extern，且不进行初始化；否则，声明为定义，导致分配存储空间：、

```C++
double up;		// definition, up is 0
extern int blem;	// blem defined elsewhere
extern char gr = 'z';	// definition because initialized
```

如果要在多个文件中使用外部变量，只需在一个文件中包含该变量的定义（单定义规则），但在使用该变量的其他所有文件中，都必须使用关键字extern声明它：

```C++
// file01.cpp
extern int cats = 20;	// definition because of initialization
int dogs = 22;	// alse a definition
int fleas;	// also a definition
...

// file02.cpp
// use cats and dogs from file01.cpp
extern int cats;	// not definitions because they use
extern int dogs;	// extern and have no initialization
...

// file98.cpp
// use cats, dog, and fleas from file01.cpp
extern int cats;
extern int dogs;
extern int fleas;
...
```

C++提供了作用域解析运算符（::）。放在变量名前面时，该运算符表示使用变量的全局版，例如：

```C++
// file01.cpp
#include <iostream>
using namespace std;
double warming = 0.3;
void update(double dt);
void local();

int main()
{
	cout << "Global warming is " << warming << " degrees.\n";
	update(0.1);
	cout << "Global warming is " << warming << " degrees.\n";
	local();
	cout << "Global warming is " << warming << " degrees.\n";

	return 0;
}

// file02.cpp
#include <iostream>
extern double warming;

void update(double dt);
void local();

using std::cout;
void update(double dt)
{
	extern double warming;
	warming += dt;
	cout << "Updating global warming to " << warming;
	cout << " degrees.\n";
}

void local()
{
	double warming = 0.8;
	cout << "Local warming = " << warming << " degrees.\n";
	cout << "But global warming = " << ::warming;
	cout << " degrees.\n";
}
```

打印：

```
Global warming is 0.3 degrees.
Updating global warming to 0.4 degrees.
Global warming is 0.4 degrees.
Local warming = 0.8 degrees.
But global warming = 0.4 degrees.
Global warming is 0.4 degrees.
```

这里提一下extern修饰函数，函数比较特殊，例如上面的例子中，两个文件都声明了local()函数，所以如下两种写法等效：

```C++
extern void local();
void local();
```

如果想突出函数定义在其他文件，可以加上extern。

### 9.2.5 静态持续性、内部链接性

将static限定符用于作用域为整个文件的变量时，该变量的链接性将为内部的。

如果要在其他文件中使用相同的名称来表示其他变量，该如何办呢？只需省略关键字extern即可吗？

```C++
// file01
int errors = 20;	// external declaration
...
// file02
int errors = 5;	// ??known to file2 only??
void froobish()
{
	cout << errors;		// fails
	...
}
```

这种做法将失败，因为它违反了单定义规则。file2中的定义试图创建一个外部变量，因此程序将包含errors的两个定义，这是错误。

但如果文件定义了一个静态外部变量，其名称与另一个文件中声明的常规外部变量相同，则在该文件中，静态变量将隐藏常规外部变量：

```c++
// file01
int errors = 20;	// external declaration
...
// file02
static int errors = 5;	// known to file2 only
void froobish()
{
	cout << errors;	// use errors defined in file2
	...
}
```

这没有违反单定义规则，因为关键字static指出标识符errors的链接性为内部，因此并非要提供外部定义。

看个例子：

```C++
// file01.cpp
#include <iostream>
int tom = 3;
int dick = 30;
static int harry = 300;
void remote_access();

int main()
{
	using namespace std;
	cout << "main() reports the following addresses:\n";
	cout << &tom << " = &tom, " << &dick << " = &dick, ";
	cout << &harry << " = &harry\n";
	remote_access();

	return 0;
}

// file02.cpp
#include <iostream>
extern int tom;
static int dick = 10;
int harry = 200;

void remote_access()
{
	using namespace std;
	cout << "remote_access() reports the following addresses:\n";
	cout << &tom << " = &tom, " << &dick << " = &dick, ";
	cout << &harry << " = &harry\n";
}
```

打印结果：

```
main() reports the following addresses:
00007FF6CDB9D000 = &tom, 00007FF6CDB9D004 = &dick, 00007FF6CDB9D008 = &harry
remote_access() reports the following addresses:
00007FF6CDB9D000 = &tom, 00007FF6CDB9D014 = &dick, 00007FF6CDB9D010 = &harry
```

### 9.2.6 静态持续性、无链接性

无链接性的局部变量是这样创建的，将static限定符用于在代码块中定义的变量。在代码块中使用static时，将导致局部变量的存储持续性为静态的。这意味着虽然该变量只在该代码块中可用，但它在该代码块不处于活动状态时仍然存在。因此在两次函数调用之间，静态局部变量的值将保持不变。（静态变量适用于再生——可以用它们将瑞士银行的秘密账号传递到下一个要去的地方）。另外，如果初始化了静态局部变量，则程序只在启动时进行一次初始化。以后再调用函数时，将不会像自动变量那样再次被初始化。

### 9.2.7 说明符和限定符

有些被称为存储说明符（storage class specifier）或cv-限定符（cv-qualifier）的C++关键字提供了其他有关存储的信息。下面是存储说明符：

- auto（在C++11中不再是说明符）
- register；
- static；
- extern；
- thread_local（C++11新增的）
- mutable

其中的大部分已经介绍过了，在同一个声明中不能使用多个说明符，但thread_local除外，它可与static或extern结合使用。前面讲过，在C++11之前，可以在声明中使用关键字auto指出变量为自动量；但在C++11中，auto用于自动类型推断。关键字register用于在声明中指示寄存器存储，而在C++11中，它只是显式地指出变量是自动的。关键字static被用在作用域为整个文件的声明中时，表示内部链接性；被用于局部声明中，表示局部变量的存储持续性为静态的。关键字extern表明是引用声明，即声明引用在其他地方定义的变量。关键字thread_local指出变量的持续性与其所属线程的持续性相同。thread_local变量之于线程，犹如常规静态变量之于整个程序。关键字mutable的含义将根据const来解释，因此先来介绍cv-限定符，然后再解释它。

下面就是cv限定符：

1. const
2. volatile

关键字volatile表明，即使程序代码没有对内存单元进行修改，值也可能发生变化。听起来似乎很神秘，实际上并非如此。例如，可将一个指针指向某个硬件位置，其中包含了来自串行端口的时间或信息。在这种情况下，硬件（而不是程序）可能修改其中的内容。或者两个程序可能互相影响，共享数据。该关键字的作用是为了改善编译器的优化能力。例如，假设编译器发现，程序在几条语句中两次使用了某个变量的值，则编译器可能不是让程序查找这个值两次，而是将这个值缓存到寄存器中。这种优化假设变量的值在这两次使用之间不会变化。如果不将变量声明为volatile，则编译器将进行这种优化；将变量声明为volatile，相当于告诉编译器，不要进行这种优化。

现在回到mutable。可以用它来指出，即使结构（或类）变量为const，其某个成员也可以被修改。例如，请看下面的代码：

```C++
struct data
{
	char name[30];
	mutable int accesses;
}

const data veep = {"Claybourne Clodde", 0, ...};
strcpy(veep.name, "Joye Joux");		// not allowed
veep.accesses++;		// allowed
```

在C++（但不是在C语言）中，const限定符对默认存储类型稍有影响。在默认情况下全局变量的链接性为外部的，但const全局变量的链接性为内部的。也就是说，在C++看来，全局const定义（如下述代码段所示）就像使用了static说明符一样。

```C++
const int fingers = 10;		// same as static const int fingers = 10;
int main(void)
{
	...
}
```

C++修改了常量类型的规则，让程序员更轻松。例如，假设将一组常量放在头文件中，并在同一个程序的多个文件中使用该头文件。那么，预处理器将头文件的内容包含到每个源文件中后，所有的源文件都将包含类似下面这样的定义：

```C++
const int fingers = 10;
```

侧面说明了const不具有外部链接性。但如果出于某种原因，程序员希望某个常量的链接性为外部的，则可
以使用extern关键字来覆盖默认的内部链接性：

```C++
extern const int states = 50;	// definition with external linkage
```

### 9.2.8 函数和链接性

和变量一样，函数也有链接性，虽然可选择的范围比变量小。和C语言一样，C++不允许在一个函数中定义另外一个函数，因此所有函数的存储持续性都自动为静态的，即在整个程序执行期间都一直存在。在默认情况下，函数的链接性为外部的，即可以在文件间共享。实际上，可以在函数原型中使用关键字extern来指出函数是在另一个文件中定义的，不过这是可选的（要让程序在另一个文件中查找函数，该文件必须作为程序的组成部分被编译，或者是由链接程序搜索的库文件）。还可以使用关键字static将函数的链接性设置为内部的，使之只能在一个文件中使用。必须同时在原型和函数定义中使用该关键字：

```C++
static int private(double x);
static int private(double x)
{
	...
}
```

这意味着该函数只在这个文件中可见，还意味着可以在其他文件中定义同名的的函数。和变量一样，在定义静态函数的文件中，静态函数将覆盖外部定义，因此即使在外部定义了同名的函数，该文件仍将使用静态函数。

单定义规则也适用于非内联函数，因此对于每个非内联函数，程序只能包含一个定义。对于链接性为外部的函数来说，这意味着在多文件程序中，只能有一个文件（该文件可能是库文件，而不是您提供的）包含该函数的定义，但使用该函数的每个文件都应包含其函数原型。

内联函数的链接性是内部的，所以内联函数的定义必须和使用函数放在同一个文件中。一般情况下，建议内联函数放在头文件中，这样使用函数的文件通过include头文件即可使用内联函数。如下情况编译不过：

```c++
// test.h
class Test
{
public:
	void test();
};

// test.cpp
#include <iostream>
#include "15.1 test.h"

inline void Test::test()
{
	std::cout << "test" << std::endl;
}

// main.cpp
#include <iostream>
#include "15.1 test.h"

int main()
{
	Test test;
    // 使用的内联函数定义不在本文件中
	test.test();

	return 0;
}
```

### 9.2.9 语言链接性

在C语言中，一个名称只对应一个函数，因此这很容易实现。为满足内部需要，C语言编译器可能将spiff这样的函数名翻译为spiff。这种方法被称为C语言链接性（C language linkage）。但在C++中，同一个名称可能对应多个函数，必须将这些函数翻译为不同的符号名称。因此，C++编译器执行名称矫正或名称修饰（参见第8章），为重载函数生成不同的符号名称。例如，可能将spiff（int）转换为spiff_i，而将spiff（double，double）转换为_spiff_d_d。这种方法被称为C++语言链接（C++ language linkage）。

链接程序寻找与C++函数调用匹配的函数时，使用的方法与C语言不同。但如果要在C++程序中使用C库中预编译的函数，将出现什么情况呢？例如，假设有下面的代码：

```C++
spiff(22);
```

它在C库文件中的符号名称为spiff，但对于我们假设的链接程序来说，C++查询约定是查找符号名称spiff_i。为解决这种问题，可以用函数原型来指出要使用的约定：

```C++
extern "C" void spiff(int);		// C
extern void spoff(int);			// C++
extern "C++" spoff(int);		// C++
```

### 9.2.10 存储方案和动态分配

new对应了动态内存分配，其生命周期是程序员手动控制的。虽然存储方案概念不适用于动态内存，但适用于用来跟踪动态内存的自动和静态指针变量。例如，假设在一个函数中包含下面的语句：

```C++
float* p_fees = new float[20];
```

由new分配的80个字节（假设float为4个字节）的内存将一直保留在内存中，直到使用delete运算符将其释放。但当包含该声明的语句块执行完毕时，p_fees指针将消失。如果希望另一个函数能够使用这80字节中的内容，则必须将其地址传递或返回给该函数。另一方面，如果将p_fees的链接性声明为外部的，则文件中位于该声明后面的所有函数都可以使用它。另外，通过在另一个文件中使用下述声明，便可在其中使用该指针：

```C++
extern float* p_fees;
```

如果要为内置的标量类型（如int或double）分配存储空间并初始化，可在类型名后面加上初始值，并将其用括号括起：

```C++
int* pi = new int(6);
double* pd = new double(99.99);
```

这种括号语法也可用于有合适构造函数的类，这将在本书后面介绍。
然而，要初始化常规结构或数组，需要使用大括号的列表初始化，这要求编译器支持C++11。C++11允许您这样做：

```c++
struct where {double x, double y, double z};
where* one = new where{2.5, 5.3, 7.2};
int* ar = new int[4]{2, 4, 6, 7};
```

C++11中，还可将列表初始化用于单值变量：

```C++
int* p1 = new int {6};
double* pdo = new double{99.99};
```

运算符new和new []分别调用如下函数：

```C++
void* operator new(std::size_t);		// used by new
void* operator new[](std::size_t);		// used by new[]
```

这些函数被称为分配函数（alloction function），它们位于全局名称空间中。同样，也有由delete和delete []调用的释放函数（deallocation function）

```C++
void operator delete(void*);
void operator delete[](void*);
```

它们使用第11章将讨论的运算符重载语法。std::size_t是一个typedef，对应于合适的整型。对于下面这样的基本语句：

```C++
int* pi = new int;
```

将被转换为下面这样：

```c++
int* pi = new (sizeof(int));
```

而下面的语句：

```C++
int* pa = new int[40];
```

将被转换为下面这样：

```C++
int* pa = new(40 * sizeof(int));
```

最后介绍下定位new运算符：

new运算符还有另一种变体，被称为定位（placement）new运算符，它让您能够指定要使用的位置。程序员可能使用这种特性来设置内存管理规程、处理需要通过特定地址进行访问的硬件或在特定位置创建对象。

要使用定位new特性，首先需要包含头文件new，它提供了这种版本的new运算符的原型；然后将new运算符用于提供了所需地址的参数。除需要指定参数外，句法与常规new运算符相同。具体地说，使用定位new运算符时，变量后面可以有方括号，也可以没有。下面的代码段演示了new运算符的4种用法：

```C++
#include <new>
struct chaff
{
	char dross[20];
	int slag;
};
char buffer1[50];
char buffer2[50];
int main()
{
	chaff* p1, *p2;
	int *p3, *p4;
	p1 = new chaff;
	p3 = new int[20];
	
	p2 = new (buffer1) chaff;
	p4 = new (buffer2) int[20];

	... 
	
	delete p1;
	delete[] p3;
}
```

p2和p4会占用bufffer1和buffer2的内存位置。注意到程序退出时只针对常规new运算符创建的p1和p3进行了delete，原因在于buffer1和buffer2是静态内存，delete无权操作。

## 9.3 名称空间

### 9.3.1 传统C++名称空间

介绍C++中新增的名称空间特性之前，先复习一下C++中已有的名称空间属性，并介绍一些术语，让读者熟悉名称空间的概念。

第一个需要知道的术语是声明区域（declaration region）。声明区域是可以在其中进行声明的区域。例如，可以在函数外面声明全局变量，对于这种变量，其声明区域为其声明所在的文件。对于在函数中声明的变量，其声明区域为其声明所在的代码块。

第二个需要知道的术语是潜在作用域（potential scope）。变量的潜在作用域从声明点开始，到其声明区域的结尾。因此潜在作用域比声明区域小，这是由于变量必须定义后才能使用。

然而，变量并非在其潜在作用域内的任何位置都是可见的。例如，它可能被另一个在嵌套声明区域中声明的同名变量隐藏。例如，在函数中声明的局部变量（对于这种变量，声明区域为整个函数）将隐藏在同一个文件中声明的全局变量（对于这种变量，声明区域为整个文件）。变量对程序而言可见的范围被称为作用域（scope），前面正是以这种方式使用该术语的。图9.5和图9.6对术语声明区域、潜在作用域和作用域进行了说明。

![image-20221231093533397](D:\git\note_md_files\images\image-20221231093533397.png)

![image-20221231093543144](D:\git\note_md_files\images\image-20221231093543144.png)

### 9.3.2 新的名称空间特性

C++新增了这样一种功能，即通过定义一种新的声明区域来创建命名的名称空间，这样做的目的之一是提供一个声明名称的区域。一个名称空间中的名称不会与另外一个名称空间的相同名称发生冲突，同时允许程序的其他部分使用该名称空间中声明的东西。例如，下面的代码使用新的关键字namespace创建了两个名称空间：Jack和Jill。

```C++
namespace Jack {
	double pail;
	void fetch();
	int pal;
	struct Well {...}
}

namespace Jill {
	double bucket(double n) {...}
	double fetch;
	int pal;
	struct Hill {...}
}
```

名称空间可以是全局的，也可以位于另一个名称空间中，但不能位于代码块中。因此，在默认情况下，在名称空间中声明的名称的链接性为外部的（除非它引用了常量）。

除了用户定义的名称空间外，还存在另一个名称空间——全局名称空间（global namespace）。它对应于文件级声明区域，因此前面所说的全局变量现在被描述为位于全局名称空间中。

名称空间是开放的（open），即可以把名称加入到已有的名称空间中。例如，下面这条语句将名称goose添加到Jill中已有的名称列表中：

```C++
namespace Jill {
	char* goose(const char*);
}
```

同样，原来的Jack名称空间为fetch( )函数提供了原型。可以在该文件后面（或另外一个文件中）再次使用Jack名称空间来提供该函数的代码：

```c++
namespace Jack {
	void fetch()
	{
		...
	}
}
```

当然，需要有一种方法来访问给定名称空间中的名称。最简单的方法是，通过作用域解析运算符::，使用名称空间来限定该名称：

```c++
Jack::pail = 12.34;
Jill::Hill mole;
Jack::fetch();
```

未被装饰的名称（如pail）称为未限定的名称（unqualified name）；包含名称空间的名称（如Jack::pail）称为限定的名称（qualified name）。

#### **using声明和using编译指令**

我们并不希望每次使用名称时都对它进行限定，因此C++提供了两种机制（using声明和using编译指令）来简化对名称空间中名称的使用。using声明使特定的标识符可用，using编译指令使整个名称空间可用。
using声明由被限定的名称和它前面的关键字using组成：

```C++
using Jill::fetch;
```

using声明将特定的名称添加到它所属的声明区域中。例如main( )中的using声明Jill::fetch将fetch添加到main( )定义的声明区域中。完成该声明后，便可以使用名称fetch代替Jill::fetch。下面的代码段说明了这几点：

```C++
namespace Jill {
	double bucket(double n) {...}
	double fetch;
	struct Hill {...};
}
char fetch;
int main()
{
	using Jill::fetch;		// using声明
    // Error，already have a local fetch
    // 如果前面是using编译指令，则不会报错，会将名称空间的fetch隐藏
	double fetch;
	cin >> fetch;			// read a value into Jill::fetch
	cin >> ::fetch;			// read a value into global fetch
}
```

由于using声明将名称添加到局部声明区域中，因此这个示例避免了将另一个局部变量也命名为fetch。另外，和其他局部变量一样，fetch也将覆盖同名的全局变量。
在函数的外面使用using声明时，将把名称添加到全局名称空间中：

```C++
void other();
namespace Jill {
	double bucket(double n) {...}
	double fetch;
	struct Hill {...};
}
using Jill::fetch;
int main()
{
	cin >> fetch;
}
void other()
{
	cout << fetch;
}
```

using声明使一个名称可用，而using编译指令使所有的名称都可用。using编译指令由名称空间名和它前面的关键字using namespace组成，它使名称空间中的所有名称都可用，而不需要使用作用域解析运算符：

```C++
using namespace Jack;
```

一般说来，使用using声明比使用using编译指令更安全，这是由于它只导入指定的名称。如果该名称与局部名称发生冲突，编译器将发出指示。using编译指令导入所有名称，包括可能并不需要的名称。如果与局部名称发生冲突，则局部名称将覆盖名称空间版本，而编译器并不会发出警告。另外，名称空间的开放性意味着名称空间的名称可能分散在多个地方，这使得难以准确知道添加了哪些名称。

#### **名称空间的其他特性**

可以将名称空间声明进行嵌套：

```C++
namespace elements
{
	namespace fire
	{
		int flame;
		...
	}
	float water;
}
```

这里，flame指的是element[::]fire::flame。同样，可以使用下面的using编译指令使内部的名称可用：

```c++
using namespace elements::fire;
```

另外，也可以在名称空间中使用using编译指令和using声明，如下所示：

```C++
namespace myth
{
	using Jill::fetch;
	using namespace elements;
	using std::cout;
	using std::cin;
}
```

假设要访问Jill::fetch。由于Jill::fetch现在位于名称空间myth（在这里，它被叫做fetch）中，因此可以这样访问它：

```C++
std::cin >> myth::fetch;
```

当然，由于它也位于Jill名称空间中，因此仍然可以称作Jill::fetch：

```c++
Jill::fetch;
std::cout << Jill::fetch;
```

现在考虑将using编译指令用于myth名称空间的情况。using编译指令是可传递的。如果A op B且B op C，则A op C，则说操作op是可传递的。例如，>运算符是可传递的（也就是说，如果A>B且B>C，则A>C）。在这个情况下，下面的语句将导入名称空间myth和elements：

```c++
using namespace myth;
```

这条编译指令与下面两条编译指令等价：

```c++
using namespace myth;
using namespace elements;
```

可以给名称空间创建别名。例如，假设有下面的名称空间：

```C++
namespace my_very_favorite_things {...}
```

则可以使用下面的语句让mvft成为my_very_favorite_things的别名：

```c++
namespace mvft = my_very_favorite_things;
```

可以使用这种技术来简化对嵌套名称空间的使用：

```c++
namespace MEF = myth::elements::fire;
using MEF::flame;
```

#### 未命名的名称空间

可以通过省略名称空间的名称来创建未命名的名称空间：

```C++
namespace
{
	int ice;
	int bandycoot;
}
```

提供了链接性为内部的静态变量的替代品，例如，假设有这样的代码：

```c++
static int counts;		// static storage, internal linkage.
int other();
int main()
{
	...
}
int other()
{
	...
}
```

采用名称空间的方法如下：

```C++
namespace
{
	int counts;		// static storage, internal linkage.
}
int other();
int main()
{
	...
}
int other()
{
	...
}
```

# 第10章 对象和类

## 10.2 抽象和类

### 10.2.2 C++中的类

```C++
class Stock
{
private:
	std::string company;
	long shares;
	double share_val;
	double total_val;
	void set_tot() { total_val = shares * share_val; }
public:
	Stock();
	Stock(const std::string& co, long n = 0, double pr = 0.0);
	~Stock();
	void buy(long num, double price);
	void sell(long num, double price);
	void update(double price);
	void show() const;
	const Stock& topval(const Stock& s) const;
};
```

其中，private可以省略，因其是默认的访问控制。为了强调，本书都加上了private。结构体中的成员默认都是public的，与类不同。

### 10.2.3 实现类成员函数

成员函数定义与常规函数定义非常相似，它们有函数头和函数体，也可以有返回类型和参数。但是它们还有两个特殊的特
征：

- 定义成员函数时，使用作用域解析运算符（::）来标识函数所属的类；
- 类方法可以访问类的private组件。

其定义位于类声明中的函数都将自动成为内联函数，因此Stock::set_tot( )是一个内联函数。类声明常将短小的成员函数作为内联函数，set_tot( )符合这样的要求。如果愿意，也可以在类声明之外定义成员函数，并使其成为内联函数。为此，只需在类实现部分中定义函数时使用inline限定符即可：

```c++
inline void Stock::set_tot()
{
	...
}
```

## 10.3 类的构造函数和析构函数

### 10.3.2 使用构造函数

C++提供了两种使用构造函数来初始化对象的方式。第一种方式是显式地调用构造函数：

```C++
Stock food = Stock("World Cabbage", 250, 1.25);
```

这将food对象的company成员设置为字符串“World Cabbage”，将shares成员设置为250，依此类推。

另一种方式是隐式地调用构造函数：

```c++
Stock garment("Furry Mason", 50, 2.5);
```

这种格式更紧凑，它与下面的显式调用等价：

```c++
Stock garment = Stock("Furry Mason", 50, 2.5);
```

每次创建类对象（甚至使用new动态分配内存）时，C++都使用类构造函数。下面是将构造函数与new一起使用的方法：

```c++
Stock* pstock = new Stock("Electroshock Games", 18, 19.0);
```

这条语句创建一个Stock对象，将其初始化为参数提供的值，并将该对象的地址赋给pstock指针。在这种情况下，对象没有名称，但可以使用指针来管理该对象。

此外，在C++11中，可将列表初始化语法用于类，只要提供与某个构造函数的参数列表匹配的内容，并用大括号将它们括起：

```c++
Stock hot_tip = {"Derivatives Plus Plus", 100, 45.0};
Stock jock {"Sport Age Storage, Inc"};
Stock temp{};
```

在前两个声明中，用大括号括起的列表与下面的构造函数匹配：

```C++
Stock::Stock(const std::string& co, long n = 0, double pr = 0.0);
```

因此，将使用该构造函数来创建这两个对象。创建对象jock时，第二和第三个参数将为默认值0和0.0。第三个声明与默认构造函数匹配，因此将使用该构造函数创建对象temp。

### 10.3.3 默认构造函数

当且仅当没有定义任何构造函数时，编译器才会提供默认构造函数。为类定义了构造函数后，程序员就必须为它提供默认构造函数。如果提供了非默认构造函数（如Stock(const char * co, int n, double pr)），但没有提供默认构造函数，则下面的声明将出错：

```c++
Stock stock1;	// not possible with current constructor
```

这样做的原因可能是想禁止创建未初始化的对象。然而，如果要创建对象，而不显式地初始化，则必须定义一个不接受任何参数的默认构造函数。定义默认构造函数的方式有两种。一种是给已有构造函数的所有参数提供默认值：

```C++
Stock(const string& co = "Error", int n = 0, double pr = 0.0);
```

另一种方式是通过函数重载来定义另一个构造函数——一个没有参数的构造函数：

```c++
Stock();
```

由于只能有一个默认构造函数，因此不要同时采用这两种方式，否则编译器报错：

```C++
Stock stock1;	// error, 类"Stock"包含多个默认构造函数
```

注意，不要使用如下方法调用默认构造函数，编译器会认为是声明了一个名为stock2的函数：

```c++
	Stock stock2();		// 声明一个名为stock2，返回值类型为Stock的函数
```

```C++
	// 下面这些都是合法的
	Stock stock1 = Stock();
	Stock stock2;
	Stock* pstock3 = new Stock();
	Stock stock4 = {};
	Stock stock5{};
```

如果没有给某个成员初始化赋值，那么其值是不会被默认赋值为0的，例如：

```C++
Person::Person(){}

int main()
{
	Person p1;		// p1的int成员不会被赋值默认值0
}
```

### 10.3.5 改进Stock类

看一个例子：

```C++
#include <iostream>
#include "10.4 stock10.h"

int main()
{
	{
		using std::cout;
		cout << "Using constructors to create new objects\n";
		Stock stock1("NanoSmart", 12, 20.0);
		stock1.show();
		Stock stock2 = Stock("Boffo Objects", 2, 2.0);
		stock2.show();

		cout << "Assigning stock1 to stock2:\n";
		stock2 = stock1;
		cout << "Listing stock1 and stock2:\n";
		stock1.show();
		stock2.show();

		cout << "Using a constructor to reset an object\n";
		stock1 = Stock("Nifty Foods", 10, 50.0);
		cout << "Revised stock1:\n";
		stock1.show();
		cout << "Done\n";
	}

	return 0;
}
```

首先看11行，这种写法在部分编译器下，会创建一个临时的Stock对象，然后赋值给stock2对象，完成赋值后再析构。

其次看第15行，这里要知道C++中对象的赋值是一个深拷贝，每个成员都会复制到目标对象，所以stock1和stock2在赋值后地址是不同的。

再看下第21行，stock1对象已经存在，因此这条语句不是对stock1进行初始化，而是将新值赋给它。这是通过让构造程序创建一个新的、临时的对象，然后将其内容复制给stock1来实现的。随后程序调用析构函数，以删除该临时对象。

综上，最好采用Stock stock1("NanoSmart", 12, 20.0);的方式初始化，效率最高。

最后，在25行代码块结束时，stock1和stock2对象要析构，这里注意先创建的后析构，由于stock1先创建，所以后析构。

**const成员函数**

请看下面的代码片段：

```c++
const Stock land = Stock("Kludgehorn Properties");
land.show();
```

对于当前的C++来说，编译器将拒绝第二行。这是什么原因呢？因为show( )的代码无法确保调用对象不被修改——调用对象和const一样，不应被修改。我们以前通过将函数参数声明为const引用或指向const的指针来解决这种问题。但这里存在语法问题：show( )方法没有任何参数。相反，它所使用的对象是由方法调用隐式地提供的。需要一种新的语法——保证函数不会修改调用对象。C++的解决方法是将const关键字放在函数的括号后面。也就是说，show( )声明应像这样：

```c++
void show() const;
```

同样，函数定义的开头应像这样：

```c++
void Stock::show() const;
```

这里稍微补充一下，后面讲到运算符重载时，提到可以使用友元函数，例如：

```c++
// 解决Complex * double
Complex Complex::operator*(double n)
{
	...
}

// 解决double * Complex
Complex operator*(double n, const Complex& complex)
{
	return complex * n;
}
```

上面的代码第10行也是编译失败的，原因在于complex * n相当于调用了complex.operator*(n)，而该方法没有被声明为const。需要如下声明方可通过编译（或者去掉第8行的const也可以）：

```c++
Complex Complex::operator*(double n) const
```

## 10.5 对象数组

声明对象数组时，如未显示初始化类对象，也会调用默认构造函数：

```c++
Stock mystuff[4];		// 调用默认构造函数创建四个对象
```

如果类包含多个构造函数，则可以对不同的元素使用不同的构造函数：

```C++
const int STKS = 10;
Stock stocks[STKS] = {
	Stock("NanoSmart", 12.5, 20),
	Stock(),
	Stock("Monolithic Obelisks", 130, 3.25),
};
```

上述代码使用Stock(const string & co, long n, double pr)初始化stock[0]和stock[2]，使用构造函数Stock( )初始化stock[1]。由于该声明只初始化了数组的部分元素，因此余下的7个元素将使用默认构造函数进行初始化。因此，要创建类对象数组，则这个类必须有默认构造函数。

## 10.6 类作用域

### 10.6.1 作用域为类的常量

有时候，使符号常量的作用域为类很有用。例如，类声明可能使用字面值30来指定数组的长度，由于该常量对于所有对象来说都是相同的，因此创建一个由所有对象共享的常量是个不错的主意。您可能以为这样做可行：

```c++
class Bakery
{
private:
	const int Months = 12;	// fails
	double costs[Months];
```

但这是行不通的，因为声明类只是描述了对象的形式，并没有创建对象。因此，在创建对象前，将没有用于存储值的空间（实际上，C++11提供了成员初始化，但不适用于前述数组声明，第12章将介绍该主题）。然而，有两种方式可以实现这个目标，并且效果相同。

第一种方式是在类中声明一个枚举。在类声明中声明的枚举的作用域为整个类，因此可以用枚举为整型常量提供作用域为整个类的符号名称。也就是说，可以这样开始Bakery声明：

```c++
class Bakery
{
private:
	enum {Months = 12};
	double costs[Months];
```

注意，用这种方式声明枚举并不会创建类数据成员。也就是说，所有对象中都不包含枚举。另外，Months只是一个符号名称，在作用域为整个类的代码中遇到它时，编译器将用30来替换它。

C++提供了另一种在类中定义常量的方式——使用关键字static：

```c++
class Bakery
{
private:
	static const int Months = 12;
	double costs[Months];
```

这将创建一个名为Months的常量，该常量将与其他静态变量存储在一起，而不是存储在对象中。因此，只有一个Months常量，被所有Bakery对象共享。

### 10.6.2 作用域内枚举

传统的枚举存在一些问题，其中之一是两个枚举定义中的枚举量可能发生冲突。假设有一个处理鸡蛋和T恤的项目，其中可能包含类似下面这样的代码：

```c++
enum egg {Small, Medium, Large, Jumbo};
enum t_shirt {Small, Medium, Large, Xlarge};
```

这将无法通过编译，因为egg Small和t_shirt Small位于相同的作用域内，它们将发生冲突。为避免这种问题，C++11提供了一种新枚举，其枚举量的作用域为类。这种枚举的声明类似于下面这样：

```c++
enum class egg {Small, Medium, Large, Jumbo};
enum class t_shirt {Small, Medium, Large, Xlarge};
```

也可使用关键字struct代替class。无论使用哪种方式，都需要使用枚举名来限定枚举量：

```c++
egg choice = egg::Large;
t_shirt Floyd = t_shirt::Large;
```

枚举量的作用域为类后，不同枚举定义中的枚举量就不会发生名称冲突了，而您可继续编写处理鸡蛋和T恤的项目。

C++11还提高了作用域内枚举的类型安全。在有些情况下，常规枚举将自动转换为整型，如将其赋给int变量或用于比较表达式时，但作用域内枚举不能隐式地转换为整型：

```c++
enum egg_old {Small, Medium, Large, Jumbo};				// unscoped
enum class t_shirt {Small, Medium, Large, Xlarge};		// scoped
egg_old one = Medium;									// unscoped
t_shirt rolf = t_shirt::Large;							// scoped
int king = one;							// implicit type conversion for unscoped
int ring = rolf;						// not allowed, no implicit type conversion
if (king < Jumbo)						// allowed
	cout << "Jumbo converted to in before comparison.\n";
if (king < t_shirt::Medium)				// not allowed
	cout << "Not allowed: < not defined for scoped enum.\n";
```

但在必要时，可进行显式类型转换：

```c++
int Frodo = int(t_shirt::Small);		// Frodo set to 0
```

# 第11章 使用类

## 11.1 运算符重载

运算符函数的格式如下：

```c++
operatorop(argument-list)
```

例如，operator +( )重载+运算符。op必须是有效的C++运算符，不能虚构一个新的符号。例如，不能有operator@( )这样的函数，因为C++中没有@运算符。然而，operator 函数将重载[ ]运算符，因为[ ]是数组索引运算符。例如，假设有一个Salesperson类，并为它定义了一个operator +( )成员函数，以重载+运算符，以便能够将两个Saleperson对象的销售额相加，则如果district2、sid和sara都是Salesperson类对象，便可以编写这样的等式：

```c++
district2 = sid + sara;
```

编译器发现，操作数是Salesperson类对象，因此使用相应的运算符函数替换上述运算符：

```c++
district2 = sid.operator+(sara);
```

## 11.2 计算时间：一个运算符重载示例

如下函数定义域类Time中，用于将两个Time相加，返回一个Time对象：

```c++
Time Time::Sum(const Time& t) const
{
	Time sum;
	sum.minutes = minutes + t.minutes;
	sum.hours = hours + t.hours + sum.minutes / 60;
	sum.minutes %= 60;
	return sum;
}
```

来看一下Sum( )函数的代码。注意参数是引用，但返回类型却不是引用。将参数声明为引用的目的是为了提高效率。如果按值传递Time对象，代码的功能将相同，但传递引用，速度将更快，使用的内存将更少。然而，返回值不能是引用。因为函数将创建一个新的Time对象（sum），来表示另外两个Time对象的和。返回对象（如代码所做的那样）将创建对象的副本，而调用函数可以使用它。然而，如果返回类型为Time &，则引用的将是sum对象。但由于sum对象是局部变量，在函数结束时将被删除，因此引用将指向一个不存在的对象。使用返回类型Time意味着程序将在删除sum之前构造它的拷贝，调用函数将得到该拷贝。
不要返回指向局部变量或临时对象的引用。函数执行完毕后，局部变量和临时对象将消失，引用将指向不存在的数据。

### 11.2.1 添加加法运算符

上述Sum()函数可以转化为如下运算符重载：

```c++
Time Time::operator+(const Time& t) const
{
	Time sum;
	sum.minutes = minutes + t.minutes;
	sum.hours = hours + t.hours + sum.minutes / 60;
	sum.minutes %= 60;
	return sum;
}
```

和Sum( )一样，operator +( )也是由Time对象调用的，它将第二个Time对象作为参数，并返回一个Time对象。因此，可以像调用Sum()那样来调用operator +( )方法：

```c++
total = coding.operator+(fixing);
```

但将该方法命令为operator +( )后，也可以使用运算符表示法：

```c++
total = coding + fixing;
```

这两种表示法都将调用operator +( )方法。注意，在运算符表示法中，运算符左侧的对象（这里为coding）是调用对象，运算符右边的对象（这里为fixing）是作为参数被传递的对象。

可以将两个以上的对象相加吗？例如，如果t1、t2、t3和t4都是Time对象，可以这样做吗：

```c++
t4 = t1 + t2 + t3;
```

为回答这个问题，来看一些上述语句将被如何转换为函数调用。由于+是从左向右结合的运算符，因此上述语句首先被转换成下面这样：

```c++
t4  = t1.operator+(t2 + t3);
```

然后，函数参数本身被转换成一个函数调用，结果如下：

```c++
t4 = t1.operator+(t2.operator+(t3));
```

上述语句合法吗？是的。函数调用t2.operator+(t3)返回一个Time对象，后者是t2和t3的和。然而，该对象成为函数调用t1.operator+( )的参数，该调用返回t1与表示t2和t3之和的Time对象的和。总之，最后的返回值为t1、t2和t3之和，这正是我们期望的。

### 11.2.2 重载限制

多数C++运算符（参见表11.1）都可以用这样的方式重载。重载的运算符（有些例外情况）不必是成员函数，但必须至少有一个操作数是用户定义的类型。下面详细介绍C++对用户定义的运算符重载的限制。

1．重载后的运算符必须至少有一个操作数是用户定义的类型，这将防止用户为标准类型重载运算符。因此，不能将减法运算符重载为计算两个double值的和，而不是它们的差。虽然这种限制将对创造性有所影响，但可以确保程序正常运行。

2．使用运算符时不能违反运算符原来的句法规则。例如，不能将求模运算符（%）重载成使用一个操作数：

```c++
int x;
Time shiva;
% x;			// invalid for modulus operator
% shiva;		// invalid for overloaded operator
```

同样，不能修改运算符的优先级。因此，如果将加号运算符重载成将两个类相加，则新的运算符与原来的加号具有相同的优先级。
3．不能创建新运算符。例如，不能定义operator **( )函数来表示求幂。

4．不能重载下面的运算符。

- sizeof：sizeof运算符。
- .：成员运算符。
- . *：成员指针运算符。
- ::：作用域解析运算符。
- ?:：条件运算符。
- typeid：一个RTTI运算符。
- const_cast：强制类型转换运算符。
- dynamic_cast：强制类型转换运算符。
- reinterpret_cast：强制类型转换运算符。
- static_cast：强制类型转换运算符。

然而，表11.1中所有的运算符都可以被重载。

5．表11.1中的大多数运算符都可以通过成员或非成员函数进行重载，但下面的运算符只能通过成员函数进行重载。

- =：赋值运算符。
- ( )：函数调用运算符。
- [ ]：下标运算符。
- ->：通过指针访问类成员的运算符。

## 11.3 友元

您知道，C++控制对类对象私有部分的访问。通常，公有类方法提供唯一的访问途径，但是有时候这种限制太严格，以致于不适合特定的编程问题。在这种情况下，C++提供了另外一种形式的访问权限：友元。友元有3种：

- 友元函数；
- 友元类；
- 友元成员函数。

介绍如何成为友元前，先介绍为何需要友元。在为类重载二元运算符时（带两个参数的运算符）常常需要友元。将Time对象乘以实数就属于这种情况，下面来看看。

```c++
	Time operator+(const Time& t) const;
	Time operator-(const Time& t) const;
	Time operator*(double n) const;
```

在前面的Time类示例中，重载的乘法运算符与其他两种重载运算符的差别在于，它使用了两种不同的类型。也就是说，加法和减法运算符都结合两个Time值，而乘法运算符将一个Time值与一个double值结合在一起。这限制了该运算符的使用方式。记住，左侧的操作数是调用对象。也就是说，下面的语句：

```C++
A = B * 2.75;
```

将被转换为下面的成员函数调用：

```c++
A = B.operator*(2.75);
```

但下面的语句又如何呢？

```c++
A = 2.75 * B;
```

从概念上说，2.75 * B应与B *2.75相同，但第一个表达式不对应于成员函数，因为2.75不是Time类型的对象。记住，左侧的操作数应是调用对象，但2.75不是对象。因此，编译器不能使用成员函数调用来替换该表达式。

解决这个难题的一种方式是，告知每个人（包括程序员自己）只能按B * 2.75这种格式编写，不能写成2.75 * B。这是一种对服务器友好-客户警惕的（server-friendly, client-beware）解决方案，与OOP无关。

然而，还有另一种解决方式——非成员函数（记住，大多数运算符都可以通过成员或非成员函数来重载）。非成员函数不是由对象调用的，它使用的所有值（包括对象）都是显式参数。这样，编译器能够将下面的表达式：

```c++
A = 2.75 * B;
```

与下面的非成员函数调用匹配：

```
A = operator*(2.75, B);
```

该函数的原型如下：

```c++
Time operator*(double m, const Time& t);
```

对于非成员重载运算符函数来说，运算符表达式左边的操作数对应于运算符函数的第一个参数，运算符表达式右边的操作数对应于运算符函数的第二个参数。而原来的成员函数则按相反的顺序处理操作数，也就是说，double值乘以Time值。

使用非成员函数可以按所需的顺序获得操作数（先是double，然后是Time），但引发了一个新问题：非成员函数不能直接访问类的私有数据，至少常规非成员函数不能访问。然而，有一类特殊的非成员函数可以访问类的私有成员，它们被称为友元函数。

### 11.3.1 创建友元

创建友元函数的第一步是将其原型放在类声明中，并在原型声明前加上关键字friend：

```c++
friend Time operator*(double m, const Time& t);
```

该原型意味着下面两点：

- 虽然operator *( )函数是在类声明中声明的，但它不是成员函数，因此不能使用成员运算符来调用；
- 虽然operator *( )函数不是成员函数，但它与成员函数的访问权限相同。

第二步是编写函数定义。因为它不是成员函数，所以不要使用Time::限定符。另外，不要在定义中使用关键字friend，定义应该如下：

```c++
Time operator*(double m, const Time& t)
{
	Time result;
	long totalminutes = t.hours * mult * 60 + t.minutes * mult;
	result.hours = totalminutes / 60;
	result.minutes = totalminutes % 60;
	return result;
}
```

有了上述声明和定义后，下面的语句：

```c++
A = 2.75 * B;
```

将转换为如下语句，从而调用刚才定义的非成员友元函数：

```c++
A = operator*(2.75, B);
```

总之，类的友元函数是非成员函数，其访问权限与成员函数相同。

实际上，按下面的方式对定义进行修改（交换乘法操作数的顺序），可以将这个友元函数编写为非友元函数：

```c++
Time operator*(double m, const Time& t)
{
	return t * m;		// use t.operator*(m)
}
```

原来的版本显式地访问t.minutes和t.hours，所以它必须是友元。这个版本将Time对象t作为一个整体使用，让成员函数来处理私有值，因此不必是友元。然而，将该版本作为友元也是一个好主意。最重要的是，它将作为正式类接口的组成部分。其次，如果以后发现需要函数直接访问私有数据，则只要修改函数定义即可，而不必修改类原型。

### 11.3.2 常用的友元：重载<<运算符

提示：一般来说，要重载<<运算符来显示c_name的对象，可使用一个友元函数，其定义如下：

```c++
ostream& operator<<(ostream& os, const c_name& obj)
{
	os << ... ;
	return os;
}
```

## 11.5 再谈重载：一个矢量类

### 11.5.2 为Vector类重载算术运算符

一元负号运算符，它只使用一个操作数。将这个运算符用于数字（如.x）时，将改变它的符号。因此，将这个运算符用
于矢量时，将反转矢量的每个分量的符号。更准确地说，函数应返回一个与原来的矢量相反的矢量（对于极坐标，长度不变，但方向相反）。下面是重载负号的原型和定义：

```c++
Vector operator-() const;
Vector Vector::operator-() const
{
	return Vector(-x, -y);
}

// 使用一元 - 运算符
Vector v1(1, 1);
Vector v2 = -v1;
```

## 11.6 类的自动转换和强制类型转换

在C++中，接受一个参数的构造函数为将类型与该参数相同的值转换为类提供了蓝图。因此，下面的构造函数用于将double类型的值转换为Stonewt类型：

```C++
Stonewt(double lbs);
```

也就是说，可以编写这样的代码：

```c++
Stonewt myCat;
myCat = 19.6;
```

程序将使用构造函数Stonewt(double)来创建一个临时的Stonewt对象，并将19.6作为初始化值。随后，采用逐成员赋值方式将该临时对象的内容复制到myCat中。这一过程称为隐式转换，因为它是自动进行的，而不需要显式强制类型转换。

只有接受一个参数的构造函数才能作为转换函数。下面的构造函数有两个参数，因此不能用来转换类型：

```c++
Stonewt(int stn, double lbs);
```

然而，如果给第二个参数提供默认值，它便可用于转换int：

```c++
Stonewt(int stn, double lbs = 0);
```

将构造函数用作自动类型转换函数似乎是一项不错的特性。然而，当程序员拥有更丰富的C++经验时，将发现这种自动特性并非总是合乎需要的，因为这会导致意外的类型转换。因此，C++新增了关键字explicit，用于关闭这种自动特性。也就是说，可以这样声明构造函数：

```c++
explicit Stonewt(double lbs);
```

这将关闭上述示例中介绍的隐式转换，但仍然允许显式转换，即显式强制类型转换：

```c++
Stonewt myCat;
myCat = 19.6;		// not valid if Stonewt(double) is delcared as explicit
myCat = Stonewt(19.6);		// ok
myCat = (Stonewt) 19.6;		// ok
```

### 11.6.1 转换函数

转换函数是用户定义的强制类型转换，可以像使用强制类型转换那样使用它们。例如，如果定义了从Stonewt到double的转换函数，就可以使用下面的转换：

```c++
Stonewt wolfe(285.7);
double host = double (wolfe);
double thinker = (double) wolfe;
```

也可以让编译器来决定如何做：

```c++
Stonewt wells(20, 3);
double star = wells;		// implicit use of conversion function
```

编译器发现，右侧是Stonewt类型，而左侧是double类型，因此它将查看程序员是否定义了与此匹配的转换函数。（如果没有找到这样的定义，编译器将生成错误消息，指出无法将Stonewt赋给double。）

那么，如何创建转换函数呢？要转换为typeName类型，需要使用这种形式的转换函数：

```c++
operator typeName();
```

- 转换函数必须是类方法；
- 转换函数不能指定返回类型；
- 转换函数不能有参数。

例如，要添加将Stonewt对象转换为int和double类型的函数，需要将下面的原型添加到类声明中：

```c++
operator int();
operator double();
```

**自动应用类型转换**

看如下代码：

```c++
Stonewt poppins(9, 2.8);
cout << "Poppins: " << int (poppins) << " pounds.\n";
```

假设Stonewt类同时定义了int和double两种转换函数，那么可以省略上面的强制类型转换吗？即：

```C++
Stonewt poppins(9, 2.8);
cout << "Poppins: " << poppins << " pounds.\n";
```

答案是否定的。在p_wt示例中，上下文表明，poppins应被转换为double类型。但在cout示例中，并没有指出应转换为int类型还是double类型。在缺少信息时，编译器将指出，程序中使用了二义性转换。该语句没有指出要使用什么类型。

有趣的是，如果类只定义了double转换函数，则编译器将接受该语句。这是因为只有一种转换可能，因此不存在二义性。

赋值的情况与此类似。对于当前的类声明来说，编译器将认为下面的语句有二义性而拒绝它。

```c++
long gone = poppins;
```

在C++中，int和double值都可以被赋给long变量，所以编译器使用任意一个转换函数都是合法的。编译器不想承担选择转换函数的责任。然而，如果删除了这两个转换函数之一，编译器将接受这条语句。例如，假设省略了double定义，则编译器将使用int转换，将poppins转换为一个int类型的值。然后在将它赋给gone时，将int类型值转换为long类型。

与转换构造函数一样，转换函数也提供了关闭隐式转换的方式，C++11可以将转换运算符声明为显式地：

```c++
class Stonewt
{
	...
	explicit operator int() const;
	explicit operator double() const;
}
```

有了这些声明后，需要强制转换时将调用这些运算符。

### 11.6.2 转换函数和友元函数

先说结论，当调用某个对象的函数时（包括运算符重载），C++不会调用转换函数进行隐式转换。某个对象作为函数参数或返回值时，是可以进行隐式转换的。

例如：

```c++
Stonewt jennySt(9, 12);
double pennyD = 146.0;
Stonewt total;
total = pennyD + jennySt;
```

pennyD是double类型，如果提供了Stonewt(double)的构造函数，同时提供了operator+的成员函数，那么上面第四行能编译通过吗。答案是否定的，pennyD+jennySt试图调用pennyD.operator+(jennySt)，由于pennyD是double类型，需要通过转换构造函数Stonewt(double)隐式转换为Stonewt类型，但是，当调用某个对象的函数时，C++不会进行隐式转换，所以上面第四行无法编译通过。

如果想编译通过，成员函数走不通，只能通过友元函数来实现。

```c++
friend Stonewt operator+(double x, Stonewt& s);	
```

**实现加法时的选择**

如果想将double和Stonewt相加，有两种方式。

方法一，将下面的函数定义为友元函数，同时提供Stonewt(double)构造函数将double类型的参数转换为Stonewt类型的参数。

```c++
operator+(const Stonewt&, const Stonewt&);
```

方法二，将加法运算符重载为一个显式使用double类型参数的函数：

```c++
Stonewt operator+(double x);		// member function	Stonewt + double
friend Stonewt operator+(double x, Stonewt& s);		// double + Stonewt
```

每一种方法都有其优点。第一种方法（依赖于隐式转换）使程序更简短，因为定义的函数较少。这也意味程序员需要完成的工作较少，出错的机会较小。这种方法的缺点是，每次需要转换时，都将调用转换构造函数，这增加时间和内存开销。第二种方法（增加一个显式地匹配类型的函数）则正好相反。它使程序较长，程序员需要完成的工作更多，但运行速度较快。

# 第12章 类和动态内存分配

本章环环相扣介绍了诸多知识，先简单总结下思路：

1. 首先，本章引入了一个简单的概念，静态类成员。被static修饰的成员，被所有类对象共有。

2. 然后，本章介绍了一种场景，即如果类的成员是通过new创建的，则需要在析构函数中执行delete。

3. 紧接着，写了一个类Stringbad用于模拟字符串。有两点说明

   1. 该类有一个static的成员标记该类被创建了多少对象，构造函数执行时+1，相反，析构时该static成员减一。
   2. 该类有一个char*指针成员，记录字符串的首地址，且构造函数中通过new初始化该成员。换句话说，对象仅保存指针，不保存指针指向的数据块。析构函数执行delete。

4. 接着写了一个使用Stringbad的类，该类中有两种赋值，一种是创建时赋值，另一种是创建后赋值：

   ```c++
   Stringbad str1 = "abc";
   // 创建时赋值（实际调用默认复制构造函数）
   Stringbad str2 = str1;
   
   // 创建后赋值（实际调用默认赋值运算符）
   Stringbad str3;
   str3 = str1;
   ```

5. 通过示例代码，发现【复制构造函数】和【赋值运算符】的默认实现，引出该默认实现下的问题：

   1. 【复制构造函数】【赋值运算符】可能的问题
      1. 直接复制成员，不复制成员指向的数据块。可能导致两个对象指向同一个数据块，一个对象析构销毁数据块或修改数据块后，影响另一个对象的问题。
   2. 【复制构造函数】可能的问题
      1. 不会修改静态类成员

6. 为了解决上述问题，建议直接提供【复制构造函数】和【赋值运算符】的显示定义，并针对其中的指针成员进行深度拷贝。

7. 紧接着，本章介绍了C++会针对如下成员函数提供默认定义：

   1. 默认构造函数
   2. 默认析构函数
   3. 默认复制构造函数
   4. 默认赋值运算符
   5. 默认地址运算符

## 12.1 动态内存和类

### 12.1.1 复习示例和静态类成员

```c++
// StringBad.h
class StringBad
{
private:
    // 静态类成员
	static int num_strings;
}

// StringBad.cpp
StringBad::num_strings = 10;
```

静态类成员有一个特点：无论创建了多少对象，程序都只创建一个静态类变量副本。也就是说，类的所有对象共享同一个静态成员。

请注意，不能在类声明中初始化静态成员变量，这是因为声明描述了如何分配内存，但并不分配内存。静态数据成员在类声明中声明，在包含类方法的文件中初始化。初始化时使用作用域运算符来指出静态成员所属的类。但如果静态成员是整型或枚举型const，则可以在类声明中初始化。

*注：在C++11之前，非静态成员也是不可以在声明处初始化的，C++11方才支持。*

```c++
class StringBad
{
	// C++11支持类声明处初始化，原理同初始化列表。
	int len = 10;
}
```

### 12.1.2 特殊成员函数

接下来看下StringBad类的完整设计：

```c++
// StringBad.h
class StringBad
{
private:
	char* str;      				// 类声明没有为字符串本身分配存储空间
	int len;
	static int num_strings; 		// 无论创建了多少对象，程序都只创建一个静态类变量副本。
public:
	// 在构造函数中使用new来为字符串分配空间。
	StringBad();                    // 默认构造函数
	StringBad(const char* s);      // 自定义构造函数
	~StringBad();

	friend std::ostream& operator<<(std::ostream& os, const StringBad& st);
};
```

```c++
// StringBad.cpp
#include <cstring>
#include "stringbad.h"
using std::cout;

int StringBad::num_strings = 0; // 初始化静态变量，用于记录创建的类数量

StringBad::StringBad()          // 在构造函数中使用new来为字符串分配空间
{
    len = 6;
    str = new char[6];
    std::strcpy(str, "happy");
    num_strings++;
    cout << num_strings << " : \"" << str << "\" object created.\n";
}

StringBad::StringBad(const char* s)
{
    // str = s;             // 这只保存了地址，而没有创建字符串副本。
    len = std::strlen(s);   // 不包括末尾的空字符
    str = new char[len + 1];  // 使用new分配足够的空间来保存字符串，然后将新内存的地址赋给str成员。
    std::strcpy(str, s);
    num_strings++;
    cout << num_strings << " : \"" << str << "\" object created.\n";
}

StringBad::~StringBad()
{
    cout << "\"" << str << "\" object delete, ";
    --num_strings;
    cout << num_strings << " left.\n";
    delete[] str;
}

std::ostream& operator<<(std::ostream& os, const StringBad& st)
{
    os << st.str;
    return os;
}
```

```c++
// useStringBad.cpp
#include "stringbad.h"
using std::cout;

void callme1(StringBad& rsb);
void callme2(StringBad sb);

int main(void)
{
    using std::endl;
    {
        cout << "Starting an inner block.\n";
        StringBad headline1("Celery Stalks at Midnight");
        StringBad headline2("Lettuce Prey");
        StringBad sports("Spinach Leaves Bowl for Dollars");
        // 至此，创建了三个对象，num_strings = 3
        
        cout << "headline1: " << headline1 << endl;
        cout << "headline2: " << headline2 << endl;
        cout << "sports: " << sports << endl;
        
        // 引入传参，不会调用复制构造函数创建副本
        callme1(headline1);
        cout << "headline1: " << headline1 << endl;
        
        // 复制构造函数被用来初始化 callme2()的形参，函数退出时析构
        // 由于没有定义复制构造函数，所以num_strings没有增加，但随着形参的析构而减少，调用后num_strings为2
        callme2(headline2);
        cout << "headline2: " << headline2 << endl;
        cout << "Initialize one object to another:\n";

        // 复制构造函数，StringBad sailor = StringBad(sports);
        StringBad sailor = sports;
        cout << "sailor: " << sailor << endl;
        cout << "Assign one object to another:\n";
        StringBad knot;

        // 赋值构造函数，knot.operator=(headline1);
        knot = headline1;   
        cout << "knot: " << knot << endl;
        cout << "Exiting the block.\n";

        // 该代码块执行完调用析构函数，否则要main函数执行完调用析构函数。
    }
    cout << "End of main()\n";
    return 0;
}
void callme1(StringBad& rsb)
{
    cout << "String passed by reference:";
    cout << " \"" << rsb << "\"\n";
}
void callme2(StringBad sb)
{
    cout << "String passed by value:";
    cout << " \"" << sb << "\"\n";
}
```

上面的代码可能执行报错，因为callme2(StringBad sb)是值传递，创建的临时对象析构时会将实参对象指向的数据块回收，当实参对象析构时，有些编译器在针对同一数据块重复调用delete时会报错。

上面的代码作为一种错误的实现，引入了复制构造函数、赋值运算符的概念，在没有显式定义二者的时候，C++会提供默认定义，但该定义仅是将所有的成员进行赋值操作。本例暴露了默认复制构造函数和默认赋值运算符的两个可能的问题：

1、 如果成员是指针，仅复制指针，不会复制指向的数据块，导致两个类指向同一个数据块，可能导致一系列问题。例如一个对象析构时，释放了该数据块的资源，导致另一个对象数据异常。再或者通过一个对象操作了该数据块，同时另一个对象指向的数据块也被修改，而这往往是不符合预期的。

2、默认复制构造函数和默认赋值运算符不会修改static变量，有些自定义的逻辑，还是要通过显式定义二者来实现。

下面按书的思路详细再看下：

#### 特殊成员函数

C++自动提供了下面这些成员函数：

1. 默认构造函数，如果没有定义构造函数；
2. 默认析构函数，如果没有定义；
3. 复制构造函数，如果没有定义；
4. 赋值运算符，如果没有定义；
5. 地址运算符，如果没有定义。

#### 默认构造函数

如果没有提供任何构造函数，C++将创建默认构造函数。例如，假如定义了一个Klunk类，但没有提供任何构造函数，则编译器将提供下述默认构造函数：

```c++
Klunk::Klunk() {}		// 隐式定义的默认构造函数
```

如果定义了构造函数，C++将不会定义默认构造函数。如果希望在创建对象时不显式地对它进行初始化，则必须显式地定义默认构造函数。这种构造函数没有任何参数，但可以使用它来设置特定的值：

```c++
Klunk::Klunk()	// 显式定义的默认构造函数
{
	klunk_ct = 0;
	...
}
```

带参数的构造函数也可以是默认构造函数，只要所有参数都有默认值。例如，Klunk类可以包含下述内联构造函数：

```c++
Klunk(int n = 0) { klunk_ct = n; }
```

但只能有一个默认构造函数。也就是说，不能这样做：

```c++
Klunk::Klunk() { klunk_ct = 0; }
Klunk(int n = 0) { klunk_ct = n; }
```

这为何有二义性呢？请看下面两个声明：

```c++
Klunk kar(10);		// 匹配 Klunk(int n)
Klunk bus;			// 两个均match
```

#### 复制构造函数

复制构造函数也叫拷贝构造函数，用于将一个对象复制到新创建的对象中。也就是说，它用于初始化过程中（包括按值传递参数），而不是常规的赋值过程中。类的复制构造函数原型通常如下：

```c++
Class_name(const Class_name &)
```

#### 何时调用复制构造函数

新建一个对象并将其初始化为同类现有对象时，复制构造函数都将被调用。例如，假设motto是一个StringBad对象，则下面4种声明都将调用复制构造函数：

```c++
StringBad ditto(motto);
StringBad metoo = motoo;
StringBad also = StringBad(motto);
StringBad* pStringBad = new StringBad(motto);
```

其中中间的2种声明可能会使用复制构造函数直接创建metoo和also，也可能使用复制构造函数生成一个临时对象，然后将临时对象的内容赋给metoo和also，这取决于具体的实现。最后一种声明使用motto初始化一个匿名对象，并将新对象的地址赋给pstring指针。

除了上面的显式创建对象的场景外，每当程序隐式生成了对象副本时，编译器都将使用复制构造函数。具体地说，当函数按值传递对象（如程序清单12.3中的callme2()）或函数返回对象时，都将使用复制构造函数。记住，按值传递意味着创建原始变量的一个副本。编译器生成临时对象时，也将使用复制构造函数。例如，将3个Vector对象相加时，编译器可能生成临时的Vector对象来保存中间结果。何时生成临时对象随编译器而异，但无论是哪种编译器，当按值传递和返回对象时，都将调用复制构造函数。

由于按值传递对象将调用复制构造函数，因此应该按引用传递对象。这样可以节省调用构造函数的时间以及存储新对象的空间。

#### 赋值运算符

赋值运算符的原型如下：

```c++
Class_name & Class_name::operator=(const Class_name &)
```

它接受并返回一个指向类对象的引用。例如，StringBad类的赋值运算符的原型如下：

```c++
StringBad & StringBad::operator=(const StringBad &)
```

赋值运算符出现的问题与隐式复制构造函数相同：数据受损，解决方法也是通过深度拷贝。

#### **解决方法**

可以选择显式定义复制构造函数和赋值运算符，并进行深度复制，也可以通过将二者声明为private的，避免隐式调用：

显式定义 + 深度复制：

```c++
// StringBad.h
class StringBad
{
public:
    StringBad(const StringBad& s); // 复制构造函数
	StringBad& operator=(const StringBad& st);    // 赋值运算符
}
```

```c++
// StringBad.cpp
StringBad::StringBad(const StringBad& st)
{
    num_strings++; 				// handle static member update
    len = st.len; 				// same length
    str = new char[len + 1]; 	// allot space
    std::strcpy(str, st.str); 	// copy string to new location
    cout << num_strings << ": \"" << str
        << "\" object created\n"; // For Your Information
}

// 代码首先检查自我复制，这是通过查看赋值运算符右边的地址
// （&st）是否与接收对象（this）的地址相同来完成的。如果相同，程序
// 将返回*this，然后结束。第10章介绍过，赋值运算符是只能由类成员
// 函数重载的运算符之一。
// 如果地址不同，函数将释放str指向的内存，这是因为稍后将把一
// 个新字符串的地址赋给str。如果不首先使用delete运算符，则上述字
// 符串将保留在内存中。由于程序中不再包含指向该字符串的指针，因此
// 这些内存被浪费掉。
// 接下来的操作与复制构造函数相似，即为新字符串分配足够的内存
// 空间，然后将赋值运算符右边的对象中的字符串复制到新的内存单元中。
StringBad& StringBad::operator=(const StringBad& st)
{
    if (this == &st) 			// object assigned to itself
        return *this; 			// all done
    delete[] str; 				// free old string
    len = st.len;
    str = new char[len + 1]; 	// get space for new string
    std::strcpy(str, st.str); 	// copy the string
    return *this; 				// return reference to invoking object
}
```

声明为private：

```c++
// StringBad.h
class StringBad
{
private:
    StringBad(const StringBad& s); // 复制构造函数
	StringBad& operator=(const StringBad& st);    // 赋值构造函数
}
```

```c++
// useStringBad.cpp

int main()
{
    StringBad sb1;
    
    // 由于隐藏了复制构造函数，如下代码无法通过编译
    StringBad sb2 = sb1;
    StringBad sb3;
    
    // 由于隐藏了赋值运算符，如下代码无法通过编译
    sb3 = sb1;
    return 0;
}
```

## 12.2 改进后的新String类

### 12.2.1 修订后的默认构造函数

请注意新的默认构造函数，它与下面类似：

```c++
String::String()
{
	len = 0;
	str = new char[1];
	str[0] = '\0';
}
```

您可能会问，为什么代码为：

```c++
str = new char[1];
```

而不是：

```c++
str = new char;
```

上面两种方式分配的内存量相同，区别在于前者与类析构函数兼容，而后者不兼容。析构函数中包含如下代码：

```c++
delete[] str;
```

delete[]与使用new[]初始化的指针和空指针都兼容。因此对于下述代码：

```c++
str = new char[1];
str[0] = '\0';
```

可修改为：

```c++
str = 0;
```

对于以其他方式初始化的指针，使用delete [ ]时，结果将是不确定的：

```c++
char words[15] = "bad idea";
char* p1 = words;
char* p2 = new char;
char* p3;
delete[] p1;		// undefined, so don't do it
delete[] p2;		// undefined, so don't do it
delete[] p3;		// undefined, so don't do it
```

**关于空指针**

在C++98中，字面值0有两个含义：可以表示数字值零，也可以表示空指针，这使得阅读程序的人和编译器难以区分。有些程序员使用（void *） 0来标识空指针（空指针本身的内部表示可能不是零），还有些程序员使用NULL，这是一个表示空指针的C语言宏。C++11提供了更好的解决方案：引入新关键字nullptr，用于表示空指针。您仍可像以前一样使用0，否则大量现有的代码将非法，但建议您使用nullptr：

### 12.2.2 比较成员函数

自定义的StringBad需要兼容C-style字符串，当需要字符串比较时，最好提供友元函数而非类成员函数。

以 < 比较为例，有几种情况：

1. C-style < string
2. C-style < C-style
3. string < string
4. string < C-style

其中，第二条可通过strcmp()实现，接下来看下友元和非友元分别如何实现1/3/4

#### **友元**

```c++
bool operator<(const String& st1, const String& st2)
```

友元提供上面的函数声明，直接匹配3，对于1和4，由于我们提供了如下构造函数，可以直接作为转换函数，所以也是匹配的：

```c++
class StringBad
{
public:
	// 构造函数，可作为C-style字符串到StringBad的转换函数
	String(const char* s);
}
```

例如，假设answer是String对象，则下面的代码：

```c++
if ("love" < answer)
```

将被转换为：

```c++
if (operator<("love", answer))
```

然后，编译器将使用某个构造函数将代码转换为：

```c++
if (operator<(StringBad("love"), answer))
```

这与原型是相匹配的。

#### 非友元

```c++
class StringBad
{
public:
	bool operator<(const String& st2);
}
```

这可以解决3/4，对于1，例如

```c++
if ("love" < answer)
```

如下转换是不会执行的：

```c++
// Error 调用方不会执行转换函数
if (StringBad("love").operator<(answer))
```

综上，需采用友元实现比较成员函数

### 12.2.3 使用中括号表示法访问字符

支持通过重载中括号运算符来实现访问字符串指定位置，但是为什么需要重载两个呢：

*（这里注意一下，返回值是不能构成重载的，下例中参数也没有重载，只能说明针对类方法，末尾是否加const是构成重载的，但是这个重载体现在调用方，即不带const的匹配调用方不是const，带const的匹配调用方也是const）*

```c++
    char& operator[](int i);
    const char& operator[](int i) const;
```

将返回类型声明为char &，便可以给特定元素赋值。例如，可以编写这样的代码：

```c++
String means("might");
means[0] = 'r';
```

第二条语句将被转换为一个重载运算符函数调用：

```c++
means.operator[](0) = 'r';
```

但假设有下面的常量对象：

```c++
const String answer("futile");
```

如果只有上述第一个operator 定义，则下面的代码将出错：

```c++
cout << answer[1];		// compile error
```

原因是answer是常量，而上述方法无法确保不修改数据（实际上，有时该方法的工作就是修改数据，因此无法确保不修改数据）。
但在重载时，C++将区分常量和非常量函数的特征标，因此可以提供另一个仅供const String对象使用的operator版本：

```c++
const char & String::operator[](int i) const
{
	return str[i];
}
```

### 12.2.4 静态类成员函数

首先，不能通过对象调用静态成员函数；实际上，静态成员函数甚至不能使用this指针。如果静态成员函数是在公有部分声明的，则可以使用类名和作用域解析运算符来调用它。例如，可以给String类添加一个名为HowMany( )的静态成员函数，方法是在类声明中添加如下原型/定义：

```c++
static int HowMany() { return num_strings; }
```

调用它的方式如下：

```c++
int count = String::HowMany();
```

静态成员函数不与特定的对象相关联，因此只能使用静态数据成员。

### 12.2.5 进一步重载赋值运算符

介绍针对String类的程序清单之前，先来考虑另一个问题。假设要将常规字符串复制到String对象中。例如，假设使用getline()读取了一个字符串，并要将这个字符串放置到String对象中，前面定义的类方法让您能够这样编写代码：

```c++
String name;
char temp[40];
cin.getline(temp, 40);
name = temp;
```

但如果经常需要这样做，这将不是一种理想的解决方案。为解释其原因，先来回顾一下最后一条语句是怎样工作的。
1．程序使用构造函数String（const char *）来创建一个临时String对象，其中包含temp中的字符串副本。第11章介绍过，只有一个参数的构造函数被用作转换函数。
2．本章后面的程序清单12.6中的程序使用String &String::operator=（const String &）函数将临时对象中的信息复制
到name对象中。
3．程序调用析构函数~String()删除临时对象。

为提高处理效率，最简单的方法是重载赋值运算符，使之能够直接使用常规字符串，这样就不用创建和删除临时对象了。下面是一种可能的实现：

``` c++
String& String::operator=(const char* s)
{
	delete[] str;
	len = std::strlen(s);
	str = new char[len + 1];
	std::strcpy(str, s);
	return *this;
}
```

## 12.3 在构造函数中使用new时应注意的事项

- 如果在构造函数中使用new来初始化指针成员，则应在析构函数中使用delete。
- new和delete必须相互兼容。new对应于delete，new[ ]对应于delete[ ]。
- 如果有多个构造函数，则必须以相同的方式使用new，要么都带中括号，要么都不带。因为只有一个析构函数，所有的构造函数都必须与它兼容。然而，可以在一个构造函数中使用new初始化指针，而在另一个构造函数中将指针初始化为空（0或C++11中的nullptr），这是因为delete（无论是带中括号还是不带中括号）可以用于空指针。

### 12.3.1 应该和不应该

对不是使用new初始化的指针使用delete时，结果将是不确定的，并可能是有害的。可将该构造函数修改为下面的任何一种形式：

```c++
String::String()
{
	len = 0;
	str = new char[1];
	str[0] = '\0';
}
```

```c++
String::String()
{
	len = 0;
	str = 0;	// or str = nullptr;
}
```

```c++
String::String()
{
	static const char* s = "C++";	// 避免重复初始化
	len = std::strlen(s);
	str = new char[len + 1];
	std::strcpy(str, s);
}
```

## 12.4 有关返回对象的说明

- 如果方法或函数要返回局部对象，则应返回对象，而不是指向对象的引用。在这种情况下，将使用复制构造函数来生成返回的对象。
- 如果方法或函数要返回一个没有公有复制构造函数的类（如ostream类）的对象，它必须返回一个指向这种对象的引用。
- 最后，有些方法和函数（如重载的赋值运算符）可以返回对象，也可以返回指向对象的引用，在这种情况下，应首选引用，因为其效率更高。

## 12.5 使用指向对象的指针

### 12.5.3 再谈定位new运算符

定位new运算符让您能够在分配内存时指定内存位置，这在创建对象时同样适用。请看程序清单12.8：

```c++
// 12.9 placenew1.cpp -- new, placement new, no delete
// 使用了定位new运算符和常规new运算符给对象分配内存.
// 该程序使用new运算符创建了一个512字节的内存缓冲区，
// 然后使用new运算符在堆中创建两个JustTesting对象，
// 并试图使用定位new运算符在内存缓冲区中创建两个JustTesting对象。
// 最后，它使用delete来释放使用new分配的内存。
#include <iostream>
#include <string>
#include <new>
using namespace std;
const int BUF = 512;
class JustTesting
{
private:
    string words;
    int number;
public:
    JustTesting(const string& s = "Just Testing", int n = 0)
    {
        words = s; number = n; cout << words << " constructed\n";
    }
    ~JustTesting() { cout << words << " destroyed\n"; }
    void Show() const { cout << words << ", " << number << endl; }
};
int main()
{
    char* buffer = new char[BUF]; // get a block of memory
    JustTesting* pc1, * pc2;
    pc1 = new (buffer) JustTesting; // place object in buffer
    pc2 = new JustTesting("Heap1", 20); // place object on heap
    cout << "Memory block addresses:\n" << "buffer: "
        << (void*)buffer << " heap: " << pc2 << endl;
    cout << "Memory contents:\n";
    cout << pc1 << ": ";
    pc1->Show();
    cout << pc2 << ": ";
    pc2->Show();
    JustTesting* pc3, * pc4;
    pc3 = new (buffer) JustTesting("Bad Idea", 6);
    pc4 = new JustTesting("Heap2", 10);
    cout << "Memory contents:\n";
    cout << pc3 << ": ";
    pc3->Show();
    cout << pc4 << ": ";
    pc4->Show();
    delete pc2; // free Heap1
    delete pc4; // free Heap2
    delete[] buffer; // free buffer
    cout << "Done\n";
    return 0;
}
```

程序清单12.8在使用定位new运算符时存在两个问题。首先，在创建第二个对象时，定位new运算符使用一个新对象来覆盖用于第一个对象的内存单元。显然，如果类动态地为其成员分配内存，这将引发问题。

其次，将delete用于pc2和pc4时，将自动为pc2和pc4指向的对象调用析构函数；然而，将delete[]用于buffer时，不会为使用定位new运算符创建的对象调用析构函数。

这里的经验教训与第9章介绍的相同：程序员必须负责管理定位new运算符创建对象的内存单元。要使用不同的内存单元，程序员需要提供两个位于缓冲区的不同地址，并确保这两个内存单元不重叠。例如，可以这样做：

```c++
pc1 = new (buffer) JustTesting;
pc3 = new (buffer + sizeof (JustTesting)) JustTesting("Better Idea", 6);
```

其中指针pc3相对于pc1的偏移量为JustTesting对象的大小。

第二个教训是，如果使用定位new运算符来为对象分配内存，必须确保其析构函数被调用。但如何确保呢？对于在堆中创建的对象，可以这样做：

```c++
delete pc2;
```

但不能像下面这样做：

```c++
delete pc1;
delete pc3;
```

原因在于delete可与常规new运算符配合使用，但不能与定位new运算符配合使用。例如，指针pc3没有收到new运算符返回的地址，因此delete pc3将导致运行阶段错误。在另一方面，指针pc1指向的地址与buffer相同，但buffer是使用new []初始化的，因此必须使用delete []而不是delete来释放。即使buffer是使用new而不是new []初始化的，delete pc1也将释放buffer，而不是pc1。这是因为new/delete系统知道已分配的512字节块buffer，但对定位new运算符对该内存块做了何种处理一无所知。

该程序确实释放了buffer：

```c++
delete[] buffer;
```

正如上述注释指出的，delete [] buffer;释放使用常规new运算符分配的整个内存块，但它没有为定位new运算符在该内存块中创建的对象调用析构函数。您之所以知道这一点，是因为该程序使用了一个显示信息的析构函数，该析构函数宣布了“Heap1”和“Heap2”的死亡，但却没有宣布“Just Testing”和“Bad Idea”的死亡。

这种问题的解决方案是，显式地为使用定位new运算符创建的对象调用析构函数。正常情况下将自动调用析构函数，这是需要显式调用析构函数的少数几种情形之一。显式地调用析构函数时，必须指定要销毁的对象。由于有指向对象的指针，因此可以使用这些指针：

```c++
pc3->~JustTesting();
pc1->~JustTesting();
```

程序清单12.9对定位new运算符使用的内存单元进行管理，加入到合适的delete和显式析构函数调用，从而修复了程序清单12.8中的问题。需要注意的一点是正确的删除顺序。对于使用定位new运算符创建的对象，应以与创建顺序相反的顺序进行删除。原因在于，晚创建的对象可能依赖于早创建的对象。另外，仅当所有对象都被销毁后，才能释放用于存储这些对象的缓冲区。

```c++
// placenew2.cpp -- new, placement new, no delete
// 该程序使用定位new运算符在相邻的内存单元中创建两个对象，并调用了合适的析构函数。
#include <iostream>
#include <string>
#include <new>
using namespace std;
const int BUF = 512;
class JustTesting
{
private:
    string words;
    int number;
public:
    JustTesting(const string& s = "Just Testing", int n = 0)
    {
        words = s; number = n; cout << words << " constructed\n";
    }
    ~JustTesting() { cout << words << " destroyed\n"; }
    void Show() const { cout << words << ", " << number << endl; }
};
int main()
{
    char* buffer = new char[BUF]; // get a block of memory
    JustTesting* pc1, * pc2;
    pc1 = new (buffer) JustTesting; // place object in buffer
    pc2 = new JustTesting("Heap1", 20); // place object on heap
    cout << "Memory block addresses:\n" << "buffer: "
        << (void*)buffer << " heap: " << pc2 << endl;
    cout << "Memory contents:\n";
    cout << pc1 << ": ";
    pc1->Show();
    cout << pc2 << ": ";
    pc2->Show();
    JustTesting* pc3, * pc4;
    // fix placement new location
    pc3 = new (buffer + sizeof(JustTesting))
        JustTesting("Better Idea", 6);
    pc4 = new JustTesting("Heap2", 10);
    cout << "Memory contents:\n";
    cout << pc3 << ": ";
    pc3->Show();
    cout << pc4 << ": ";
    pc4->Show();
    delete pc2; // free Heap1
    delete pc4; // free Heap2
    // explicitly destroy placement new objects
    pc3->~JustTesting(); // destroy object pointed to by pc3
    pc1->~JustTesting(); // destroy object pointed to by pc1
    delete[] buffer; // free buffer
    cout << "Done\n";
    return 0;
}
```

## 12.7 队列模拟

### 12.7.1 队列类

#### **类的嵌套**

本节介绍了类的嵌套，类可以嵌套结构体、类或枚举，例如：

```c++
class Queue
{
private:
    // class scope definitions
    // Node is a nested structure definition local to this class
    struct Node { Item item; struct Node* next; };
};
```

在类声明中声明的结构、类或枚举被称为是被嵌套在类中，其作用域为整个类。这种声明不会创建数据对象，而只是指定了可以在类中使用的类型。如果声明是在类的私有部分进行的，则只能在这个类使用被声明的类型；如果声明是在公有部分进行的，则可以从类的外部通过作用域解析运算符使用被声明的类型。例如，如果Node是在Queue类的公有部分声明的，则可以在类的外面声明Queue::Node类型的变量。

#### 初始化列表

先看下类Queue的声明：

```c++
class Queue
{
private:
    // class scope definitions
    // Node is a nested structure definition local to this class
    // 结构体表示链表的每一个节点，存放节点信息和下一个指向的位置
    // 当前节点保存的是一个类的对象，链表中依次存类的对象
    struct Node { Item item; struct Node* next; };
    enum { Q_SIZE = 10 };
    // private class members
    Node* front;   // pointer to front of Queue
    Node* rear;    // pointer to rear of Queue
    int items;      // current number of items in Queue
    const int qsize; // maximum number of items in Queue
public:
    Queue(int qs = Q_SIZE);         // create queue with a qs limit
    ~Queue();
};
```

类构造函数应提供类成员的值。由于在这个例子中，队列最初是空的，因此队首和队尾指针都设置为NULL（0或nullptr），并将items设置为0。另外，还应将队列的最大长度qsize设置为构造函数参数qs的值。下面的实现方法无法正常运行：

```c++
Queue::Queue(int qs)
{
	front = rear = NULL;
	items = 0;
	qsize = qs;
}
```

问题在于qsize是常量，所以可以对它进行初始化，但不能给它赋值。从概念上说，调用构造函数时，对象将在括号中的代码执行之前被创建。因此，调用Queue（int qs）构造函数将导致程序首先给4个成员变量分配内存。然后，程序流程进入到括号中，使用常规的赋值方式将值存储到内存中。因此，对于const数据成员，必须在执行到构造函数体之前，即创建对象时进行初始化。C++提供了一种特殊的语法来完成上述工作，它叫做成员初始化列表（member initializer list）。成员初始化列表由逗号分隔的初始化列表组成（前面带冒号）。它位于参数列表的右括号之后、函数体左括号之前。如果数据成员的名称为mdata，并需要将它初始化为val，则初始化器为mdata（val）。使用这种表示法，可以这样编写Queue的构造函数：

```c++
Queue::Queue(int qs) : qsize(qs) 
{
	front = rear = NULL;
	items = 0;
}
```

通常，初值可以是常量或构造函数的参数列表中的参数。这种方法并不限于初始化常量，可以将Queue构造函数写成如下所示：

```c++
Queue::Queue(int qs) : qsize(qs), front(NULL), rear(NULL), items(0) 
{
}
```

只有构造函数可以使用这种初始化列表语法。如上所示，对于const类成员，必须使用这种语法。另外，对于被声明为引用的类成员，也必须使用这种语法：

```c++
class Agency {...};
class Agent
{
private:
	Agency& belong;
}

Agent::Agent(Agency& a) : belong(a) {...}
```

这是因为引用与const数据类似，只能在被创建时进行初始化。对于简单数据成员（例如front和items），使用成员初始化列表和在函数体中使用赋值没有什么区别。然而，正如第14章将介绍的，对于本身就是类对象的成员来说，使用成员初始化列表的效率更高。

## 12.9 复习题

### 12.9.1 关于初始化

```c++
char* str;

// 1、 str没有初始化，不会默认初始化为NULL，如下bool为false
bool flag = str == NULL;

// 2、strcpy()接收的第一个参数必须是初始化后的，如下代码无法通过编译
strcpy(str, "abc");
```

## 12.10 编程练习

### 12.10.1 打印空指针

cout 不能打印空指针，如下代码运行时报错：

```c++
	char* str = 0;
	cout << str;
```

# 第13章 类继承

## 13.1 一个简单的基类

看如下两个构造函数写法的区别：

```c++
TableTennisPlayer::TableTennisPlayer(const string& fn,
	const string& ln, bool ht) : firstname(fn),
	lastname(ln), hasTable(ht) {}
	
TableTennisPlayer::TableTennisPlayer(const string& fn,
	const string& ln, bool ht) {
		firstname = fn;
		lastname = ln;
		hasTable = ht;
	}
```

第二种写法首先为firstname调用string的默认构造函数，再调用string的赋值运算符将firstname设置为fn，但初始化列表语法可减少一个步骤，它直接使用string的复制构造函数将firstname初始化为fn。

### 13.1.1 派生一个类

C++中类继承的写法：

```c++
class RatedPlayer : public TableTennisPlayer
{
...
}
```

冒号指出RatedPlayer类的基类是TableTennisplayer类。上述特殊的声明头表明TableTennisPlayer是一个公有基类，这被称为公有派生。派生类对象包含基类对象。使用公有派生，基类的公有成员将成为派生类的公有成员；基类的私有部分也将成为派生类的一部分，但只能通过基类的公有和保护方法访问（稍后将介绍保护成员）。

### 13.1.2 构造函数：访问权限的考虑

创建派生类对象时，程序首先创建基类对象。从概念上说，这意味着基类对象应当在程序进入派生类构造函数之前被创建。C++使用成员初始化列表语法来完成这种工作。例如，下面是第一个RatedPlayer构造函数的代码：

```c++
RatedPlayer::RatedPlayer(unsigned int r, const string& fn,
	const string& ln, bool ht) : TableTennisPlayer(fn, ln, ht)
{
	rating = r;
}
```

如果省略成员初始化列表，情况将如何呢？

```c++
RatedPlayer::RatedPlayer(unsigned int r, const string& fn,
	const string& ln, bool ht) // what if no initializer list?
{
	rating = r;
}
```

必须首先创建基类对象，如果不调用基类构造函数，程序将使用默认的基类构造函数，因此上述代码与下面等效：

```c++
RatedPlayer::RatedPlayer(unsigned int r, const string& fn,
	const string& ln, bool ht) // : TableTennisPlayer()
{
	rating = r;
}
```

除非要使用默认构造函数，否则应显式调用正确的基类构造函数。

下面来看第二个构造函数的代码：

```c++
RatedPlayer::RatedPlayer(unsigned int r, const TableTennisPlayer& tp)
	: TableTennisPlayer(tp)
{
	rating = r;
}
```

这里也将TableTennisPlayer的信息传递给了TableTennisPlayer构造函数：

```c++
TableTennisPlayer(tp)
```

由于tp的类型为TableTennisPlayer &，因此将调用基类的复制构造函数。基类没有定义复制构造函数，但第12章介绍过，如果需要使用复制构造函数但又没有定义，编译器将自动生成一个。在这种情况下，执行成员复制的隐式复制构造函数是合适的，因为这个类没有使用动态内存分配（string成员确实使用了动态内存分配，但本书前面说过，成员复制将使用string类的复制构造函数来复制string成员）。

如果愿意，也可以对派生类成员使用成员初始化列表语法。在这种情况下，应在列表中使用成员名，而不是类名。所以，第二个构造函数可以按照下述方式编写：

```c++
RatedPlayer::RatedPlayer(unsigned int r, const TableTennisPlayer& tp)
	: TableTennisPlayer(tp), rating(r)
{
}
```

**注意**

创建派生类对象时，程序首先调用基类构造函数，然后再调用派生类构造函数。基类构造函数负责初始化继承的数据成员；派生类构造函数主要用于初始化新增的数据成员。派生类的构造函数总是调用一个基类构造函数。可以使用初始化器列表语法指明要使用的基类构造函数，否则将使用默认的基类构造函数。
派生类对象过期时，程序将首先调用派生类析构函数，然后再调用基类析构函数。

### 13.1.4 派生类和基类之间的特殊关系

派生类与基类之间有一些特殊关系。其中之一是派生类对象可以使用基类的方法，条件是方法不是私有的：

```c++
RatedPlayer rplayer1(1140, "Malloy", "Duck", true);
rplayer1.Name();		// 派生类使用基类函数
```

另外两个重要的关系是：基类指针可以在不进行显式类型转换的情况下指向派生类对象；基类引用可以在不进行显式类型转换的情况下引用派生类对象：

```c++
RatedPlayer rplayer1(1140, "Malloy", "Duck", true);
TableTennisPlayer& rt = rplayer;
TableTennisPlayer* pt = &rplayer;
rt.Name();		// invoke Name() with reference
pt->Name();		// invoke Name() with pointer
```

然而，基类指针或引用只能用于调用基类方法，因此，不能使用rt或pt来调用派生类的ResetRanking方法。

通常，C++要求引用和指针类型与赋给的类型匹配，但这一规则对继承来说是例外。然而，这种例外只是单向的，不可以将基类对象和地址赋给派生类引用和指针：

```c++
TableTennisPlayer player("Betsy", "Bloop", true);
RatedPlayer& rr = player;		// NOT ALLOWED
RatedPlayer* pr = player;		// NOT ALLOWED
```

上述规则是有道理的。例如，如果允许基类引用隐式地引用派生类对象，则可以使用基类引用为派生类对象调用基类的方法。因为派生类继承了基类的方法，所以这样做不会出现问题。如果可以将基类对象赋给派生类引用，将发生什么情况呢？派生类引用能够为基对象调用派生类方法，这样做将出现问题。例如，将RatedPlayer::Rating( )方法用
于TableTennisPlayer对象是没有意义的，因为TableTennisPlayer对象没有rating成员。

如果基类引用和指针可以指向派生类对象，将出现一些很有趣的结果。其中之一是基类引用定义的函数或指针参数可用于基类对象或派生类对象。例如，在下面的函数中：

```c++
void Show(const TableTennisPlayer& rt)
{
	using std::cout;
	cout << "Name: ";
	rt.Name();
	cout << "\nTable: ";
	if (rt.HasTable())
		cout << "yes\n";
	else
		cout << "no\n";
}
```

形参rt是一个基类引用，它可以指向基类对象或派生类对象，所以可以在Show( )中使用TableTennisPlayer参数或Ratedplayer参数：

```c++
TableTennisPlayer player1("Tara", "Boomdea", false);
RatedPlayer rplayer1(1140, "Malloy", "Duck", true);
Show(player1);		// works with TableTennisPlayer argument
Show(rplayer1);		// works with RatedPlayer argument
```

对于形参为指向基类的指针的函数，也存在相似的关系。它可以使用基类对象的地址或派生类对象的地址作为实参：

```c++
void Wohs(const TableTennisPlayer* pt);		// function with pointer parameter
..
TableTennisPlayer player1("Tara", "Boomdea", false);
RatedPlayer rplayer1(1140, "Malloy", "Duck", true);
Wohs(&player1);			// works with TableTennisPlayer* argument
Wohs(&rplayer1);		// works with RatedPlayer* argument
```

引用兼容性属性也让您能够将基类对象初始化为派生类对象，尽管不那么直接。假设有这样的代码：

```c++
RatedPlayer olaf1(1840, "Olaf", "Loaf", true);
TableTennisPlayer olaf2(olaf1);
```

要初始化olaf2，匹配的构造函数的原型如下：

```c++
TableTennisPlayer(const RatedPlayer&);		// doesn't exist
```

类定义中没有这样的构造函数，但存在隐式复制构造函数：

```c++
// implicit copy constructor
TableTennisPlayer(const TableTennisPlayer&);
```

形参是基类引用，因此它可以引用派生类。这样，将olaf2初始化为olaf1时，将要使用该构造函数，它复制firstname、lastname和hasTable成员。换句话来说，它将olaf2初始化为嵌套在RatedPlayer对象olaf1中的TableTennisPlayer对象。

同样，也可以将派生对象赋给基类对象：

```c++
RatedPlayer olaf1(1840, "Olaf", "Loaf", true);
TableTennisPlayer winner;
winner = olaf1;		// assign derived to base object
```

在这种情况下，程序将使用隐式重载赋值运算符：

```c++
TableTennisPlayer& operator=(const TableTennisPlayer&) const;
```

基类引用指向的也是派生类对象，因此olaf1的基类部分被复制给winner。

## 13.2 继承：is-a关系

C++有3种继承方式：公有继承、保护继承和私有继承。

## 13.3 多态公有继承

RatedPlayer继承示例很简单。派生类对象使用基类的方法，而未做任何修改。然而，可能会遇到这样的情况，即希望同一个方法在派生类和基类中的行为是不同的。换句话来说，方法的行为应取决于调用该方法的对象。这种较复杂的行为称为多态——具有多种形态，即同一个方法的行为随上下文而异。有两种重要的机制可用于实现多态公有继承；

- 在派生类中重新定义基类的方法。
- 使用虚方法。

### 13.3.1 开发Brass类和BrassPlus类

先看如下程序：

```c++
// brass.h  -- bank account classes
#ifndef BRASS_H_
#define BRASS_H_
#include <string>
// Brass Account Class
class Brass
{
private:
	std::string fullName;
	long acctNum;
	double balance;
public:
	Brass(const std::string& s = "Nullbody", long an = -1,
		double bal = 0.0);
	void Deposit(double amt);
	virtual void Withdraw(double amt);
	double Balance() const;
	virtual void ViewAcct() const;
	virtual ~Brass() {}
};

//Brass Plus Account Class
class BrassPlus : public Brass
{
private:
	double maxLoan;
	double rate;
	double owesBank;
public:
	BrassPlus(const std::string& s = "Nullbody", long an = -1,
		double bal = 0.0, double ml = 500,
		double r = 0.11125);
	BrassPlus(const Brass& ba, double ml = 500,
		double r = 0.11125);
	virtual void ViewAcct()const;
	virtual void Withdraw(double amt);
	void ResetMax(double m) { maxLoan = m; }
	void ResetRate(double r) { rate = r; };
	void ResetOwes() { owesBank = 0; }
};

#endif // BRASS_H_
```

- Brass类和BrassPlus类都声明了ViewAcct( )和Withdraw( )方法，但BrassPlus对象和Brass对象的这些方法的行为是不同的；
- Brass类在声明ViewAcct( )和Withdraw( )时使用了新关键字virtual。这些方法被称为虚方法（virtual method）
- Brass类还声明了一个虚析构函数，虽然该析构函数不执行任何操作。

第一点介绍了声明如何指出方法在派生类的行为的不同。两个ViewAcct( )原型表明将有2个独立的方法定义。基类版本的限定名为Brass::ViewAcct( )，派生类版本的限定名为BrassPlus::ViewAcct()。程序将使用对象类型来确定使用哪个版本：

```c++
Brass dom("Dominic Banker", 11224, 4183.45);
BrassPlus dot("Dorothy Banker", 12118, 2592.00);
dom.ViewAcct();			// use Brass::viewAcct()
dot.ViewAcct();			// use BrassPlus::viewAcct()
```

同样，Withdraw( )也有2个版本，一个供Brass对象使用，另一个供BrassPlus对象使用。对于在两个类中行为相同的方法（如Deposit()和Balance( )），则只在基类中声明。

第二点（使用virtual）比第一点要复杂。如果方法是通过引用或指针而不是对象调用的，它将确定使用哪一种方法。如果没有使用关键字virtual，程序将根据引用类型或指针类型选择方法；如果使用了virtual，程序将根据引用或指针指向的对象的类型来选择方法。如果ViewAcct( )不是虚的，则程序的行为如下：

```c++
// behavior with non-virtual ViewAcct()
// method chosen according to reference type
Brass dom("Dominic Banker", 11224, 4183.45);
BrassPlus dot("Dorothy Banker", 12118, 2592.00);
Brass& b1_ref = dom;
Brass& b2_ref = dot;
b1_ref.viewAcct();			// use Brass::viewAcct()
b2_ref.viewAcct();			// use Brass::viewAcct()
```

引用变量的类型为Brass，所以选择了Brass::ViewAcct( )。使用Brass指针代替引用时，行为将与此类似。

如果ViewAcct( )是虚的，则行为如下：

```c++
// behavior with virtual ViewAcct()
// method chosen according to reference type
Brass dom("Dominic Banker", 11224, 4183.45);
BrassPlus dot("Dorothy Banker", 12118, 2592.00);
Brass& b1_ref = dom;
Brass& b2_ref = dot;
b1_ref.viewAcct();			// use Brass::viewAcct()
b2_ref.viewAcct();			// use BrassPlus::viewAcct()
```

这里两个引用的类型都是Brass，但b2_ref引用的是一个BrassPlus对象，所以使用的是BrassPlus::ViewAcct( )。使用Brass指针代替引用时，行为将类似。

#### 类实现

派生类并不能直接访问基类的私有数据，而必须使用基类的公有方法才能访问这些数据。访问的方式取决于方法。构造函数使用一种技术，而其他成员函数使用另一种技术。

派生类构造函数在初始化基类私有数据时，采用的是成员初始化列表语法。RatedPlayer类构造函数和BrassPlus构造函数都使用这种技术：

```c++
BrassPlus::BrassPlus(const string& s, long an, double bal,
	double ml, double r) : Brass(s, an, bal)
{
	maxLoan = ml;
    owesBank = 0.0;
    rate = r;
}

BrassPlus::BrassPlus(const Brass& ba, double ml, double r)
	: Brass(ba)		// uses implicit copy constructor
{
	maxLoan = ml;
	owesBank = 0.0;
	rate = r;
}
```

这几个构造函数都使用成员初始化列表语法，将基类信息传递给基类构造函数，然后使用构造函数体初始化BrassPlus类新增的数据项。

非构造函数不能使用成员初始化列表语法，但派生类方法可以调用公有的基类方法。例如，BrassPlus版本的ViewAcct( )核心内容如下（忽略了格式方面）:

```c++
// redefine how ViewAcct() works
void BrassPlus::ViewAcct() const
{
...
	Brass::ViewAcct();			// display base portion
	cout << "Maximum loan: $" << maxLoan << endl;
	cout << "Owed to bank: $" << owesBank << endl;
	cout << "Loan Rate: " <<`100 * rate << "%\n";
...
}
```

换句话说，BrassPlus::ViewAcct( )显示新增的BrassPlus数据成员，并调用基类方法Brass::ViewAcct( )来显示基类数据成员。在派生类方法中，标准技术是使用作用域解析运算符来调用基类方法。

代码必须使用作用域解析运算符。假如这样编写代码：

```c++
// redefine erroneously how ViewAcct() works
void BrassPlus::ViewAcct() const
{
...
	ViewAcct();		// oops! recursive(递归) call
...
}
```

如果代码没有使用作用域解析运算符，编译器将认为ViewAcct( )是BrassPlus::ViewAcct( )，这将创建一个不会终止的递归函数——这可不好。

#### 为何需要虚析构函数

```c++
class Brass
{
public:
	virtual ~Brass() {}
};
```

在程序清单13.10中，使用delete释放由new分配的对象的代码说明了为何基类应包含一个虚析构函数，虽然有时好像并不需要析构函数。如果析构函数不是虚的，则将只调用对应于指针类型的析构函数。对于程序清单13.10，这意味着只有Brass的析构函数被调用，即使指针指向的是一个BrassPlus对象。如果析构函数是虚的，将调用相应对象类型的析构函数。因此，如果指针指向的是BrassPlus对象，将调用BrassPlus的析构函数，然后自动调用基类的析构函数。因此，使用虚析构函数可以确保正确的析构函数序列被调用。对于程序清单13.10，这种正确的行为并不是很重要，因为析构函数没有执行任何操作。然而，如果BrassPlus包含一个执行某些操作的析构函数，则Brass必须有一个虚析构函数，即使该析构函数不执行任何操作。

## 13.4 静态联编和动态联编

程序调用函数时，将使用哪个可执行代码块呢？编译器负责回答这个问题。将源代码中的函数调用解释为执行特定的函数代码块被称为函数名联编（binding）。在C语言中，这非常简单，因为每个函数名都对应一个不同的函数。在C++中，由于函数重载的缘故，这项任务更复杂。编译器必须查看函数参数以及函数名才能确定使用哪个函数。然而，C/C++编译器可以在编译过程完成这种联编。在编译过程中进行联编被称为静态联编（static binding），又称为早期联编（early binding）。然而，虚函数使这项工作变得更困难。正如在程序清单13.10所示的那样，使用哪一个函数是不能在编译时确定的，因为编译器不知道用户将选择哪种类型的对象。所以，编译器必须生成能够在程序运行时选择正确的虚方法的代码，这被称为动态联编（dynamic binding），又称为晚期联编（late binding）。

### 13.4.1 指针和引用类型的兼容性

通常，C++不允许将一种类型的地址赋给另一种类型的指针，也不允许一种类型的引用指向另一种类型：

```c++
double x = 2.5;
int* pi = &x;	// invalid assignment, mismatched pointer types
long& rl = x;	// invalid assignment, mismatched reference type
```

然而，正如您看到的，指向基类的引用或指针可以引用派生类对象，而不必进行显式类型转换。例如，下面的初始化是允许的：

```c++
BrassPlus dilly("Annie Dill", 493222, 2000);
Brass* pb = &dilly;		// OK
Brass& rb = dilly;		// OK
```

将派生类引用或指针转换为基类引用或指针被称为向上强制转换（upcasting），这使公有继承不需要进行显式类型转换。该规则是is-a关系的一部分。BrassPlus对象都是Brass对象，因为它继承了Brass对象所有的数据成员和成员函数。所以，可以对Brass对象执行的任何操作，都适用于BrassPlus对象。因此，为处理Brass引用而设计的函数可以对BrassPlus对象执行同样的操作，而不必担心会导致任何问题。将指向对象的指针作为函数参数时，也是如此。向上强制转换是可传递的，也就是说，如果从BrassPlus派生出BrassPlusPlus类，则Brass指针或引用可以引用Brass对象、BrassPlus对象或BrassPlusPlus对象。

相反的过程——将基类指针或引用转换为派生类指针或引用——称为向下强制转换（downcasting）。如果不使用显式类型转换，则向下强制转换是不允许的。原因是is-a关系通常是不可逆的。派生类可以新增数据成员，因此使用这些数据成员的类成员函数不能应用于基类。例如，假设从Employee类派生出Singer类，并添加了表示歌手音域的数据成员和用于报告音域的值的成员函数range( )，则将range( )方法应用于Employee对象是没有意义的。但如果允许隐式向下强制转换，则可能无意间将指向Singer的指针设置为一个Employee对象的地址，并使用该指针来调用range( )方法（参见图13.4）。

对于使用基类引用或指针作为参数的函数调用，将进行向上转换。请看下面的代码段，这里假定每个函数都调用虚方法ViewAcct( )：

```c++
void fr(Brass& rb);		// uses rb.ViewAcct()
void fp(Brass* pb);		// uses pb->ViewAcct()
void fv(Brass b);		// uses b.ViewAcct()
int main()
{
	Brass b("Billy Bee", 123432, 10000.0);
	BrassPlus bp("Betty Beep", 232313, 12345.0);
	fr(b);		// uses Brass::ViewAcct()
	fr(bp);		// uses BrassPlus::ViewAcct()
	fp(b);		// uses Brass::ViewAcct()
	fp(bp);		// uses BrassPlus::ViewAcct()
	fv(b);		// uses Brass::ViewAcct()
	fv(bp);		// uses Brass::ViewAcct()
}
```

按值传递导致只将BrassPlus对象的Brass部分传递给函数fv( )。但随引用和指针发生的隐式向上转换导致函数fr( )和fp( )分别为Brass对象和BrassPlus对象使用Brass::ViewAcct( )和BrassPlus::ViewAcct( )。

### 13.4.2 虚成员函数和动态联编

在大多数情况下，动态联编很好，因为它让程序能够选择为特定类型设计的方法。因此，您可能会问：

- 为什么有两种类型的联编？
- 既然动态联编如此之好，为什么不将它设置成默认的？
- 动态联编是如何工作的？

#### 为什么有两种类型的联编以及为什么默认为静态联编

如果动态联编让您能够重新定义类方法，而静态联编在这方面很差，为何不摒弃静态联编呢？原因有两个——效率和概念模型。

首先来看效率。为使程序能够在运行阶段进行决策，必须采取一些方法来跟踪基类指针或引用指向的对象类型，这增加了额外的处理开销（稍后将介绍一种动态联编方法）。例如，如果类不会用作基类，则不需要动态联编。同样，如果派生类（如RatedPlayer）不重新定义基类的任何方法，也不需要使用动态联编。在这些情况下，使用静态联编更合理，效率也更高。由于静态联编的效率更高，因此被设置为C++的默认选择。Strousstrup说，C++的指导原则之一是，不要为不使用的特性付出代价（内存或者处理时间）。仅当程序设计确实需要虚函数时，才使用它们。

接下来看概念模型。在设计类时，可能包含一些不在派生类重新定义的成员函数。例如，Brass::Balance( )函数返回账户结余，不应该重新定义。不将该函数设置为虚函数，有两方面的好处：首先效率更高；其次，指出不要重新定义该函数。这表明，仅将那些预期将被重新定义的方法声明为虚的。

#### 虚函数的工作原理

C++规定了虚函数的行为，但将实现方法留给了编译器作者。不需要知道实现方法就可以使用虚函数，但了解虚函数的工作原理有助于更好地理解概念，因此，这里对其进行介绍。

通常，编译器处理虚函数的方法是：给每个对象添加一个隐藏成员。隐藏成员中保存了一个指向函数地址数组的指针。这种数组称为虚函数表（virtual function table，vtbl）。虚函数表中存储了为类对象进行声明的虚函数的地址。例如，基类对象包含一个指针，该指针指向基类中所有虚函数的地址表。派生类对象将包含一个指向独立地址表的指针。如果派生类提供了虚函数的新定义，该虚函数表将保存新函数的地址；如果派生类没有重新定义虚函数，该vtbl将保存函数原始版本
的地址。如果派生类定义了新的虚函数，则该函数的地址也将被添加到vtbl中（参见图13.5）。注意，无论类中包含的虚函数是1个还是10个，都只需要在对象中添加1个地址成员，只是表的大小不同而已。

![image-20230126170737977](D:\git\note_md_files\images\image-20230126170737977.png)

调用虚函数时，程序将查看存储在对象中的vtbl地址，然后转向相应的函数地址表。如果使用类声明中定义的第一个虚函数，则程序将使用数组中的第一个函数地址，并执行具有该地址的函数。如果使用类声明中的第三个虚函数，程序将使用地址为数组中第三个元素的函数。

总之，使用虚函数时，在内存和执行速度方面有一定的成本，包括：

- 每个对象都将增大，增大量为存储地址的空间；
- 对于每个类，编译器都创建一个虚函数地址表（数组）
- 对于每个函数调用，都需要执行一项额外的操作，即到表中查找地址。

虽然非虚函数的效率比虚函数稍高，但不具备动态联编功能。

### 13.4.3 有关虚函数注意事项

#### 析构函数

析构函数应当是虚函数，除非类不用做基类。例如，假设Employee是基类，Singer是派生类，并添加一个char *成员，该成员指向由new分配的内存。当Singer对象过期时，必须调用~Singer( )析构函数来释放内存。

请看下面的代码：

```c++
Employee* pe = new Singer;		// legal because Employee is base for Singer
...
delete pe;			// ~Employee() or ~Singer() ?
```

如果使用默认的静态联编，delete语句将调用~Employee( )析构函数。这将释放由Singer对象中的Employee部分指向的内存，但不会释放新的类成员指向的内存。但如果析构函数是虚的，则上述代码将先调用~Singer析构函数释放由Singer组件指向的内存，然后，调用～Employee( )析构函数来释放由Employee组件指向的内存。

这意味着，即使基类不需要显式析构函数提供服务，也不应依赖于默认构造函数，而应提供虚析构函数，即使它不执行任何操作：

```c++
virtual ~BaseClass() {}
```

顺便说一句，给类定义一个虚析构函数并非错误，即使这个类不用做基类；这只是一个效率方面的问题。

#### 重新定义将隐藏方法

假设创建了如下所示的代码：

```c++
class Dwelling
{
public:
	virtual void showperks(int a) const;
...
};

class Hovel : public Dwelling
{
public:
	virtual void showperks() const;
...
}
```

这将导致问题，可能会出现类似于下面这样的编译器警告：

```
Warning: Hovel::showperks(void) hides Dwelling::showperks(int)
```

也可能不会出现警告。但不管结果怎样，代码将具有如下含义：

```c++
Hovel trump;
trump.showperks();			// valid
trump.showperks(5);			// invalid
```

新定义将showperks( )定义为一个不接受任何参数的函数。重新定义不会生成函数的两个重载版本，而是隐藏了接受一个int参数的基类版本。总之，重新定义继承的方法并不是重载。如果在派生类中重新定义函数，将不是使用相同的函数特征标覆盖基类声明，而是隐藏同名的基类方法，不管参数特征标如何。

这引出了两条经验规则：第一，如果重新定义继承的方法，应确保与原来的原型完全相同，但如果返回类型是基类引用或指针，则可以修改为指向派生类的引用或指针（这种例外是新出现的）。这种特性被称为返回类型协变（covariance of return type），因为允许返回类型随类类型的变化而变化：

```c++
class Dwelling
{
public:
// a base method
	virtual Dwelling& build(int n);
...
};

class Hovel : public Dwelling
{
public:
// a derived method with a convariant return type
	virtual Hovel& build(int n);		// same function signature, 编译器不会告警
...
}
```

注意，这种例外只适用于返回值，而不适用于参数。

第二，如果基类声明被重载了，则应在派生类中重新定义所有的基类版本。

```c++
class Dwelling
{
public:
// three overloaded showperks()
	virtual void showperks(int a) const;
	virtual void showperks(double x) const;
	virtual void showperks() const;
...
};

class Hovel : public Dwelling
{
public:
// three redefined showperks()
	virtual void showperks(int a) const;
	virtual void showperks(double x) const;
	virtual void showperks() const;
...
}
```

如果只重新定义一个版本，则另外两个版本将被隐藏，派生类对象将无法使用它们。注意，如果不需要修改，则新定义可只调用基类版本：

```c++
void Hovel::showperks() const { Dwelling::showperks(); }
```

## 13.5 访问控制：protected

关键字protected与private相似，在类外只能用公有类成员来访问protected部分中的类成员。private和protected之间的区别只有在基类派生的类中才会表现出来。派生类的成员可以直接访问基类的保护成员，但不能直接访问基类的私有成员。因此，对于外部世界来说，保护成员的行为与私有成员相似；但对于派生类来说，保护成员的行为与公有成员相似。

例如，假如Brass类将balance成员声明为保护的：

```c++
class Brass
{
protected:
	double balance;
...
};
```

在这种情况下，BrassPlus类可以直接访问balance，而不需要使用Brass方法。例如，可以这样编写BrassPlus::Withdraw( )的核心代码：

```c++
void BrassPlus::Withdraw(double amt)
{
	if (amt < 0)
        cout << "Withdrawal amount must be positive; "
        	<< "withdrawal canceled.\n";
	else if (amt < balance) 	// access balance directly
        balance -= amt;
    eles if (amt <= balance + maxLoan - owesBank)
	{
		double advance = amt - balance;
		owesBank += advance * (1.0 + rate);
		cout << "Bank advance: $" << advance << endl;
		cout << "Finance charge: $" << advance * rate << endl;
		Deposit(advance);
		balance -= amt;
	}
	else
		cout << "Credit limit exceeded. Transaction cancelled.\n";
}
```

使用保护数据成员可以简化代码的编写工作，但存在设计缺陷。例如，继续以BrassPlus为例，如果balance是受保护的，则可以按下面的方式编写代码：

```c++
void BrassPlus::Reset(double amt)
{
	balance = amt;
}
```

Brass类被设计成只能通过Deposit( )和Withdraw( )才能修改balance。但对于BrassPlus对象，Reset( )方法将忽略Withdraw( )中的保护措施，实际上使balance成为公有变量。

最好对类数据成员采用私有访问控制，不要使用保护访问控制；同时通过基类方法使派生类能够访问基类数据。

## 13.6 抽象基类

至此，介绍了简单继承和较复杂的多态继承。接下来更为复杂的是抽象基类（abstract base class，ABC）。我们来看一些可使用ABC的编程情况。

```c++
class BaseEllipse // abstract base class
{
private:
	double x;
	double y;
	...
public:
	BaseEllipse(double x0 = 0, double y0 = 0) : x(x0), y(y0) {}
	virtual ~BaseEllipse() {}
	void Move(int nx, int ny) { x = nx; y = ny; }
	virtual double Area() const = 0;	// a pure-virtual function
}
```

当类声明中包含纯虚函数时，则不能创建该类的对象。这里的理念是，包含纯虚函数的类只用作基类。要成为真正的ABC，必须至少包含一个纯虚函数。原型中的=0使虚函数成为纯虚函数。这里的方法Area()没有定义，但C++甚至允许纯虚函数有定义。例如，也许所有的基类方法都与Move( )一样，可以在基类中进行定义，但您仍需要将这个类声明为抽象的。在这种情况下，可以将原型声明为虚的：

```c++
void Move(int nx, ny) = 0;
```

这将使基类成为抽象的，但您仍可以在实现文件中提供方法的定义：

```c++
void BaseEllipse::Move(int nx, ny) { x = nx; y = ny; }
```

## 13.7 继承和动态内存分配

继承是怎样与动态内存分配（使用new和delete）进行互动的呢？例如，如果基类使用动态内存分配，并重新定义赋值和复制构造函数，这将怎样影响派生类的实现呢？这个问题的答案取决于派生类的属性。如果派生类也使用动态内存分配，那么就需要学习几个新的小技巧。下面来看看这两种情况。

### 13.7.1 第一种情况：派生类不使用new

假设基类使用了动态内存分配：

```c++
// Base Class Using DMA
class baseDMA
{
private:
	char* label;
	int rating;
public:
	baseDMA(const char* l = "null", int r = 0);
	baseDMA(const baseDMA& rs);
	virtual ~baseDMA();
	baseDMA& operator=(const baseDMA& rs);
...
}
```

声明中包含了构造函数使用new时需要的特殊方法：析构函数、复制构造函数和重载赋值运算符。

现在，从baseDMA派生出lackDMA类，而后者不使用new，也未包含其他一些不常用的、需要特殊处理的设计特性：

```c++
// derived class without DMA
class lackDMA : public baseDMA
{
private:
	char color[40];
public:
...
}
```

是否需要为lackDMA类定义显式析构函数、复制构造函数和赋值运算符呢？不需要。

首先，来看是否需要析构函数。如果没有定义析构函数，编译器将定义一个不执行任何操作的默认析构函数。实际上，派生类的默认析构函数总是要进行一些操作：执行自身的代码后调用基类析构函数。因为我们假设lackDMA成员不需执行任何特殊操作，所以默认析构函数是合适的。

接着来看复制构造函数。第12章介绍过，默认复制构造函数执行成员复制，这对于动态内存分配来说是不合适的，但对于新的lacksDMA成员来说是合适的。因此只需考虑继承的baseDMA对象。要知道，成员复制将根据数据类型采用相应的复制方式，因此，将long复制到long中是通过使用常规赋值完成的；但复制类成员或继承的类组件时，则是使用该类的复制构造函数完成的。所以，lacksDMA类的默认复制构造函数使用显式baseDMA复制构造函数来复制lacksDMA对象的baseDMA部分。因此，默认复制构造函数对于新的lacksDMA成员来说是合适的，同时对于继承的baseDMA对象来说也是合适的。

对于赋值来说，也是如此。类的默认赋值运算符将自动使用基类的赋值运算符来对基类组件进行赋值。因此，默认赋值运算符也是合适的。

派生类对象的这些属性也适用于本身是对象的类成员。例如，第10章介绍过，实现Stock类时，可以使用string对象而不是char数组来存储公司名称。标准string类和本书前面创建的String类一样，也采用动态内存分配。现在，读者知道了为何这不会引发问题。Stock的默认复制构造函数将使用string的复制构造函数来复制对象的company成员；Stock的默认赋值运算符将使用string的赋值运算符给对象的company成员赋值；而Stock的析构函数（默认或其他析构函数）将自动调用string的析构函数。

### 13.7.2 第二种情况：派生类使用new

假设派生类使用了new：

```c++
// derived class with DMA
class hasDMA : public baseDMA
{
private:
	char* style;	//	use new in constructors
public:
...
};
```

在这种情况下，必须为派生类定义显式析构函数、复制构造函数和赋值运算符。下面依次考虑这些方法。

派生类析构函数自动调用基类的析构函数，故其自身的职责是对派生类构造函数执行工作的进行清理。因此，hasDMA析构函数必须释放指针style管理的内存，并依赖于baseDMA的析构函数来释放指针label管理的内存。

```c++
baseDMA::~baseDMA()		// takes care of baseDMA stuff
{
	delete[] label;
}

hasDMA::~hasDMA()		// takes care of hasDMA stuff
{
	delete[] style;
}
```

接下来看复制构造函数。BaseDMA的复制构造函数遵循用于char数组的常规模式，即使用strlen( )来获悉存储C-风格字符串所需的空间、分配足够的内存（字符数加上存储空字符所需的1字节）并使用函数strcpy( )将原始字符串复制到目的地：

```c++
baseDMA::baseDMA(const baseDMA& rs)
{
	label = new char[std::strlen(rs.label) + 1];
	std::strcpy(label, rs.label);
	rating = rs.rating;
}
```

hasDMA复制构造函数只能访问hasDMA的数据，因此它必须调用baseDMA复制构造函数来处理共享的baseDMA数据：

```c++
hasDMA::hasDMA(const hasDMA& hs)
	: baseDMA(hs)
{
	style = new char[std::strlen(hs.style) + 1];
    std::strcpy(style, hs.style);
}
```

需要注意的一点是，成员初始化列表将一个hasDMA引用传递给baseDMA构造函数。没有参数类型为hasDMA引用的baseDMA构造函数，也不需要这样的构造函数。因为复制构造函数baseDMA有一个baseDMA引用参数，而基类引用可以指向派生类型。因此，baseDMA复制构造函数将使用hasDMA参数的baseDMA部分来构造新对象的baseDMA部分。

接下来看赋值运算符。BaseDMA赋值运算符遵循下述常规模式：

```c++
baseDMA& baseDMA::operator=(const baseDMA& rs)
{
	if (this == &rs)
		return *this;
	delete[] label;
	label = new char[std::strlen(rs.label) + 1];
	std::strcpy(label, rs.label);
	rating = rs.rating;
	return *this;
}
```

由于hasDMA也使用动态内存分配，所以它也需要一个显式赋值运算符。作为hasDMA的方法，它只能直接访问hasDMA的数据。然而，派生类的显式赋值运算符必须负责所有继承的baseDMA基类对象的赋值，可以通过显式调用基类赋值运算符来完成这项工作，如下所示：

```c++
hasDMA& hasDMA::operator=(const hasDMA& hs)
{
	if (this == &hs)
		return *this;
	baseDMA::operator=(hs);		// copy base portion
	delete[] style;
	style = new char[std::strlen(hs.style) + 1];
	std::strcpy(style, hs.style);
	return *this;
}
```

下述语句看起来有点奇怪：

```c++
baseDMA::operator=(hs);
```

但通过使用函数表示法，而不是运算符表示法，可以使用作用域解析运算符。实际上，该语句的含义如下：

```c++
*this = hs;		// use baseDMA::operator=()
```

当然编译器将忽略注释，所以使用后面的代码时，编译器将使用hasDMA ::operator=( )，从而形成递归调用。使用函数表示法使得赋值运算符被正确调用。

总之，当基类和派生类都采用动态内存分配时，派生类的析构函数、复制构造函数、赋值运算符都必须使用相应的基类方法来处理基类元素。这种要求是通过三种不同的方式来满足的。对于析构函数，这是自动完成的；对于构造函数，这是通过在初始化成员列表中调用基类的复制构造函数来完成的；如果不这样做，将自动调用基类的默认构造函数。对于赋值运算符，这是通过使用作用域解析运算符显式地调用基类的赋值运算符来完成的。

### 13.7.3 使用动态内存分配和友元的继承示例

先看如下程序：

```c++
// dma.h  -- inheritance and dynamic memory allocation
#ifndef DMA_H_
#define DMA_H_
#include <iostream>

//  Base Class Using DMA
class baseDMA
{
private:
	char* label;
	int rating;

public:
	baseDMA(const char* l = "null", int r = 0);
	baseDMA(const baseDMA& rs);
	virtual ~baseDMA();
	baseDMA& operator=(const baseDMA& rs);
	friend std::ostream& operator<<(std::ostream& os,
		const baseDMA& rs);
};

class hasDMA :public baseDMA
{
private:
	char* style;
public:
	hasDMA(const char* s = "none", const char* l = "null",
		int r = 0);
	hasDMA(const char* s, const baseDMA& rs);
	hasDMA(const hasDMA& hs);
	~hasDMA();
	hasDMA& operator=(const hasDMA& rs);
	friend std::ostream& operator<<(std::ostream& os,
		const hasDMA& rs);
};

#endif
```

看下两个友元函数时如何实现的：

```c++
std::ostream& operator<<(std::ostream& os, const baseDMA& rs)
{
	os << "Label: " << rs.label << std::endl;
	os << "Rating: " << rs.rating << std::endl;
	return os;
}

std::ostream& operator<<(std::ostream& os, const hasDMA& hs)
{
	os << (const baseDMA&)hs;
	os << "Style: " << hs.style << std::endl;
	return os;
}
```

在上述代码中，需要注意的新特性是，派生类如何使用基类的友元。例如，请考虑下面这个hasDMA类的友元：

```c++
	friend std::ostream& operator<<(std::ostream& os,
		const hasDMA& rs);
```

作为hasDMA类的友元，该函数能够访问style成员。然而，还存在一个问题：该函数如不是baseDMA类的友元，那它如何访问成员lable和rating呢？答案是使用baseDMA类的友元函数operator<<( )。下一个问题是，因为友元不是成员函数，所以不能使用作用域解析运算符来指出要使用哪个函数。这个问题的解决方法是使用强制类型转换，以便匹配原型时能够选择正确的函数。因此，代码将参数const hasDMA &转换成类型为const baseDMA &的参数：

```c++
std::ostream& operator<<(std::ostream& os, const hasDMA& hs)
{
	os << (const baseDMA&)hs;
	os << "Style: " << hs.style << std::endl;
	return os;
}
```

## 13.8 类设计回顾

### 13.8.1 编译器生成的成员函数

#### 默认构造函数

假设Star是一个类，则下述代码需要使用默认构造函数：

```c++
Star rigel;		// create an object without explicit initialization

// 声明数组的同时会直接创建对象
Star pleiades[6];	// create an array of objects
```

#### 复制构造函数

在下述情况下，将使用复制构造函数：

- 将新对象初始化为一个同类对象；
- 按值将对象传递给函数；
- 函数按值返回对象；
- 编译器生成临时对象。

#### 赋值运算符

```c++
Star dogstart;
dogstart = "abc";
```

编译器不会生成将一种类型赋给另一种类型的赋值运算符。如果希望能够将字符串赋给Star对象，则方法之一是显式定义下面的运算符：

```c++
Star& Star::operator=(const char*) {...}
```

另一种方法是使用转换函数（参见下一节中的“转换”小节）将字符串转换成Star对象，然后使用将Star赋给Star的赋值函数。第一种方法的运行速度较快，但需要的代码较多，而使用转换函数可能导致编译器出现混乱。

第18章将讨论C++11新增的两个特殊方法：移动构造函数和移动赋值运算符。

### 13.8.2 其他的类方法

#### 转换函数

应理智地使用转换函数，仅当它们有帮助时才使用。另外，对于某些类，包含转换函数将增加代码的二义性。例如，假设已经为第11章的Vector类型定义了double转换，并编写了下面的代码：

```c++
Vector ius(6.0, 0.0);
Vector lux = ius + 20.2;		// ambiguous
```

编译器可以将ius转换成double并使用double加法，或将20.2转换成vector（使用构造函数之一）并使用vector加法。但除了指出二义性外，它什么也不做。

#### 按值传递对象与传递引用

通常，编写使用对象作为参数的函数时，应按引用而不是按值来传递对象。这样做的原因之一是为了提高效率。按值传递对象涉及到生成临时拷贝，即调用复制构造函数，然后调用析构函数。调用这些函数需要时间，复制大型对象比传递引用花费的时间要多得多。如果函数不修改对象，应将参数声明为const引用。

按引用传递对象的另外一个原因是，在继承使用虚函数时，被定义为接受基类引用参数的函数可以接受派生类。

#### 使用const

通常，可以将返回引用的函数放在赋值语句的左侧，这实际上意味着可以将值赋给引用的对象。但可以使用const来确保引用或指针返回的值不能用于修改对象中的数据：

```c++
const Stock& Stock::topval(const Stock& s) const
{
	if (s.total_val > total_val)
		return s;		// argument object
	else
		return *this;	// invoking object
}
```

该方法返回对this或s的引用。因为this和s都被声明为const，所以函数不能对它们进行修改，这意味着返回的引用也必须被声明为const。

注意，如果函数将参数声明为指向const的引用或指针，则不能将该参数传递给另一个函数，除非后者也确保了参数不会被修改。

### 13.8.3 公有继承的考虑因素

#### 什么不能被继承

构造函数是不能继承的，也就是说，创建派生类对象时，必须调用派生类的构造函数。然而，派生类构造函数通常使用成员初始化列表语法来调用基类构造函数，以创建派生对象的基类部分。如果派生类构造函数没有使用成员初始化列表语法显式调用基类构造函数，将使用基类的默认构造函数。在继承链中，每个类都可以使用成员初始化列表将信息传递给相邻的基类。C++11新增了一种让您能够继承构造函数的机制，但默认仍不继承构造函数。

析构函数也是不能继承的。然而，在释放对象时，程序将首先调用派生类的析构函数，然后调用基类的析构函数。如果基类有默认析构函数，编译器将为派生类生成默认析构函数。通常，对于基类，其析构函数应设置为虚的。

赋值运算符是不能继承的，原因很简单。派生类继承的方法的特征标与基类完全相同，但赋值运算符的特征标随类而异，这是因为它包含一个类型为其所属类的形参。赋值运算符确实有一些有趣的特征，下面介绍它们。

#### 赋值运算符

将派生类对象赋给基类对象将会如何呢？（注意，这不同于将基类引用初始化为派生类对象。）请看下面的例子：

```c++
Brass blips;		// base class
BrassPlus snips("Rafe Plosh", 91191, 3993.19, 600.0, 0.12);		// derived class
blips = snips;
```

这将使用哪个赋值运算符呢？赋值语句将被转换成左边的对象调用的一个方法：

```c++
blips.operator=(snips);
```

其中左边的对象是Brass对象，因此它将调用Brass ::operator=（const Brass &）函数。is-a关系允许Brass引用指向派生类对象，如Snips。赋值运算符只处理基类成员，所以上述赋值操作将忽略Snips的maxLoan成员和其他BrassPlus成员。总之，可以将派生对象赋给基类对象，但这只涉及基类的成员。

相反的操作将如何呢？即可以将基类对象赋给派生类对象吗？请看下面的例子：

```c++
Brass gp("Griff Hexbait", 21234, 1200);
BrassPlus temp;
temp = gp;
```

上述赋值语句将被转换为如下所示：

```c++
temp.operator=(gp);
```

左边的对象是BrassPlus对象，所以它调用BrassPlus::operator=（const BrassPlus &）函数。然而，派生类引用不能自动
引用基类对象，因此上述代码不能运行，除非有下面的转换构造函数：

```c++
BrassPlus(const Brass&);
```

与BrassPlus类的情况相似，转换构造函数可以接受一个类型为基类的参数和其他参数，条件是其他参数有默认值：

```c++
BrassPlus(const Brass& ba, double ml = 500, double r = 0.1);
```

如果有转换构造函数，程序将通过它根据gp来创建一个临时BrassPlus对象，然后将它用作赋值运算符的参数。

另一种方法是，定义一个用于将基类赋给派生类的赋值运算符：

```c++
BrassPlus& BrassPlus::operator=(const Brass&) {...}
```

该赋值运算符的类型与赋值语句完全匹配，因此无需进行类型转换。

总之，问题“是否可以将基类对象赋给派生对象？”的答案是“也许”。如果派生类包含了这样的构造函数，即对将基类对象转换为派生类对象进行了定义，则可以将基类对象赋给派生对象。如果派生类定义了用于将基类对象赋给派生对象的赋值运算符，则也可以这样做。如果上述两个条件都不满足，则不能这样做，除非使用显式强制类型转换。

#### 虚方法

请注意，不适当的代码将阻止动态联编。例如，请看下面的两个函数：

```c++
void show(const Brass& rba)
{
	rba.ViewAcct();
	cout << endl;
}

void inadequate(Brass ba)
{
	ba.ViewAcct();
	cout << endl;
}
```

第一个函数按引用传递对象，第二个按值传递对象。

现在，假设将派生类参数传递给上述两个函数：

```c++
BrassPlus buzz("Buzz Parsec", 00001111, 4300);
show(buzz);
inadequate(buzz);
```

show( )函数调用使rba参数成为BrassPlus对象buzz的引用，因此，rba.ViewAcct( )被解释为BrassPlus版本，正如应该的那样。但在inadequate( )函数中（它是按值传递对象的），ba是Brass（constBrass &）构造函数创建的一个Brass对象（自动向上强制转换使得构造函数参数可以引用一个BrassPlus对象）。因此，在inadequate( )中，ba.ViewAcct( )是Brass版本，所以只有buzz的Brass部分被显示。

# 第14章 C++中的代码重用

## 14.1 包含对象成员的类

### 14.1.1 valarray类简介

C++提供的数组类除了vector和array之外， 这里介绍一下valarray：

```c++
valarray<int> q_values;		// an array of int
valarray<double> weights;	// an array of double
```

```c++
// 一些例子
double gpa[5] = {3.1, 3.5, 3.8, 2.9, 3.3};
valarray<double> v1;		// an array of double, size 0
valarray<int> v2(8);		// an array of 8 int elements
valarray<int> v3(10, 8);	// an array of 8 int elements
							// each set to 10
valarray<double> v4(gpa, 4);	// an array of 4 elements
				// initialized to the first 4 elements of gpa
```

### 14.1.2 Student类的设计

![image-20230205213746805](D:\git\note_md_files\images\image-20230205213746805.png)

使用公有继承时，类可以继承接口，可能还有实现（基类的纯虚函数提供接口，但不提供实现）。获得接口是is-a关系的组成部分。而使用组合，类可以获得实现，但不能获得接口。不继承接口是has-a关系的组成部分。

### 14.1.3 Student类示例

C++包含让程序员能够限制程序结构的特性——使用explicit防止单参数构造函数的隐式转换，使用const限制方法修改数据，等等。这样做的根本原因是：在编译阶段出现错误优于在运行阶段出现错误。

#### 1．初始化被包含的对象

对于继承的对象，构造函数在成员初始化列表中使用类名来调用特定的基类构造函数。对于成员对象，构造函数则使用成员名。例如，请看程序清单14.3的最后一个构造函数：

```c++
Student(const char* str, const double* pd, int n)
	: name(str), scores(pd, n),  {}
```

因为该构造函数初始化的是成员对象，而不是继承的对象，所以在初始化列表中使用的是成员名，而不是类名。初始化列表中的每一项都调用与之匹配的构造函数，即name(str)调用构造函数string(const char *)，scores(pd, n)调用构造函数ArrayDb(const double *, int)。

如果不使用初始化列表语法，情况将如何呢？C++要求在构建对象的其他部分之前，先构建继承对象的所有成员对象。因此，如果省略初始化列表，C++将使用成员对象所属类的默认构造函数。

当初始化列表包含多个项目时，这些项目被初始化的顺序为它们被声明的顺序，而不是它们在初始化列表中的顺序。例如，假设Student构造函数如下：

```c++
Student(const char* str, const double* pd, int n)
	: scores(pd, n), name(str) {}
```

则name成员仍将首先被初始化，因为在类定义中它首先被声明。对于这个例子来说，初始化顺序并不重要，但如果代码使用一个成员的值作为另一个成员的初始化表达式的一部分时，初始化顺序就非常重要了。

## 14.2 私有继承

使用公有继承，基类的公有方法将成为派生类的公有方法。总之，派生类将继承基类的接口；这是is-a关系的一部分。使用私有继承，基类的公有方法将成为派生类的私有方法。总之，派生类不继承基类的接口。正如从被包含对象中看到的，这种不完全继承是has-a关系的一部分。

### 14.2.1 Student类示例（新版本）

要进行私有继承，请使用关键字private而不是public来定义类（实际上，private是默认值，因此省略访问限定符也将导致私有继承）。Student类应从两个类派生而来，因此声明将列出这两个类：

```c++
class Student : private std::string, private std::valarray<double>
{
public:
	...
};
```

#### 1．初始化基类组件

隐式地继承组件而不是成员对象将影响代码的编写，因为再也不能使用name和scores来描述对象了，而必须使用用于公有继承的技术。例如，对于构造函数，包含将使这样的构造函数：

```c++
Student(const char* str, const double* pd, int n)
	: name(str), scores(pd, n) {}		// 使用成员名
```

对于继承类，新版本的构造函数将使用成员初始化列表语法，它使用类名而不是成员名来标识构造函数：

```c++
Student(const char* str, const double* pd, int n)
	: std::string(str), ArrayDb(pd, n) {}	// 使用类名
```

在这里，ArrayDb是std::valarray<double>的别名。

#### 2．访问基类的方法

使用包含时将使用对象名来调用方法，而使用私有继承时将使用类名和作用域解析运算符来调用方法。

```c++
// 包含
double Student::Average() const
{
	if (Scores.size() > 0)
		return Scores.sum() / Scores.size();
	else
		return 0;
}
```

```c++
// 继承
double Student::Average() const
{
	if (ArrayDb::size() > 0)
		return ArrayDb::sum() / ArrayDb::size();
	else
		return 0;
}
```

#### 3．访问基类对象

使用作用域解析运算符可以访问基类的方法，但如果要使用基类对象本身，该如何做呢？

答案是使用强制类型转换。由于Student类是从string类派生而来的，因此可以通过强制类型转换，将Student对象转换为string对象；结果为继承而来的string对象。本书前面介绍过，指针this指向用来调用方法的对象，因此*this为用来调用方法的对象，在这个例子中，为类型为Student的对象。为避免调用构造函数创建新的对象，可使用强制类型转换来创建一个引用：

```c++
const string& Student::Name() const
{
	return (const string&)*this;
}
```

### 14.2.2 使用包含还是私有继承

由于既可以使用包含，也可以使用私有继承来建立has-a关系，那么应使用种方式呢？大多数C++程序员倾向于使用包含。首先，它易于理解。类声明中包含表示被包含类的显式命名对象，代码可以通过名称引用这些对象，而使用继承将使关系更抽象。其次，继承会引起很多问题，尤其从多个基类继承时，可能必须处理很多问题，如包含同名方法的独立的基类或共享祖先的独立基类。总之，使用包含不太可能遇到这样的麻烦。另外，包含能够包括多个同类的子对象。如果某个类需要3个string对象，可以使用包含声明3个独立的string成员。而继承则只能使用一个这样的对象（当对象都没有名称时，将难以区分）。

然而，私有继承所提供的特性确实比包含多。例如，假设类包含保护成员（可以是数据成员，也可以是成员函数），则这样的成员在派生类中是可用的，但在继承层次结构外是不可用的。如果使用组合将这样的类包含在另一个类中，则后者将不是派生类，而是位于继承层次结构之外，因此不能访问保护成员。但通过继承得到的将是派生类，因此它能够访问保护成员。

另一种需要使用私有继承的情况是需要重新定义虚函数。派生类可以重新定义虚函数，但包含类不能。使用私有继承，重新定义的函数将只能在类中使用，而不是公有的。

通常，应使用包含来建立has-a关系；如果新类需要访问原有类的保护成员，或需要重新定义虚函数，则应使用私有继承。

### 14.2.3 保护继承

保护继承是私有继承的变体。保护继承在列出基类时使用关键字protected：

```c++
class Student : protected std::string,
				protected std::valarray<double>
{...};
```

使用保护继承时，基类的公有成员和保护成员都将成为派生类的保护成员。和私有继承一样，基类的接口在派生类中也是可用的，但在继承层次结构之外是不可用的。当从派生类派生出另一个类时，私有继承和保护继承之间的主要区别便呈现出来了。使用私有继承时，第三代类将不能使用基类的接口，这是因为基类的公有方法在派生类中将变成私有方法；使用保护继承时，基类的公有方法在第二代中将变成受保护的，因此第三代派生类可以使用它们。

表14.1总结了公有、私有和保护继承。隐式向上转换（implicit upcasting）意味着无需进行显式类型转换，就可以将基类指针或引用指向派生类对象。

| 特征             | 公有继承             | 保护继承               | 私有继承             |
| ---------------- | -------------------- | ---------------------- | -------------------- |
| 公有成员变成     | 派生类的公有成员     | 派生类的保护成员       | 派生类的私有成员     |
| 保护成员变成     | 派生类的保护成员     | 派生类的保护成员       | 派生类的私有成员     |
| 私有成员变成     | 只能通过基类接口访问 | 只能通过基类接口访问   | 只能通过基类接口访问 |
| 能否隐式向上转换 | 是                   | 是（但只能在派生类中） | 否                   |

### 14.2.4 使用using重新定义访问权限

假设要让基类的方法在派生类外面可用，方法之一是定义一个使用该基类方法的派生类方法。例如，假设希望Student类能够使用valarray类的sum( )方法，可以在Student类的声明中声明一个sum( )方法，然后像下面这样定义该方法：

```c++
double Student::sum() const		// public Student method
{
	return std::valarray<double>::sum();	// use privately-inherited method
}
```

另一种方法是，将函数调用包装在另一个函数调用中，即使用一个using声明（就像名称空间那样）来指出派生类可以使用特定的基类成员，即使采用的是私有派生。例如，假设希望通过Student类能够使用valarray的方法min( )和max( )，可以在studenti.h的公有部分加入如下using声明：

```c++
class Student : private std::string, private std::valarray<double>
{
...
public:
	using std::valarray<double>::min;
	using std::valarray<double>::max;
}
```

上述using声明使得valarray<double>::min( )和valarray<double>::max( )可用，就像它们是Student的公有方法一
样：

```c++
cout << "high score: " << ada[i].max() << endl;
```

注意，using声明只使用成员名——没有圆括号、函数特征标和返回类型。例如，为使Student类可以使用valarray的operator 方法，只需在Student类声明的公有部分包含下面的using声明：

```c++
using std::valarray<double>::operator[];
```

这将使两个版本（const和非const）都可用。

## 14.3 多重继承

MI描述的是有多个直接基类的类。与单继承一样，公有MI表示的也是is-a关系。例如，可以从Waiter类和Singer类派生出SingingWaiter类：

```c++
class SingingWaiter : public Waiter, public Singer {...};
```

请注意，必须使用关键字public来限定每一个基类。这是因为，除非特别指出，否则编译器将认为是私有派生：

```c++
class SingingWaiter : public Waiter, Singer {...};		// Singer is a private base
```

MI可能会给程序员带来很多新问题。其中两个主要的问题是：从两个不同的基类继承同名方法；从两个或更多相关基类那里继承同一个类的多个实例。为解决这些问题，需要使用一些新规则和不同的语法。因此，与使用单继承相比，使用MI更困难，也更容易出现问题。

下面来看一个例子，并介绍有哪些问题以及如何解决它们。要使用MI，需要几个类。我们将定义一个抽象基类Worker，并使用它派生出Waiter类和Singer类。然后，便可以使用MI从Waiter类和Singer类派生出SingingWaiter类（参见图14.3）。这里使用两个独立的派生来使基类（Worker）被继承，这将导致MI的大多数麻烦。

![image-20230205220431314](D:\git\note_md_files\images\image-20230205220431314.png)

这种设计看起来是可行的：使用Waiter指针来调用Waiter::Show()和Waiter::Set( )；使用Singer指针来调用Singer::Show( )和Singer::Set( )。然后，如果添加一个从Singer和Waiter类派生出的SingingWaiter类后，将带来一些问题。具体地说，将出现以下问题。

- 有多少Worker？
- 哪个方法？

### 14.3.1 有多少Worker

假设首先从Singer和Waiter公有派生出SingingWaiter：

```c++
class SingWaiter : public Singer, public Waiter {...};
```

因为Singer和Waiter都继承了一个Worker组件，因此SingingWaiter将包含两个Worker组件（参见图14.4）。

正如预期的，这将引起问题。例如，通常可以将派生类对象的地址赋给基类指针，但现在将出现二义性：

```c++
SingingWaiter ed;
Worker* pw = &ed;	// 二义性
```

通常，这种赋值将把基类指针设置为派生对象中的基类对象的地址。但ed中包含两个Worker对象，有两个地址可供选择，所以应使用类型转换来指定对象：

```c++
Worker* pw1 = (Waiter *) &ed;	// the Worker in Waiter
Worker* pw2 = (Singer *) &ed;	// the Worker in Singer
```

![image-20230205220820329](D:\git\note_md_files\images\image-20230205220820329.png)

#### 1．虚基类

虚基类使得从多个类（它们的基类相同）派生出的对象只继承一个基类对象。例如，通过在类声明中使用关键字virtual，可以使Worker被用作Singer和Waiter的虚基类（virtual和public的次序无关紧要）

```c++
class Singer : virtual public Worker {...}
class Waiter : public virtual Worker {...}
```

然后，可以将SingingWaiter类定义为：

```c++
class SingingWaiter : public Singer, public Waiter {...};
```

现在，SingingWaiter对象将只包含Worker对象的一个副本。从本质上说，继承的Singer和Waiter对象共享一个Worker对象，而不是各自引入自己的Worker对象副本（请参见下图）。因为SingingWaiter现在只包含了一个Worker子对象，所以可以使用多态。

![image-20230205221058156](D:\git\note_md_files\images\image-20230205221058156.png)

#### 2．新的构造函数规则

使用虚基类时，需要对类构造函数采用一种新的方法。对于非虚基类，唯一可以出现在初始化列表中的构造函数是即时基类构造函数。但这些构造函数可能需要将信息传递给其基类。例如，可能有下面一组构造函数：

```c++
class A
{
	int a;
public:
	A (int n = 0) {a = n;}
	...
};

class B : public A
{
	int b;
public:
	B(int m = 0, int n = 0) : A(n) {b = m;}
	...
};

class C : public B
{
	int c;
public:
	C(int q = 0, int m = 0, int n = 0) : B(m, n) {c = q;}
	...
};
```

C类的构造函数只能调用B类的构造函数，而B类的构造函数只能调用A类的构造函数。这里，C类的构造函数使用值q，并将值m和n传递给B类的构造函数；而B类的构造函数使用值m，并将值n传递给A类的构造函数。

如果Worker是虚基类，则这种信息自动传递将不起作用。例如，对于下面的MI构造函数：

```c++
SingingWaiter(const Worker & wk, int p = 0, int v = Singer::other)
		: Waiter(wk, p), Singer(wk, v) {} 	//有缺陷的
```

存在的问题是，自动传递信息时，将通过2条不同的途径（Waiter和 Singer）将wk传递给Worker对象。为避免这种冲突，C++在基类是虚拟的时，禁止信息通过中间类自动传递给基类。因此，上述构造函数将初始化成员panache和 voice，但wk参数中的信息将不会传递给子对象Waiter。不过，编译器必须在构造派生对象之前构造基类对象组件；在上述情况下，编译器将使用Worker 的默认构造函数。

如果不希望默认构造函数来构造虚基类对象，则需要显式地调用所需的基类构造函数。因此，构造函数应该是这样：

```c++
SingingWaiter(const Worker & wk, int p = 0, int v = Singer::other)
		: Worker(wk), Waiter(wk, p), Singer(wk, v) {}
```

上述代码将显式地调用构造函数worker ( const Worker &)。请注意，这种用法是合法的，对于虚基类，必须这样做；但对于非虚基类，则是非法的。

### 14.3.2 哪个方法

除了修改类构造函数规则外，Ml通常还要求调整其他代码。假设要在SingingWaiter类中扩展Show()方法。因为SingingWaiter对象没有新的数据成员，所以可能会认为它只需使用继承的方法即可。这引出了第一个问题。假设没有在SingingWaiter类中重新定义Show()方法，并试图使用SingingWaiter对象调用继承的 Show()方法：

```c++
SingingWaiter newhire ("Elise Hawks", 2005, 6, soprano);
newhire.Show(); 		//有歧义的
```

对于单继承，如果没有重新定义Show()，则将使用最近祖先中的定义。而在多重继承中，每个直接祖先都有一个 Show()函数，这使得上述调用是二义性的。

可以使用作用域解析操作符来澄清编程者的意图：

```c++
SingingWaiter newhire ("Elise Hawks", 2005, 6, soprano);
newhire.Singer::Show(); //使用Singer的
```

不过，更好的方法是在. SingingWaiter中重新定义Show()，并指出要使用哪个Show()。例如，如果希望SingingWaiter对象使用Singer 版本的 Show()，则可以这样做：

```c++
void SingingWaiter::Show() 
{
	Singer::Show();
}
```

对于单继承来说，让派生方法调用基类的方法是可以的。例如，假设HeadWaiter类是从Waiter类派生而来的，则可以使用下面的定义序列，其中每个派生类使用其基类显示信息，并添加自己的信息：

```c++
void Worker::Show() const
{
	cout << "Name: " << fullname << endl;
	cout << "Employee ID: " << id << endl;
}

void Waiter::Show() const
{
	Worker::Show();
	cout << "Panache rating: " << panache << "\n";
}

void HeadWaiter::Show() const
{
	Waiter::Show();
	cout << "Presence rating: " << presence << "\n";
}
```

不过，这种递增的方式对SingingWaiter范例无效。方法：

```c++
void SingingWaiter::Show()
{
	Singer::Show();
}
```

可以通过同时调用Waiter版本的Show()来补救：

```c++
void SingingWaiter::Show()
{
	Singer::Show();
	Waiter::Show();
}
```

这将显示姓名和 ID两次，因为 Singer::Show()和 Waiter::Show()都调用了Worker::Show()。

如何解决呢?一种办法是使用模块化方式，而不是递增方式，即提供一个只显示Worker组件的方法和一个只显示Waiter组件或Singer组件（而不是Waiter和 Worker组件〉的方法。然后，在SingingWaiter::Show()方法中将组件组合起来。例如，可以这样做：

```c++
void Worker::Data() const
{
	cout << "Name: " << fullname << endl;
	cout << "Employee ID: " << id << endl;
}

void Waiter::Data() const
{
	cout << "Panache rating: " << panache << endl;
}

void Singer::Data() const
{
	cout << "Vocal range: " << pv[voice] << endl;
}

void SingingWaiter::Show() const
{
	cout << "Category: singing waiter\n";
	Worker::Data();
	Data();
}
```

另一种办法是将所有的数据组件都设置为保护的，而不是私有的，不过使用保护方法（而不是保护数据）将可以更严格地控制对数据的访问。

下面介绍其他一些有关MI的问题。

#### 1．混合使用虚类和非虚基类

再来看一下通过多种途径继承一个基类的派生类的情况。如果基类是虚基类，派生类将包含基类的一个子对象；如果基类不是虚基类，派生类将包含多个子对象。当虚基类和非虚基类混合时，情况将如何呢？例如，假设类B被用作类C和D的虚基类，同时被用作类X和Y的非虚基类，而类M是从C、D、X和Y派生而来的。在这种情况下，类M从虚派生祖先（即类C和D）那里共继承了一个B类子对象，并从每一个非虚派生祖先（即类X和Y）分别继承了一个B类子对象。因此，它包含三个B类子对象。当类通过多条虚途径和非虚途径继承某个特定的基类时，该类将包含一个表示所有的虚途径的基类子对象和分别表示各条非虚途径的多个基类子对象。

#### 2．虚基类和支配

使用虚基类将改变C++解析二义性的方式。使用非虚基类时，规则很简单。如果类从不同的类那里继承了两个或更多的同名成员(数据或方法)，则使用该成员名时，如果没有用类名进行限定，将导致二义性。但如果使用的是虚基类，则这样做不一定会导致二义性。在这种情况下，如果某个名称优先于(dominates )其他所有名称，则使用它时，即便不使用限定符，也不会导致二义性。

那么，一个成员名如何优先于另一个成员名呢？派生类中的名称优先于直接或间接祖先类中的相同名称。例如，在下面的定义中：

```c++
class B
{
public:
	short q();
	...
};

class C : virtual public B
{
public:
	long q();
	int omb()
	...
};

class D : public C
{
	...
};

class E: virtual public B
{
private:
	int omb()
	...
};

class F : public D, public E
{
	...
};
```

类C中的q()定义优先于类B中的q()定义，因为类C是从类B派生而来的。因此，F中的方法可以使用q()来表示C::q()。而任何一个omb()定义都不优先于其他omb()定义，因为C和E都不是对方的基类。所以，在F中使用非限定的omb()将导致二义性。

虚拟二义性规则与访问规则无关，也就是说，即使E::omb()是私有的，不能在F类中直接访问，但使用omb()仍将导致二义性。同样，即使C::q()是私有的，它也将优先于D::q()。在这种情况下，可以在类F中调用B::q()，但如果不限定q()，则将意味着要调用不可访问的C::q()。

## 14.4 类模板

### 14.4.1 定义类模板

以第10章的 Stack 类为基础来建立模板。原来的类声明如下：

```c++
typedef unsigned long Item;

class Stack{
private:
	enum {MAX=10};		// constant specific to class
	Item items[MAX];		// holds stack items
	int top;						// index for top stakc item
public:
	Stack();
	bool isemtpy() const;
	bool isfull() const;
	// push() returns fasle if stack already is full, true otherwise
	bool push(const Item & item);		// add item to stack
	// pop() returns false if stack already is empty, true otherwise
	bool pop(Item & item);		// pop top into item
};
```

采用模板时，将使用模板定义替换 Stack 声明，使用模板成员函数替换 Stack 的成员函数。和模板函数一样，模板类以下面这样的代码开头：

```c++
template <class Type>
```

关键字 template 告诉编译器，将要定义一个模板。尖括号中的内容相当于函数的参数列表。

较新的C++实现允许在这种情况下使用不太容易混淆的关键字typename代替class：

```c++
template<typename Type>	// newer choice
```

下面的程序列出了类模板和成员函数模板。知道这些模板不是类和成员函数定义至关重要。它们是 C++ 编译器指令，说明了如何生成类和成员函数定义。模板的具体实现——如用来处理 string 对象的栈类——被称为实例化（instantiation）或具体化（specialization）。不能将模板成员函数放在独立的实现文件中（以前，C++标准确实提供了关键字 export，让您能够将模板成员函数放在独立的实现文件中，但支持该关键字的编译器不多；C++11不再这样使用关键字export，而将其保留用于其他用途）。由于模板不是函数，它们不能单独编译。模板必须与特定的模板实例化请求一起使用。为此，最简单的方法是将所有模板信息放在一个头文件中，并在要使用这些模板的文件中包含该头文件。

```c++
// stacktp.h -- a stack template
#ifndef STACKTP_H_
#define STACKTP_H_

template<class Type>
class Stack{
private:
    enum {MAX = 10};    // constant specific to class
    Type items[MAX];    // holds stack items
    int top;            // index for top stack item
public:
    Stack();
    bool isempty();
    bool isfull();
    bool push(const Type & item);   // add item to stack
    bool pop(Type & item);          // pop top into item
};

template<class Type>
Stack<Type>::Stack(){
    top = 0;
}

template<class Type>
bool Stack<Type>::isempty(){
    return top == 0;
}

template<class Type>
bool Stack<Type>::isfull(){
    return top==MAX;
}

template<class Type>
bool Stack<Type>::push(const Type & item){
    if(top<MAX){
        items[top++] = item;
        return true;
    }
    else{
        return false;
    }
}

template<class Type>
bool Stack<Type>::pop(Type & item){
    if (top > 0){
        item = items[--top];
        return true;
    }
    else{
        return false;
    }
}
#endif
```

### 14.4.2 使用模板类

仅在程序包含模板并不能生成模板类，而必须请求实例化。为此，需要声明一个类型为模板类的对象，方法是使用所需的具体类型替换泛型名。例如，下面的代码创建两个栈，一个用于存储 int，另一个用于存储 string 对象:

```c++
Stack<int> kernels;				// create a stack of ints
Stack<string> colonels;			// create a stack of string objects
```

注意，必须显式地提供所需的类型，这与常规的函数模板是不同的，因为编译器可以根据函数的参数类型来确定要生成哪种函数：

```c++
template<class T>
void simple(T t) { cout << t << '\n'; }
...
simple(s);		// generate void simple(int)
simple("two")	// generate void simple(const char *)
```

### 14.4.4 数组模板示例和非类型参数

首先介绍一个允许指定数组大小的简单数组模板。一种方法是在类中使用动态数组和构造函数参数来提供元素数目，最后一个版本的 Stack 模板采用的就是这种方法。另一种方法是使用模板参数来提供常规数组的大小，C++11 新增的模板 array 就是这样做的。下面的程序演示了如何做。

```c++
// arraytp.h --  Array Template
#ifndef ARRAYTP_H_
#define ARRAYTP_H_

#include<iostream>
#include<cstdlib>

template <class T, int n>
class ArrayTP{
private:
    T ar[n];
public:
    ArrayTP() {};
    explicit ArrayTP(const T & v);
    virtual T & operator[](int i);
    virtual T operator[](int i) const;
};

template <class T, int n>
ArrayTP<T,n>::ArrayTP(const T & v){
    for (int i = 0; i < n; i++){
        ar[i] = v;
    }
}

template <class T, int n>
T & ArrayTP<T,n>::operator[](int i){
    if (i < 0 || i >= n){
        std::cerr << "Error in array limits: " << i
            << " is out of range\n";
        std::exit(EXIT_FAILURE);
    }
    return ar[i];
}

template <class T, int n>
T ArrayTP<T,n>::operator[](int i) const{
    if (i < 0 || i >= n){
        std::cerr << "Error in array limits: " << i
            << " is out of range\n";
    }
    return ar[i];
}
#endif
```

请注意以上程序的模板头：

```c++
template<class T, int n>
```

关键字 class（或在这种上下文中等价的关键字 typename）指出 T 为类型参数，int 指出 n 的类型为 int。这种参数（指定特殊的类型而不是用作泛型名）称为非类型（non-type）或表达式（expression）参数。假设有下面的声明：

```c++
ArrayTP<double, 12> eggweights;
```

这将导致编译器定义名为 ArrayTP<double,12>的类，并创建一个类型为 ArrayTP<double, 12> 的 eggweight 对象。定义类时，编译器将使用 double 替换 T，使用12替换n。

表达式参数有一些限制。表达式参数可以是整型、枚举、引用或指针。因此，double m 是不合法的。但 double & rm 和 double *pm 是合法的。另外，模板代码不能修改参数的值，也不能使用参数的地址。所以，在 ArrayTP 模板中不能使用诸如 n++ 和 &n 等表达式。另外，实例化模板时，用作表达式参数的值必须是常量表达式。

与 Stack 中使用的构造函数方法相比，这种改变数组大小的方法有一个优点。构造函数方法使用的是通过 new 和 delete 管理的堆内存，而表达式参数方法使用的是为自动变量维护的内存栈。这样，执行速度将更快，尤其是在使用了很多小型数组时。

表达式参数方法的主要缺点是，每种数组大小都将生成自己的模板。也就是说，下面的声明将生成两个独立的类声明：

```c++
ArrayTP<double, 12> eggweights;
ArrayTP<double, 13> donuts;
```

但下面的声明只生成一个类声明，并将数组大小信息传递给类的构造函数：

```c++
Stack<int> eggs(12);
Stack<int> dunkers(13);
```

另一个区别是，构造函数方法更通用，这是因为数组大小是作为类成员（而不是硬编码）存储在定义中的。这样可以将一种尺寸的数组赋给另一种尺寸的数组，也可以创建允许数组大小可变的类。

### 14.4.5 模板多功能性

#### 1．递归使用模板

一个模板多功能性的例子是，可以递归使用模板。例如，对于前面的数组模板定义，可以这样使用它：

```c++
ArrayTP<ArrayTP<int, 5>, 10> twodee;
```

这使得 twodee 是一个包含 10 个元素的数组，其中每个元素都是一个包含5个int元素的数组.与之等价的常规数组声明如下：

```c++
int twodee[10][5];
```

#### 3．默认类型模板参数

类模板的另一项新特性是，可以为参数提供默认值：

```c++
template <class T1, class T2 = int> class Topo { ... };
```

这样，如果省略 T2 的值，编译器将使用 int:

```c++
Topo<double, double> m1;		// T1 is double, T2 is double
Topo<double>m2;					// T1 is double, T2 is int
```

第 16 章将讨论的标准模板库经常使用该特性，将默认类型设置为类。

虽然可以为类模板类型参数提供默认值，但不能为函数模板参数提供默认值。然而，可以为非类型参数提供默认值，这对于类模板和函数模板都是适用的。

### 14.4.6 模板的具体化

类模板与函数模板很相似，因为可以有隐式实例化、显式实例化和显式具体化，它们统称为具体化（specialization）。模板以泛型的方式描述类，而具体化是使用具体的类型生成类声明。

#### 1．隐式实例化

到目前为止，本章所有的模板示例使用的都是隐式实例化（implicit instantiation），即它们声明一个或多个对象，指出所需的类型，而编译器使用通用模板提供的处方生成具体的类定义：

```c++
ArrayTP<int, 100> stuff;		// implicit instantiation
```

编译器在需要对象之前，不会生成类的隐式实例化：

```c++
ArrayTP<double, 30> * pt;		// a pointer, no object needed yet
pt = new ArrayTP<double, 30>;	// now an object is needed
```

第二条语句导致编译器生成类定义，并根据该定义创建一个对象。

#### 2．显式实例化

当使用关键字 template 并指出所需类型来声明类时，编译器将生成类声明的显式实例化（explicit instantiation）。声明必须位于模板定义所在的名称空间中。例如，下面的声明将 ArrayTP<string, 100> 声明为一个类：

```c++
template class ArrayTP<string, 100>;	// generate ArrayTP<string, 100> class
```

在这种情况下，虽然没有创建或提及类对象，编译器也将生成类声明（包括方法定义）。和隐式实例化一样，也将根据通用模板来生成具体化。

#### 3．显式具体化

显式具体化（explicit specialiaztion）是特定类型（用于替换模板中的泛型）的定义。有时候，可能需要在为特殊类型实例化时，对模板进行修改，使其行为不同。在这种情况下，可以创建显式具体化。例如，假设已经为用于表示排序后数组的类（元素在加入时被排序）定义了一个模板：

```c++
template<typename T>
class SortedArray {
	... // details omitted
}
```

另外，假设模板使用>运算符来对值进行比较。对于数字，这管用；如果 T 表示一种类，则只要定义了 T::operator>() 方法，这也管用；但如果T是由 const char * 表示的字符串，这将不管用。实际上，模板倒是可以正常工作，但字符串将按地址（按照字母顺序）排序。这要求类定义使用 strcmp()，而不是>来对值进行比较。在这种情况下，可以提供一个显式模板具体化，这将采用为具体类型定义的模板，而不是为泛型定义的模板。当具体化模板和通用模板都与实例化请求匹配时，编译器将使用具体化版本。

具体化类模板定义的格式如下：

```c++
template <> class Classname<specialized-type-name> { ... };
```

要使用新的表示法提供一个专供 const char * 类型使用的 SortedArray 模板，可以使用类似于下面的代码：

```c++
template <> class SortedArray<const char *>{
	... // details omitted
};
```

其中的实现代码将使用 strcmp() 而不是> 来比较数组值。现在，当请求 const char * 类型的 SortedArray 模板时，编译器将使用上述专用的定义，而不是通用的模板定义：

```c++
SortedArray<int> scores;			// use general definition
SortedArray<const char *> dates;	// use specialized definition
```

#### 4．部分具体化

C++ 还允许部分具体化（partial specialization），即部分限制模板的通用性。例如，部分具体化可以给类型参数之一指定具体的类型：

```c++
// general template
template <class T1, class T2> class Pair { ... };
// spcialization with T2 set to int
template <class T1> class Pair<T1, int> { ... };
```

关键字 template 后面的 <> 声明的是没有被具体化的类型参数。因此，上述第二个声明将 T2 具体化为 int，但 T1 保持不变。注意，如果指定所有的类型，则 <> 内将为空，这将导致显式具体化：

```c++
// specialiaztion with T1 and T2 set to int
template<> class Pair <int, int> { ... };
```

如果有多个模板可供选择，编译器将使用具体化程度最高的模板。给定上述三个模板，情况如下：

```c++
Pair<double, double> p1;	// use general Pair template
Pair<double, int> p2;		// use Pair<T1, int> partial specializtion
Pair<int, int> p3;			// use Pair<int, int> explicit specialization
```

也可以通过为指针提供特殊版本来部分具体化现有的模板：

```c++
template<class T>		// general version
class Feeb { ... };
template<class T*>		// pointer partial specialization
class Feeb { ... };		// modified code
```

如果没有进行部分具体化，则第二个声明将使用通用模板，将 T 转换为 char* 类型。如果进行了部分具体化，则第二个声明将使用具体化模板，将T转换为char。

部分具体化特性使得能够设置各种限制。例如，可以这样做：

```c++
// general template
template <class T1, class T2, class T3> class Trio { ... };
// specialization with T3 set to T2
template<class T1, class T2> class Trio<T1,T2,T2> { ... };
// specialzation with T2 and T3 set to T1*
template<class T1> class Trio<T1, T1*, T1*> { .. };
```

给定上述声明，编译器将作出如下选择：

```c++
Trio<int, short, char *> t1;		// use general template
Trio<int, short>;					// use Trio<T1, T2, T2>
Trio<char, char*, char*> t3;		// use Trio<T1, T1*, T1*>
```

### 14.4.7 成员模板

模板可用作结构、类或模板类的成员。要完全实现 STL 的设计，必须使用这项特性。下面的程序是一个简短的模板类示例，该模板类将另一个模板类和模板函数作为其成员。

```c++
// tempmemb.cpp -- template members
#include<iostream>

using std::cout;
using std::endl;

template<typename T>
class beta{
private:
    template <typename V> // nested template class member
    class hold{
    private:
        V val;
    public:
        hold(V v = 0) : val(v) {}
        void show() const { cout << val << endl; }
        V Value() const { return val; }
    };
    hold<T> q;      // template object
    hold<int> n;    // template object
public:
    beta( T t, int i) : q(t), n(i) {}
    template<typename U>    // template method
    U blab(U u, T t) { return (n.Value()+q.Value())*u / t;}
    void Show() const { q.show(); n.show(); }
};

int main(){
    beta<double> guy(3.5, 3);
    cout << "T was set to double\n";
    guy.Show();
    cout << "V was set to T, which is double, then V was set to int\n";

    cout << guy.blab(10, 2.3) << endl;
    cout << "U was set to int\n";

    cout << guy.blab(10.0, 2.3) << endl;
    cout << "U was set to double\n";

    cout << "Done\n";

    return 0;
}
```

在上面的程序中，hold模板是在私有部分声明的，因此只能在 beta 类中访问它。beta 类使用 hold 模板声明了两个数据成员：

```c++
hold<T> q;		// template object
hold<int> n;	// template object
```

n 是基于 int 类型的 hold 对象，而 q 成员是基于 T 类型（beta模板参数）的hold 对象。在 main() 中，下述声明使得T表示的是 double，因此q的类型为 hold<double>：

```c++
beta<double> guy(3.5, 3);
```

blab() 方法的 U 类型由该方法被调用时的参数值显式确定，T 类型由对象的实例化类型确定。在这个例子中，guy的声明将 T 的类型设置为 double，而下述方法调用的第一个参数将 U 的类型设置为 int（参数10对应的类型）：

```c++
cout << guy.blab(10, 2.5) << endl;
```

### 14.4.8 将模板用作参数

您知道，模板可以包含类型参数（如typename T）和非类型参数（如 int n）。模板还可以包含本身就是模板的参数，这种参数是模板新增的特性，用于实现 STL。

在下面的程序中，开头的代码如下：

```c++
template <template <typename T> class Thing>
class Crab{
...
};
```

模板参数是 template<typename T> class Thing，其中 template<typename T> class 是类型，Thing 是参数。这意味着什么呢？假设有下面的声明：

```c++
Crab<King> legs;
```

为使上述声明被接受，模板参数King必须是一个模板类，其声明与模板参数Thing的声明匹配：

```c++
template<typename T>
class King {
	...
};
```

在下面的程序中，Crab 的声明声明了两个对象：

```c++
Thing<int> s1;
Thing<double> s2;
```

前面的 legs 声明将用 King<int> 替换 Thing<int>，用King<double> 替换 Thing<double>。然而，下面的程序清单包含下面的声明：

```c++
Crab<Stack> nebula;
```

因此，Thing<int> 将被实例化为 Stack<int>，而 Thing<double> 将被实例化为 Stack<double>。总之，模板参数 Thing 将被替换为声明 Crab 对象时被用作模板参数的模板类型。

Crab 类的声明对 Thing 代表的模板类做了另外 3 个假设，即这个类包含一个 push() 方法，包含一个 pop() 方法，且这些方法有特定的接口。Crab 类可以使用任何与 Thing 类型声明匹配，并包含方法 push() 和 pop() 的模板类。本章恰巧有一个这样的类——stacktp.h 中定义的 Stack 模板，因此这个例子将使用它。

```c++
// tempparm.cpp - template as parameters
#include <iostream>
#include"14.13_stacktp.h"

template <template <typename T> class Thing>
class Crab{
private:
    Thing<int> s1;
    Thing<double> s2;
public:
    Crab() { };
    // assume the thing class push() and pop() members
    bool push(int a, double x) { return s1.push(a) && s2.push(x); }
    bool pop(int &a, double & x) { return s1.pop(a) && s2.pop(x); }
};

int main() {
    using std::cout;
    using std::cin;
    using std::endl;
    Crab<Stack> nebula; // Stack must match template <typename T> class thing
    int ni;
    double nb;
    cout << "Enter int double pairs, such as 4 3.5 (0 0 to end):\n";
    while (cin >> ni >> nb && ni > 0 && nb > 0){
        if (!nebula.push(ni,nb)){
            break;
        }
    }
    while (nebula.pop(ni,nb))
        cout << ni << ", " << nb << endl;
    cout << "Done.\n";

    return 0;
}
```

可以混合使用模板参数和常规参数，例如，Crab 类的声明可以像下面这样打头：

```c++
template<template <typename T> class Thing, typename U, typename V>
class Crab{
private:
	Thing<U> s1;
	Thing<V> s2;
	...
```

现在，成员 s1 和 s2 可存储的数据类型为泛型，而不是用硬编码指定的类型。这要求将程序中的 nebula 的声明修改成下面这样：

```c++
Crab<Stack, int, double> nebula;		// T = Stack, U = int, V =  double
```

模板参数T表示一种模板类型，而类型参数U和V表示非模板类型。

### 14.4.9 模板类和友元

模板类声明也可以有友元。模板的友元分3类：

- 非模板友元；
- 约束（bound）模板友元，即友元的类型取决于类被实例化时的类型；
- 非约束（unbound）模板友元，即友元的所有具体化都是类的每一个具体化的友元。

#### 1．模板类的非模板友元函数

在模板类中将一个常规函数声明为友元：

```c++
template <class T>
class HasFriend{
public:
	friend void counts();		// friend to all HasFriend instantiations
	...
};
```

上述声明使 counts() 函数成为模板所有实例化的友元。例如，它将是类HasFriend<int>和HasFriend<string>的友元。

counts() 函数不是通过对象调用的（它是友元，不是成员函数），也没有对象参数，那么它如何访问 HasFriend 对象呢？有很多种可能性。它可以访问全局对象；可以使用全局指针访问非全局对象；可以创建自己的对象；可以访问独立于对象的模板类的静态数据成员。

假设要为友元函数提供模板类参数，可以如下所示来进行友元声明吗？

```c++
friend void report(HasFriend &);	// possible?
```

答案是不可以。原因是不存在 HasFriend 这样的对象，而只有特定的具体化，如 HasFriend<short> 。要提供模板类参数，必须指明具体化。例如，可以这样做：

```c++
template<class T>
class HasFriend{
	friend void report(HasFriend<T> &);	// bound template friend
	...
};
```

也就是说，带 HasFriend<int> 参数的 report() 将成为 HasFriend<int> 类的友元。同样，带 HasFriend<double> 参数的 report() 将是 report() 的一个重载版本——它是 HasFriend<double> 类的友元。

注意，report() 本身并不是模板函数，而只是使用一个模板作参数。这意味着必须为要使用的友元定义显式具体化：

```c++
void report(HasFriend<short> &) { ... };		// explicit specialization for short
void report(HasFriend<int> & ) { ... };			// explicit specialization for int
```

下面的程序说明了上面几点。HasFriend 模板有一个静态成员 ct。这意味着这个类的每一个特定的具体化都将有自己的静态成员。count() 方法是所有 HasFriend 具体化的友元，它报告两个特定的具体化（HasFriend<int> 和 HasFriend<double>）的 ct 的值。该程序还提供两个 report() 函数，它们分别是某个特定 HasFriend 具体化的友元。

```c++
// frnd2tmp.cpp -- template class with non-template friends

#include <ctime>
#include<iostream>
using std::cout;
using std::endl;

template<typename T>
class HasFriend{
private:
    T item;
    static int ct;
public:
    HasFriend(const T & i) : item(i) { ct++; }
    ~HasFriend() { ct--; }
    friend void counts();
    friend void report(HasFriend<T> &); // template parameter
};

// each specialization has its own static data member
template<typename T>
int HasFriend<T>::ct = 0;

// non-template friend to all HasFriend<T> classes
void counts(){
    cout << "int count: " << HasFriend<int>::ct << "; ";
    cout << "double count: " << HasFriend<double>::ct << endl;
}

// non-template friend to the HasFriend<int> class
void report(HasFriend<int> & hf){
    cout << "HasFriend<int>: " << hf.item << endl;
}

// non-template friend to the HasFriend<double class
void report(HasFriend<double> & hf){
    cout << "HasFriend<double>: " << hf.item << endl;
}

int main(){
    cout << "No objects declared: ";
    counts();
    HasFriend<int>hfil(10);
    cout <<"After hfil declared: ";
    counts();
    HasFriend<int>hfil2(20);
    cout << "After hfil2 declared: ";
    counts();
    HasFriend<double>hfdb(10.5);
    cout << "After hfdb declared: ";
    counts();
    report(hfil);
    report(hfil2);
    report(hfdb);

    return 0;
}
```

#### 2．模板类的约束模板友元函数

可以修改前一个示例，使友元函数本身成为模板。具体地说，为约束模板友元作准备，要使类的每一个具体化都获得与友元匹配的具体化。这比非模板友元要复杂些，包含以下 3 步。

首先，在类定义的前面声明每个模板函数。

```c++
template<typename T> void counts();
template<typename T> void report(T &);
```

然后，在函数中再次将模板声明为友元。这些语句根据类模板参数的类型声明具体化：

```c++
template <typename TT>
class HasFriendT{
...
	friend void counts<TT>();
	friend void report<>(HasFriendT<TT> &);
};
```

声明中的<>指出这是模板具体化。对于 report()，<> 可以为空，因为可以从函数参数推断出如下模板类型参数：

```c++
HasFriendT<TT>
```

然而，也可以使用：

```c++
report<HasFriendT<TT> > (HasFriendT<TT> &)
```

但counts() 函数没有参数，因此必须使用模板参数语法（<TT>）来指明其具体化。还需要注意的是，TT 是 HasFriendT 类的参数类型。

同样，理解这些声明的最佳方式也是设想声明一个具体化的对象时，它们将变成什么样。例如，假设声明了这样一个对象：

```c++
HasFriendT<int> squack;
```

编译器将用 int 替换 TT，并生成下面的类定义：

```c++
class HasFriendT<int>{
...
	friend void counts<int>();
	friend void reports<>(HasFriendT<int> &);
};
```

基于 TT 的具体化将变为 int，基于 HasFriend<TT> 的具体化将变为 HasFriend<int>。因此，模板具体化 counts<int>() 和 report<HasFriendT<int>() 被声明为 HasFriendT<int>类的友元。

程序必须满足的第三个要求是，为友元提供模板定义。下面的程序说明了这3个方面。请注意，上一个程序包含1个count()函数，它是所有 HasFriend 类的友元；而下面的程序包含两个 count() 函数，它们分别是某个被实例化的类类型的友元。因为 count() 函数调用没有可被编译器用来推断出所需具体化的函数参数，所以这些调用使用 count<int> 和 count<double>() 指明具体化。但对于 report() 调用，编译器可以从参数类型推断出要使用的具体化。使用<>格式也能获得同样的效果：

```c++
report<HasFriendT<int> >(hfil2);		// same as report(hfil2);
```

```c++
// tmp2tmp.cpp -- template friends to a template class
#include<iostream>
using std::cout;
using std::endl;

// template prototypes
template<typename T> void counts();
template<typename T> void report(T &);

// template class
template <typename TT>
class HasFriendT{
private:
    TT item;
    static int ct;
public:
    HasFriendT(const TT & i) : item(i) { ct++; }
    ~HasFriendT() { ct--;}
    friend void counts<TT>();
    friend void report<>(HasFriendT<TT> &);
};

template<typename T>
int HasFriendT<T>::ct = 0;

// template friend functions definitions
template<typename T>
void counts(){
    cout << "template size: " << sizeof(HasFriendT<T>) << ": ";
    cout << "template counts(): " << HasFriendT<T>::ct << endl;
}

template<typename T>
void report(T & hf){
    cout << hf.item << endl;
}

int main(){
    counts<int>();
    HasFriendT<int> hfil(10);
    HasFriendT<int> hfil2(20);
    HasFriendT<double> hfdb(10.5);
    report(hfil); // generate report(HasFriendT<int> &)
    report(hfil2);  // generate report(HasFriendT<int> &)
    report(hfdb);   // generate report(HasFriendT<double> &)
    cout << "counts<int>() output: \n";
    counts<int>();
    cout << "counts<double>() output: \n";
    counts<double>();

    return 0;
}
```

#### 3．模板类的非约束模板友元函数

前一节中的约束模板友元函数是在类外面声明的模板的具体化。int 类具体化获得 int 函数具体化，依此类推。通过在类内部声明模板，可以创建非约束友元函数，即每个函数具体化都是每个类具体化的友元。对于非约束友元，友元模型类型参数与模板类类型参数是不同的：

```c++
template<typename T>
class ManyFriend{
...
	template <typename C, typename D> friend void show2(C &, D & );
};
```

下面的程序是一个使用非约束友元的例子。其中，函数调用 show2(hfi1, hfi2) 与下面的具体化匹配：

```c++
void show2<ManyFriend<int> & , ManyFriend<int> &>
					(ManyFriend<int> & c, ManyFriend<int> & d);
```

因为它是所有ManyFriend具体化的友元，所以能够访问所有具体化的item成员，但它只访问了ManyFriend<int>对象。

同样，show2(hfd, hfi2)与下面具体化匹配：

```c++
void show2<ManyFriend<double> & , ManyFriend<int> &>
					(ManyFriend<double> & c, ManyFriend<int> & d);
```

它也是所有 ManyFriend 具体化的友元。并访问了 ManyFriend<int> 对象的 item 成员和 ManyFriend<double> 对象的 item 成员。

```c++
// manyfrnd.cpp -- unbound template friend to a template class
#include<iostream>

using std::cout;
using std::endl;

template<typename T>
class ManyFriend{
private:
    T item;
public:
    ManyFriend(const T & i) : item(i) { }
    template <typename C, typename D> friend void show2(C &, D &);
};

template <typename C, typename D> void show2(C & c, D & d){
    cout << c.item << ", " << d.item << endl;
}

int main(){
    
    ManyFriend<int> hfil(10);
    ManyFriend<int> hfil2(20);
    ManyFriend<double> hfdb(10.5);
    cout << "hfil, hfil2: ";
    show2(hfil, hfil2);
    cout << "hfdb, hfil2: ";
    show2(hfdb, hfil2);

    return 0;
}
```

### 14.4.10 模板别名（C++11）

如果能为类型指定别名，将很方便，在模板设计中尤其如此。可使用 typedef 为模板具体化指定别名：

```c++
// define three typedef aliases
typedef std::array<double, 12> arrd;
typedef std::array<int, 12> arri;
typedef std::array<std::string, 12> arrst;
arrd gallons;	// gallons is type std::array<double, 12>
arri days;		// days is type std::array<int, 12>
arrst months;	// months is type std::array<std::string, 12>
```

C++11 新增了一项功能——使用模板提供一系列别名，如下所示：

```c++
template<typename T>
	using arrtype = std::array<T, 12>;	// template to create multiple aliases
```

这将 arrtype 定义为一个模板别名，可使用它来指定类型，如下所示：

```c++
arrtype<double> gallons;		// gallons is type std::array<double, 12>
arrtype<int> days;				// days is type std::array<int, 12>
arrtype<std::string> months;	// months is type std::array<std::string, 12>
```

总之，arrtype<T> 表示类型 std::array<T, 12>。

C++ 11 允许将语法 using = 用于非模板。用于非模板时，这种语法与常规 typedef 等价：

```c++
typedef const char * pc1;		// typedef syntax
using pc2 = const char *;		// using = syntax
typedef const int *(*pa1)[10];	// typedef syntax
using pa2 = const int *(*)[10];	// using = syntax
```

# 第15章 友元、异常和其他

## 15.1 友元

### 15.1.1 友元类

看一个友元类的例子：

```c++
// tv.h -- Tv and Remote classes
#ifndef TV_H_
#define TV_H_

class Tv
{
public:
	friend class Remote;   // Remote can access Tv private parts
	enum { Off, On };
	enum { MinVal, MaxVal = 20 };
	enum { Antenna, Cable };
	enum { TV, DVD };

	Tv(int s = Off, int mc = 125) : state(s), volume(5),
		maxchannel(mc), channel(2), mode(Cable), input(TV) {}
	void onoff() { state = (state == On) ? Off : On; }
	bool ison() const { return state == On; }
	bool volup();
	bool voldown();
	void chanup();
	void chandown();
	void set_mode() { mode = (mode == Antenna) ? Cable : Antenna; }
	void set_input() { input = (input == TV) ? DVD : TV; }
	void settings() const; // display all settings
private:
	int state;             // on or off
	int volume;            // assumed to be digitized
	int maxchannel;        // maximum number of channels
	int channel;           // current channel setting
	int mode;              // broadcast or cable
	int input;             // TV or DVD
};

class Remote
{
private:
	int mode;              // controls TV or DVD
public:
	Remote(int m = Tv::TV) : mode(m) {}
	bool volup(Tv& t) { return t.volup(); }
	bool voldown(Tv& t) { return t.voldown(); }
	void onoff(Tv& t) { t.onoff(); }
	void chanup(Tv& t) { t.chanup(); }
	void chandown(Tv& t) { t.chandown(); }
	void set_chan(Tv& t, int c) { t.channel = c; }
	void set_mode(Tv& t) { t.set_mode(); }
	void set_input(Tv& t) { t.set_input(); }
};
#endif
```

友元声明可以位于公有、私有或保护部分，其所在的位置无关紧要。友元类可以访问关联类的私有或保护内容。在上例中，大多数类方法都被定义为内联的。除构造函数外，所有的Romote方法都将一个Tv对象引用作为参数，这表明遥控
器必须针对特定的电视机。

这个练习的主要目的在于表明，类友元是一种自然用语，用于表示一些关系。如果不使用某些形式的友元关系，则必须将Tv类的私有部分设置为公有的，或者创建一个笨拙的、大型类来包含电视机和遥控器。这种解决方法无法反应这样的事实，即同一个遥控器可用于多台电视机。

### 15.1.2 友元成员函数

从上一个例子中的代码可知，大多数Remote方法都是用Tv类的公有接口实现的。这意味着这些方法不是真正需要作为友元。事实上，唯一直接访问Tv成员的Remote方法是Remote::set_chan( )，因此它是唯一需要作为友元的方法。确实可以选择仅让特定的类成员成为另一个类的友元，而不必让整个类成为友元，但这样做稍微有点麻烦，必须小心排列各种声明和定义的顺序。下面介绍其中的原因。

让Remote::set_chan( )成为Tv类的友元的方法是，在Tv类声明中将其声明为友元：

```c++
class Tv {
	friend void Remote::set_chan(Tv & t, int c);
}
```

然而，要使编译器能够处理这条语句，它必须知道Remote的定义。否则，它无法知道Remote是一个类，而set_chan是这个类的方法。这意味着应将Remote的定义放到Tv的定义前面。Remote的方法提到了Tv对象，而这意味着Tv定义应当位于Remote定义之前。避开这种循环依赖的方法是，使用前向声明（forward declaration）。为此，需要在Remote定义的前面插入下面的语句：

```c++
class Tv;		// forward declaration
```

这样，排列次序应如下：

```c++
class Tv; // forward declaration
class Remote{...};
class Tv{...};
```

能否像下面这样排列呢？

```c++
class Remote ;  //forward declaration
class Tv {...};
class Remote {...};
```

答案是不能。原因在于，在编译器在Tv类的声明中看到Remote的一个方法被声明为Tv类的友元之前，应该先看到Remote类的声明和set_chan( )方法的声明。

还有一个麻烦。程序清单15.1的Remote声明包含了内联代码，例如：

```c++
void onoff(Tv & t) {t.onoff();}
```

由于这将调用Tv的一个方法，所以编译器此时必须已经看到了Tv类的声明，这样才能知道Tv有哪些方法，但正如看到的，该声明位于Remote声明的后面。这种问题的解决方法是，使Remote声明中只包含方法声明，并将实际的定义放在Tv类之后。这样，排列顺序将如下：

```c++
class Tv;  // forward declaration
class Remote {...};  // Tv-using methods as prototypes only  只包含方法的声明
class Tv {...};
// put Remote method definitions here  定义在这里写
```

Remote方法的原型与下面类似：

```C++
void onoff(Tv & t);
```

检查该原型时，所有的编译器都需要知道Tv是一个类，而前向声明提供了这样的信息。当编译器到达真正的方法定义时，它已经读取了Tv类的声明，并拥有了编译这些方法所需的信息。通过在方法定义中使用inline关键字，仍然可以使其成为内联方法。程序清单15.4列出了修订后的头文件。

```C++
// tvfm.h -- Tv and Remote classes using a friend member
#ifndef TVFM_H_
#define TVFM_H_

// 与15.2和15.3一起编译
class Tv;                       // forward declaration

class Remote
{
public:
	enum State { Off, On };
	enum { MinVal, MaxVal = 20 };
	enum { Antenna, Cable };
	enum { TV, DVD };
private:
	int mode;
public:
	Remote(int m = TV) : mode(m) {}
	bool volup(Tv& t);         // prototype only
	bool voldown(Tv& t);
	void onoff(Tv& t);
	void chanup(Tv& t);
	void chandown(Tv& t);
	void set_mode(Tv& t);
	void set_input(Tv& t);
	void set_chan(Tv& t, int c);
};

class Tv
{
public:
	friend void Remote::set_chan(Tv& t, int c);
	enum State { Off, On };
	enum { MinVal, MaxVal = 20 };
	enum { Antenna, Cable };
	enum { TV, DVD };

	Tv(int s = Off, int mc = 125) : state(s), volume(5),
		maxchannel(mc), channel(2), mode(Cable), input(TV) {}
	void onoff() { state = (state == On) ? Off : On; }
	bool ison() const { return state == On; }
	bool volup();
	bool voldown();
	void chanup();
	void chandown();
	void set_mode() { mode = (mode == Antenna) ? Cable : Antenna; }
	void set_input() { input = (input == TV) ? DVD : TV; }
	void settings() const;
private:
	int state;
	int volume;
	int maxchannel;
	int channel;
	int mode;
	int input;
};

// Remote methods as inline functions
inline bool Remote::volup(Tv& t) { return t.volup(); }
inline bool Remote::voldown(Tv& t) { return t.voldown(); }
inline void Remote::onoff(Tv& t) { t.onoff(); }
inline void Remote::chanup(Tv& t) { t.chanup(); }
inline void Remote::chandown(Tv& t) { t.chandown(); }
inline void Remote::set_mode(Tv& t) { t.set_mode(); }
inline void Remote::set_input(Tv& t) { t.set_input(); }
inline void Remote::set_chan(Tv& t, int c) { t.channel = c; }
#endif
```

如果在tv.cpp和use_tv.cpp中包含tvfm.h而不是tv.h，程序的行为与前一个程序相同，区别在于，只有一个Remote方法是Tv类的友元，而在原来的版本中，所有的Remote方法都是Tv类的友元。图15.1说明了这种区别。

![image-20230218202414994](D:\git\note_md_files\images\image-20230218202414994.png)

本书前面介绍过，内联函数的链接性是内部的，这意味着函数定义必须在使用函数的文件中。在这个例子中，内联定义位于头文件中，因此在使用函数的文件中包含头文件可确保将定义放在正确的地方。也可以将定义放在实现文件中，但必须删除关键字inline，这样函数的链接性将是外部的。

顺便说一句，让整个Remote类成为友元并不需要前向声明，因为友元语句本身已经指出Remote是一个类：

```c++
friend class Remote;
```

### 15.1.3 其他友元关系

一些Remote方法能够像前面那样影响Tv对象，而一些Tv方法也能影响Remote对象。这可以通过让类彼此成为对方的友元来实现，即除了Remote是Tv的友元外，Tv还是Remote的友元。这种方案与下面类似：

```c++
class Tv
{
    friend class Remote;
public:
    void buzz(Remote& r);
    //...
};
class Remote
{
    friend class Tv;
public:
    void volup(Tv& t) { t.volup(); }
}
inline void Tv::buzz(Remote& r)
{
}
```

由于Remote的声明位于Tv声明的后面，所以可以在类声明中定义Remote::volup( )，但Tv::buzz( )方法必须在Tv声明的外部定义，使其位于Remote声明的后面。如果不希望buzz( )是内联的，则应在一个单独的方法定义文件中定义它。

### 15.1.4 共同的友元

需要使用友元的另一种情况是，函数需要访问两个类的私有数据。从逻辑上看，这样的函数应是每个类的成员函数，但这是不可能的。它可以是一个类的成员，同时是另一个类的友元，但有时将函数作为两个类的友元更合理。例如，假定有一个Probe类和一个Analyzer类，前者表示某种可编程的测量设备，后者表示某种可编程的分析设备。这两个类都有内部时钟，且希望它们能够同步，则应该包含下述代码行：

```c++
class Analyzer;
class Probe
{
    friend void sync(Analyzer& a, const Probe& p);   // sync a to p
    friend void sync(Probe& p, const Analyzer& a);   // sync p to a
    // ...
};
class Analyzer
{
    friend void sync(Analyzer& a, const Probe& p);   // sync a to p
    friend void sync(Probe& p, const Analyzer& a);   // sync p to a
    // ...
};
//define the friend functions
inline void sync(Analyzer& a, const Probe& p)
{
    // ...
}
inline void sync(Probe& p, const Analyzer& a)
{
    // ...
}
```

前向声明使编译器看到Probe类声明中的友元声明时，知道Analyzer是一种类型。

## 15.2 嵌套类

对类进行嵌套与包含并不同。包含意味着将类对象作为另一个类的成员，而对类进行嵌套不创建类成员，而是定义了一种类型，该类型仅在包含嵌套类声明的类中有效。

对类进行嵌套通常是为了帮助实现另一个类，并避免名称冲突。Queue类示例（第12章的程序清单12.8）嵌套了结构定义，从而实现了一种变相的嵌套类：

```c++
class Queue
{
private:
   //classs scope definitions
　　//Node is a nested structure definition local to this class
　　struct node {Item item; struct Node * next;};
}
```

将Node变成嵌套类：

```c++
class Queue
{
    // Node is a nested calss definition local to this class
    class Node
    {
    public:
        Item item;
        Node * next;
        Node(const Item & i):item(i),next(0) { }
    };
    ...
}
```

对应的enqueue()：

```c++
bool Queue::enqueue(const Item & item)
{
    if(isfull())
        return false;
    Node * add = new Node(item);  //create, initialize node
//on failure,new throws std::bad_alloc exception
...
}
```

这个例子在类声明中定义了构造函数。假设想在方法文件中定义构造函数，则定义必须指出Node类是在Queue类中定义的。这是通过使用两次作用域解析运算符来完成的：

```c++
Queue::Node::Node(const Item & i):item(i),next(0) { }
```

### 15.2.1 嵌套类和访问权限

有两种访问权限适合于嵌套类。首先，嵌套类的声明位置决定了嵌套类的作用域，即它决定了程序的哪些部分可以创建这种类的对象。其次，和其他类一样，嵌套类的公有部分、保护部分和私有部分控制了对类成员的访问。在哪些地方可以使用嵌套类以及如何使用嵌套类，取决于作用域和访问控制。

#### 1．作用域

如果嵌套类是在另一个类的私有部分声明的，则只有后者知道它。在前一个例子中，被嵌套在Queue声明中的Node类就属于这种情况（看起来Node是在私有部分之前定义的，但别忘了，类的默认访问权限是私有的），因此，Queue成员可以使用Node对象和指向Node对象的指针，但是程序的其他部分甚至不知道存在Node类。对于从Queue派生而来的类，Node也是不可见的，因为派生类不能直接访问基类的私有部分。

如果嵌套类是在另一个类的保护部分声明的，则它对于后者来说是可见的，但是对于外部世界则是不可见的。然而，在这种情况中，派生类将知道嵌套类，并可以直接创建这种类型的对象。

如果嵌套类是在另一个类的公有部分声明的，则允许后者、后者的派生类以及外部世界使用它，因为它是公有的。然而，由于嵌套类的作用域为包含它的类，因此在外部世界使用它时，必须使用类限定符。例如，假设有下面的声明：

```c++
//公有部分声明嵌套类
class Team
{
public:
	class Coach {...};
};
```

现在假定有一个失业的教练，他不属于任何球队。要在Team类的外面创建Coach对象，可以这样做：

```c++
Team::Coach forhire;	// create a Coach Object outside the Team class
```

嵌套结构体和枚举的作用域与此相同。其实，很多程序员都使用公有枚举来提供可供客户程序员使用的类常数。例如，很多类实现都被定义为支持iostream使用这种技术来提供不同的格式选项，前面已经介绍过这方面的内容，第17章将更加全面地进行介绍。表15.1总结了嵌套类、结构体和枚举的作用域特征。


| 声明位置 | 包含它的类是否可以使用它 | 从包含它的类派生而来的类是否可以使用它 | 在外部是否可以使用     |
| -------- | ------------------------ | -------------------------------------- | ---------------------- |
| 私有部分 | 是                       | 否                                     | 否                     |
| 保护部分 | 是                       | 是                                     | 否                     |
| 共有部分 | 是                       | 是                                     | 是，通过类限定符来使用 |

#### 2．访问控制

类可见后，起决定作用的将是访问控制。对嵌套类访问权的控制规则与对常规类相同。在Queue类声明中声明Node类并没有赋予Queue类任何对Node类的访问特权，也没有赋予Node类任何对Queue类的访问特权。因此，Queue类对象只能显示地访问Node对象的公有成员。由于这个原因，在Queue示例中，Node类的所有成员都被声明为公有的。这样有悖于应将数据成员声明为私有的这一惯例，但Node类是Queue类内部实现的一项特性，对外部世界是不可见的。这是因为Node类是在Queue类的私有部分声明的。所以，虽然Queue的方法可直接访问Node的成员，但使用Queue类的客户不能这样做。

总之，类声明的位置决定了类的作用域或可见性。类可见后，访问控制规则（公有、保护、私有、友元）将决定程序对嵌套类成员的访问权限。

### 15.2.2 模板中的嵌套

看一个例子：

```c++
// queuetp.h -- queue template with a nested class
#ifndef QUEUETP_H_
#define QUEUETP_H_

template <class Item>
class QueueTP
{
private:
	enum { Q_SIZE = 10 };
	// Node is a nested class definition
	class Node
	{
	public:
		Item item;
		Node* next;
		Node(const Item& i) :item(i), next(0) { }
	};
	Node* front;       // pointer to front of Queue
	Node* rear;        // pointer to rear of Queue
	int items;          // current number of items in Queue
	const int qsize;    // maximum number of items in Queue
	QueueTP(const QueueTP& q) : qsize(0) {}
	QueueTP& operator=(const QueueTP& q) { return *this; }
public:
	QueueTP(int qs = Q_SIZE);
	~QueueTP();
	bool isempty() const
	{
		return items == 0;
	}
	bool isfull() const
	{
		return items == qsize;
	}
	int queuecount() const
	{
		return items;
	}
	bool enqueue(const Item& item); // add item to end
	bool dequeue(Item& item);       // remove item from front
};

// 模板函数定义略

#endif
```

上面的程序中模板有趣的一点是，Node是利用通用类型Item来定义的。所以，下面的声明将导致Node被定义成用于存储double值：

```c++
QueueTp<double> dq;
```

而下面的声明将导致Node被定义成用于存储char值：

```c++
QueueTp<char> cq;
```

这两个Node类将在两个独立的QueueTP类中定义，因此不会发生名称冲突。即一个节点的类型为QueueTP<double>::Node，另一个节点的类型为QueueTP<char>::Node。

## 15.3 异常

看下C++有哪些方法可以处理程序错误：

### 15.3.1 调用abort( )

对于这种问题，处理方式之一是，如果其中一个参数是另一个参数的负值，则调用abort( )函数。Abort( )函数的原型位于头文件cstdlib（或stdlib.h）中，其典型实现是向标准错误流（即cerr使用的错误流）发送消息abnormal program termination（程序异常终止），然后终止程序。

```c++
// error1.cpp -- using the abort() function
#include <iostream>
#include <cstdlib>
double hmean(double a, double b);

int main()
{
	double x, y, z;

	std::cout << "Enter two numbers: ";
	while (std::cin >> x >> y)
	{
		z = hmean(x, y);
		std::cout << "Harmonic mean of " << x << " and " << y
			<< " is " << z << std::endl;
		std::cout << "Enter next set of numbers <q to quit>: ";
	}
	std::cout << "Bye!\n";
	return 0;
}

double hmean(double a, double b)
{
	if (a == -b)
	{
		std::cout << "untenable arguments to hmean()\n";
		std::abort();
	}
	return 2.0 * a * b / (a + b);
}
```

程序运行的结果如下：

```
Enter two numbers: 3 6
Harmonic mean of 3 and 6 is 4
Enter next set of numbers <q to quit>: 10 -10
untenable arguments to hmean()
```

### 15.3.2 返回错误码

略

### 15.3.3 异常机制

下面介绍如何使用异常机制来处理错误。C++异常是对程序运行过程中发生的异常情况（例如被0除）的一种响应。异常提供了将控制权从程序的一个部分传递到另一部分的途径。对异常的处理有3个组成部分：

- 引发异常；
- 使用处理程序捕获异常；
- 使用try块。

程序在出现问题时将引发异常。例如，可以修改程序清单15.7中的hmean( )，使之引发异常，而不是调用abort( )函数。throw语句实际上是跳转，即命令程序跳到另一条语句。throw关键字表示引发异常，紧随其后的值（例如字符串或对象）指出了异常的特征。

程序使用异常处理程序（exception handler）来捕获异常，异常处理程序位于要处理问题的程序中。catch关键字表示捕获异常。处理程序以关键字catch开头，随后是位于括号中的类型声明，它指出了异常处理程序要响应的异常类型；然后是一个用花括号括起的代码块，指出要采取的措施。catch关键字和异常类型用作标签，指出当异常被引发时，程序应跳到这个位置执行。异常处理程序也被称为catch块。

try块标识其中特定的异常可能被激活的代码块，它后面跟一个或多个catch块。try块是由关键字try指示的，关键字try的后面是一个由花括号括起的代码块，表明需要注意这些代码引发的异常。

要了解这3个元素是如何协同工作的，最简单的方法是看一个简短的例子，如程序清单15.9所示。

```c++
// error3.cpp -- using an exception
#include <iostream>
double hmean(double a, double b);

int main()
{
	double x, y, z;

	std::cout << "Enter two numbers: ";
	while (std::cin >> x >> y)
	{
		try {                   // start of try block
			z = hmean(x, y);
		}                       // end of try block
		catch (const char* s)  // start of exception handler
		{
			std::cout << s << std::endl;
			std::cout << "Enter a new pair of numbers: ";
			continue;
		}                       // end of handler
		std::cout << "Harmonic mean of " << x << " and " << y
			<< " is " << z << std::endl;
		std::cout << "Enter next set of numbers <q to quit>: ";
	}
	std::cout << "Bye!\n";
	return 0;
}

double hmean(double a, double b)
{
	if (a == -b)
		throw "bad hmean() arguments: a = -b not allowed";
	return 2.0 * a * b / (a + b);
}
```

程序的运行情况如下：

```
Enter two numbers: 3 6
Harmonic mean of 3 and 6 is 4
Enter next set of numbers <q to quit>: 10 -10
bad hmean() arguments: a = -b not allowed
Enter a new pair of numbers: 1 19
Harmonic mean of 1 and 19 is 1.9
Enter next set of numbers <q to quit>: q
Bye!
```

在程序清单15.9中，try块与下面类似：

```c++
try {					// start of try block
	z = hmean(x,y);
}						// end of try block
```

如果其中的某条语句导致异常被引发，则后面的catch块将对异常进行处理。如果程序在try块的外面调用hmean( )，将无法处理异常。

引发异常的代码与下面类似：

```c++
if (a == -b)
    throw "bad hmean() arguments: a = -b not allowed";
```

其中被引发的异常是字符串“bad hmean( )arguments: a = -b not allowed”。异常类型可以是字符串（就像这个例子中那样）或其他C++类型；通常为类类型，本章后面的示例将说明这一点。

执行throw语句类似于执行返回语句，因为它也将终止函数的执行；但throw不是将控制权返回给调用程序，而是导致程序沿函数调用序列后退，直到找到包含try块的函数。在程序清单15.9中，该函数是调用函数。稍后将有一个沿函数调用序列后退多步的例子。另外，在这个例子中，throw将程序控制权返回给main( )。程序将在main( )中寻找与引发的异常类型匹配的异常处理程序（位于try块的后面）。

处理程序（或catch块）与下面类似：

```c++
 catch (char * s)	// start of exception handler
 {
     std::cout << s << std::endl;
     std::cout << "Enter a new pair of numbers: ";
     continue;
 }
```

catch块有点类似于函数定义，但并不是函数定义。关键字catch表明这是一个处理程序，而char* s则表明该处理程序与字符串异常匹配。s与函数参数定义极其类似，因为匹配的引发将被赋给s。另外，当异常与该处理程序匹配时，程序将执行括号中的代码。

执行完try块中的语句后，如果没有引发任何异常，则程序跳过try块后面的catch块，直接执行处理程序后面的第一条语句。因此处理值3和6时，程序清单15.9中程序执行报告结果的输出语句。

接下来看将10和-10传递给hmean( )函数后发生的情况。If语句导致hmean( )引发异常。这将终止hmean( )的执行。程序向后搜索时发现，hmean( )函数是从main( )中的try块中调用的，因此程序查找与异常类型匹配的catch块。程序中唯一的一个catch块的参数为char*，因此它与引发异常匹配。程序将字符串“bad hmean( )arguments: a = -b not allowed”赋给变量s，然后执行处理程序中的代码。处理程序首先打印s——捕获的异常，然后打印要求用户输入新数据的指示，最后执行continue语句，命令程序跳过while循环的剩余部分，跳到起始位置。continue使程序跳到循环的起始处，这表明处理程序语句是循环的一部分，而catch行是指引程序流程的标签（参见图15.2）。

![image-20230218205744653](D:\git\note_md_files\images\image-20230218205744653.png)

您可能会问，如果函数引发了异常，而没有try块或没有匹配的处理程序时，将会发生什么情况。在默认情况下下，程序最终将调用abort( )函数，但可以修改这种行为。稍后将讨论这个问题。

### 15.3.4 将对象用作异常类型

通常，引发异常的函数将传递一个对象。这样做的重要优点之一是，可以使用不同的异常类型来区分不同的函数在不同情况下引发的异常。另外，对象可以携带信息，程序员可以根据这些信息来确定引发异常的原因。同时，catch块可以根据这些信息来决定采取什么样的措施。例如，下面是针对函数hmean( )引发的异常而提供的一种设计：

```c++
class bad_hmean
{
private:
	double v1;
	double v2;
public:
	bad_hmean(int a=0,int b=0):v1(a),v2(b){}
	void mesg();
};
inline void bad_hmean::mesg()
{
	cout<<"hmean("<<v1<<","<<v2<<"):"
	<<"invalid arguments:a=-b/n";
}
```

可以将一个bad_hmean对象初始化为传递给函数hmean( )的值，而方法mesg( )可用于报告问题（包括传递给函数hmean( )的值）。函数hmean( )可以使用下面这样的代码：

```c++
if(a == -b)
	throw bad_hmean(a,b);
```

上述代码调用构造函数bad_hmean( )，以初始化对象，使其存储参数值。

程序清单15.10和15.11添加了另一个异常类bad_gmean以及另一个名为gmean( )的函数，该函数引发bad_gmean异常。函数gmean( )计算两个数的几何平均值，即乘积的平方根。这个函数要求两个参数都不为负，如果参数为负，它将引发异常。程序清单15.10是一个头文件，其中包含异常类的定义；而程序清单15.11是一个示例程序，它使用了该头文件。注意，try块的后面跟着两个catch块：

```c++
try { // start of try block
} 	// end of try block
catch (bad_hmean & bg) // start of catch block
{
}
catch (bad_gmean &hg)
{
) // end of catch block
```

如果函数hmean( )引发bad_hmean异常，第一个catch块将捕获该异常；如果gmean( )引发bad_gmean异常，异常将逃过第一个catch块，被第二个catch块捕获。

```c++
// exc_mean.h  -- exception classes for hmean(), gmean()
#include <iostream>

class bad_hmean
{
private:
	double v1;
	double v2;
public:
	bad_hmean(double a = 0, double b = 0) : v1(a), v2(b) {}
	void mesg();
};

inline void bad_hmean::mesg()
{
	std::cout << "hmean(" << v1 << ", " << v2 << "): "
		<< "invalid arguments: a = -b\n";
}

class bad_gmean
{
public:
	double v1;
	double v2;
	bad_gmean(double a = 0, double b = 0) : v1(a), v2(b) {}
	const char* mesg();
};

inline const char* bad_gmean::mesg()
{
	return "gmean() arguments should be >= 0\n";
}
```

```c++
//error4.cpp using exception classes
#include <iostream>
#include <cmath> // or math.h, unix users may need -lm flag
#include "15.10 exc_mean.h"
// function prototypes
double hmean(double a, double b);
double gmean(double a, double b);
int main()
{
    using std::cout;
    using std::cin;
    using std::endl;

    double x, y, z;

    cout << "Enter two numbers: ";
    while (cin >> x >> y)
    {
        try {                  // start of try block
            z = hmean(x, y);
            cout << "Harmonic mean of " << x << " and " << y
                << " is " << z << endl;
            cout << "Geometric mean of " << x << " and " << y
                << " is " << gmean(x, y) << endl;
            cout << "Enter next set of numbers <q to quit>: ";
        }// end of try block
        catch (bad_hmean& bg)    // start of catch block
        {
            bg.mesg();
            cout << "Try again.\n";
            continue;
        }
        catch (bad_gmean& hg)
        {
            cout << hg.mesg();
            cout << "Values used: " << hg.v1 << ", "
                << hg.v2 << endl;
            cout << "Sorry, you don't get to play any more.\n";
            break;
        } // end of catch block
    }
    cout << "Bye!\n";
    // cin.get();
    // cin.get();
    return 0;
}

double hmean(double a, double b)
{
    if (a == -b)
        throw bad_hmean(a, b);
    return 2.0 * a * b / (a + b);
}

double gmean(double a, double b)
{
    if (a < 0 || b < 0)
        throw bad_gmean(a, b);
    return std::sqrt(a * b);
}
```

```
Enter two numbers: 4 12
Harmonic mean of 4 and 12 is 6
Geometric mean of 4 and 12 is 6.9282
Enter next set of numbers <q to quit>: 5 -5
hmean(5, -5): invalid arguments: a = -b
Try again.
5 -2
Harmonic mean of 5 and -2 is -6.66667
gmean() arguments should be >= 0
Values used: 5, -2
Sorry, you don't get to play any more.
Bye!
```

### 15.3.6 栈解退

假设try块没有直接调用引发异常的函数，而是调用了对引发异常的函数进行调用的函数，则程序流程将从引发异常的函数跳到包含try块和处理程序的函数。这涉及到栈解退（unwinding the stack），下面进行介绍。

首先来看一看C++通常是如何处理函数调用和返回的。C++通常通过将信息放在栈（参见第9章）中来处理函数调用。具体地说，程序将调用函数的指令的地址（返回地址）放到栈中。当被调用的函数执行完毕后，程序将使用该地址来确定从哪里开始继续执行。另外，函数调用将函数参数放到栈中。在栈中，这些函数参数被视为自动变量。如果被调用的函数创建了新的自动变量，则这些变量也将被添加到栈中。如果被调用的函数调用了另一个函数，则后者的信息将被添加到栈中，依此类推。当函数结束时，程序流程将跳到该函数被调用时存储的地址处，同时栈顶的元素被释放。因此，函数通常都返回到调用它的函数，依此类推，同时每个函数都在结束时释放其自动变量。如果自动变量是类对象，则类的析构函数（如果有的话）将被调用。

现在假设函数由于出现异常（而不是由于返回）而终止，则程序也将释放栈中的内存，但不会在释放栈的第一个返回地址后停止，而是继续释放栈，直到找到一个位于try块（参见图15.3）中的返回地址。随后，控制权将转到块尾的异常处理程序，而不是函数调用后面的第一条语句。这个过程被称为栈解退。引发机制的一个非常重要的特性是，和函数返回一样，对于栈中的自动类对象，类的析构函数将被调用。然而，函数返回仅仅处理该函数放在栈中的对象，而throw语句则处理try块和throw之间整个函数调用序列放在栈中的对象。如果没有栈解退这种特性，则引发异常后，对于中间函数调用放在栈中的自动类对象，其析构函数将不会被调用。

![image-20230218210555407](D:\git\note_md_files\images\image-20230218210555407.png)

看如下示例：

```c++
//error5.cpp -- unwinding the stack
#include <iostream>
#include <cmath> // or math.h, unix users may need -lm flag
#include <string>
#include "15.10 exc_mean.h"

class demo
{
private:
	std::string word;
public:
	demo(const std::string& str)
	{

		word = str;
		std::cout << "demo " << word << " created\n";
	}
	~demo()
	{
		std::cout << "demo " << word << " destroyed\n";
	}
	void show() const
	{
		std::cout << "demo " << word << " lives!\n";
	}
};

// function prototypes
double hmean(double a, double b);
double gmean(double a, double b);
double means(double a, double b);

int main()
{
	using std::cout;
	using std::cin;
	using std::endl;

	double x, y, z;
	{
		demo d1("found in block in main()");
		cout << "Enter two numbers: ";
		while (cin >> x >> y)
		{
			try {                  // start of try block
				z = means(x, y);
				cout << "The mean mean of " << x << " and " << y
					<< " is " << z << endl;
				cout << "Enter next pair: ";
			} // end of try block
			catch (bad_hmean& bg)    // start of catch block
			{
				bg.mesg();
				cout << "Try again.\n";
				continue;
			}
			catch (bad_gmean& hg)
			{
				cout << hg.mesg();
				cout << "Values used: " << hg.v1 << ", "
					<< hg.v2 << endl;
				cout << "Sorry, you don't get to play any more.\n";
				break;
			} // end of catch block
		}
		d1.show();
	}
	cout << "Bye!\n";
	// cin.get();
	// cin.get();
	return 0;
}

double hmean(double a, double b)
{
	if (a == -b)
		throw bad_hmean(a, b);
	return 2.0 * a * b / (a + b);
}

double gmean(double a, double b)
{
	if (a < 0 || b < 0)
		throw bad_gmean(a, b);
	return std::sqrt(a * b);
}

double means(double a, double b)
{
	double am, hm, gm;
	demo d2("found in means()");
	am = (a + b) / 2.0;    // arithmetic mean
	try
	{
		hm = hmean(a, b);
		gm = gmean(a, b);
	}
	catch (bad_hmean& bg) // start of catch block
	{
		bg.mesg();
		std::cout << "Caught in means()\n";
		throw;             // rethrows the exception 
	}
	d2.show();
	return (am + hm + gm) / 3.0;
}
```

上例中，main( )调用了means( )，而means( )又调用了hmean( )和gmean( )。函数main( )中的try块能够捕获bad_hmean和bad_gmean异常，而函数means( )中的try块只能捕获bad_hmean异常：

```c++
	catch (bad_hmean& bg) // start of catch block
	{
		bg.mesg();
		std::cout << "Caught in means()\n";
		throw;             // rethrows the exception 
	}
```

上述代码显示消息后，重新引发异常，这将向上把异常发送给main( )函数。一般而言，重新引发的异常将由下一个捕获这种异常的try-catch块组合进行处理，如果没有找到这样的处理程序，默认情况下程序将异常终止。

### 15.3.7 其他异常特性

引发异常时编译器总是创建一个临时拷贝，即使异常规范和catch块中指定的是引用。例如，请看下面的代码：

```c++
class problem {...}
...
void super() throw (problem)
{
	...
	if(oh_no)
	{
		problem oops;   //construct object
		throw oops;       //throw it
		...
}
...
try{
	super();
}
catch(problem & p)
{
//statements
}
```

p将指向oops的副本而不是oops本身。这是件好事，因为函数super( )执行完毕后，oops将不复存在。顺便说一句，将引发异常和创建对象组合在一起将更简单：

```c++
throw problem();		// construct and throw default problem object
```

您可能会问，既然throw语句将生成副本，为何代码中使用引用呢？毕竟，将引用作为返回值的通常原因是避免创建副本以提高效率。答案是，引用还有另一个重要特征：基类引用可以执行派生类对象。假设有一组通过继承关联起来的异常类型，则在异常规范中只需列出一个基类引用，它将与任何派生类对象匹配。

假设有一个异常类层次结构，并要分别处理不同的异常类型，则使用基类引用将能够捕获任何异常对象；而使用派生类对象只能捕获它所属类及从这个类派生而来的类的对象。引发的异常对象将被第一个与之匹配的catch块捕获。这意味着catch块的排列顺序应该与派生顺序相反：

```c++
class bad_1 {...};
class bad_2 : public bad_1 {...};
class bad_3 : public bad_2 {...};
...
void duper()
{
...
    if(oh_no)
    	throw bad_1()
    if(rats)
        throw bad_2()
    if(drat)
    	throw bad_3()
}
...
try {
	duper();
}
catch(bad_3 &be)
{// statements }
catch(bad_2 &be)
{// statements }
catch(bad_1 &be)
{// statements }
```

如果将bad_1 &处理程序放在最前面，它将捕获异常bad_1、bad_2和bad_3；通过按相反的顺序排列，bad_3异常将被bad_3 &处理程序所捕获。

通过正确地排列catch块的顺序，让您能够在如何处理异常方面有选择的余地。然而，有时候可能不知道会发生哪些异常。例如，假设您编写了一个调用另一个函数的函数，而您并不知道被调用的函数可能引发哪些异常。在这种情况下，仍能够捕获异常，即使不知道异常的类型。方法是使用省略号来表示异常类型，从而捕获任何异常：

```c++
catch (...) { // statements } // catches any type exception
```

如果知道一些可能会引发的异常，可以将上述捕获所有异常的catch块放在最后面，这有点类似于switch语句中的default：

```c++
try{
    duper();
}
catch(bad_3& be)
{ 
    // statements
}
catch(bad_2& be)
{
    // statements
}
catch(bad_1& be)
{
    // statements
}
catch(bad_hmean& h)
{
    // statements
}
catch(...)
{
    // statements
}
```

可以创建捕获对象而不是引用的处理程序。在catch语句中使用基类对象时，将捕获所有的派生类对象，但派生特性将被剥去，因此将使用虚方法的基类版本。

### 15.3.8 exception类

C++异常的主要目的是为设计容错程序提供语言级支持，即异常使得在程序设计中包含错误处理功能更容易，以免事后采取一些严格的错误处理方式。异常的灵活性和相对方便性激励着程序员在条件允许的情况下在程序设计中加入错误处理功能。总之，异常是这样一种特性：类似于类，可以改变您的编程方式。

较新的C++编译器将异常合并到语言中。例如，为支持该语言，exception头文件（以前为exception.h或except.h）定义了exception类，C++可以把它用作其他异常类的基类。代码可以引发exception异常，也可以将exception类用作基类。有一个名为what( )的虚拟成员函数，它返回一个字符串，该字符串的特征随实现而异。然而，由于这是一个虚方法，因此可以在从exception派生而来的类中重新定义它：

```c++
#include <exception>
class bad_hmean: public std::exception
{
public:
    const char* what() { return "bad arguments to hmean()"; }
    // ...
};
class bad_gmean: public std::exception
{
public:
    const char* what() { return "bad arguments to gmean()"; }
    // ...
};
```

如果不想以不同的方式处理这些派生而来的异常，可以在同一个基类处理程序中捕获它们：

```c++
try{
    // ...
}
catch(std::exception& e)
{
    cout << e.what() << endl;
    // ...
}
```

否则，可以分别捕获它们。

C++库定义了很多基于exception的异常类型。

#### 1．stdexcept异常类

头文件stdexcept定义了其他几个异常类。首先，该文件定义了logic_error和runtime_error类，它们都是以公有方式从exception派生而来的：

```c++
class logic_error: public exception
{
public:
    explicit logic_error(const string& what_arg);
    // ...    
};
 
class domain_error: public logic_error
{
public:
    explicit domain_error(const string& what_arg);
    // ...
};
```

注意，这些类的构造函数接受一个string对象作为参数，该参数提供了方法what( )以C-风格字符串方式返回的字符数据。

这两个新类被用作两个派生类系列的基类。异常类系列logic_error描述了典型的逻辑错误。总体而言，通过合理的编程可以避免这种错误，但实际上这些错误还是可能发生的。每个类的名称指出了它用于报告的错误类型：

- domain_error；
- invalid_argument；
- length_error；
- out_of_bounds。

每个类独有一个类似于logic_error的构造函数，让您能够提供一个供方法what( )返回的字符串。

数学函数有定义域（domain）和值域（range）。定义域由参数的可能取值组成，值域由函数可能的返回值组成。如果您编写一个函数，该函数将一个参数传递给函数std::sin( )，则可以让该函数在参数不在定义域- 1到+1之间时引发domain_error异常。

异常invalid_argument指出给函数传递了一个意料外的值。例如，如果函数希望接受一个这样的字符串：其中每个字符要么是‘0’要么是‘1’，则当传递的字符串中包含其他字符时，该函数将引发invalid_argument异常。

异常length_error用于指出没有足够的空间来执行所需的操作。例如，string类的append( )方法在合并得到的字符串长度超过最大允许长度时，将引发length_error异常。

异常out_of_bounds通常用于指示索引错误。例如，您可以定义一个类似于数组的类，其operator( ) [ ]在使用的索引无效时引发out_of_bounds异常。

接下来，runtime_error异常系列描述了可能在运行期间发生但难以预计和防范的错误。每个类的名称指出了它用于报告的错误类型：

- range_error；
- overflow_error；
- underflow_error。

每个类独有一个类似于runtime_error的构造函数，让您能够提供一个供方法what( )返回的字符串。

下溢（underflow）错误在浮点数计算中。一般而言，存在浮点类型可以表示的最小非零值，计算结果比这个值还小时将导致下溢错误。整型和浮点型都可能发生上溢错误，当计算结果超过了某种类型能够表示的最大数量级时，将发生上溢错误。计算结果可能不再函数允许的范围之内，但没有发生上溢或下溢错误，在这种情况下，可以使用range_error异常。

一般而言，logic_error系列异常表明存在可以通过编程修复的问题，而runtime_error系列异常表明存在无法避免的问题。所有这些错误类有相同的常规特征，它们之间的主要区别在于：不同的类名让您能够分别处理每种异常。另一方面，继承关系让您能够一起处理它们（如果您愿意的话）。例如，下面的代码首先单独捕获out_of_bounds异常，然后统一捕获其他logic_error系列异常，最后统一捕获exception异常、runtime_error系列异常以及其他从exception派生而来的异常：

```c++
try {
...
}
catch (out_of_bounds& oe) // catch out_of_bounds error
{...}
catch (logic_error& oe)	// catch remaining logic_error family
{...}
catch (exception& oe)	// catch runtime_error, exception objects
{...}
```

如果上述库类不能满足您的需求，应该从logic_error或runtime_error派生一个异常类，以确保您异常类可归入同一个继承层次结构中。

#### 2．bad_alloc异常和new

对于使用new导致的内存分配问题，C++的最新处理方式是让new引发bad_alloc异常。头文件new包含bad_alloc类的声明，它是从exception类公有派生而来的。但在以前，当无法分配请求的内存量时，new返回一个空指针。

程序清单15.13演示了最新的方法。捕获到异常后，程序将显示继承的what( )方法返回的消息（该消息随实现而异），然后终止。

```c++
// newexcp.cpp -- the bad_alloc exception
#include <iostream>
#include <new>
#include <cstdlib>  // for exit(), EXIT_FAILURE
using namespace std;

struct Big
{
	double stuff[20000];
};

int main()
{
	Big* pb;
	try {
		cout << "Trying to get a big block of memory:\n";
		pb = new Big[10000]; // 1,600,000,000 bytes
		cout << "Got past the new request:\n";
	}
	catch (bad_alloc& ba)
	{
		cout << "Caught the exception!\n";
		cout << ba.what() << endl;
		exit(EXIT_FAILURE);
	}
	cout << "Memory successfully allocated\n";
	pb[0].stuff[0] = 4;
	cout << pb[0].stuff[0] << endl;
	delete[] pb;
	// cin.get();
	return 0;
}
```

下面该程序在某个系统中的输出：

```c++
Trying to get a big block of memory:
Caught the exception!
std::badalloc
```

在这里，方法what( )返回字符串“std::bad_alloc”。

如果程序在您的系统上运行时没有出现内存分配问题，可尝试提高请求分配的内存量。

#### 3．空指针和new

很多代码都是在new在失败时返回空指针时编写的。为处理new的变化，有些编译器提供了一个标记（开关），让用户选择所需的行为。当前，C++标准提供了一种在失败时返回空指针的new，其用法如下：

```c++
int* pi = new (std::nothrow) int;
int* pa = new (std::nothrow) int[500];
```

使用这种new，可将程序清单15.13的核心代码改为如下所示：

```c++
Big* pb;
pb = new (std::nothrow) Big[10000];	// 1, 600, 000, 000 bytes
if (pb == 0)
{
	cout << "Could not allocate memory. Bye.\n";
	exit(EXIT_FAILURE);
}
```

### 15.3.10 异常何时会迷失方向

异常被引发后，在两种情况下，会导致问题。首先，如果它是在带异常规范的函数中引发的，则必须与规范列表中的某种异常匹配（在继承层次结构中，类类型与这个类及其派生类的对象匹配），否则称为意外异常（unexpected exception）。在默认情况下，这将导致程序异常终止（虽然C++11摒弃了异常规范，但仍支持它，且有些现有的代码使用了它）。如果异常不是在函数中引发的（或者函数没有异常规范），则必须捕获它。如果没被捕获（在没有try块或没有匹配的catch块时，将出现这种情况），则异常被称为未捕获异常（uncaught exception）。在默认情况下，这将导致程序异常终止。然而，可以修改程序对意外异常和未捕获异常的反应。下面来看如何修改，先从未捕获异常开始。

未捕获异常不会导致程序立刻异常终止。相反，程序将首先调用函数terminate( )。在默认情况下，terminate( )调用abort( )函数。可以指定terminate( )应调用的函数（而不是abort( )）来修改terminate( )的这种行为。为此，可调用set_terminate( )函数。set_terminate( )和terminate( )都是在头文件exception中声明的：

```c++
typedef void (*terminate_handle)() ;
terminate_handle set_terminate(terminate_handle f) throw();//c++ 98
terminate_handle set_terinate(terminate_handle f) noexcept; //c++11
void teminate(); //c++98
void teminate() noexcept ; //c++11
```

其中的typedef使terminate_handler成为这样一种类型的名称：指向没有参数和返回值的函数的指针。set_terminate( )函数将不带任何参数且返回类型为void的函数的名称（地址）作为参数，并返回该函数的地址。如果调用了set_terminate( )函数多次，则terminate( )将调用最后一次set_terminate( )调用设置的函数。

来看一个例子。假设希望未捕获的异常导致程序打印一条消息，然后调用exit( )函数，将退出状态值设置为5。首先，请包含头文件exception。可以使用using编译指令、适当的using声明或std ::限定符，来使其声明可用。

```c++
#include <exception>
using namespace std;
```

然后，设计一个完成上述两种操作所需的函数，其原型如下：

```c++
void myQuit()
{
	cout << "Terminating due to uncaught exception\n";
	exit(5);
}
```

最后，在程序的开头，将终止操作指定为调用该函数。

```c++
set_terminate(myQuit);
```

现在，如果引发了一个异常且没有被捕获，程序将调用terminate( )，而后者将调用MyQuit( )。

接下来看意外异常。通过给函数指定异常规范，可以让函数的用户知道要捕获哪些异常。假设函数的原型如下：

```c++
double Argh(double, double) throw(out_of_bounds);
```

则可以这样使用该函数：

```c++
try {
	x = Argh(a, b);
}
catch (out_of_bounds& ex)
{
	...
}
```

知道应捕获哪些异常很有帮助，因为默认情况下，未捕获的异常将导致程序异常终止。

原则上，异常规范应包含函数调用的其他函数引发的异常。例如，如果Argh( )调用了Duh( )函数，而后者可能引发retort对象异常，则Argh( )和Duh( )的异常规范中都应包含retort。除非自己编写所有的函数，并且特别仔细，否则无法保证上述工作都已正确完成。例如，可能使用的是老式商业库，而其中的函数没有异常规范。这表明应进一步探讨这样一点，即如果函数引发了其异常规范中没有的异常，情况将如何？这也表明异常规范机制处理起来比较麻烦，这也是C++11将其摒弃的原因之一。

在这种情况下，行为与未捕获的异常极其类似。如果发生意外异常，程序将调用unexpected( )函数（您没有想到是unexpected( )函数吧？谁也想不到！）。这个函数将调用terminate( )，后者在默认情况下将调用abort( )。正如有一个可用于修改terminate( )的行为的set_terminate( )函数一样，也有一个可用于修改unexpected( )的行为的set_unexpected( )函数。这些新函数也是在头文件exception中声明的：

```c++
typedef void (* unexpected_handle)();
unexpected_handle set_unexpected(unexpected_handle f) throw();//c++98
unexpected_handle set_unexpected(unexpected_handle f) noexpect;//c++11
void unexpected(); c++ 98
void unexpected() noexcept ;//c+ 0x
```

然而，与提供给set_terminate( )的函数的行为相比，提供给set_unexpected( )的函数的行为受到更严格的限制。具体地说，unexpected_handler函数可以：

- 通过调用terminate( )（默认行为）、abort( )或exit( )来终止程序；
- 引发异常。

引发异常（第二种选择）的结果取决于unexpected_handler函数所引发的异常以及引发意外异常的函数的异常规范：

- 如果新引发的异常与原来的异常规范匹配，则程序将从那里开始进行正常处理，即寻找与新引发的异常匹配的catch块。基本上，这种方法将用预期的异常取代意外异常；
- 如果新引发的异常与原来的异常规范不匹配，且异常规范中没有包括std ::bad_exception类型，则程序将调用terminate( )。bad_exception是从exception派生而来的，其声明位于头文件exception中；
- 如果新引发的异常与原来的异常规范不匹配，且原来的异常规范中包含了std ::bad_exception类型，则不匹配的异常将被std::bad_exception异常所取代。

总之，如果要捕获所有的异常（不管是预期的异常还是意外异常），则可以这样做：

首先确保异常头文件的声明可用：

```c++
#include <exception>
using namespace std;
```

然后，设计一个替代函数，将意外异常转换为bad_exception异常，该函数的原型如下：

```c++
void myUnexpected()
{
	throw std::bad_exception();		// or just throw;
}
```

仅使用throw，而不指定异常将导致重新引发原来的异常。然而，如果异常规范中包含了这种类型，则该异常将被bad_exception对象所取代。

接下来在程序的开始位置，将意外异常操作指定为调用该函数：

```c++
set_unexpected(myUnexpected);
```

最后，将bad_exception类型包括在异常规范中，并添加如下catch块序列：

```c++
double Argh(double double) throw(out_of_bounds,bad_exception);
...
try{
	x=Argh(a,b);
}
catch(out_of_bounds & ex)
{
	...
}
catch(bad_exception & ex)
{
	...
}
```

### 15.3.11 有关异常的注意事项

异常规范不适用于模板，因为模板函数引发的异常可能随特定的具体化而异。

下面进一步讨论动态内存分配和异常。首先，请看下面的函数：

```c++
void test1(int n)
{
	string mesg("I'm trapped in an endless loop");
	...
	if(oh_no)
		throw exception();
	...
	return;
}
```

string类采用动态内存分配。通常，当函数结束时，将为mesg调用string的析构函数。虽然throw语句过早地终止了函数，但它仍然使得析构函数被调用，这要归功于栈解退。因此在这里，内存被正确地管理。

接下来看下面这个函数：

```c++
void test2(int n)
{
    double * ar =new double[n];
    ...
    if(oh_no)
    	throw exception();
    ...
    delete [] ar;
    return;
}
```

这里有个问题。解退栈时，将删除栈中的变量ar。但函数过早的终止意味着函数末尾的delete[ ]语句被忽略。指针消失了，但它指向的内存块未被释放，并且不可访问。总之，这些内存被泄漏了。

这种泄漏是可以避免的。例如，可以在引发异常的函数中捕获该异常，在catch块中包含一些清理代码，然后重新引发异常：

```c++
void test3(int n)
{
    double * ar=new double [n];
    ...
    try{
        if(oh_no)
        throw exception();
    }
    catch(exception & ex)
    {
        delete [] ar;
        throw;
    }
    ...
    delete [] ar;
    return;
}
```

然而，这将增加疏忽和产生其他错误的机会。另一种解决方法是使用第16章将讨论的智能指针模板之一。

总之，虽然异常处理对于某些项目极为重要，但它也会增加编程的工作量、增大程序、降低程序的速度。另一方面，不进行错误检查的代价可能非常高。

## 15.4 RTTI

RTTI是运行阶段类型识别（Runtime Type Identification）的简称。这是新添加到C++中的特性之一，很多老式实现不支持。另一些实现可能包含开关RTTI的编译器设置。RTTI旨在为程序在运行阶段确定对象的类型提供一种标准方式。

### 15.4.1 RTTI的用途

想跟踪生成的对象的类型，RTTI提供解决方案。

### 15.4.2 RTTI的工作原理

C++有3个支持RTTI的元素。

- 如果可能的话，dynamic_cast运算符将使用一个指向基类的指针来生成一个指向派生类的指针；否则，该运算符返回0——空指针。
- typeid运算符返回一个指出对象的类型的值。
- type_info结构存储了有关特定类型的信息。

只能将RTTI用于包含虚函数的类层次结构，原因在于只有对于这种类层次结构，才应该将派生对象的地址赋给基类指针。

#### 1．dynamic_cast运算符

dynamic_cast运算符是最常用的RTTI组件，它不能回答“指针指向的是哪类对象”这样的问题，但能够回答“是否可以安全地将对象的地址赋给特定类型的指针”这样的问题。我们来看一看这意味着什么。假设有下面这样的类层次结构：

```c++
class Grand { // has virtual methods};
class Superb : public Grand {...};
class Magnificent : public Superb {...};
```

接下来假设有下面的指针：

```c++
Grand* pg = new Grand;
Grand* ps = new Superb;
Grand* pm = new Magnificent;
```

最后，对于下面的类型转换：

```c++
Magnificent* p1 = (Magnificent*) pm;		// #1
Magnificent* p1 = (Magnificent*) ps;		// #2
Superb* p3 = (Magnificent*) pm;		// #3
```

哪些是安全的？根据类声明，它们可能全都是安全的，但只有那些指针类型与对象的类型（或对象的直接或间接基类的类型）相同的类型转换才一定是安全的。例如，类型转换#1就是安全的，因为它将Magificent类型的指针指向类型为Magnificent的对象。类型转换#2就是不安全的，因为它将基数对象（Grand）的地址赋给派生类（Magnificent）指针。因此，程序将期望基类对象有派生类的特征，而通常这是不可能的。例如，Magnificent对象可能包含一些Grand对象没有的数据成员。然而，类型转换#3是安全的，因为它将派生对象的地址赋给基类指针。即公有派生确保Magnificent对象同时也是一个Superb对象（直接基类）和一个Grand对象（间接基类）。因此，将它的地址赋给这3种类型的指针都是安全的。虚函数确保了将这3种指针中的任何一种指向Magnificent对象时，都将调用Magnificent方法。

注意，与问题“指针指向的是哪种类型的对象”相比，问题“类型转换是否安全”更通用，也更有用。通常想知道类型的原因在于：知道类型后，就可以知道调用特定的方法是否安全。要调用方法，类型并不一定要完全匹配，而可以是定义了方法的虚拟版本的基类类型。下面的例子说明了这一点。

然而，先来看一下dynamic_cast的语法。该运算符的用法如下，其中pg指向一个对象：

```c++
Superb* pm = dynamic_cast<Superb *> (pg);
```

这提出了这样的问题：指针pg的类型是否可被安全地转换为Superb *？如果可以，运算符将返回对象的地址，否则返回一个空指针。

**注意：通常，如果指向的对象（*pt）的类型为Type或者是从Type直接或间接派生而来的类型，则下面的表达式将指针pt转换为Type类型的指针：**

```c++
dynamic_cast<Type*>(pt)
```

**否则，结果为0，即空指针。**

程序清单15.17演示了这种处理。首先，它定义了3个类，名称为Grand、Superb和Magnificent。Grand类定义了一个虚函数Speak( )，而其他类都重新定义了该虚函数。Superb类定义了一个虚函数Say()，而Manificent也重新定义了它（参见图15.4）。程序定义了GetOne( )函数，该函数随机创建这3种类中某种类的对象，并对其进行初始化，然后将地址作为Grand*指针返回（GetOne( )函数模拟用户做出决定）。循环将该指针赋给Grand *变量pg，然后使用pg调用Speak( )函数。因为这个函数是虚拟的，所以代码能够正确地调用指向的对象的Speak( )版本。

```c++
for (int i = 0; i < 5; i++)
{
	pg = GetOne();
	pg->Speak();
}
```

然而，不能用相同的方式（即使用指向Grand的指针）来调用Say()函数，因为Grand类没有定义它。然而，可以使用dynamic_cast运算符来检查是否可将pg的类型安全地转换为Superb指针。如果对象的类型为Superb或Magnificent，则可以安全转换。在这两种情况下，都可以安全地调用Say( )函数：

```c++
if (ps = dynamic_cast<Superb *>(pg))
	ps->Say();
```

赋值表达式的值是它左边的值，因此if条件的值为ps。如果类型转换成功，则ps的值为非零（true）；如果类型转换失败，即pg指向的是一个Grand对象，ps的值将为0（false）。程序清单15.17列出了所有的代码。顺便说一句，有些编译器可能会对无目的赋值（在if条件语句中，通常使用= =运算符）提出警告。

![image-20230219172838721](D:\git\note_md_files\images\image-20230219172838721.png)

```c++
// rtti1.cpp -- using the RTTI dynamic_cast operator
#include <iostream>
#include <cstdlib>
#include <ctime>

using std::cout;

class Grand
{
private:
	int hold;
public:
	Grand(int h = 0) : hold(h) {}
	virtual void Speak() const { cout << "I am a grand class!\n"; }
	virtual int Value() const { return hold; }
};

class Superb : public Grand
{
public:
	Superb(int h = 0) : Grand(h) {}
	void Speak() const { cout << "I am a superb class!!\n"; }
	virtual void Say() const
	{
		cout << "I hold the superb value of " << Value() << "!\n";
	}
};

class Magnificent : public Superb
{
private:
	char ch;
public:
	Magnificent(int h = 0, char c = 'A') : Superb(h), ch(c) {}
	void Speak() const { cout << "I am a magnificent class!!!\n"; }
	void Say() const {
		cout << "I hold the character " << ch <<
			" and the integer " << Value() << "!\n";
	}
};

Grand* GetOne();
int main()
{
	std::srand(std::time(0));
	Grand* pg;
	Superb* ps;
	for (int i = 0; i < 5; i++)
	{
		pg = GetOne();
		pg->Speak();
		if (ps = dynamic_cast<Superb*>(pg))
			ps->Say();
	}
	// std::cin.get();
	return 0;
}

Grand* GetOne()    // generate one of three kinds of objects randomly
{
	Grand* p = nullptr;
	switch (std::rand() % 3)
	{
	case 0: p = new Grand(std::rand() % 100);
		break;
	case 1: p = new Superb(std::rand() % 100);
		break;
	case 2: p = new Magnificent(std::rand() % 100,
		'A' + std::rand() % 26);
		break;
	}
	return p;
}
```

也可以将dynamic_cast用于引用，其用法稍微有点不同：没有与空指针对应的引用值，因此无法使用特殊的引用值来指示失败。当请求不正确时，dynamic_cast将引发类型为bad_cast的异常，这种异常是从exception类派生而来的，它是在头文件typeinfo中定义的。因此，可以像下面这样使用该运算符，其中rg是对Grand对象的引用：

```c++
#include <typeinfo>	// for bad_cast
...
try {
	Super_b& rs = dynamic_cast<Superb &>(rg);
	...
}
catch(bad_cast &) {
	...
};
```

#### 2．typeid运算符和type_info类

typeid运算符使得能够确定两个对象是否为同种类型。它与sizeof有些相像，可以接受两种参数：

- 类名；
- 结果为对象的表达式。

typeid运算符返回一个对type_info对象的引用，其中，type_info是在头文件typeinfo（以前为typeinfo.h）中定义的一个类。type_info类重载了= =和!=运算符，以便可以使用这些运算符来对类型进行比较。例如，如果pg指向的是一个Magnificent对象，则下述表达式的结果为bool值true，否则为false：

```c++
typeid(Magnificent) == typeid(*pg);
```

如果pg是一个空指针，程序将引发bad_typeid异常。该异常类型是从exception类派生而来的，是在头文件typeinfo中声明的。

type_info类的实现随厂商而异，但包含一个name( )成员，该函数返回一个随实现而异的字符串：通常（但并非一定）是类的名称。例如，下面的语句显示指针pg指向的对象所属的类定义的字符串：

```c++
cout <<	"Now processing type " << typeid(*pg).name() << ".\n";
```

程序清单15.18对程序清单15.17作了修改，以使用typeid运算符和name( )成员函数。注意，它们都适用于dynamic_cast和virtual函数不能处理的情况。typeid测试用来选择一种操作，因为操作不是类的方法，所以不能通过类指针调用它。name( )方法语句演示了如何将方法用于调试。注意，程序包含了头文件typeinfo。

```c++
// rtti2.cpp  -- using dynamic_cast, typeid, and type_info
#include <iostream>
#include <cstdlib>
#include <ctime>
#include <typeinfo>
using namespace std;

class Grand
{
private:
	int hold;
public:
	Grand(int h = 0) : hold(h) {}
	virtual void Speak() const { cout << "I am a grand class!\n"; }
	virtual int Value() const { return hold; }
};

class Superb : public Grand
{
public:
	Superb(int h = 0) : Grand(h) {}
	void Speak() const { cout << "I am a superb class!!\n"; }
	virtual void Say() const
	{
		cout << "I hold the superb value of " << Value() << "!\n";
	}
};

class Magnificent : public Superb
{
private:
	char ch;
public:
	Magnificent(int h = 0, char cv = 'A') : Superb(h), ch(cv) {}
	void Speak() const { cout << "I am a magnificent class!!!\n"; }
	void Say() const {
		cout << "I hold the character " << ch <<
			" and the integer " << Value() << "!\n";
	}
};

Grand* GetOne();
int main()
{
	srand(time(0));
	Grand* pg;
	Superb* ps;
	for (int i = 0; i < 5; i++)
	{
		pg = GetOne();
		cout << "Now processing type " << typeid(*pg).name() << ".\n";
		pg->Speak();
		if (ps = dynamic_cast<Superb*>(pg))
			ps->Say();
		if (typeid(Magnificent) == typeid(*pg))
			cout << "Yes, you're really magnificent.\n";
	}
	// std::cin.get();
	return 0;
}

Grand* GetOne()
{
	//Grand * p;
	Grand* p = nullptr;

	switch (rand() % 3)
	{
	case 0: p = new Grand(rand() % 100);
		break;
	case 1: p = new Superb(rand() % 100);
		break;
	case 2: p = new Magnificent(rand() % 100, 'A' + rand() % 26);
		break;
	}
	return p;
}
```

与前一个程序的输出一样，每次运行该程序的输出都可能不同，因为它使用rand( )来选择类型。另外，调用name()时，有些编译器可能提供不同的输出，如5Grand（而不是Grand）。

## 15.5 类型转换运算符

在C++的创始人Bjarne Stroustrup看来，C语言中的类型转换运算符太过松散。例如，请看下面的代码：

```c++
struct Data
{
	double data[200];
}
struct Junk
{
	int junk[100];
}
Data d = {2.5e33, 3.5e-19, 20.2e32};
char* pch = (char*) (&d);		// type cast #1 - convert to string
char ch = char (&d);			// type cast #2 - convert address to a char
Junk* pj = (Junk*) (&d);		// type cast #3 - convert to Junk pointer
```

首先，上述3种类型转换中，哪一种有意义？除非不讲理，否则它们中没有一个是有意义的。其次，这3种类型转换中哪种是允许的呢？在C语言中都是允许的。

对于这种松散情况，Stroustrop采取的措施是，更严格地限制允许的类型转换，并添加4个类型转换运算符，使转换过程更规范：

- dynamic_cast；
- const_cast；
- static_cast；
- reinterpret_cast。

可以根据目的选择一个适合的运算符，而不是使用通用的类型转换。这指出了进行类型转换的原因，并让编译器能够检查程序的行为是否与设计者想法吻合。

dynamic_cast运算符已经在前面介绍过了。总之，假设High和Low是两个类，而ph和pl的类型分别为High *和Low  * ，则仅当Low是High的可访问基类（直接或间接）时，下面的语句才将一个Low*指针赋给pl：

```c++
pl = dynamic_cast<Low *> ph;
```

否则，该语句将空指针赋给pl。通常，该运算符的语法如下：

```c++
dynamic_cast<type-name>(expression)
```

该运算符的用途是，使得能够在类层次结构中进行向上转换（由于is-a关系，这样的类型转换是安全的），而不允许其他转换。

const_cast运算符用于执行只有一种用途的类型转换，即改变值为const或volatile，其语法与dynamic_cast运算符相同：

```c++
const_cast<type-name>(expression);
```

如果类型的其他方面也被修改，则上述类型转换将出错。也就是说，除了const或volatile特征（有或无）可以不同外，type_name和expression的类型必须相同。再次假设High和Low是两个类：

```c++
High bar;
const High* pbar = &bar;
...
High* pb = const_cast<High*> (pbar);	// valid
const Low* pl = const_cast<const Low*> (pbar);	// invalid
```

第一个类型转换使得*pb成为一个可用于修改bar对象值的指针，它删除const标签。第二个类型转换是非法的，因为它同时尝试将类型从const High *改为const Low *。

提供该运算符的原因是，有时候可能需要这样一个值，它在大多数时候是常量，而有时又是可以修改的。在这种情况下，可以将这个值声明为const，并在需要修改它的时候，使用const_cast。这也可以通过通用类型转换来实现，但通用转换也可能同时改变类型：

```c++
High bar;
const High* pbar = &bar;
...
High* pb = (High *) (pbar);		// valid
Low* pl = (Low *) (pbar);		// also valid
```

由于编程时可能无意间同时改变类型和常量特征，因此使用const_cast运算符更安全。

const_cast不是万能的。它可以修改指向一个值的指针，但修改const值的结果是不确定的。程序清单15.19的简单示例阐明了这一点：

```c++
// constcast.cpp -- using const_cast<>
#include <iostream>
using std::cout;
using std::endl;

void change(const int* pt, int n);

int main()
{
	int pop1 = 38383;
	const int pop2 = 2000;

	cout << "pop1, pop2: " << pop1 << ", " << pop2 << endl;
	change(&pop1, -103);
	change(&pop2, -103);
	cout << "pop1, pop2: " << pop1 << ", " << pop2 << endl;
	// std::cin.get();
	return 0;
}

void change(const int* pt, int n)
{
	int* pc;

	pc = const_cast<int*>(pt);
	*pc += n;
}
```

const_cast运算符可以删除const int* pt中的const，使得编译器能够接受change( )中的语句：

```c++
*pc += n;
```

但由于pop2被声明为const，因此编译器可能禁止修改它，如下面的输出所示：

```
pop1, pop2: 38383, 2000
pop1, pop2: 38280, 2000
```

正如您看到的，调用change( )时，修改了pop1，但没有修改pop2。在change()中，指针被声明为const int *，因此不能用来修改指向的int。指针pc删除了const特征，因此可用来修改指向的值，但仅当指向的值不是const时才可行。因此，pc可用于修改pop1，但不能用于修改pop2。

static_cast运算符的语法与其他类型转换运算符相同：

```c++
static_cast<type-name> (expression);
```

仅当type_name可被隐式转换为expression所属的类型或expression可被隐式转换为type_name所属的类型时，上述转换才是合法的，否则将出错。假设High是Low的基类，而Pond是一个无关的类，则从High到Low的转换、从Low到High的转换都是合法的，而从Low到Pond的转换是不允许的：

```c++
High bar;
Low blow;
...
High* pb = static_cast<High *> (*blow);		// valid upcast
Low* pl = static_cast<Low *> (&bar);		// valid downcast
Pond* pmer = static_cast<Pond *> (&blow);	// invalid, Pond unrelated
```

第一种转换是合法的，因为向上转换可以显示地进行。第二种转换是从基类指针到派生类指针，在不进行显示类型转换的情况下，将无法进行。但由于无需进行类型转换，便可以进行另一个方向的类型转换，因此使用static_cast来进行向下转换是合法的。

同理，由于无需进行类型转换，枚举值就可以被转换为整型，所以可以用static_cast将整型转换为枚举值。同样，可以使用static_cast将double转换为int、将float转换为long以及其他各种数值转换。

reinterpret_cast运算符用于天生危险的类型转换。它不允许删除const，但会执行其他令人生厌的操作。有时程序员必须做一些依赖于实现的、令人生厌的操作，使用reinterpret_cast运算符可以简化对这种行为的跟踪工作。该运算符的语法与另外3个相同：

```c++
reinterpret_cast<type-name> (expression);
```

下面是一个使用示例：

```c++
struct dat {short a; short b;};
long value = 0xA224B118;
dat * pd = reinterpret_cast<dat *> (&value);
cout << hex << pd->a;		// display first 2 bytes of value
```

通常，这样的转换适用于依赖于实现的底层编程技术，是不可移植的。例如，不同系统在存储多字节整型时，可能以不同的顺序存储其中的字节。

然而，reinterprete_cast运算符并不支持所有的类型转换。例如，可以将指针类型转换为足以存储指针表示的整型，但不能将指针转换为更小的整型或浮点型。另一个限制是，不能将函数指针转换为数据指针，反之亦然。

# 第16章 string类和标准模板库

## 16.1 string类

### 16.1.1 构造字符串

size_type是一个依赖于实现的整型，是在头文件string中定义的。string类将string::npos定义为字符串的最大长度，通常为unsigned int的最大值。另外，表格中使用缩写NBTS（null-terminated string）来表示以空字符结束的字符串。

![image-20230312202023550](D:\git\note_md_files\images\image-20230312202023550.png)

![image-20230312202041049](D:\git\note_md_files\images\image-20230312202041049.png)

```c++
// str1.cpp -- introducing the string class
#include <iostream>
#include <string>
// using string constructors

int main()
{
	using namespace std;
	string one("Lottery Winner!");     // ctor #1
	cout << one << endl;               // overloaded <<
	string two(20, '$');               // ctor #2
	cout << two << endl;
	string three(one);                 // ctor #3
	cout << three << endl;
	one += " Oops!";                   // overloaded +=
	cout << one << endl;
	two = "Sorry! That was ";
	three[0] = 'P';
	string four;                       // ctor #4
	four = two + three;                // overloaded +, =
	cout << four << endl;
	char alls[] = "All's well that ends well";
	string five(alls, 20);              // ctor #5
	cout << five << "!\n";
	string six(alls + 6, alls + 10);     // ctor #6
	cout << six << ", ";
	string seven(&five[6], &five[10]); // ctor #6 again
	cout << seven << "...\n";
	string eight(four, 7, 16);         // ctor #7
	// 将four的第八个字符(位置7)开始，将16个字符复制到eight中
	cout << eight << " in motion!" << endl;
	// std::cin.get();
	return 0;
}
```

程序清单16.1中的程序首先演示了可以将string对象初始化为常规的C-风格字符串，然后使用重载的<<运算符来显示它：

```c++
	string one("Lottery Winner!");     // ctor #1
	cout << one << endl;               // overloaded <<
```

接下来的构造函数将string对象two初始化为由20个$字符组成的字符串：

```c++
	string two(20, '$');               // ctor #2
```

复制构造函数将string对象three初始化为string对象one：

```c++
	string three(one);                 // ctor #3
```

重载的+=运算符将字符串“Oops!”附加到字符串one的后面：

```c++
	one += " Oops!";                   // overloaded +=
```

第5个构造函数将一个C-风格字符串和一个整数作为参数，其中的整数参数表示要复制多少个字符：

```c++
	char alls[] = "All's well that ends well";
	string five(alls, 20);              // ctor #5
```

从输出可知，这里只使用了前20个字符（“All's well that ends”）来初始化five对象。正如表16.1指出的，如果字符数超过了C-风格字符串的长度，仍将复制请求数目的字符。所以在上面的例子中，如果用40代替20，将导致15个无用字符被复制到five的结尾处（即构造函数将内存中位于字符串“All's well that ends well”后面的内容作为字符）。

第6个构造函数有一个模板参数：

```c++
template<class Iter> string(Iter begin, Iter end);
```

begin和end将像指针那样，指向内存中两个位置（通常，begin和end可以是迭代器——广泛用于STL中的广义化指针）。构造函数将使用begin和end指向的位置之间的值，对string对象进行初始化。[begin,end)来自数学中，意味着包括begin，但不包括end在内的区间。也就是说，end指向被使用的最后一个值后面的一个位置。请看下面的语句：

```c++
string six(alls + 6, alls + 10);
```

由于数组名相当于指针，所以alls + 6和alls +10的类型都是char *，因此使用模板时，将用类型char *替换Iter。第一个参数指向数组alls中的第一个w，第二个参数指向第一个well后面的空格。因此，six将被初始化为字符串“well”。图16.1说明了该构造函数的工作原理。

![image-20230312202503945](D:\git\note_md_files\images\image-20230312202503945.png)

现在假设要用这个构造函数将对象初始化为另一个string对象（假设为five）的一部分内容，则下面的语句不管用：

```c++
string seven(five + 6, five + 10);
```

原因在于，对象名（不同于数组名）不会被看作是对象的地址，因此five不是指针，所以five + 6是没有意义的。然而，five[6]是一个char值，所以&five[6]是一个地址，因此可被用作该构造函数的一个参数。

```c++
string seven(&five[6], &five[10]);
```

第7个构造函数将一个string对象的部分内容复制到构造的对象中：

```
string eight(four, 7, 16);
```

上述语句从four的第8个字符（位置7）开始，将16个字符复制到eight中。

### 16.1.3 使用字符串

可以确定字符串的长度。size( )和length( )成员函数都返回字符串中的字符数：

```c++
if (snake1.length() == snake2.size())
	cout << "Both strings have the save length.\n";
```

为什么这两个函数完成相同的任务呢？length( )成员来自较早版本的string类，而size( )则是为提供STL兼容性而添加的。

### 16.1.5 字符串种类

string库实际上是基于一个模板类的：

```c++
template<class charT, class traits = char_traits<charT>,
	class Allocator = allocator<charT>>
basic_string {...}
```

模板basic_string有4个具体化，每个具体化都有一个typedef名称：

```c++
typedef basic_string<char> string;
typedef basic_string<wchar_t> wstring;
typedef basic_string<char16_t> u16string;
typedef basic_string<char32_t> u32string;
```

## 16.2 智能指针模板类

### 16.2.1 使用智能指针

这三个智能指针模板（auto_ptr、unique_ptr和shared_ptr）都定义了类似指针的对象，可以将new获得（直接或间接）的地址赋给这种对象。当智能指针过期时，其析构函数将使用delete来释放内存。因此，如果将new返回的地址赋给这些对象，将无需记住稍后释放这些内存：在智能指针过期时，这些内存将自动被释放。图16.2说明了auto_ptr和常规指针在行为方面的差别；shared_ptr和unique_ptr的行为与auto_ptr相同。

![image-20230312204335644](D:\git\note_md_files\images\image-20230312204335644.png)

要创建智能指针对象，必须包含头文件memory，该文件模板定义。然后使用通常的模板语法来实例化所需类型的指针。例如，模板auto_ptr包含如下构造函数：

```c++
template<class X> class auto_ptr {
public:
	explicit auto_ptr(X* p = 0) throw();
}
```

本书前面说过，throw( )意味着构造函数不会引发异常；与auto_ptr一样，throw()也被摒弃。因此，请求X类型的auto_ptr将获得一个指向X类型的auto_ptr：

```c++
auto_ptr<double> pd(new double);
auto_ptr<string> ps(new string);
```

new double是new返回的指针，指向新分配的内存块。它是构造函数auto_ptr<double>的参数，即对应于原型中形参p的实参。同样，new string也是构造函数的实参。其他两种智能指针使用同样的语法：

```c++
unique_ptr<double> pdu(new double);
shared_ptr<string> pss(new string);
```

因此，要转换remodel( )函数，应按下面3个步骤进行：

1. 包含头文件memory；
2. 将指向string的指针替换为指向string的智能指针对象；
3. 删除delete语句。

下面是使用auto_ptr修改该函数的结果：

```C++
#include <memory>
void remodel(std::string& str)
{
	std::auto_ptr<std::string> ps(new std::string(str));
	...
	if (weird_thing())
		throw exception();
	str = *ps;
	// delete ps;	// NO LONGER NEEDED
	return;
}
```

注意到智能指针模板位于名称空间std中。程序清单16.5是一个简单的程序，演示了如何使用全部三种智能指针。要编译该程序，您的编译器必须支持C++11新增的类shared_ptr和unique_ptr。每个智能指针都放在一个代码块内，这样离开代码块时，指针将过期。Report类使用方法报告对象的创建和销毁。

```C++
// smrtptrs.cpp -- using three kinds of smart pointers
#include <iostream>
#include <string>
#include <memory>

class Report
{
private:
	std::string str;
public:
	Report(const std::string s) : str(s) { std::cout << "Object created!\n"; }
	~Report() { std::cout << "Object deleted!\n"; }
	void comment() const { std::cout << str << "\n"; }
};

int main()
{
	{
		std::auto_ptr<Report> ps(new Report("using auto_ptr"));
		ps->comment();   // use -> to invoke a member function
	}
	{
		std::shared_ptr<Report> ps(new Report("using shared_ptr"));
		ps->comment();
	}
	{
		std::unique_ptr<Report> ps(new Report("using unique_ptr"));
		ps->comment();
	}
	// std::cin.get();  
	return 0;
}
```

所有智能指针类都一个explicit构造函数，该构造函数将指针作为参数。因此不需要自动将指针转换为智能指针对象：

```c++
shared_ptr<double> pd;
double* p_reg = new double;
pd = p_reg;								// not allowed (implicit conversion)
pd = shared_ptr<double>(p_reg);			// allowed (explicit conversion)
shared_ptr<double> pshared = p_reg;		// not allowed (implicit conversion)
shared_ptr<double> pshared(p_reg);		// allowed (explicit conversion)
```

由于智能指针模板类的定义方式，智能指针对象的很多方面都类似于常规指针。例如，如果ps是一个智能指针对象，则可以对它执行解除引用操作（* ps）、用它来访问结构成员（ps->puffIndex）、将它赋给指向相同类型的常规指针。还可以将智能指针对象赋给另一个同类型的智能指针对象，但将引起一个问题，这将在下一节进行讨论。

但在此之前，先说说对全部三种智能指针都应避免的一点：

```C++
string vacation("I wandered lonely as a cloud.");
shared_ptr<string> pvac(&vacation);		// NO
```

pvac过期时，程序将把delete运算符用于非堆内存，这是错误的。

就程序清单16.5演示的情况而言，三种智能指针都能满足要求，但情况并非总是这样简单。

### 16.2.2 有关智能指针的注意事项

为何有三种智能指针呢？实际上有4种，但本书不讨论weak_ptr。为何摒弃auto_ptr呢？

先来看下面的赋值语句：

```c++
auto_ptr<string> ps(new string("I reigned lonely as a cloud."));
auto_ptr<string> vocation;
vocation = ps;
```

上述赋值语句将完成什么工作呢？如果ps和vocation是常规指针，则两个指针将指向同一个string对象。这是不能接受的，因为程序将试图删除同一个对象两次——一次是ps过期时，另一次是vocation过期时。要避免这种问题，方法有多种。

- 定义赋值运算符，使之执行深复制。这样两个指针将指向不同的对象，其中的一个对象是另一个对象的副本。
- 建立所有权（ownership）概念，对于特定的对象，只能有一个智能指针可拥有它，这样只有拥有对象的智能指针的构造函数会删除该对象。然后，让赋值操作转让所有权。这就是用于auto_ptr和unique_ptr的策略，但unique_ptr的策略更严格。
- 创建智能更高的指针，跟踪引用特定对象的智能指针数。这称为引用计数（reference counting）。例如，赋值时，计数将加1，而指针过期时，计数将减1。仅当最后一个指针过期时，才调用delete。这是shared_ptr采用的策略。

当然，同样的策略也适用于复制构造函数。

每种方法都有其用途。程序清单16.6是一个不适合使用auto_ptr的示例。

```c++
// fowl.cpp  -- auto_ptr a poor choice
#include <iostream>
#include <string>
#include <memory>

int main()
{
	using namespace std;
	auto_ptr<string> films[5] =
	{
		auto_ptr<string>(new string("Fowl Balls")),
		auto_ptr<string>(new string("Duck Walks")),
		auto_ptr<string>(new string("Chicken Runs")),
		auto_ptr<string>(new string("Turkey Errors")),
		auto_ptr<string>(new string("Goose Eggs"))
	};
	auto_ptr<string> pwin;
	pwin = films[2];   // films[2] loses ownership

	cout << "The nominees for best avian baseball film are\n";
	for (int i = 0; i < 5; i++)
		cout << *films[i] << endl;
	cout << "The winner is " << *pwin << "!\n";
	// cin.get();
	return 0;
}
```

消息core dumped表明，错误地使用auto_ptr可能导致问题（这种代码的行为是不确定的，其行为可能随系统而异）。这里的问题在于，下面的语句将所有权从films[2]转让给pwin：

```c++
pwin = filems[2];		// films[2] loses ownership
```

这导致films[2]不再引用该字符串。在auto_ptr放弃对象的所有权后，便可能使用它来访问该对象。当程序打印films[2]指向的字符串时，却发现这是一个空指针，这显然讨厌的意外。

如果在程序清单16.6中使用shared_ptr代替auto_ptr（这要求编译器支持C++11新增的shared_ptr类），则程序将正常运行。

差别在于程序的如下部分：

这次pwin和films[2]指向同一个对象，而引用计数从1增加到2。在程序末尾，后声明的pwin首先调用其析构函数，该析构函数将引用计数降低到1。然后，shared_ptr数组的成员被释放，对filmsp[2]调用析构函数时，将引用计数降低到0，并释放以前分配的空间。

因此使用shared_ptr时，程序清单16.6运行正常；而使用auto_ptr时，该程序在运行阶段崩溃。如果使用unique_ptr，结果将如何呢？与auto_ptr一样，unique_ptr也采用所有权模型。但使用unique_ptr时，程序不会等到运行阶段崩溃，而在编译器因下述代码行出现错误：

```c++
pwin = films[2];
```

显然，该进一步探索auto_ptr和unique_ptr之间的差别。

### 16.2.3 unique_ptr为何优于auto_ptr

请看下面的语句：

```c++
auto_ptr<string> p1(new string("auto"));
auto_ptr<string> p2;
p2 = p1;
```

在语句#3中，p2接管string对象的所有权后，p1的所有权将被剥夺。前面说过，这是件好事，可防止p1和p2的析构函数试图删除同一个对象；但如果程序随后试图使用p1，这将是件坏事，因为p1不再指向有效的数据。

下面来看使用unique_ptr的情况：

```c++
unique_ptr<string> p2(new string("auto"));
unique_ptr<string> p4;
p4 = p3;
```

编译器认为语句#6非法，避免了p3不再指向有效数据的问题。因此，unique_ptr比auto_ptr更安全（编译阶段错误比潜在的程序崩溃更安全）。

但有时候，将一个智能指针赋给另一个并不会留下危险的悬挂指针。假设有如下函数定义：

```c++
unique_ptr<string> demo(const char* s)
{
	unique_ptr<string> temp(new string(s));
	return temp;
}
```

并假设编写了如下代码：

```c++
unique_ptr<string> ps;
ps = demo("Uniquely special");
```

demo( )返回一个临时unique_ptr，然后ps接管了原本归返回的unique_ptr所有的对象，而返回的unique_ptr被销毁。这没有问题，因为ps拥有了string对象的所有权。但这里的另一个好处是，demo( )返回的临时unique_ptr很快被销毁，没有机会使用它来访问无效的数据。换句话说，没有理由禁止这种赋值。神奇的是，编译器确实允许这种赋值！

总之，程序试图将一个unique_ptr赋给另一个时，如果源unique_ptr是个临时右值，编译器允许这样做；如果源unique_ptr将存在一段时间，编译器将禁止这样做：

```c++
using namespace std;
unique_ptr<string> pu1(new string("Hi ho!"));
unique_ptr<string> pu2;
pu2 = pu1;						// #1 not allowed
unique_ptr<string> pu3;
pu3 = unique_ptr<string>(new string("Yo!"));			// #2 allowed
```

语句#1将留下悬挂的unique_ptr（pul），这可能导致危害。语句#2不会留下悬挂的unique_ptr，因为它调用unique_ptr的构造函数，该构造函数创建的临时对象在其所有权转让给pu后就会被销毁。这种随情况而异的行为表明，unique_ptr优于允许两种赋值的auto_ptr。这也是禁止（只是一种建议，编译器并不禁止）在容器对象中使用auto_ptr，但允许使用unique_ptr的原因。如果容器算法试图对包含unique_ptr的容器执行类似于语句#1的操作，将导致编译错误；如果算法试图执行类似于语句#2的操作，则不会有任何问题。而对于auto_ptr，类似于语句#1的操作可能导致不确定的行为和神秘的崩溃。

当然，您可能确实想执行类似于语句#1的操作。仅当以非智能的方式使用遗弃的智能指针（如解除引用时），这种赋值才不安全。要安全地重用这种指针，可给它赋新值。C++有一个标准库函数std::move()，让您能够将一个unique_ptr赋给另一个。下面是一个使用前述demo()函数的例子，该函数返回一个unique_ptr<string>对象：

```c++
using namespace std;
unique_ptr<string> ps1, ps2;
ps1 = demo("Uniquely special");
ps2 = move(ps1);
ps1 = demo(" and more");
cout << *ps2 << *ps1 << endl;
```

您可能会问，unique_ptr如何能够区分安全和不安全的用法呢？答案是它使用了C++11新增的移动构造函数和右值引用，这将在第18章讨论。

相比于auto_ptr，unique_ptr还有另一个优点。它有一个可用于数组的变体。别忘了，必须将delete和new配对，将delete []和new [ ]配对。模板auto_ptr使用delete而不是delete [ ]，因此只能与new一起使用，而不能与new [ ]一起使用。但unique_ptr有使用new [ ]和delete [ ]的版本：

```c++
std::unique_ptr<double[]> pda(new double(5));
```

### 16.2.4 选择智能指针

应使用哪种智能指针呢？如果程序要使用多个指向同一个对象的指针，应选择shared_ptr。这样的情况包括：有一个指针数组，并使用一些辅助指针来标识特定的元素，如最大的元素和最小的元素；两个对包含都指向第三个对象的指针；STL容器包含指针。很多STL算法都支持复制和赋值操作，这些操作可用于shared_ptr，但不能用于unique_ptr（编译器发出警告）和auto_ptr（行为不确定）。如果您的编译器没有提供shared_ptr，可使用Boost库提供的shared_ptr。

如果程序不需要多个指向同一个对象的指针，则可使用unique_ptr。如果函数使用new分配内存，并返回指向该内存的指针，将其返回类型声明为unique_ptr是不错的选择。这样，所有权将转让给接受返回值的unique_ptr，而该智能指针将负责调用delete。可将unique_ptr存储到STL容器中，只要不调用将一个unique_ptr复制或赋给另一个的方法或算法（如sort( )）。例如，可在程序中使用类似于下面的代码段，这里假设程序包含正确的include和using语句：

```c++
unique_ptr<int> make_int(int n)
{
	return unique_ptr<int>(new int(n));
}
void show(unique_ptr<int>& pi)
{
	cout << *a << ' ';
}
int main()
{
...
	vector<unique_ptr<>int> vp(size);
	for (int i = 0; i < vp.size(); i++)
		vp[i] = make_int(rand() % 1000);	// copy temporary unique_ptr
	vp.push_back(make_int(rand() % 1000));	// ok because arg is tempporary
	for_each(vp.begin(), vp.end(), show);
...
}
```

其中的push_back( )调用没有问题，因为它返回一个临时unique_ptr，该unique_ptr被赋给vp中的一个unique_ptr。另外，如果按值而不是按引用给show( )传递对象，for_each( )语句将非法，因为这将导致使用一个来自vp的非临时unique_ptr初始化pi，而这是不允许的。前面说过，编译器将发现错误使用unique_ptr的企图。

在unique_ptr为右值时，可将其赋给shared_ptr，这与将一个unique_ptr赋给另一个需要满足的条件相同。与前面一样，在下面的代码中，make_int( )的返回类型为unique_ptr<int>：

```c++
unique_ptr<int> pup(make_int(rand() % 1000));		// ok
shared_ptr<int> spp(pup);							// not allowed, pup an lvalue
shared_ptr<int> spr(make_int(rand() % 1000));		// ok
```

模板shared_ptr包含一个显式构造函数，可用于将右值unique_ptr转换为shared_ptr。shared_ptr将接管原来归unique_ptr所有的对象。

在满足unique_ptr要求的条件时，也可使用auto_ptr，但unique_ptr是更好的选择。如果您的编译器没有提供unique_ptr，可考虑使用BOOST库提供的scoped_ptr，它与unique_ptr类似。

## 16.3 标准模板库

Alex Stepanov和Meng Lee在Hewlett-Packard实验室开发了STL，并于1994年发布其实现。ISO/ANSI C++委员会投票同意将其作为C++标准的组成部分。STL不是面向对象的编程，而是一种不同的编程模式——泛型编程（generic programming）。这使得STL在功能和方法方面都很有趣。

### 16.3.2 可对矢量执行的操作

要为vector的double类型规范声明一个迭代器，可以这样做：

```c++
vector<double>::iterator pd;		// pd an iterator
```

假设scores是一个vector<double>对象：

```c++
vector<double> scores;
```

则可以使用迭代器pd执行这样的操作：

```c++
pd = scores.begin();		// have pd point to the first element
*pd = 22.3;		// dereference pd and assign value to first element
++pd;		// make pd point to the next element
```

正如您看到的，迭代器的行为就像指针。顺便说一句，还有一个C++11自动类型推断很有用的地方。例如，可以不这样做：

```c++
vector<double>::iterator pd = scores.begin();
```

而这样做：

```c++
auto pd = scores.begin();		// C++11 automatic type deduction
```

### 16.3.3 对矢量可执行的其他操作

程序员通常要对数组执行很多操作，如搜索、排序、随机排序等。矢量模板类包含了执行这些常见的操作的方法吗？没有！STL从更广泛的角度定义了非成员（non-member）函数来执行这些操作，即不是为每个容器类定义find( )成员函数，而是定义了一个适用于所有容器类的非成员函数find( )。这种设计理念省去了大量重复的工作。例如，假设有8个容器类，需要支持10种操作。如果每个类都有自己的成员函数，则需要定义80（8*10）个成员函数。但采用STL方式时，只需要定
义10个非成员函数即可。在定义新的容器类时，只要遵循正确的指导思想，则它也可以使用已有的10个非成员函数来执行查找、排序等操作。

另一方面，即使有执行相同任务的非成员函数，STL有时也会定义一个成员函数。这是因为对有些操作来说，类特定算法的效率比通用算法高，因此，vector的成员函数swap( )的效率比非成员函数swap( )高，但非成员函数让您能够交换两个类型不同的容器的内容。

下面来看3个具有代表性的STL函数：for_each( )、random_shuffle( )和sort( )。for_each( )函数可用于很多容器类，它接受3个参数。前两个是定义容器中区间的迭代器，最后一个是指向函数的指针（更普遍地说，最后一个参数是一个函数对象，函数对象将稍后介绍）。for_each( )函数将被指向的函数应用于容器区间中的各个元素。被指向的函数不能修改容器元素的值。可以用for_each( )函数来代替for循环。例如，可以将代码：

```c++
vector<Review>::iterator pr;
for (pr = books.begin(); pr != books.end(); pr++)
	ShowReview(*pr);
```

替换为：

```c++
for_each(books.begin(). books.end(), ShowReview);
```

这样可避免显式地使用迭代器变量。

Random_shuffle( )函数接受两个指定区间的迭代器参数，并随机排列该区间中的元素。例如，下面的语句随机排列books矢量中所有元素：

```c++
random_shuffle(books.begin(), books.end());
```

与可用于任何容器类的for_each不同，该函数要求容器类允许随机访问，vector类可以做到这一点。

sort( )函数也要求容器支持随机访问。该函数有两个版本，第一个版本接受两个定义区间的迭代器参数，并使用为存储在容器中的类型元素定义的<运算符，对区间中的元素进行操作。例如，下面的语句按升序对coolstuff的内容进行排序，排序时使用内置的<运算符对值进行比较：

```c++
vector<int> coolstuff;
...
sort(coolstuff.begin() ,coolstuff.end());
```

如果容器元素是用户定义的对象，则要使用sort( )，必须定义能够处理该类型对象的operator<( )函数。例如，如果为Review提供了成员或非成员函数operator<( )，则可以对包含Review对象的矢量进行排序。由于Review是一个结构，因此其成员是公有的，这样的非成员函数将为：

```c++
bool operator<(const Review& r1, const Review& r2)
{
	if (r1.title < r2.title)
		return true;
	else if (r1.title == r2.title && r1.rating < r2.rating)
		return true;
	else
		return false;
}
```

有了这样的函数后，就可以对包含Review对象（如books）的矢量进行排序了：

```c++
sort(books.begin(), books.end());
```

上述版本的operator<( )函数按title成员的字母顺序排序。如果title成员相同，则按照rating排序。然而，如果想按降序或是按rating（而不是title）排序，该如何办呢？可以使用另一种格式的sort( )。它接受3个参数，前两个参数也是指定区间的迭代器，最后一个参数是指向要使用的函数的指针（函数对象），而不是用于比较的operator<( )。返回值可转换为bool，false表示两个参数的顺序不正确。下面是一个例子：

```c++
bool WorseThan(const Review& r1, const Review& r2)
{
	if (r1.rating < r2.rating)
		return true;
	else
		return false;
}
```

有了这个函数后，就可以使用下面的语句将包含Review对象的books矢量按rating升序排列：

```c++
sort(books.begin(), books.end(), WorseThan);
```

### 16.3.4 基于范围的for循环（C++11）

```c++
double prices[5] = {4.99, 10.99, 6.87, 7.99, 8.49};
for (double x : prices)
	cout << x << std::endl;
```

不同于for_each( )，基于范围的for循环可修改容器的内容，诀窍是指定一个引用参数。例如，假设有如下函数：

```c++
void InflateReview(Review& r) {r.rating++};
```

可使用如下循环对books的每个元素执行该函数：

```c++
for (auto& x : books) InflateReview(x);
```

## 16.4 泛型编程

### 16.4.1 为何使用迭代器

为区分++运算符的前缀版本和后缀版本，C++将operator++作为前缀版本，将operator++（int）作为后缀版本；其中的参数永远也不会被用到，所以不必指定其名称。

```c++
class iterator
{
...
	iterator& operator++()		// for ++it
	{
		...
	}
	
	iterator operator++(int)	// for it++
	{
		...
	}
}
```

## 16.5 函数对象

很多STL算法都使用函数对象——也叫函数符（functor）。函数符是可以以函数方式与( )结合使用的任意对象。这包括函数名、指向函数的指针和重载了( )运算符的类对象（即定义了函数operator( )( )的类）。例如，可以像这样定义一个类：

```c++
class Linear
{
private :
	double slope;
	double y0;
public:
	Linear(double s1_ = 1, double y_ = 0)
		: slope(s1_), y0(y_) {}
	double operator()(double x) {return y0 + slope * x}
} 
```

这样，重载的( )运算符将使得能够像函数那样使用Linear对象：

```c++
Linear f1;
Linear f2(2.5, 10.0);
double y1 = f1(12.5);		// f1.operator()(12.5)
double y2 = f2(0.4);
```

其中y1将使用表达式0 + 1 * 12.5来计算，y2将使用表达式10.0 + 2.5 * 0.4来计算。在表达式y0 + slope * x中，y0和slope的值来自对象的构造函数，而x的值来自operator( ) ( )的参数。

还记得函数for_each吗？它将指定的函数用于区间中的每个成员：

```c++
for_each(books.begin(), books.end(), ShowReview);
```

通常，第3个参数可以是常规函数，也可以是函数符。实际上，这提出了一个问题：如何声明第3个参数呢？不能把它声明为函数指针，因为函数指针指定了参数类型。由于容器可以包含任意类型，所以预先无法知道应使用哪种参数类型。STL通过使用模板解决了这个问题。for_each的原型看上去就像这样：

```c++
template<class InputIterator, class Function>
Functioni for_each(InputIterator first, InputIterator last, Function f);
```

ShowReview( )的原型如下：

```c++
void ShowFunction(const Review&);
```

这样，标识符ShowReview的类型将为void(*)(const Review &)，这也是赋给模板参数Function的类型。对于不同的函数调用，Function参数可以表示具有重载的( )运算符的类类型。最终，for_each( )代码将具有一个使用f( )的表达式。在ShowReview( )示例中，f是指向函数的指针，而f( )调用该函数。如果最后的for_each( )参数是一个对象，则f( )将是调用其重载的( )运算符的对象。

### 16.5.1 函数符概念

正如STL定义了容器和迭代器的概念一样，它也定义了函数符概念。

- 生成器（generator）是不用参数就可以调用的函数符。
- 一元函数（unary function）是用一个参数可以调用的函数符。
- 二元函数（binary function）是用两个参数可以调用的函数符。

例如，提供给for_each( )的函数符应当是一元函数，因为它每次用于一个容器元素。

当然，这些概念都有相应的改进版：

- 返回bool值的一元函数是谓词（predicate）
- 返回bool值的二元函数是二元谓词（binary predicate）。

一些STL函数需要谓词参数或二元谓词参数。例如，程序清单16.9使用了sort( )的这样一个版本，即将二元谓词作为其第3个参数：

```c++
bool WorseThan(const Review& r1, const Review& r2);
...
sort(books.begin(), books.end(), WorseThan);
```

list模板有一个将谓词作为参数的remove_if( )成员，该函数将谓词应用于区间中的每个元素，如果谓词返回true，则删除这些元素。例如，下面的代码删除链表three中所有大于100的元素：

```c++
bool tooBig(int n) {return n > 100;}
list<int> scores;
...
scores.remove_if(tooBig);
```

最后这个例子演示了类函数符适用的地方。假设要删除另一个链表中所有大于200的值。如果能将取舍值作为第二个参数传递给tooBig()，则可以使用不同的值调用该函数，但谓词只能有一个参数。然而，如果设计一个TooBig类，则可以使用类成员而不是函数参数来传递额外的信息：

```c++
template<class T>  // functor class defines operator()()
class TooBig
{
private:
	T cutoff;
public:
	TooBig(const T& t) : cutoff(t) {}
	bool operator()(const T& v) { return v > cutoff; }
};
```

这里，一个值（V）作为函数参数传递，而第二个参数（cutoff）是由类构造函数设置的。有了该定义后，就可以将不同的TooBig对象初始化为不同的取舍值，供调用remove_if( )时使用。程序清单16.15演示了这种技术。

```c++
// functor.cpp -- using a functor
#include <iostream>
#include <list>
#include <iterator>
#include <algorithm>

template<class T>  // functor class defines operator()()
class TooBig
{
private:
	T cutoff;
public:
	TooBig(const T& t) : cutoff(t) {}
	bool operator()(const T& v) { return v > cutoff; }
};

void outint(int n) { std::cout << n << " "; }

int main()
{
	using std::list;
	using std::cout;
	using std::endl;
	using std::for_each;
	using std::remove_if;

	TooBig<int> f100(100); // limit = 100
	int vals[10] = { 50, 100, 90, 180, 60, 210, 415, 88, 188, 201 };
	list<int> yadayada(vals, vals + 10); // range constructor
	list<int> etcetera(vals, vals + 10);

	// C++0x can use the following instead
   //  list<int> yadayada = {50, 100, 90, 180, 60, 210, 415, 88, 188, 201};
   //  list<int> etcetera {50, 100, 90, 180, 60, 210, 415, 88, 188, 201};

	cout << "Original lists:\n";
	for_each(yadayada.begin(), yadayada.end(), outint);
	cout << endl;
	for_each(etcetera.begin(), etcetera.end(), outint);
	cout << endl;
	yadayada.remove_if(f100);               // use a named function object
	etcetera.remove_if(TooBig<int>(200));   // construct a function object
	cout << "Trimmed lists:\n";
	for_each(yadayada.begin(), yadayada.end(), outint);
	cout << endl;
	for_each(etcetera.begin(), etcetera.end(), outint);
	cout << endl;
	// std::cin.get();
	return 0;
}
```

假设已经有了一个接受两个参数的模板函数：

```c++
template <class T>
bool tooBig(const T& val, const T& lim)
{
	return val > lim;
}
```

则可以使用类将它转换为单个参数的函数对象：

```c++
template<class T>  // functor class defines operator()()
class TooBig2
{
private:
	T cutoff;
public:
	TooBig2(const T& t) : cutoff(t) {}
	bool operator()(const T& v) { return tooBig<T>(v, cutoff); }
};
```

即可以这样做：

```c++
TooBig2<int> tB100(100);
int x;
cin >> x;
if (tB100(x))	// same as if (tooBig(x, 100))
...
```

因此，调用tB100(x)相当于调用tooBig(x, 100)，但两个参数的函数被转换为单参数的函数对象，其中第二个参数被用于构建函数对象。简而言之，类函数符TooBig2是一个函数适配器，使函数能够满足不同的接口。

### 16.5.2 预定义的函数符

STL定义了多个基本函数符，它们执行诸如将两个值相加、比较两个值是否相等操作。提供这些函数对象是为了支持将函数作为参数的STL函数。例如，考虑函数transform( )。它有两个版本。第一个版本接受4个参数，前两个参数是指定容器区间的迭代器（现在您应该已熟悉了这种方法），第3个参数是指定将结果复制到哪里的迭代器，最后一个参数是一个函数符，它被应用于区间中的每个元素，生成结果中的新元素。例如，请看下面的代码：

```c++
const int LIM = 5;
double arr1[LIM] = {36, 39, 42, 45, 48};
vector<double> gr8(arr1, arr1 + LIM);
ostream_iterator<double, char> out(cout, " ");
transform(gr8.begin(), gr8.end(), out, sqrt);
```

上述代码计算每个元素的平方根，并将结果发送到输出流。目标迭代器可以位于原始区间中。例如，将上述示例中的out替换为gr8.begin( )后，新值将覆盖原来的值。很明显，使用的函数符必须是接受单个参数的函数符。

第2种版本使用一个接受两个参数的函数，并将该函数用于两个区间中元素。它用另一个参数（即第3个）标识第二个区间的起始位置。例如，如果m8是另一个vector<double>对象，mean（double，double）返回两个值的平均值，则下面的的代码将输出来自gr8和m8的值的平均值：

```c++
transform(gr8.begin(), gr8.end(), m8.begin(), out, mean);
```

现在假设要将两个数组相加。不能将+作为参数，因为对于类型double来说，+是内置的运算符，而不是函数。可以定义一个将两个数相加的函数，然后使用它：

```c++
double add(double x, double y) {return x + y;}
...
transform(gr8.begin(), gr8.end(), m8.begin(), out, add);
```

然而，这样必须为每种类型单独定义一个函数。更好的办法是定义一个模板（除非STL已经有一个模板了，这样就不必定义）。头文件functional（以前为function.h）定义了多个模板类函数对象，其中包括plus< >( )。

可以用plus< >类来完成常规的相加运算：

```c++
#include <functional>
...
plus<double> add;		// create a plus<double> object
double y = add(2.2, 3.4);	// using plus<double>::operator()()
```

它使得将函数对象作为参数很方便：

```c++
transform(gr8.begin(), gr8.end(), m8.begin(), out, plus<double>());
```

这里，代码没有创建命名的对象，而是用plus<double>构造函数构造了一个函数符，以完成相加运算（括号表示调用默认的构造函数，传递给transform( )的是构造出来的函数对象）。

对于所有内置的算术运算符、关系运算符和逻辑运算符，STL都提供了等价的函数符。表16.12列出了这些函数符的名称。它们可以用于处理C++内置类型或任何用户定义类型（如果重载了相应的运算符）。

### 16.5.3 自适应函数符和函数适配器

表16.12列出的预定义函数符都是自适应的。实际上STL有5个相关的概念：自适应生成器（adaptable generator）、自适应一元函数（adaptable unary function）、自适应二元函数（adaptable binary function）、自适应谓词（adaptable predicate）和自适应二元谓词（adaptable binary predicate）。

使函数符成为自适应的原因是，它携带了标识参数类型和返回类型的typedef成员。这些成员分别是result_type、first_argument_type和second_argument_type，它们的作用是不言自明的。例如，plus<int>对象的返回类型被标识为plus<int>::result_type，这是int的typedef。

函数符自适应性的意义在于：函数适配器对象可以使用函数对象，并认为存在这些typedef成员。例如，接受一个自适应函数符参数的函数可以使用result_type成员来声明一个与函数的返回类型匹配的变量。

STL提供了使用这些工具的函数适配器类。例如，假设要将矢量gr8的每个元素都增加2.5倍，则需要使用接受一个一元函数参数的transform( )版本，就像前面的例子那样：

```c++
transform(gr8.begin(), gr8.end(), out, sqrt);
```

multiplies( )函数符可以执行乘法运行，但它是二元函数。因此需要一个函数适配器，将接受两个参数的函数符转换为接受1个参数的函数符。前面的TooBig2示例提供了一种方法，但STL使用binder1st和binder2nd类自动完成这一过程，它们将自适应二元函数转换为自适应一元函数。

来看binder1st。假设有一个自适应二元函数对象f2( )，则可以创建一个binder1st对象，该对象与一个将被用作f2( )的第一个参数的特定值（val）相关联：

```c++
binder1st(f2, val) f1;
```

这样，使用单个参数调用f1(x)时，返回的值与将val作为第一参数、将f1( )的参数作为第二参数调用f2( )返回的值相同。即f1(x)等价于f2(val, x)，只是前者是一元函数，而不是二元函数。f2( )函数被适配。同样，仅当f2( )是一个自适应函数时，这才能实现。

看上去有点麻烦。然而，STL提供了函数bind1st( )，以简化binder1st类的使用。可以问其提供用于构建binder1st对象的函数名称和值，它将返回一个这种类型的对象。例如，要将二元函数multiplies( )转换为将参数乘以2.5的一元函数，则可以这样做：

```c++
bind1st(multiplies<double>(), 2.5);
```

因此，将gr8中的每个元素与2.5相乘，并显示结果的代码如下：

```c++
transform(gr8.begin(), gr8.end(), out,
	bind1st(multiplies<double>(), 2.5));
```

binder2nd类与此类似，只是将常数赋给第二个参数，而不是第一个参数。它有一个名为bind2nd的助手函数，该函数的工作方式类似于bind1st。

## 16.7 其他库

### 16.7.1 vector、valarray和array

您可能会问，C++为何提供三个数组模板：vector、valarray和array。这些类是由不同的小组开发的，用于不同的目的。vector模板类是一个容器类和算法系统的一部分，它支持面向容器的操作，如排序、插入、重新排列、搜索、将数据转移到其他容器中等。而valarray类模板是面向数值计算的，不是STL的一部分。例如，它没有push_back( )和insert( )方法，但为很多数学运算提供了一个简单、直观的接口。最后，array是为替代内置数组而设计的，它通过提供更好、更安全的接口，让数组更紧凑，效率更高。array表示长度固定的数组，因此不支持push_back( )和insert( )，但提供了多个STL方法，包括begin( )、end( )、rbegin( )和rend( )，这使得很容易将STL算法用于array对象。

### 16.7.2 模板initializer_list（C++11）

模板initializer_list是C++11新增的。您可使用初始化列表语法将STL容器初始化为一系列值：

```c++
std::vector<double> payments {45.99, 39.23, 19.95, 89.01};
```

这将创建一个包含4个元素的容器，并使用列表中的4个值来初始化这些元素。这之所以可行，是因为容器类现在包含将
initializer_list<T>作为参数的构造函数。例如，vector<double>包含一个将initializer_list<double>作为参数的构造函数，因此上述声明与下面的代码等价：

```c++
std::vector<double> payments({45.99, 39.23, 19.95, 89.01});
```

这里显式地将列表指定为构造函数参数。

通常，考虑到C++11新增的通用初始化语法，可使用表示法{}而不是()来调用类构造函数：

```c++
shared_ptr<double> pd {new double};	// ok to use {} instead of ()
```

但如果类也有接受initializer_list作为参数的构造函数，这将带来问题：

```c++
std::vector<int> vi{10};	// ?
```

这将调用哪个构造函数呢？

```c++
std::vector<int> vi(10);
std::vector<int> vi({10});
```

答案是，如果类有接受initializer_list作为参数的构造函数，则使用语法{}将调用该构造函数。因此在这个示例中，对应的是情形B。

### 16.7.3 使用initializer_list

要在代码中使用initializer_list对象，必须包含头文件initializer_list。这个模板类包含成员函数begin( )和end( )，您可
使用这些函数来访问列表元素。它还包含成员函数size( )，该函数返回元素数。程序清单16.22是一个简单的initializer_list使用示例，它要求编译器支持C++11新增的initializer_list。

```c++
// ilist.cpp  -- use initializer_list
#include <iostream>
#include <initializer_list>

double sum(std::initializer_list<double> il);
double average(const std::initializer_list<double>& ril);

int main()
{
	using std::cout;

	cout << "List 1: sum = " << sum({ 2,3,4 })
		<< ", ave = " << average({ 2,3,4 }) << '\n';
	std::initializer_list<double> dl = { 1.1, 2.2, 3.3, 4.4, 5.5 };
	cout << "List 2: sum = " << sum(dl)
		<< ", ave = " << average(dl) << '\n';
	dl = { 16.0, 25.0, 36.0, 40.0, 64.0 };
	cout << "List 3: sum = " << sum(dl)
		<< ", ave = " << average(dl) << '\n';
	// std::cin.get();
	return 0;
}

double sum(std::initializer_list<double> il)
{
	double tot = 0;
	for (auto p = il.begin(); p != il.end(); p++)
		tot += *p;
	return tot;
}

double average(const std::initializer_list<double>& ril)
{
	double tot = 0;
	int n = ril.size();
	double ave = 0.0;

	if (n > 0)
	{
		for (auto p = ril.begin(); p != ril.end(); p++)
			tot += *p;
		ave = tot / n;
	}
	return ave;
}
```

可按值传递initializer_list对象，也可按引用传递，如sum()和average()所示。这种对象本身很小，通常是两个指针（一个指向开头，一个指向末尾的下一个元素），也可能是一个指针和一个表示元素数的整数，因此采用的传递方式不会带来重大的性能影响。STL按值传递它们。

函数参数可以是initializer_list字面量，如{2, 3, 4}，也可以是initializer_list变量，如dl。

initializer_list的迭代器类型为const，因此您不能修改initializer_list中的值：

```c++
*dl.begin() = 2011.6;		// not allowed
```

但正如程序清单16.22演示的，可以将一个initializer_list赋给另一个initializer_list：

```c++
dl = { 16.0, 25.0, 36.0, 40.0, 64.0 };
```

然而，提供initializer_list类的初衷旨在让您能够将一系列值传递给构造函数或其他函数。

# 第17章 输入、输出和文件

## 17.1 C++输入和输出概述

首先看一看C++ I/O的概念框架。

### 17.1.1 流和缓冲区

C++程序把输入和输出看作字节流。输入时，程序从输入流中抽取字节；输出时，程序将字节插入到输出流中。对于面向文本的程序，每个字节代表一个字符，更通俗地说，字节可以构成字符或数值数据的二进制表示。输入流中的字节可能来自键盘，也可能来自存储设备（如硬盘）或其他程序。同样，输出流中的字节可以流向屏幕、打印机、存储设备或其他
程序。流充当了程序和流源或流目标之间的桥梁。这使得C++程序可以以相同的方式对待来自键盘的输入和来自文件的输入。C++程序只是检查字节流，而不需要知道字节来自何方。同理，通过使用流，C++程序处理输出的方式将独立于其去向。因此管理输入包含两步：

- 将流与输入去向的程序关联起来。
- 将流与文件连接起来。

换句话说，输入流需要两个连接，每端各一个。文件端部连接提供了流的来源，程序端连接将流的流出部分转储到程序中（文件端连接可以是文件，也可以是设备，如键盘）。同样，对输出的管理包括将输出流连接到程序以及将输出目标与流关联起来。这就像将字节（而不是水）引入到水管中（参见图17.1）。

![image-20230326172639574](D:\git\note_md_files\images\image-20230326172639574.png)

通常，通过使用缓冲区可以更高效地处理输入和输出。缓冲区是用作中介的内存块，它是将信息从设备传输到程序或从程序传输给设备的临时存储工具。通常，像磁盘驱动器这样的设备以512字节（或更多）的块为单位来传输信息，而程序通常每次只能处理一个字节的信息。缓冲区帮助匹配这两种不同的信息传输速率。例如，假设程序要计算记录在硬盘文件中
的金额。程序可以从文件中读取一个字符，处理它，再从文件中读取下一个字符，再处理，依此类推。从磁盘文件中每次读取一个字符需要大量的硬件活动，速度非常慢。缓冲方法则从磁盘上读取大量信息，将这些信息存储在缓冲区中，然后每次从缓冲区里读取一个字节。因为从内存中读取单个字节的速度比从硬盘上读取快很多，所以这种方法更快，也更方便。当然，到达缓冲区尾部后，程序将从磁盘上读取另一块数据。这种原理与水库在暴风雨中收集几兆加仑流量的水，然后以比较文明的速度给您家里供水是一样的（见图17.2）。输出时，程序首先填满缓冲区，然后把整块数据传输给硬盘，并清空缓冲区，以备下一批输出使用。这被称为刷新缓冲区（flushing the buffer）。

![image-20230326172725020](D:\git\note_md_files\images\image-20230326172725020.png)

键盘输入每次提供一个字符，因此在这种情况下，程序无需缓冲区来帮助匹配不同的数据传输速率。然而，对键盘输入进行缓冲可以让用户在将输入传输给程序之前返回并更正。C++程序通常在用户按下回车键时刷新输入缓冲区。这是为什么本书的例子没有一开始就处理输入，而是等到用户按下回车键后再处理的原因。对于屏幕输出，C++程序通常在用户发送换行符时刷新输出缓冲区。程序也可能会在其他情况下刷新输入，例如输入即将到来时，这取决于实现。也就是说，当程序到达输入语句时，它将刷新输出缓冲区中当前所有的输出。与ANSI C一致的C++实现是这样工作的。

## 17.3 使用cin进行输入

### 17.3.1 cin>>如何检查输入

不同版本的抽取运算符查看输入流的方法是相同的。它们跳过空白（空格、换行符和制表符），直到遇到非空白字符。

![image-20230326173825484](D:\git\note_md_files\images\image-20230326173825484.png)



假设键入下面的字符：

-123Z

运算符将读取字符-、1、2和3，因为它们都是整数的有效部分。但Z字符不是有效字符，因此输入中最后一个可接受的字符是3。Z将留在输入流中，下一个cin语句将从这里开始读取。与此同时，运算符将字符序列-123转换为一个整数值，并将它赋给elevation。

### 17.3.3 其他istream类方法

- 方法get(char&)和get(void)提供不跳过空白的单字符输入功能；
- 函数get(char *, int, char)和getline(char *, int, char)在默认情况下读取整行而不是一个单词。

它们被称为非格式化输入函数（unformatted input functions），因为它们只是读取字符输入，而不会跳过空白，也不进行数据转换。

#### 1．单字符输入

在使用char参数或没有参数的情况下，get( )方法读取下一个输入字符，即使该字符是空格、制表符或换行符。get(char & ch)版本将输入字符赋给其参数，而get(void)版本将输入字符转换为整型（通常是int），并将其返回。

```c++
char c1, c2, c3;
cin.get(c1).get(c2) >> c3;

char c1, c2, c3;

c1 = cin.get();
c2 = cin.get();
c3 = cin.get();
```

#### 2．采用哪种单字符输入形式

cin.get(void)可以替换c语言的getchar()

#### 3．字符串输入：getline( )、get( )和ignore( )

```c++
istream& get(char*, int, char);
istream& get(char*, int);
istream& getline(char*, int, char);
istream& getline(char*, int);
```

第一个参数是用于放置输入字符串的内存单元的地址。第二个参数比要读取的最大字符数大1（额外的一个字符用于存储结尾的空字符，以便将输入存储为一个字符串）。第3个参数指定用作分界符的字符，只有两个参数的版本将换行符用作分界符。上述函数都在读取最大数目的字符或遇到换行符后为止。

例如，下面的代码将字符输入读取到字符数组line中：

```c++
char line[50];
cin.get(line, 50);
```

cin.get( )函数将在到达第49个字符或遇到换行符（默认情况）后停止将输入读取到数组中。get( )和getline( )之间的主要区别在于，get()将换行符留在输入流中，这样接下来的输入操作首先看到是将是换行符，而gerline( )抽取并丢弃输入流中的换行符。



















