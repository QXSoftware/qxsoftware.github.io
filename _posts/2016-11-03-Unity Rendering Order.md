---
title: Unity 渲染顺序详解
header-img: img/umbrella.jpg
tags: Unity 渲染
---

#### 准备工作

工欲善其事，必先利其器。为了搞清楚 Unity 的渲染顺序，首先我们要准备一个能方便修改 SortingLayer 以及 RenderQueue 的工具，下面这个编辑器脚本可以满足这个需求：

[MeshRendererInspector.cs](https://gist.github.com/QXSoftware/499135b6482ca57727c69e11b91a12bf)

效果如下：

![](/post_img/render-queue/mesh-renderer-inspector.jpg)

如何添加新的 SortingLayer？请通过 <strong>Edit/Project Settings/Tags and Layers</strong> 菜单选项打开：

![](/post_img/render-queue/tags-layers.jpg)

然后添加三个 Sorting Layer，如上图所示。注意，Sorting Layer 添加后是可以拖动调整顺序的。

便捷修改 Camera 的 opaqueSortMode 和 transparencySortMode：

[CameraInspector.cs](https://gist.github.com/QXSoftware/1821915377e333e45f7a632ae07e64aa)

效果如下：

![](/post_img/render-queue/sort-mode.jpg)

#### 实际测试

测试之前首先明确两点：

+ 什么因素影响绘制顺序
+ 深度缓冲区对绘制结果的影响

为了弄清楚第一点，什么因素影响绘制顺序，我们需要把 ZWrite 关闭。同时结合 Unity Frame Debugger来观察绘制顺序。

##### Render Queue

RenderQueue 是材质（Material）的关键属性，当我们在 Unity 中创建一个材质后，Unity 会给这个材质设置一个默认 Shader，然后这个材质的 RenderQueue 被改为其 Shader 中设定的 RenderQueue。如果给这个材质球换了 Shader，Unity 会更新它的 RenderQueue（但是请注意，假如当前材质的 Shader 的 RenderQueue 是 3000，然后你修改 Shader 让其 RenderQueue 变成 3100，对应材质球的 RenderQueue 并不会更新！）。

<strong>Unity 最终以 Material 的 RenderQueue 为准。</strong>

Unity 内置了几个 RenderQueue 的字面值：

|    RenderQueue      |     值    |                                            说明                                               |
|---------------------|-----------|-----------------------------------------------------------------------------------------------|
|     Background      |   1000    | 这个渲染队列最先渲染，一般用于渲染背景                                                        |
|  Geometry(默认值)   |   2000    | 这个渲染队列是大多数物体的默认队列，用于渲染不透明物体                                        |
|     AlphaTest       |   2456    | 使用了 AlphaTest 的物体在这个队列渲染，当所有的不透明物体都渲染完了再渲染这个，有助于提升性能 |
|    Transparent      |   3000    | 在 Geometry 和 AlphaTest 之后、从后往前渲染，所有的半透明物体都应该在这里渲染                 |
|      Overlay        |   4000    | 在之前的所有渲染队列都渲染完了之后渲染，比如镜头光晕                                          |

可以看到，RenderQueue 越大，渲染越靠后。

##### SortingLayer



参考资料：

[Transparency and sorting in Unity](https://jakobknudsen.wordpress.com/2013/07/20/transparency-and-sorting/)

