---
title: 快速排序
tags: 算法
header-img: img/sky.jpg
published: true
---

快速排序是一种分治算法 (Divide and Conquer)。主要思路如下：

* 从序列中挑选一个中心（Pivot）
* 然后开始分区（Partitioning），使得所有比中心小的元素位于中心左边，所有比中心大的元素位于中心右边，分区结束后，中心元素的位置就是最终位置
* 对子分区重复上一步，直到自分区大小为 1 个元素

在对 n 个元素的排序中，快速排序的最好、平均时间复杂度为 O(nlog(n))，最差时间复杂度为 O(n<sup>2</sup>)；空间复杂度为 O(1)。

下面是 C 语言的实现：


```c

#include <stdio.h>

static void swap(int *a, int *b)
{
	int tmp = *a;
	*a = *b;
	*b = tmp;
}

static void partition(int * arr, int iBegin, int iEnd)
{
	int pivot;
	int low = iBegin;
	int high = iEnd - 1;
	if(iEnd - iBegin < 2)
		return;
	pivot = arr[iBegin];
	while(low < high)
	{
		while(low < high && arr[high] >= pivot)
			--high;		
		swap(&arr[low], &arr[high]);
		while(low < high && arr[low] <= pivot)
			++low;
		swap(&arr[low], &arr[high]);
	}
	arr[low] = pivot;
	partition(arr, iBegin, low);
	partition(arr, low + 1, iEnd);
}

static void quick_sort(int * arr, int len)
{
	partition(arr, 0, len);
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

	quick_sort(data, LEN);

	printf("After:\n");
	for(i = 0; i < LEN; i++)
		printf("%d ", data[i]);
	printf("\n");

	return 0;
}

```


参考资料：

+ [Quick Sort](https://en.wikipedia.org/wiki/Quicksort)
+ [快速排序](http://www.tuicool.com/articles/VVBvIf)
