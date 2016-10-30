---
title: Unity Shader 内置变换矩阵
tags: Unity 渲染 Shader
---

|      变量名         |                           描述                                            |
| ------------------- | ------------------------------------------------------------------------- |
| UNITY_MATRIX_MVP    | 当前的模型观察投影矩阵，用于将顶点/方向矢量从模型空间转换到裁剪空间       |
| UNITY_MATRIX_MV     | 当前的模型观察矩阵，用于将顶点/方向矢量从模型空间转换到观察空间           |
| UNITY_MATRIX_V      | 当前的观察矩阵，用于将顶点/方向矢量从世界空间转换到观察空间               |
| UNITY_MATRIX_P      | 当前的投影矩阵，用于将顶点/方向矢量从观察空间转换到裁剪空间               |
| UNITY_MATRIX_VP     | 当前的观察投影矩阵，用于将顶点/方向矢量从世界空间转换到裁剪空间           |
| UNITY_MATRIX_T_MV   | UNITY_MATRIX_VP 的转置                                                    |
| UNITY_MATRIX_IT_MV  | UNITY_MATRIX_VP 的逆转置矩阵，常用于将法线从模型空间转换到观察空间        |
| _Object2World       | 当前的模型矩阵，用于将顶点/方向矢量从模型空间转换到世界空间               |
| _World2Object       | _Object2World 的逆矩阵，用于将顶点/方向矢量从世界空间转换到模型空间       |
