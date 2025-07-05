---
title: 《C++ Primer Plus》学习笔记
date: 2025-05-23
last_modified: 2025-05-27
author: Cheng Jun
desc: 
tags: [C++, 进阶]
categories: Technical sharing
---
### 头文件提供的库函数
1. **`cstring`** - strlen( ) ; strcpy( ) ; strcat( ) ; strcmp( )
2. **`string`** - size( );
3. **`ctime`** - clock()
4. **`climits`** 包括很多符号常量 -- `INT_MAX`;`INT_MIN`;
5. **`cstdlib`** exit()
6. **`cctype`** 它可以简化诸如确定字符是否为大写字母、数字、标点符号等工作

### 清单

```C++
// if 通用格式
if (test-condition)
    statement
// if-else 通用格式：
if (test-condition)
    statement1
else
    statement2
// ？：运算符
expression1 ? expression2 : expression3 // 如果1为true，则为2，否则为3!
// switch 通用格式
switch (integer-expression){
    case label1 : statement(s);
                  break
    case label2 : statement(s);
    ...
    default     : statement(s); 
}
// 数组处理函数 常用编写方式
void f_modify(double ar[], int n);
void f_no_change(const double ar[], int n);
```

### 程序应该避免的问题
1. 防止丢失数据；防止发生内存泄露。

#### 第四章 复合类型
1. 使用`delete`和`new`是需要注意，通用的格式为`type_name * pointer_name = new type_name [num_elements]`。
- 不要使用 delete 来释放不是 new 分配的内存
- 不要使用 delete 释放同一个内存块两次
- 如果使用 new [ ]为数组分配内存，则应使用 delete [ ]来释放
- 如果使用 new  为一个实体分配内存，则应使用 delete （没有方括号） 来释放
- 对空指针应用 delete 是安全的
2. 创建**动态数组**时，指针指向的是数组的**第一个**元素，因为指针是变量，所以可以修改他的值。指针加 1 其实是增加地址的字节数。
3. C++ 将**数组名**解释为地址，可以用相同的方式使用**指针名**和**数组名**。**指向数组第一个元素的指针**。

```C++
// 方法1：使用数组下标（推荐）
p[i].generator    // ✓ 正确 p[i] 相当于 *(p + i),所以 p[i]是对象。
p[i]->generator   // ✗ 错误，p[i] 是对象不是指针

// 方法2：使用指针算术
(p + i)->generator  // ✓ 正确，(p + i) 是指针
(*(p + i)).generator // ✓ 正确，等价于 p[i].generator
```

4. 枚举是对**整数**的枚举，本质上也算一种类型。
5. `cout`对`char*`类型有特殊的重载,会直接输出字符串，对于其他类型的指针（如`int*`、`float*`等），`cout`才会输出地址。
6. `const`是运行是常量，`constexpr`是编译时常量，值必须在编译时就能确定。

#### 第五章 循环和关系表达式
1. `i++` $\Rightarrow$ `i = i + 1`; `i++`返回的表达式的值是`i`,`++i`返回的表达式的值是`i+1`。
2. 关系运算符的优先级比算数运算符低。再一次提醒**数组名是数组的地址**。
3. 表达式**一定**会返回一个值。
4. `cin.get( )`是读取下一个字符。但是`get()`并不会读取和丢弃换行符，会将其留在输入队列中。`getline()`会读取和丢弃换行符。
5. P115页文件尾(EOF)条件部分,有点晦涩难懂。
6. `getline(cin, string_variable)` 是读取包含空格的字符串的正确方式.

#### 第六章 分支语句和逻辑运算符
1. C++提供三种逻辑运算符,分别是 **`or(||) and(&&) not(!)`**。`|| &&`的优先级比关系运算符的低,所以不需要在表达式中使用括号。`!`的优先级是最高的,记得一定要用括号。`&&` 优先级高于 `||`, 
2. `: , || &&` 是顺序点。
3. `switch` 和 `while` 语句都会将枚举量提升为`int`类型。
4. `break`适用于`switch`和任何循环语句,`continue`语句用于循环中,让程序跳过循环体中余下的代码,并开始新一轮的循环。(还有`goto`语句,但是不常用,最好是不用。)
5. **字符是不需要转换**的，因为字符直接用字符编码，但是整数，浮点数这些类型的数据，对于输入的字符，需要将输入的字符转换成为**值的二进制编码**。
6. 补充：cin >> ch, cin.get(), cin.getline()的一些区别，主要是关于怎么处理换行符的。
- 回车键本身不是换行符，但按下回车键后会产生换行符。不同的操作系统按下回车键产生的不一样，但是在C++输入流会统一处理成为`\n`换行符。
- 对于 >> 操作符，**跳过前导空白字符**：包括空格、制表符、换行符等；**读取数据直到遇到空白字符**：遇到空格、制表符、换行符时停止读取；**换行符留在输入缓冲区中**：不会被消费掉。
- 行输入函数（getline），**读取整行内容**：直到遇到换行符；**消费换行符**：读取并丢弃换行符，但不包含在结果字符串中。
- 字符输入函数get(), **get() 函数会读取包括换行符在内的任何字符**。
7. C++中cin输入流的状态机制。cin对象内部维护着几个状态标志位,输入后会进行状态判断，返回Ture,False和其他的错误。下面是完整的错误处理模版:
```cpp
#include <limits>  // 需要这个头文件

double temp;
cin >> temp;

if (!cin) {
    cin.clear();  // 1. 清除错误标志位
    cin.ignore(numeric_limits<streamsize>::max(), '\n');  // 2. 清空缓冲区
    cout << "输入错误，已清理\n";
}
```

#### 函数--C++的编程模块（25年6月21日重新捡起）
1. 分清`int * ar1 [4]`和`int (* ar2)[4]`的区别；
- `int * ar1 [4]`其中ar1是一个数组，包含4个指向int的指针。`ar1 [4]`是一个数组，里面四个元素都是指针。
- `int (* ar2)[4]`比如 `int * p = &age` 其中，p是指针，用*来声明一个指针，这个指针指向的是age的地址。反过来看`int (* ar2)[4]`因为有括号，所以\*声明了一个ar2指针，因为数组名就是指针，所以（\* ar2）和 data 等价。
```cpp
// 方式1：指针形式（推荐）
int sum(int (*ar2)[4], int size);

// 方式2：数组形式（等价但可读性更强）
int sum(int ar2[][4], int size);
```