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

在C++中，每个表达式都有值。

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
typedef double real;     // makes real another name for double
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

recycle(&ink)调用与#1模板匹配，匹配时将Type解释为blot *。recycle（&ink）函数调用也与#2模板匹配，这次Type被解释为blot。因此将两个隐式实例——recycle<blot *>(blot *)和recycle <blot>(blot *)发送到可行函数池中。

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

引用声明使用关键字extern，且不进行初始化；否则，声明为定义，导致分配存储空间：

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

这里，flame指的是element:: fire::flame。同样，可以使用下面的using编译指令使内部的名称可用：

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

