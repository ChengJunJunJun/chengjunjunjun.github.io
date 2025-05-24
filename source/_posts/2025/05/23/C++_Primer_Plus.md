---
title: 《C++ Primer Plus》学习笔记
date: 2025-05-23
last_modified: 2025-05-23
author: Cheng Jun
desc: 
tags: [C++, 进阶]
categories: Technical sharing
---
#### 头文件提供的库函数
1. **`cstring`** - strlen( ) ; strcpy( ) ; strcat( ) ; strcmp( )
2. **`string`** - size( );

#### 第四章 复合类型
1. 使用`delete`和`new`是需要注意，通用的格式为`type_name * pointer_name = new type_name [num_elements]`。
- 不要使用 delete 来释放不是 new 分配的内存
- 不要使用 delete 释放同一个内存块两次
- 如果使用 new [ ]为数组分配内存，则应使用 delete [ ]来释放
- 如果使用 new  为一个实体分配内存，则应使用 delete （没有方括号） 来释放
- 对空指针应用 delete 是安全的
2. 创建**动态数组**时，指针指向的是数组的**第一个**元素，因为指针是变量，所以可以修改他的值。指针加 1 其实是增加地址的字节数。
3. C++ 将**数组名**解释为地址，可以用相同的方式使用**指针名**和**数组名**。
4. 枚举是对**整数**的枚举，本质上也算一种类型。
5. `cout`对`char*`类型有特殊的重载,会直接输出字符串，对于其他类型的指针（如`int*`、`float*`等），`cout`才会输出地址。
6. `const`是运行是常量，`constexpr`是编译时常量，值必须在编译时就能确定。

#### 第五章 循环和关系表达式
1. `i++` $\Rightarrow$ `i = i + 1`; `i++`返回的表达式的值是`i`,`++i`返回的表达式的值是`i+1`。
2. 关系运算符的优先级比算数运算符低。再一次提醒**数组名是数组的地址**。
3. 表达式**一定**会返回一个值。