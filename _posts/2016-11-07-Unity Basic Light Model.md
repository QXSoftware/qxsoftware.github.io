---
title: Unity 基本光照模型
header-img: img/alone.jpg
tags: Unity 渲染 Shader
published: true
---

### 基本光照成分

实际使用中的光照模型把光分成 4 种独立的成分：环境光（Ambient Light）、散射光（Diffuse Light）、镜面光（Specular Light）和自发光（Emissive Light）。


#### 环境光（Ambient Light）

环境光是那些在环境中进行了充分的散射，无法分辨其原始方向的光。它似乎来自于所有方向。当环境光撞击表面时，它会向所有方向均匀发散，所以我们看到的反射环境光不随视角变化。

环境光一般是一个全局变量：
**c<sub>ambient</sub> = g<sub>ambient</sub>**

在 Unity 中，环境光是通过 **Window/Lighting** 菜单选项设置：

![ambient light](/post_img/light-model/ambient-light-settings.jpg)

#### 漫反射（Diffuse Light）

散射光来自于某个方向，所以如果灯光从正面照射表面，它就看起来亮一些，如果从侧面照射，就会暗一些。但是当灯光撞击表面时，它会均匀地向所有方向发散，所以散射光也不随视角变化。

散射光计算：

**c<sub>diffuse</sub> = c<sub>light</sub> \* c<sub>material</sub> \* max(0, dot(N, L))**

![diffuse light](/post_img/light-model/diffuse-light.jpg)

#### 高光（Specular Light）

镜面高光来自于某个方向，并且倾向于从表面向某个特定方向反射。高光会随视角变化。

* phong

	**c<sub>specular</sub> = c<sub>light</sub> \* c<sub>material</sub> \* max(0, dot(V, R))<sup>gloss</sup>**

	![phong](/post_img/light-model/phong.jpg)

* blinn phong

	**c<sub>specular</sub> = c<sub>light</sub> \* c<sub>material</sub> \* max(0, dot(N, H))<sup>gloss</sup>**

	其中，H 是 V 和 L 的归一化和向量。

	![blinn-phong](/post_img/light-model/blinn-phong.jpg)

#### 自发光（Emissive Light）

自发光一般就是材质的一个颜色属性，自发光不会作为光源照亮别的物体。

**c<sub>emissive</sub> = m<sub>emissive</sub>**

### 合成光照

**c = c<sub>ambient</sub> + c<sub>diffuse</sub> + c<sub>specular</sub> + c<sub>emissive</sub>**

参考资料：

+ 《OpenGL编程指南》第五版，机械工业出版社
+ 《Unity Shader入门精要》，人民邮电出版社
