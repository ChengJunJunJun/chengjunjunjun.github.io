---
title: 《流畅的Python》学习笔记
date: 2025-05-22
last_modified: 2025-05-22
author: Cheng Jun
desc: 感觉自己对于python，还有很多地方需要进一步的学习，有些内容是需要结合项目一起学习的，《流畅的python》是我个人觉得比较好的一本python书籍。希望自己在2个月内读完，并做一点笔记。
tags: [Python, 进阶]
categories: Technical sharing
---

#### 第四部分 面向对象的惯用法
##### 8，对象引用，可变性和垃圾回收
1. **变量**不是盒子，**对象**在赋值之前就创建了，所以在对变量赋值的过程中，实际上就是在给变量贴**标签**，顺理成章的可以给**一个**对象贴很**多个**标签，引申出**别名**的概念。
2. **元组**具有相对不可变性，所以**浅复制**对于可变对象会同时改变*原件*和*副本*，但是对于元组（不可变对象），只会应用在改变的部分。理解浅复制**副本共享内部对象的引用**。
3. **深复制**需要用到`copy`库的`copy()`和`deepcopy()`这两个方法。
4. python唯一支持的参数传递模式是**共享传参**，函数内部的**形参**是**实参**的别名，从而函数可能会修改接收到的任何可变对象。
5. 默认的**实参值**(在括号里面参数)应该不可变，如果是可变的实参，会导致在实例化的时候该参数被共享，下面是一个简单的例子。
```python
class Bus:
    def __init__(self, passengers = []):
        self.passengers = passengers
    def pick(self, name):
        self.passengers.append(name)
    def drop(self, name):
        self.passengers.remove(name)
        
t1 = Bus()
t1.passengers
Out[4]: []
Bus().passengers
Out[5]: []
t1.pick(['a','b'])
Bus().passengers
Out[7]: [['a', 'b']]
```
6. 创建或者调用类，函数的时候，要思考调用方法是否会修改传入的参数。解决的办法一般是创建副本。
```python
a = [1 ,2 , 3, 4]
b = list(a)
b.append(1)
a
Out[5]: [1, 2, 3, 4]
b
Out[6]: [1, 2, 3, 4, 1]
## 特别注意，浅复制对可变对象是复制的对象的引用
a = [1,(1, 2),[1, 2, 3]]
b = list(a)
b[2].append(4)
a
Out[10]: [1, (1, 2), [1, 2, 3, 4]]
b
Out[11]: [1, (1, 2), [1, 2, 3, 4]]
```
7. 第八章看完，对于其中的弱引用，动态内存管理的理解还是不够，希望以后会理解的更加明白。