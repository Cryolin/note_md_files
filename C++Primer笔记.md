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
最后，ANSI/ISO C++标准对那些抱怨必须在ma·in( )函数最后包含一条返回语句过于繁琐的人做出了让步。如果编译器到达main( )函数末尾时没有遇到返回语句，则认为main( )函数以如下语句结尾：

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

但const比#defien好。首先，它能够明确指定类型。其次，可以使用C++的作用域规则将定义限制在特定的函数或文件中（作用域规则描述了名称在各种模块中的可知程度，将在第9章讨论）。第三，可以将const用于更复杂的类型，如第4章将介绍的数组和结构。

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

C++不能将一个数组赋值给另一个数组，但是java可以。

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
char cat[4] = ['c', 'a', 't', '\0'];	// a string
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

### 4.3.3 string类的其他操作

在C++新增string类之前，程序员也需要完成诸如给字符串赋值等工作。对于C-风格字符串，程序员使用C语言库中的函数来完成这些任务。头文件cstring（以前为string.h）提供了这些函数。例如，可以使用函数strcpy( )将字符串复制到字符数组中，使用函数strcat( )将
字符串附加到字符数组末尾：  

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
