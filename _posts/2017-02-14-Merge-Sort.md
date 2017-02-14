---
title: 归并排序
tags: 算法
header-img: img/elephant.jpg
published: true
---

归并排序是一种分治算法。主要思路如下：

* 将未排序的序列“分开”为 n 个子序列，每个子序列只包含 1 个元素，这时就认为每个子序列都是有序的
* 重复合并子序列，从而生成新的子序列，直到只剩下一个子序列，这个子序列就是有序的

下面是 C 语言的实现：


```c

#include <stdio.h>
#include <string.h>

static void merge(int *arr, int iBegin, int iMid, int iEnd, int *dst)
{
	int i = iBegin;
	int j = iMid;
	int k;
	for(k = iBegin; k < iEnd; k++)
	{
		if(i < iMid && (j >= iEnd || arr[i] <= arr[j]))
			dst[k] = arr[i++];
		else
			dst[k] = arr[j++];
	}
}

static void merge_sort_impl(int *arr, int iBegin, int iEnd, int *dst)
{
	int iMid;
	if (iEnd - iBegin < 2)
		return;
	iMid = (iBegin + iEnd) / 2;
	merge_sort_impl(dst, iBegin, iMid, arr);
	merge_sort_impl(dst, iMid, iEnd, arr);
	merge(arr, iBegin, iMid, iEnd, dst);
}

static void merge_sort(int *arr, int iBegin, int iEnd, int *dst)
{
	merge_sort_impl(arr, iBegin, iEnd, dst);
}

int main()
{
	const int LEN = 10;
	int data[] = {2,3,1,45,32,6,7,8,-6,9};
	int sorted[LEN];
	int i;

	memcpy(sorted, data, LEN * sizeof(int));

	printf("Before:\n");
	for(i = 0; i < LEN; i++)
		printf("%d ", data[i]);
	printf("\n");

	merge_sort(data, 0, LEN, sorted);

	printf("After:\n");
	for(i = 0; i < LEN; i++)
		printf("%d ", sorted[i]);
	printf("\n");

	return 0;
}

```


参考资料：

+ [Merge sort](https://en.wikipedia.org/wiki/Merge_sort)
+ [数据结构、算法可视化](https://visualgo.net/sorting)
