---
title: Unity Shader 语义
header-img: img/umbrella.jpg
tags: Unity 渲染 Shader
---

Shader 语法和 C 语言类似，但是有一个很特别的东西：语义（Semantics）。

语义用来表明一个变量到底代表什么含义。下面就 vertex shader 和 fragment shader 的输入输出语义进行总结。

###  Vertex Shader Input Semantics

|  Semantics   |      Type     |                       含义                        |
|--------------|---------------|---------------------------------------------------|
|  POSITION    |  float3/4     | 表示模型的顶点坐标                                |
|  NORMAL      |  float3       | 表示模型的法线                                    |
|  TEXCOORD0~N |  float2/3/4   | TEXCOORD0 表示第一套 uv，TEXCOORDN 表示第 N 套 nv |
|  TANGENT     |  float4       | 表示模型的切线                                    |
|  COLOR       |  float4       | 表示模型的顶点颜色                                |

### Vertex Shader Output Semantics / Fragment Shader Input Semantics

Vertex Shader 的最重要目的就是输出顶点在裁剪空间中的坐标。

|  Semantics   |        Type        |                       含义                        |
|--------------|--------------------|---------------------------------------------------|
|  SV_POSITION |  float4            | 表示模型顶点在裁剪空间中的坐标                    |
|  TEXCOORD0~N |  高精度（float4）  | 用来保存高精度数据，例如纹理坐标，位置坐标等      |
|  COLOR0~N    |  低精度（fixed4）  | 用来表示低精度数据，比如 0~1 之间的颜色值         |

Vertex Shader 中的输出值将按照三角片面进行差值，随后作为 Fragment Shader 的输入。

TEXCOORD 系列语义又叫做插值器（Interpolator），在不同的系统和GPU上，插值器的数量限制也是不同的。

|  插值器数量  |                                                  限制条件                                                    |
|--------------|--------------------------------------------------------------------------------------------------------------|
|      8       |  OpenGL ES 2.0 (iOS/Android) Direct3D 11 9.x level (Windows Phone) and Direct3 9 shader model 2.0 (old PCs)  |
|      10      |  Direct3D 9 shader model 3.0 (#pragma target 3.0)                                                            |
|      16      |  OpenGL ES 3.0 (iOS/Android), Metal (iOS)                                                                    |
|      32      |  Direct3D 10 shader model 4.0 (#pragma target 4.0)                                                           |

### Fragment Shader Output Semantics

Fragment Shader 最重要的输出就是这个片元对应的颜色。

|    Semantics     |        Type        |                                             含义                                        |
|------------------|--------------------|-----------------------------------------------------------------------------------------|
|  SV_Target0~N    |  fixed4/float4     | 表示片元的最终颜色，一般很少用到 SV_Target1 这些输出，SV_Target0 于是等价于 SV_Target   |
|  SV_Depth        |  float             | 输出片元深度值，这个会有性能损耗，一般很少用到                                          |

还有一些特殊语义，这些语义的使用一般都需要 Shader Model 2.0 以上。

##### VPOS

表示屏幕空间坐标。使用这个语义需要 Shader Model 3.0，也就是 `#pragma target 3.0`。

##### VFACE

表示一个渲染表面是否面朝相机。使用这个语义需要 Shader Model 3.0，也就是 `#pragma target 3.0`。


参考资料：

[Shader Semantics](https://docs.unity3d.com/Manual/SL-ShaderSemantics.html)

[Vertex Program Inputs](https://docs.unity3d.com/Manual/SL-VertexProgramInputs.html)
