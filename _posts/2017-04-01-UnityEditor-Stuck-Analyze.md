---
title: 记一次 Unity 编辑器卡死分析
tags: Unity
header-img: img/plane.jpg
published: true
---

今天项目中有人反映编辑器频繁卡死，具体表现为取代码重新编译后，点播放按钮后编辑器卡死。于是亲自尝试了一下，发现不编译代码也能触发编辑器卡死。而且是游戏启动进行了一定阶段之后，点退出然后再点播放，就卡死了。

#### 问题分析

于是祭出`WinDbg`查看`Unity`的线程栈：

![](/post_img/unity_main.jpg)

可以看到，`Unity`主线程阻塞在了`mono_domain_unload`这一步，所以是有什么东西阻止了`mono domain`的卸载。由于编辑器卡死是昨天才开始出现的，所以查看这两天的提交，发现有一处同步网络时间的代码嫌疑重大。

关键代码是：

```csharp

ThreadPool.QueueUserItem(x=>
{
	var hostEntry = Dns.GetHostEntry();
	// bla  bla  bla
})

```

于是怀疑是在解析`Dns`的时候，退出编辑器，然后立刻重新进入游戏，造成死锁。

所以构造了一个简单的测试用例来证明我的想法：

![](/post_img/test_case.jpg)

我构造了一个`tcp socket`并且去连百度的`1234`端口，可以预料到，`connect`时一定会阻塞线程，然后百度并没有开放`1234`端口，所以经过大概21秒后，会抛异常，并打印一条日志。

![](/post_img/timeout.jpg)

如果在超时前，关闭编辑器，然后重新进入游戏，发现编辑器稳定卡死。而要是等到打印了日志后，再退出游戏重进游戏编辑器就不会卡死。

#### 结论

所以卡死的原因找到了，解析`Dns`时，如果底层的`socket`处于阻塞状态，那么这个线程是无法退出的，并且当前`mono domain`也是无法卸载的。

推而广之，任何线程如果没有退出，那么当前的`mono domain`都是无法卸载的，下次重新进入游戏都会卡死编辑器。

> 以上测试基于 Unity5.3.4f1
