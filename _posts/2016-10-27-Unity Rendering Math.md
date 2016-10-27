---
title: 渲染所需的数学知识
tags: Unity 渲染 Shader
---

## Unity Shader 编写所必须的基础数学知识

### 矩阵（Matrix）

#### 矩阵性质

* 不满足交换律

	*AB ≠ BA*

* 满足结合律

	*ABCD = A(BC)D = AB(CD)*

#### 特殊的矩阵

* 方阵
行数和列数相等的矩阵叫做方阵（Square Matrix），渲染常用的是 3x3 和 4x4 矩阵。如果除了对角线之外的元素都是 0，那么这个矩阵也叫做对角矩阵（Diagonal Matrix）。

	方阵：
	![](/post_img/square-matrix.jpg)

	对角矩阵：
	![](/post_img/diagonal-matrix.jpg)

* 单位矩阵
单位矩阵（Identity Matrix）是对角线的元素都为 1 的对角矩阵。单位矩阵一般用 I 表示。

	*MI = IM*

	单位矩阵：
	![](/post_img/identity-matrix.jpg)

* 转置矩阵
转置矩阵（Transpose Matrix）就是将一个 mxn 的矩阵翻转过来变成 nxm 的矩阵。

	转置矩阵：
	![](/post_img/transpose-matrix.jpg)

* 逆矩阵
首先，并不是所有的矩阵都有逆矩阵（Inverse Matrix）。矩阵 M 存在逆矩阵 M<sup>-1</sup> 的前提之一就是，M 必须是一个方阵。M 和 M<sup>-1</sup> 相乘得到单位矩阵：

	*MM<sup>-1</sup> = M<sup>-1</sup>M = I*

	逆矩阵有以下性质：

	* 一个可逆矩阵的逆矩阵的逆矩阵是它本身

		*(M<sup>-1</sup>)<sup>-1</sup> = M*

	* 单位矩阵的逆矩阵还是单位矩阵

		*I<sup>-1</sup> = I*

	* 转置的逆矩阵是逆矩阵的转置

		*(M<sup>T</sup>)<sup>-1</sup> = (M<sup>-1</sup>)<sup>T</sup>*

	* 矩阵乘积的逆等于反向逆矩阵的乘积

		*(ABCD)<sup>-1</sup> = D<sup>-1</sup>C<sup>-1</sup>B<sup>-1</sup>A<sup>-1</sup>*

* 正交矩阵
正交矩阵（Orthogonal Matrix）是方阵。如果 M 和 M<sup>T</sup> 的乘积是单位矩阵的话，则 M 是正交的。

	*MM<sup>T</sup> = I*

	这等价于，M 的逆矩阵等于转置矩阵。

	*M<sup>-1</sup> = M<sup>T</sup>*

> 在 Unity 中，旋转矩阵是正交矩阵，包含统一缩放比的缩放矩阵除以缩放系数 k 之后也是正交矩阵。

####  行向量还是列向量？

> 首先要明确的是，Unity Shader 中使用的是列向量。

列向量：

![列向量](/post_img/column-vector.jpg)

行向量：

![行向量](/post_img/row-vector.jpg)

> 行向量和列向量与矩阵相乘的结果是不一样的，所以必须事先清楚用的是行向量还是列向量。

行向量相乘：

![](/post_img/row-vector-mul.jpg)

列向量相乘：

![](/post_img/column-vector-mul.jpg)

由于 Unity 使用列向量，所以一般情况下，矩阵乘法都是右乘，例如将顶点坐标从模型空间变换到裁剪空间：

`o.pos = mul(UNITY_MATRIX_MVP, v.position);`

或者将矩阵转置一下，用行向量乘法也是一样的：

`o.pos = mul(v.position, transpose(UNITY_MATRIX_MVP));`
