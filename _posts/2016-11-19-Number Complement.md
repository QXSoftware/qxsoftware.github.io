---
title: 数字的补码
header-img: img/umbrella.jpg
tags: 计算机科学
published: false
---

### 补数计算法（Method of complements）

首先，网上流传的“补码”一般是指二进制补码，而这里说的是广义的“补码”。我们说一个数字的时候，必须明确这个数字是什么进制的。比如 **12345<sub>10</sub>**，对于常用的十进制数，一般都会把下标省去。

那么，我们如何算出 **12345** 的补码？计算步骤：

* 对于这个数字的每一位，都用 9 减去它
* 把上一步的结果从左到右写出来就是答案：**87654**

补码的意义是什么？补码的意义就在于，用加法计算减法。比如计算 **123 - 45**：

```

    123 (x)
-    45 (y)
--------------------
    123 (x)
+   954 (999 - y)
--------------------
   1077 (x + 999 - y)

舍去最高位的进位：

    077 (x + 999 - y - 1000 = x - y -1)

加上 1 就是所得结果：

    078 (x - y)


```

### 计算机如何存储有符号数

人类使用十进制数字的时候，会在数字前面加一个 **-** 来表示负数，但是计算机只能用“位串”来表示数字，所以历史上出现过多种方式来表示有符号数。

按照维基百科的介绍，有如下几种二进制有符号数的表示法，其中有的方法已经过时了：

+ 有符号长度表示法（Signed magnitude representation）
+ 反码表示法（Ones' complement）
+ 补码表示法（Two's complement）
+ Excess-K
+ Base −2
+ Google's Protocol Buffers zig-zag encoding

其中，Ones' complement 又叫 diminished radix complement（基数减一的补码），这个就是平常说的反码。

现在的计算机绝大部分都用的是补码表示法（Two's complement）。

### 二进制补码

参考资料：

+ [Method of complements](https://en.wikipedia.org/wiki/Method_of_complements)
+ [Signed number representations](https://en.wikipedia.org/wiki/Signed_number_representations)
+ [One's complement](https://en.wikipedia.org/wiki/Ones'_complement)
+ [Two's complement](https://en.wikipedia.org/wiki/Two's_complement)
