---
title: 浮点数精度
header-img: img/elephant.jpg
tags: 计算机科学
published: true
---

之前只知道浮点数存在精度问题，但是不理解为什么。现在通过维基百科学习了，在此记录下来。

### 浮点数的格式

浮点数是按照 **符号位 + 指数位 + 小数位** 来存储的。按照位数的不同，分为单精度浮点数和双精度浮点数。

### 单精度浮点数（float）

![single](/post_img/float-precision/float.png)

单精度浮点数的第 1 位是符号位。后面 8 位是指数部分，用 **e** 表示，**e** 的范围是 -126 ~ 127，因为 **00000000** 和 **11111111** 设计为保留用。再后面 23 位是小数位。所以一个 32 位的浮点数的十进制值为：

![float value](/post_img/float-precision/float-val.svg)
![float value2](/post_img/float-precision/float-val2.svg)

以上图这个单精度浮点数为例，它的计算过程如下：

![float value3](/post_img/float-precision/float-val3.jpg)

### 双精度浮点数（double）

双精度浮点数的第 1 位是符号位，后面 11 位是指数位，再后面 52 位是小数位。

![double](/post_img/float-precision/double.png)

### 精度和取值范围

很显然，精度是由小数位数决定，而取值范围是由指数范围决定。

对于单精度浮点数，23 位小数部分其十进制值是 8388608，所以十进制的精度就是 6~7 位。8 位指数部分，由前面的描述可知 **e** 的取值范围是 -126~127，小数部分取值是 1~2（不包含2），所以对应的十进制范围大概是 -3.402823e38~3.402823e38。

对于双精度浮点数，精度 15~16 位。取值范围大概是 -1.79769e308~1.79769e308。

参考资料：

+ [浮点数精度](http://blog.csdn.net/dxy612/article/details/5518477/)
+ [维基百科 float](https://en.wikipedia.org/wiki/Single-precision_floating-point_format)
+ [维基百科 double](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)
