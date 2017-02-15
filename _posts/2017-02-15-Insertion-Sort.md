---
title: 插入排序
tags: 算法
header-img: img/sky.jpg
published: true
---

### 直接插入排序

直接插入排序的思路很简单，对于一个给定序列，首先将第一个元素分出来变成一个“有序”的部分，然后依次执行 n - 1 次插入，每次插入后的子序列都是有序的。

下面是 C 语言的实现：


```c

#include <stdio.h>

static void insertion_sort(int *arr, int len)
{
	int i,j,k;
	if(len == 1)
		return;
	for(i = 1; i < len; i++)
	{
		if(arr[i] < arr[i - 1])
		{
			k = arr[i];
			arr[i] = arr[i - 1];
			for(j = i - 2; j > -1 && k < arr[j]; j--)
				arr[j + 1] = arr[j];
			arr[j + 1] = k;
		}
	}
}

int main()
{
	const int LEN = 10;
	int data[] = {2,3,1,45,32,6,7,8,-6,9};
	int i;

	printf("Before:\n");
	for(i = 0; i < LEN; i++)
		printf("%d ", data[i]);
	printf("\n");

	insertion_sort(data, LEN);

	printf("After:\n");
	for(i = 0; i < LEN; i++)
		printf("%d ", data[i]);
	printf("\n");

	return 0;
}

```

### 监视哨（Sentinel）

对于上面的插入排序，可以看到在插入的过程中有一个 for 循环，但是 for 循环的判断条件是 `j > -1 && k < arr[j]`。
这个地方需要执行两个逻辑运算，其中`j > -1`是为了防止下标越界而存在的，`k < arr[j]`才是真正的插入过程的逻辑需求。
通过设置监视哨可以避免`j > -1`这一步运算，从而提升性能。

改进后的代码：

```c

#include <stdio.h>

static void insertion_sort(int *arr, int len)
{
	// int i,j,k;
	int i,j;
	if(len == 1)
		return;
	for(i = 1; i < len; i++)
	{
		if(arr[i] < arr[i - 1])
		{
			// k = arr[i];
			// arr[0] is used for sentinel
			arr[0] = arr[i];
			arr[i] = arr[i - 1];
			// for(j = i - 2; j > -1 && k < arr[j]; j--)
			for(j = i - 2; arr[0] < arr[j]; j--)
				arr[j + 1] = arr[j];
			// arr[j + 1] = k;
			arr[j + 1] = arr[0];
		}
	}
}

int main()
{
	const int LEN = 11;
	// int data[] = {2,3,1,45,32,6,7,8,-6,9};
	// the first element is used for sentinel
	int data[] = {0, 2,3,1,45,32,6,7,8,-6,9};
	int i;

	printf("Before:\n");
	for(i = 1; i < LEN; i++)
		printf("%d ", data[i]);
	printf("\n");

	insertion_sort(data, LEN);

	printf("After:\n");
	for(i = 1; i < LEN; i++)
		printf("%d ", data[i]);
	printf("\n");

	return 0;
}

```

> 实际测试了一下，使用监视哨确实可以提升效率，不过从时间上来看，用了监视哨程序运行缩短的时间非常小，基本可以忽略不计。
所以我想这也是为何 .Net、JDK 之类的类库没有把监视哨内置到诸如 List 这样的基础数据结构中。
如果真的对某一段代码运行效率要求非常高，估计就会在 C 或者汇编级别去优化了。

参考资料：

+ [插入排序](http://www.cnblogs.com/mengdd/archive/2012/11/24/2786490.html)
+ [插入排序和监视哨](http://blog.csdn.net/xuyangcao123/article/details/51684321)
