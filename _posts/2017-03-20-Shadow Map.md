---
title: Unity 阴影
tags: Unity 渲染 Shader
header-img: img/elephant.jpg
published: false
---

Unity5 自带的阴影技术就是传统的阴影映射或者是屏幕空间的阴影映射。屏幕空间的阴影映射需要`MRT（Multiple Render Targets）`技术，有的移动端设备不支持这种技术。

渲染阴影还有很多别的方法，但是在目前的移动设备硬件条件下，最常用的就是阴影映射了。

### 传统的阴影映射（Shadow Map）

`Shadow Map`本质就是一张深度图（Z-Buffer），这个深度图是在光源空间下渲染需要投射阴影的物体，并且把这些物体的深度值渲染到深度图中，这个深度图就是阴影映射纹理。

使用阴影映射纹理技术进行阴影渲染的过程如下：

* 在光源空间下渲染所有需要投射阴影的物体到阴影映射纹理
* 正常渲染的过程中，将某一空间中的点映射到光源空间，取得其深度值，然后和从阴影映射纹理中取得的深度值进行比较，如果比阴影映射纹理中的深度值大，则说明这个点在阴影中；如果比阴影映射纹理中的深度值小，则说明这个点不在阴影中。

### 屏幕空间阴影映射（Screenspace Shadow Map）

屏幕空间阴影映射使用的技术可以说是传统阴影映射的一个变体。屏幕空间阴影映射需要`MRT`支持，它的工作方式是通过投射阴影的光源的阴影映射纹理和摄像机的深度图，计算出一个“阴影图”，后续的渲染直接从这个生成的“阴影图”中进行采样即可。

计算“阴影图”的过程和传统阴影映射类似，如果摄像机深度图中某个点的深度值比相对应的光源阴影映射纹理中的深度值大，则说明这个点在阴影中，否则不在阴影中。这个“阴影图”是屏幕空间下的。

### 透明物体的阴影

通过上面的论述，我们知道了渲染阴影的基本原理。可以看到，阴影映射需要深度图，投射阴影的物体必须要写入深度值。

然而透明物体恰好不写入深度，这可怎么办呢？



### 阴影瑕疵（Shadow Acne）



> acne 发音 [ˈækni]
n.	<医>痤疮，粉刺; 青春美丽痘; 粉剌;

参考资料：

+ [Multiple Render Targets](https://en.wikipedia.org/wiki/Multiple_Render_Targets)
+ [Shadow Mapping 图解](http://www.cnblogs.com/yzwalkman/p/3149072.html)
+ [阴影瑕疵](https://www.zhihu.com/question/49090321/answer/114208279)
+ [catlikecoding shadow tutorial](http://catlikecoding.com/unity/tutorials/rendering/part-7/)
