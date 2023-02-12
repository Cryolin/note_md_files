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

















