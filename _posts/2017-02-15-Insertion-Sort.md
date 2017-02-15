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


参考资料：
