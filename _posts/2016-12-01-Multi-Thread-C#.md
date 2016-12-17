---
title: C#多线程编程
header-img: img/autumn2.jpg
tags: 计算机科学
published: false
---

多线程编程是一个复杂并且很容易出错的事情，为了彻底弄明白多线程编程，我们必须熟知常用的系统组件（比如临界区、信号量等），以及 .Net 的常用类和编程接口。

多线程编程所需的知识点非常多，想要彻底精通非常不容易，因为这涉及到不同的操作系统使用的种类各异的同步工具。不过好在很多基本概念是相通的，所以掌握这些基本概念之后，实际操练起来就容易多了。

### 基础知识

首先，讨论多线程同步之前，我们需要明确是什么操作系统，不同的操作系统提供了不同的同步工具。

##### Windows System

在 Windows 平台下，主要有以下 4 种同步工具：

* 临界区（Critical Section）
* 互斥量（Mutex）
* 事件（Event）
* 信号量（Semaphore）

在以上几种同步工具的基础上，可以扩展出更上层的同步工具，比如：

* 条件变量（Conditional Variable）
* 读写锁（Readers Writer Lock）

> 这些更高级的同步工具是微软在后来的系统版本里面提供的，需要 Windows Server 2008 或者 Windows Vista 及以上才可以使用。当然我们也用一开始说的那 4 个基础设施自己实现条件变量或者读写锁。

##### Unix-Like System

在类 Unix 系统中，我们主要关注 Pthread（POSIX Threads），这是一个多线程同步标准，大多数类 Unix 系统 **(FreeBSD, NetBSD, OpenBSD, Linux, Mac OS X 以及 Solaris)** 都支持这个标准。这些 api 被实现到一个库里：libpthread。Windows 操作系统也有对应的支持库：pthread-win32。

pthread 支持的同步工具包括：

* Mutex
* Conditional Variable
* Readers Writer Lock

> 注意，信号量（Semaphore）可以和 POSIX 线程共同工作，但是它不是线程标准的一部分。所以信号量的 api 是以 "sem_" 开头而不是 "pthread_" 开头的。

##### C# 中的多线程同步

C# 只是一门语言，使用 C# 的生产环境主要有 .net 和 mono（Unity3D 使用的还是比较老旧的 mono）。mono 是一个由Xamarin公司（先前是Novell，最早为Ximian）所主持的自由开放源代码项目。该项目的目标是创建一系列匹配 ECMA 标准（Ecma-334 和 Ecma-335）的 .net 工具，包括 C# 编译器和通用语言架构。与微软的 .net 不同，mono 不仅可以运行于 Windows 系统上，还可以运行于 Linux，FreeBSD，Unix，Mac OS X 和 Solaris，甚至一些游戏平台，例如：Playstation 3，Wii 和 XBox 360。现在世界上的手机基本上只有 Android（基于 Linux 系统扩展，开源）和 iOS（基于 Unix 扩展，闭源），mono 的出现让一份 C# 代码运行在几乎所有设备上成为了可能。

.net 主要支持以下同步工具：

* Monitor（Conditional Variable）
* Mutex
* Semaphore
* AutoResetEvent
* ManualResetEvent
* ReaderWriterLock

可以看到，.net 在跨平台这块儿做了很多努力。

##### 可重入性

在计算机科学中，可重入性表示一个函数或者程序可以在执行到一半时被中断，然后成功地从中断返回并正确执行完毕。注意，这里的中断，可以由内部的 call 或者 jump 触发，也可以由硬件触发，或者是信号触发。

总的来说，函数中如果操纵了全局变量，那么这个函数就是不可重入的。

最后一点，线程安全和可重入紧密相关，但却是两个不同的概念。一个函数可以是线程安全的，但同时也可以是不可重入的。比如有以下代码：

```

int function()
{
    mutex_lock();
    // ...
    function body
    // ...
    mutex_unlock();
}

```

这段代码是“线程安全”的，但是不是“可重入”的。如果有多个线程同时执行这个函数，这是没问题的，但是如果这个函数被用作“中断处理函数”，那么第二个中断处理将无法获取互斥体并且阻塞，这可能导致整个系统遭殃。

### 进阶

### 高级篇

##### 虚假唤醒

参考资料：

+ [Reentrancy](https://en.wikipedia.org/wiki/Reentrancy_(computing))
+ [Condition Variables](https://msdn.microsoft.com/en-us/library/windows/desktop/ms682052(v=vs.85).aspx)
+ [POSIX_Threads](https://en.wikipedia.org/wiki/POSIX_Threads)
+ [Monitor Madness Part One](https://ericlippert.com/2015/11/16/monitor-madness-part-one/)
+ [Monitor Madness Part Two](https://ericlippert.com/2015/11/19/monitor-madness-part-two/)
