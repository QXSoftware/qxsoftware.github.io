---
title: Unity MonoBehaviour 边界条件下的生命周期
tags: Unity
header-img: img/alone.jpg
published: true
---

从`MonoBehaviour`继承的脚本类有一个特定的生命周期，可以在`Unity`的文档里面看到：

<img src=/post_img/monobehaviour_flowchart.svg style="max-width: 99999"></img>

一般情况下，一个`MonoBehaviour`的生命周期如下：

`Awake => OnEnable => Start => FixedUpdate => Update => LateUpdate => OnDisable => OnDestroy`

其中，`FixedUpdate`一帧内可能会执行多次。不过实际测试的结果并不是严格遵循这个结论的。

### 简单脚本生命周期测试

测试分为以下三种情况：

+ 直接在场景中摆好一个`GameObject`，并且在上面挂上`DynamicAdd1`脚本
+ 动态创建`GameObject`，然后动态添加`DynamicAdd1`脚本
+ 动态创建`GameObject go`，然后`go.SetActive(false)`，然后添加脚本`DynamicAdd1 dyn`，然后`dyn.enabled = false`，然后等一秒`go.SetActive(true)`，再等一秒`dyn.enabled = true`

测试结果1：

![](/post_img/in_scene.jpg)

这个是符合预期的，也是和`Unity`文档描述相符的。

测试结果2：

![](/post_img/dynamic_add_normal.jpg)

这个测试中，`FixedUpdate`没有在`Update`之前调用。但是`Update`和`LateUpdate`的相对顺序是对的。

测试结果3：

![](/post_img/dynamic_add_special.jpg)

`LateUpdate`跑到了`Update`前面，悲剧了。

所以平时编码不能太过依赖`Update`系列方法的调用顺序，这些东西不可靠。

相比之下，`Awake => OnEnable => Start`和`OnDisable => OnDestroy`还是很可靠的。

### 资源加载中的脚本生命周期测试

如果脚本是资源的一部分，那么这个地方需要格外小心。

+ 测试通过`AssetBundle`加载
+ 测试通过`Resources`加载

实际测试发现，不管是那种方式加载资源，对于被加载的资源`goA(GameObject)`来说，如果`goA`在打`AssetBundle`时或者在`Resources`文件夹中`active`是`false`，那么`Instantiate`生成的对象上面的脚本不会执行任何生命周期函数。

如果`goA`的`active`是`true`，但是上面的脚本`enable`是`false`，那么`Instantiate`后，脚本只会触发`Awake`，销毁时触发`OnDestroy`。

`OnEnable`和`OnDisable`一定是成对调用。只要触发了`Awake`，则一定会触发`OnDestroy`。

> 以上测试基于 Unity5.3.4f1

