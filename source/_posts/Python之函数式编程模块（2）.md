title: Python之函数式编程模块（2）
author: Crise
tags: [函数式编程, 高级语法]
category: Python
date: 2020-06-19 14:35:04
---
这篇文章主要是讲解一些函数式编程常用的模块，比如：`itertools`, `functools`以及`operator`。

## 一、itertools --用以创建迭代器的模块

<!--more-->

## 二、functools -- 高阶函数可调用对象上的操作

## 三、operator -- 标准运算符替代函数
在函数式编程中，我们经常需要把算术运算符当作函数使用，因此可以借助operator模块。operator 模块提供了一套与Python的内置运算符对应的高效率函数。例如，operator.add(x, y) 与表达式 x+y 相同。

operator 模块为多个算术运算符提供了对应的函数，从而避免编写
`lambda a, b: a * b` 这种平凡的匿名函数。这两种做法具体如下：

* 使用`lambda a, b: a * b`匿名函数来计算阶乘：

```
>>> from functools import reduce
>>> from operator import mul
>>> def fact(n):
...     return reduce(mul, range(1, n+1))
...
>>> fact(5)
120

```
* 使用`operator.mul`函数来计算阶乘：

```
>>> from functools import reduce
>>> from operator import mul
>>> def fact(n):
...     return reduce(mul, range(1, n+1))
...
>>> fact(5)
120

```

operator 模块中还有一类函数，能替代从系列中取出元素或读取对象属性的lambda表达式：