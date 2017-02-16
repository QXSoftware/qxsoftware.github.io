---
title: 希尔排序
tags: 算法
header-img: img/sky.jpg
published: true
---

希尔排序是一种“加强版”的插入排序。主要思路如下：

* 将待排序序列分为若干个子序列，每个子序列的“步长”一致，然后对每个子序列执行插入排序
* 逐步缩短步长，对每个子序列执行插入排序
* 最后执行一次步长为 1 的子序列插入排序

希尔排序的时间复杂度按照步长的选择算法大有不同。但是合理选择步长可以达到一个较优的结果。


下面是 C 语言的实现：


```c

#include <stdio.h>

static void shell_insert(int *arr, int len, int step)
{
	int i,j,k;
	for(i = step; i < len; i += step)
	{
		if(arr[i] < arr[i - step])
		{
			k = arr[i];
			for(j = i - step; j > -1 && k < arr[j]; j -= step)
				arr[j + step] = arr[j];
			arr[j + step] = k;
		}
	}
}

static void shell_sort_impl(int *arr, int len, int* steps, int count)
{
	int i;
	for(i = 0; i < count; i++)
		shell_insert(arr, len, steps[i]);
}

static void shell_sort(int *arr, int len)
{
	const int count = 3;
	int steps[] = {7,3,1};
	shell_sort_impl(arr, len, steps, count);
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

	shell_sort(data, LEN);

	printf("After:\n");
	for(i = 0; i < LEN; i++)
		printf("%d ", data[i]);
	printf("\n");

	return 0;
}

```


参考资料：

+ [Shell Sort](https://en.wikipedia.org/wiki/Shellsort)
+ [希尔排序](http://www.cnblogs.com/jingmoxukong/p/4303279.html)
