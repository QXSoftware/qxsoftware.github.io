---
title: C++ 类型声明
tags: C++
header-img: img/fantasy.jpg
published: true
---

本文总结一下 C++ 的常见类型声明，使用 gcc 编译器进行测试。

编译命令：

> gcc test.cpp -o test

常见的类型声明有如下：

+ const T x;
+ T const x;

这两种写法表示的含义是一致的，都表示 x 是一个 T 类型的常量。

+ const T * x;
+ T const * x;

这两种写法都表示 x 是一个指向 T 类型的常量的指针。

+ const T * const x;
+ T const * const x;

这两种写法都表示 x 是一个指向 T 类型的常量的指针，并且 x 本身也是个常量。

所以总结一下，C++ 的类型声明为如下结构：

type[* [const]] variable-name;

其中，type 可以是 int 或者 const int（这是两种不同的类型），后面的两个方括号中的内容可以省略。

参考资料：

+ [cpp declarations](http://en.cppreference.com/w/cpp/language/declarations)
