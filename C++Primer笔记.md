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

```
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

当判定表达式的值这种操作改变了内存中数据的值时，我们说表达式有副作用（sideeffect）。

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

由于C++将C-风格字符串视为地址，因此如果使用关系运算符来比较它们，将无法得到满意的结果。相反，应使用C-风格字符串库中的strcmp( )函数来比较。该函数接受两个字符串地址作为参数。这意味着参数可以是指针、字符串常量或字符数组名。如果两个字符串相同，该函数将返回零；如果第一个字符串按字母顺序排在第二个字符串之前，则strcmp( )将返回一个负数值；如果第一个字符串按字母顺序排在第二个字符串之后，则strcpm( )将返回一个正数值。

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

cin.get（char）成员函数调用通过返回转换为false的bool值来指出已到达EOF，而cin.get( )成员函数调用则通过返回EOF值来指出已到达EOF，EOF是在文件iostream中定义的。

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

输入行中的第一个字符被赋给ch。在这里，第一个字符是数字3，其字符编码（二进制）被存储在变量ch中。输入和目标变量都是字符，因此不需要进行转换。注意，这里存储的数值3，而是字符3的编码。执行上述输入语句后，输入队列中的下一个字符为字符8，下一个输入操作将对其进行处理。

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

文件输入的库<fstream>与I/O库<iostram>的使用极其类似。

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

指针cookies和cookies+ 20定义了区间。首先，数组名cookies指向第一个元素。表达式cookies+ 19指向最后一个元素（即
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

### 7.6 函数和结构

本节提到了三个传参的方式：直接传递，地址传递和引用传递，其中，直接传递会在函数创建一份拷贝，造成额外的开销。

## 7.10 函数指针

这段内容直接看原书。
