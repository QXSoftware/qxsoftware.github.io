---
title: Unity3D 罕见闪退
header-img: img/grass.jpg
tags: Unity3D
published: true
---

最近项目研发很紧张，很久没时间更新博客了。期间分析了很多 Android 和 iOS 的闪退，大部分都是可以解释的，比如内存耗尽被系统杀死，以及五花八门的 native crash，但是依旧有小部分闪退无法解释。这次就分享一个 mono 的 bug 导致的闪退。

#### 准备条件

Unity5.3.4f1、小米2A，具体型号参加下图：

![mi2A](/post_img/unity-crash/mi2A.jpg)

#### 测试代码

```

public class TestCrash : MonoBehaviour
{
	private string GetStr()
	{
		return null;
	}

	private void OnGUI()
	{
		if (GUILayout.Button("Crash", GUILayout.Width(300), GUILayout.Height(200)))
		{
			try
			{
				var str = GetStr();
				Debug.Log(str.Length);
			}
			catch (Exception ex)
			{
				Debug.LogException(ex);
			}
		}
	}
}

```

#### 运行结果

点击 **Crash** 按钮后，游戏直接闪退。通过 logcat 不能看到任何有效信息，Android 的 tombstone 也没有相关信息，Android adb shell bugreport 也不能发现任何线索。

以下是典型的 logcat 输出，可以看到没有任何有价值的信息：

![logcat](/post_img/unity-crash/logcat.jpg)

而期望的结果是，`Debug.Log(str.Length);`的执行抛出 NullReferenceException 然后被 catch 住，并进一步打印日志。下面是在我的 Samsung NoteII 上进行的相同测试：

![logcat2](/post_img/unity-crash/logcat2.jpg)

可以看到，mono 正确捕获了异常，游戏也没有闪退。

#### 总结

Unity3D 使用的 mono 版本比较旧，bug 不少。这次的闪退完全是由于访问了空引用导致 mono crash，虽然写了 try catch，但是依旧无法避免闪退的发生。所以必须判断是否是空引用。

> 这个闪退只在小米2A上稳定复现，在别的手机上均不会闪退。
