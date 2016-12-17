---
title: C#多线程编程
header-img: img/autumn2.jpg
tags: 计算机科学
published: true
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
* Interlocked

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

前面说了半天，还是不知道怎么写代码。现在进入正题，如何使用 C#（.net）进行多线程编程。

首先，C# 中有个关键字 lock，lock 是 C# 的语法糖之一。


```

lock (x)
{
    DoSomething();
}

```

等价于：

```

System.myLockect myLock = (System.myLockect)x;
System.Threading.Monitor.Enter(myLock);
try
{
    DoSomething();
}
finally
{
    System.Threading.Monitor.Exit(myLock);
}

```

```Monitor.Enter()``` 相当于进入临界区（Critical Section），这个操作是互斥的，一旦一个线程进入了临界区，那么别的线程就会阻塞。这是最简单也是最常见的同步手段。

```Monitor``` 还有另外两个方法，```Monitor.Wait()``` 和 ```Monitor.Pulse()```。这两个方法常用语同一个进程内的多个线程之间的同步和通信。这和条件变量（Conditional Variable）的功能是一样的，但是 Monitor 底层实现并没有使用条件变量。

```

void produce()
{
    lock(myLock)
    {
        while(q.is_full())
            Monitor.Wait(myLock);
        q.put(date);
        Monitor.Pulse(myLock);
    }
}

void consume()
{
    lock(myLock)
    {
        while(q.is_empty())
            Monitor.Wait(myLock);
        data = q.get();
        Monitor.Pulse(myLock);
    }
}

```

上面的代码片段展示了一个典型的生产者消费者问题，produce 负责生产数据，但是如果消费者正在访问数据，或者队列满了，都必须阻塞生产。

produce 第一步是通过 ```lock(myLock)``` 获取锁，如果这时消费者正在访问数据，则这一步就阻塞。如果 produce 成功获取了锁，这时就需要检测队列是否已经满了，如果满了也需要阻塞，这时 ```Monitor.Wait(myLock)``` 会释放锁，并且使 produce 线程进入睡眠状态；如果队列未满，就往队列放入一个数据，并且通过 ```Monitor.Pulse(myLock)``` 通知可能处于 wait 状态的 consume 线程。

注意，如果 produce 执行 ```Monitor.Pulse(myLock)``` 时，consume 并没有处于 wait 状态，则这次 Pulse 没有任何作用。

> .net 要求 Monitor.Pulse() 必须在获取锁之后调用，否则就会抛异常。但是 POSIX Threads 的 Conditional Variable 并没有这个要求，《Unix 网络编程》里面甚至鼓励在释放锁之后再 Pulse（第 7 章，互斥锁和条件变量，避免上锁冲突）。

### 高级篇

上面的生产者消费者代码例子中，调用 ```Monitor.Wait(myLock)``` 是在一个 while 循环中执行的，这个地方为何需要 while 循环呢？原因是这样写可以使代码更健壮，而且可以防止虚假唤醒（Spurious wakeup）带来的问题。

在 Windows 以及 POSIX Threads 的 API 中，一个条件变量可能从 wait 状态唤醒，哪怕没有任何线程调用过 Pulse，而且虚假唤醒可能会多次发生，所以正确的做法是反复检测等待条件。

> C# 的 Monitor.Wait 并没有使用条件变量，所以可以幸免于虚假唤醒的问题。但是使用 while 循环包裹 wait 是个不错的选择，因为它总是正确的。

参考资料：

+ [Reentrancy](https://en.wikipedia.org/wiki/Reentrancy_(computing))
+ [Condition Variables](https://msdn.microsoft.com/en-us/library/windows/desktop/ms682052(v=vs.85).aspx)
+ [POSIX_Threads](https://en.wikipedia.org/wiki/POSIX_Threads)
+ [Thread Synchronization(C#)](https://msdn.microsoft.com/en-us/library/mt679037.aspx)
+ [Monitor Pulse](https://msdn.microsoft.com/zh-cn/library/system.threading.monitor.pulse.aspx)
+ [Monitor Madness Part One](https://ericlippert.com/2015/11/16/monitor-madness-part-one/)
+ [Monitor Madness Part Two](https://ericlippert.com/2015/11/19/monitor-madness-part-two/)
+ [Spurious wakeup](https://en.wikipedia.org/wiki/Spurious_wakeup)
