---
title: Dispose 模式
tags: C# .Net
header-img: img/autumn2.jpg
published: true
---

.Net 中对于托管资源和非托管资源，有不同的释放策略，为了能更好地管理资源，我们需要使用 Dispose 模式。

对于托管资源，只要该资源的引用计数为 0，该资源就会被垃圾回收；而对于非托管资源，比如一个分配在 native code 中的内存，就需要主动释放了。

Dispose 模式的基本“样式”如下：

```c#

public class DisposableResourceHolder : IDisposable {
  
    private SafeHandle resource; // 托管资源
    private IntPtr buffer; // 非托管资源
    private bool disposed;

    public DisposableResourceHolder(){
        this.resource = ... // 分配资源
        this.buffer = ... // 分配非托管资源
    }

    public void Close(){
        ((IDisposable)this).Dispose();
    }

    void IDisposable.Dispose(){
        if(disposed)
            return;
        disposed = true;
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    ~DisposableResourceHolder(){
        Dispose(false);
    }

    protected virtual void Dispose(bool disposing){
        if (disposing){
            // 释放托管资源
            if (resource!= null) resource.Dispose();
        }
        // 释放非托管资源
        CloseHandle(buffer);
    }
}

```

以下需要注意的：

+ Finalizer 不是必须的，只有在存在非托管资源时，才需要一个 Finalizer
+ 如果是通过 Finalizer 执行释放资源的逻辑，则最好不要使用到别的托管对象成员，这样做会导致对象“复活”

参考资料：

+ [Dispose Pattern](https://msdn.microsoft.com/en-us/library/b1yfkh5e(v=vs.110).aspx)
+ [Implemeting a Dispose Method](https://msdn.microsoft.com/en-us/library/fs2xkftw(v=vs.110).aspx)
+ [.net Finalizer/Dispose](http://stackoverflow.com/questions/898828/finalize-dispose-pattern-in-c-sharp)
