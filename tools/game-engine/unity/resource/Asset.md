### Asset

Asset 是 Unity 中的文件，任何一个文件（非文件夹）都是 Asset。

**Asset 具有**

* 原始资源文件
* .meta 文件，唯一 GUID
* Library 中的二进制拷贝

```text
graph TD
原始资源+.meta文件 --> B(Library 二进制)
场景 --> InstanceId --> fileId+LocalId --> B
```

**文件类型**

- 原生文件
	- Scene，Scriptable Objects，Materials 等
- 外部文件
	- Texture，Shader，Mesh，FBX
- 外部文件格式不定，原生文件很多使用 Yaml 书写。

**Unity Yaml 格式**

- FileID 序列化后文件内的某个对象的唯一 ID，代表一个对象的 ID，可以被文件内的其他对象引用

**Unity  Serialization**

- Unity 序列化系统
- 内存中的对象可以通过 Unity Serialization 序列化为文本文件

Binary，Yaml，Json 等

- 通过给类对象，属性添加特性 Attribute 告诉 Unity Serialization 应当以何种形式进行序列化操作

**.meta  文件**

* 资源的导入配置
* GUID

**Binary2text**

- Unity/Editor/Data/Tools/binary2text
- 从 Binary 中的二进制转换到原文本文件

**引擎端**

- Unity C++ 端的引擎根据序列化后的文件 Library 赋值给 C# Monobehaviour 实例化
- Script Asset 转化到 Library 中的程序集。

**Play Mode**

Unity 后端进入 Play Mode 后从 Library 处

1. 反序列化场景文件
2. 反序列化场景中的所有依赖的资源
3. 实例化所有的游戏物体
4. 创建所有的脚本对象
5. 添加脚本对象的所有生命周期方法到引擎的生命周期事件中。

#### Reference

- [Unity 的 Asset 管理和序列化](https://connect.unity.com/p/unity-de-asset-guan-li-he-xu-lie-hua)
- [[官方直播] 详解Unity Asset的一生](https://www.bilibili.com/video/BV1Wv41167i2)