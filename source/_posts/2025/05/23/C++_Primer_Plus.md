---
title: 《C++ Primer Plus》学习笔记
date: 2025-05-23
last_modified: 2025-05-26
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
expression1 ? expression2 : expression3
// switch 通用格式
switch (integer-expression){
    case label1 : statement(s)
                  break
    case label2 : statement(s)
    ...
    default     : statement(s)
}
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