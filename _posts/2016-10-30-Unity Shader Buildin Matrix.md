---
title: Unity Shader 内置变换矩阵
header-img: img/elephant.jpg
tags: Unity 渲染 Shader
---

Unity 有很多内置变换矩阵：

|      变量名         |                           描述                                            |
| ------------------- | ------------------------------------------------------------------------- |
| UNITY_MATRIX_MVP    | 当前的模型观察投影矩阵，用于将顶点/方向矢量从模型空间转换到裁剪空间       |
| UNITY_MATRIX_MV     | 当前的模型观察矩阵，用于将顶点/方向矢量从模型空间转换到观察空间           |
| UNITY_MATRIX_V      | 当前的观察矩阵，用于将顶点/方向矢量从世界空间转换到观察空间               |
| UNITY_MATRIX_P      | 当前的投影矩阵，用于将顶点/方向矢量从观察空间转换到裁剪空间               |
| UNITY_MATRIX_VP     | 当前的观察投影矩阵，用于将顶点/方向矢量从世界空间转换到裁剪空间           |
| UNITY_MATRIX_T_MV   | UNITY_MATRIX_MV 的转置                                                    |
| UNITY_MATRIX_IT_MV  | UNITY_MATRIX_MV 的逆转置矩阵，常用于将法线从模型空间转换到观察空间        |
| _Object2World       | 当前的模型矩阵，用于将顶点/方向矢量从模型空间转换到世界空间               |
| _World2Object       | _Object2World 的逆矩阵，用于将顶点/方向矢量从世界空间转换到模型空间       |

有了这些矩阵，在 vertex shader 中进行顶点变换就可以有多种选择：

```GLSL
v2f vert (appdata v)
{
	v2f o;
	
	// 方式一	
	o.vertex = mul(UNITY_MATRIX_P, mul(UNITY_MATRIX_MV, v.vertex));
	
	// 方式二
	o.vertex = mul(UNITY_MATRIX_P, mul(UNITY_MATRIX_V, mul(_Object2World, v.vertex)));
	
	// 方式三	
	o.vertex = mul(UNITY_MATRIX_VP, mul(_Object2World, v.vertex));

	// 方式四，通过构造一个模型观察投影矩阵，然后变换顶点坐标	
	float4x4 m = mul(UNITY_MATRIX_P, UNITY_MATRIX_MV);
	o.vertex = mul(m, v.vertex);

	// 方式五，最常见也是最高效的做法	
	o.vertex = mul(UNITY_MATRIX_MVP, v.vertex);

	o.uv = v.uv;
	return o;
}

```
