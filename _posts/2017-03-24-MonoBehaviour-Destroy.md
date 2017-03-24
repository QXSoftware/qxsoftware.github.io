---
title: Unity MonoBehaviour 销毁事件
tags: Unity
header-img: img/seagulls.jpg
published: true
---

对于`MonoBehaviour`脚本的销毁，有两个接口：`Object.Destroy`和`Object.DestroyImmediate`，`Unity`的文档推荐常规情况使用`Object.Destroy`。关于这两个接口，也有很多微妙的地方需要注意。

### 测试用例

首先创建一个测试脚本，如下：

```csharp

public class TestDestroy1 : MonoBehaviour
{
	public string TestString;

	private int num;
	public int TestNumber
	{
		get
		{
			Debug.Log("Get TestNumber[" + Time.frameCount + "]");
			return num;
		}
		set
		{
			num = value;
			Debug.Log("Set TestNumber:" + value + "[" + Time.frameCount + "]");
		}
	}

	private void Awake()
	{
		Debug.Log("TestDestroy1 Awake[" + Time.frameCount + "]");
	}

	private void OnEnable()
	{
		Debug.Log("TestDestroy1 OnEnable[" + Time.frameCount + "]");
	}

	private void Start()
	{
		Debug.Log("TestDestroy1 Start[" + Time.frameCount + "]");
	}

	private void OnDisable()
	{
		Debug.Log("TestDestroy1 OnDisable[" + Time.frameCount + "]");
	}

	private void OnDestroy()
	{
		Debug.Log("TestDestroy1 OnDestroy[" + Time.frameCount + "]");
	}
}

```

点击按钮执行测试：

```csharp

if (GUILayout.Button("TestDestroy", GUILayout.Width(300), GUILayout.Height(100)))
{
	var go = new GameObject("xxx");
	var d = go.AddComponent<TestDestroy1>();
	Destroy(d);
	Debug.Log("go == null: " + (go == null ? "true" : "false") + "[" + Time.frameCount + "]");
	Debug.Log("d == null: " + (d == null ? "true" : "false") + "[" + Time.frameCount + "]");
	d.TestNumber = 666;
	Debug.Log(d.TestNumber);
	d.TestString = "666";
	Debug.Log(d.TestString);
}

```

### Object.Destroy

调用这个接口之后，脚本的`OnDisable`会立刻触发，调用栈如下：

```csharp

TestDestroy1 OnDisable[60]
UnityEngine.Debug:Log(Object)
TestDestroy1:OnDisable() (at Assets/Code/TestDestroy1.cs:40)
UnityEngine.Object:Destroy(Object)
TestAssetBundleAwake:OnGUI() 

```

脚本的`OnDestroy`会在调用帧的最后触发，而不是立即触发。

完整的时序如下：

![](/post_img/destroy_test/destroy.jpg)

### Object.DestroyImmediate

调用这个接口之后，脚本的`OnDisable`和`OnDestroy`会立刻触发：

```csharp

TestDestroy1 OnDestroy[60]
UnityEngine.Debug:Log(Object)
TestDestroy1:OnDestroy() (at Assets/Code/TestDestroy1.cs:45)
UnityEngine.Object:DestroyImmediate(Object)
TestAssetBundleAwake:OnGUI()

```

完整的时序如下：

![](/post_img/destroy_test/destroy_immediate.jpg)

### 判断是否为`null`

通过上面的测试发现，不管调用哪个销毁接口，在当前帧销毁了对象之后，`go == null`和`d == null`都为`false`，并且可以正常访问对象的属性和字段。

所以推荐做法是，在`OnDestroy`里面设置一个字段`hadDestroied = true`，然后在别的地方检测这个字段从而得知对象是否已经被销毁。

> 以上测试基于 Unity5.3.4f1

