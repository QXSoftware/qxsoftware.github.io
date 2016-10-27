---
title: 渲染所需的数学知识
tags: Unity 渲染 Shader
---

## Unity Shader 编写所必须的基础数学知识

### 矩阵（Matrix）


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
