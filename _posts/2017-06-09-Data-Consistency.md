---
title: 数据一致性
tags: Unity
header-img: img/seagulls.jpg
published: true
---

项目中遇到一次“事故”，发现同样的 xml 配置，序列化为二进制数据后，在 Windows 下生成的和在 Mac 下生成的 binary 文件 md5 不一样。

序列化工具是我们自己写的，用的是 C#，但是由于发包需要，序列化过程可能会在 Windows 或 Mac 下执行。

在 Windows 下，是使用 .Net 自带的编译器 csc 把序列化代码编译为可执行文件，然后生成 binary。

![](/post_img/data-consistency/csc.jpg)

在 Mac 下，使用 mono 的 mcs 编译器生成可执行文件，然后用 mono 虚拟机跑可执行文件，生成 binary。

结果发现不同平台生成的 binary 的 md5 不一致！

后来我就把这个完整的 binary 进行拆分，每个/组 xml 对应的 binary 文件单独写入磁盘，结果发现有几个 binary 确实在不同平台下是不一样的。

最先想到的导致不一致的原因就是我们的数据中包含字典（Dictionary），在对这种数据结构进行序列化时，没有对它进行排序，而只是`foreach`遍历然后逐条序列化。

于是把这个改了，对字典进行排序，重新测试后发现不一样的 binary 数量确实减少了，但是还是有不一样的。

于是又想到了，不同机器遍历文件夹返回的路径列表很可能也不一样，所以把所有的遍历路径的地方进行了排序，但是这个地方恰好对本次事故没有影响。

后来就挑了一个不一样的 binary，打开它对应的 xml 文件看，突然发现很多配置项是一串长长的小数，于是就怀疑是浮点数导致的：

![](/post_img/data-consistency/conf.jpg)

下面这个图是这段配置生成的二进制：

![](/post_img/data-consistency/diff.jpg)

于是写了一段小程序验证：

![](/post_img/data-consistency/Program.jpg)

在 Windows 下测试：

![](/post_img/data-consistency/win_compile_n_run.jpg)

在 Mac 下测试：

![](/post_img/data-consistency/mac_compile_n_run.jpg)

实际测试发现，在 Windows 下，`.116984966588606` 这个数字的二进制位级表示是`3F BD F2 BA 0D 9F 28 BF`,而在 Mac 下却是`3F BD F2 BA 0D 9F 28 BE`。最后一个字节一个是 `BF` 一个是 `BE`。和上面的 diff 截图一致。

### 总结

+ 注意不同平台的文件、文件夹遍历的结果可能是不一样的
+ 注意 Dictionary、HashSet 等数据结构的 foreach 遍历顺序在不同平台可能是不一样的
+ 在配置中配 double、float 在不同平台可能会导致数据不一致性的发生
