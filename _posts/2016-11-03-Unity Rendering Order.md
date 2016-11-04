---
title: Unity 渲染顺序详解
header-img: img/umbrella.jpg
tags: Unity 渲染
---

#### 准备工作

工欲善其事，必先利其器。为了搞清楚 Unity 的渲染顺序，首先我们要准备一个能方便修改 SortingLayer 以及 RenderQueue 的工具，下面这个编辑器脚本可以满足这个需求：

[MeshRendererInspector.cs](https://gist.github.com/QXSoftware/499135b6482ca57727c69e11b91a12bf)

效果如下：

![](/post_img/mesh-renderer-inspector.jpg)

如何添加新的 SortingLayer？请通过 <strong>Edit/Project Settings/Tags and Layers</strong> 菜单选项打开：

![](/post_img/tags-layers.jpg)

然后添加三个 Sorting Layer，如上图所示。注意，Sorting Layer 添加后是可以拖动调整顺序的。

#### 实际测试



参考资料：

[Transparency and sorting in Unity](https://jakobknudsen.wordpress.com/2013/07/20/transparency-and-sorting/)

