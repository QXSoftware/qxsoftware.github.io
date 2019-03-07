---
title: 使用 symbolicatecrash 工具解析 iOS 程序的 crash 文件
tags: Xcode
header-img: img/autumn2.jpg
published: true
---

symbolicatecrash 工具位于：

`/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash`

可以在 `/usr/local/bin` 中创建一个脚本 symbolicatecrash：
```shell
#!/bin/sh

export DEVELOPER_DIR=/Applications/XCode.app/Contents/Developer
tool=/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework/Versions/A/Resources/symbolicatecrash

exec ${tool} $@

```

增加可执行属性：
```shell
chmod +x symbolicatecrash
```

这样，平时使用的时候就可以直接：
```shell
symbolicatecrash -h
```

补充资料：
+ https://www.jianshu.com/p/b6031c6fa63d
