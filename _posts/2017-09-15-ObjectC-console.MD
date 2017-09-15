---
title: 测试 Objective-C 语法的命令行小工具
tags: Objective-C
header-img: img/plane.jpg
published: true
---

如果想要快速测试 Objective-C 的代码片段，可以使用命令行完成。

创建测试代码：

```objective-c
#import <Foundation/Foundation.h>

int main(int argc, const char *argv[])
{
	NSString *str = [NSString stringWithFormat:@"%@%@\n", @"This is Objective-C:", nil];
	NSLog(str);
	return 0;
}
```

保存为`test.m`，然后编译：

```shell
gcc -framework Foundation test.m -o test
./test
```

可以看到输出：

> 2017-09-15 13:32:25.087 test[66825:5333621] This is Objective-C:(null)

也可以打开 Xcode 然后创建一个命令行工程进行测试。这个方法的好处是可以不用打开 Xcode 就可以进行测试，而且可以 ssh 到对应的 Mac 机器然后在命令行中进行所有操作，这对平时在 Windows 下工作的人来说很方便。

参考资料：

+ [Write Your First Program in Objective-C](http://www.informit.com/articles/article.aspx?p=1315356)
