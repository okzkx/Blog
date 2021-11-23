# 资源管理

## 资源管理方式

* Asset
* AssetBundle
* Addressable

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

原生文件

Scene，Scriptable Objects，Materials 等

外部文件

Texture，Shader，Mesh，FBX

外部文件格式不定，原生文件很多使用 Yaml 书写。

**Unity Yaml 格式**

FileID 序列化后文件内的某个对象的唯一 ID，代表一个对象的 ID，可以被文件内的其他对象引用

**Unity  Serialization**

Unity 序列化系统

内存中的对象可以通过 Unity Serialization 序列化为文本文件

Binary，Yaml，Json 等

通过给类对象，属性添加特性 Attribute 告诉 Unity Serialization 应当以何种形式进行序列化操作

**.meta  文件**

* 资源的导入配置
* GUID

**Binary2text**

Unity/Editor/Data/Tools/binary2text

从 Binary 中的二进制转换到原文本文件

**引擎端**

Unity C++ 端的引擎根据序列化后的文件 Library 赋值给 C\# Monobehaviour 实例化

Script Asset 转化到 Library 中的程序集。

**Play Mode**

Unity 后端进入 Play Mode 后从 Library 处

1. 反序列化场景文件
2. 反序列化场景中的所有依赖的资源
3. 实例化所有的游戏物体
4. 创建所有的脚本对象
5. 添加脚本对象的所有生命周期方法到引擎的生命周期事件中。

#### Reference

[Unity 的 Asset 管理和序列化](https://connect.unity.com/p/unity-de-asset-guan-li-he-xu-lie-hua)

[\[官方直播\] 详解Unity Asset的一生](https://www.bilibili.com/video/BV1Wv41167i2)

### AssetBundle

**官方教程**

[AssetBundles](https://docs.unity3d.com/Manual/AssetBundlesIntro.html)

**详细分析**

[Unity 资源管理 AssetBundle使用详解](https://zhuanlan.zhihu.com/p/102273941)

[\[2018 TGDF\] AssetBundle 细节解析](https://www.bilibili.com/video/BV17t411H74C?t=2)

**siki 学院**

* [_AssetBundle_学习之路（一） ------ 定义/作用/打包流程](https://blog.csdn.net/dengshunhao/article/details/80480617?ops_request_misc=%7B%22request%5Fid%22%3A%22159427318519724848362005%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427318519724848362005&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-6-80480617.pc_v2_rank_blog_v1&utm_term=Assetbundle)
* [_AssetBundle_学习之路（二） ------ .manifest解析/读取.manifest依赖](https://blog.csdn.net/dengshunhao/article/details/80482223?ops_request_misc=%7B%22request%5Fid%22%3A%22159427318519724848362005%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427318519724848362005&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-5-80482223.pc_v2_rank_blog_v1&utm_term=Assetbundle)
* [_AssetBundle_学习之路（三） ------ asset bundle分组策略/依赖打包/资源重复问题解决方案](https://blog.csdn.net/dengshunhao/article/details/80492836?ops_request_misc=%7B%22request%5Fid%22%3A%22159427318519724848362005%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427318519724848362005&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-1-80492836.pc_v2_rank_blog_v1&utm_term=Assetbundle)
* [_AssetBundle_学习之路（四） ------ asset bundle加载及获取里面的资源](https://blog.csdn.net/dengshunhao/article/details/80486774?ops_request_misc=%7B%22request%5Fid%22%3A%22159427318519724848362005%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427318519724848362005&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-2-80486774.pc_v2_rank_blog_v1&utm_term=Assetbundle)
* [_AssetBundle_学习之路（五） ------ asset bundle卸载/文件校验](https://blog.csdn.net/dengshunhao/article/details/80513882?ops_request_misc=%7B%22request%5Fid%22%3A%22159427318519724848362005%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427318519724848362005&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-3-80513882.pc_v2_rank_blog_v1&utm_term=Assetbundle)
* [_AssetBundle_学习之路（六） ------ asset bundle浏览器](https://blog.csdn.net/dengshunhao/article/details/80519767?ops_request_misc=%7B%22request%5Fid%22%3A%22159427318519724848362005%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427318519724848362005&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-4-80519767.pc_v2_rank_blog_v1&utm_term=Assetbundle)

**基于 AssetBundle 的资源加载框架**

* [_游戏底层资源管理加载框架_\(一\) ------ 架构以及预备知识](https://blog.csdn.net/dengshunhao/article/details/84831760?ops_request_misc=%7B%22request%5Fid%22%3A%22159427655319724845061952%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427655319724845061952&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-1-84831760.pc_v2_rank_blog_v1&utm_term=游戏底层资源管理加载框架)
* [_游戏底层资源管理加载框架_\(二\) ------ AssetBundle打包管理](https://blog.csdn.net/dengshunhao/article/details/84889498?ops_request_misc=%7B%22request%5Fid%22%3A%22159427655319724845061952%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427655319724845061952&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-3-84889498.pc_v2_rank_blog_v1&utm_term=游戏底层资源管理加载框架)
* [_游戏底层资源管理加载框架_\(三\) ------ 基于AssetBundle的资源管理池](https://blog.csdn.net/dengshunhao/article/details/85332361?ops_request_misc=%7B%22request%5Fid%22%3A%22159427655319724845061952%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427655319724845061952&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-4-85332361.pc_v2_rank_blog_v1&utm_term=游戏底层资源管理加载框架)
* [_游戏底层资源管理加载框架_\(四\) ------ ResourceManager](https://blog.csdn.net/dengshunhao/article/details/85333935?ops_request_misc=%7B%22request%5Fid%22%3A%22159427655319724845061952%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=159427655319724845061952&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_blog_v1-2-85333935.pc_v2_rank_blog_v1&utm_term=游戏底层资源管理加载框架)

**可能存在的问题**

* 资源和AB 的管理不好做

代码写死不好调整

引擎升级，AB 改变

两次 AB 构建不同

AB 黑盒

资源所在平台不同格式

[场景AB重复打包问题](https://zhuanlan.zhihu.com/p/82568860)

**AssetBundle 解析工具**

AssetＢｕｎｄｌｅ

* WebExtract 解压缩 AB
* Binary2Test 二进制文件转为文本文件

Asset 重建

AB 关联

Asset id 唯一，Instance ID 不唯一

美术资源预加载

Exteranl References 所依赖的 Bundle

AssetBundle 是压缩 Library 而不是直接进行资源的压缩

建议size：2m

**普通 AB，场景 AB**

场景 AB 体积巨大的问题

按照正常思路，我们认为场景中如果有 Prefab，那么场景的加载应当是依赖于这个 Prefab 的加载。

场景中的所有 Prefab 加载完后，场景才会加载。但实际上并不是这样，Prefab 放在场景中，最后打包时，会将其看作是场景中的一个实例。

和原本的 Prefab 没有任何关系。

这将导致重复打包。

为什么这么设计：

为了保证场景的加载一定是没问题的？，Prefab 丢失也能正常加载吗？

这个问题有两个解决方案，

1. 场景中不使用 Prefab，采用动态加载。
2. 场景中 Prefab 内的资源单独打成  AssetBundle 包做成依赖。

#### Reference

### Addressable

只需要获取信标 Address，进行 Load 和 UnLoad 即可，不需要关心资源所在位置。

解决的问题：

1. 资源的本地与远端验证，是否要本地加载或远端加载，
2. 可视化的 AssetBandle 视图，对 AssetBandle 的整理
3. 不同平台的 AssetBundle，需要打不同的包
4. 依赖加载依赖卸载？
5. 异步实例化取代 Instantiate （同步实例化），帧工作结束才开始，大量加载卡顿？
6. 资源查询

没解决的问题：

1. 依赖打包设计

[达哥直播：_Addressable_ Asset System全新资源管理](https://www.bilibili.com/video/BV1N4411q7NA?from=search&seid=1274696876219718682)

[使用Unity Addressable Asset系统提升性能（英文）](https://www.bilibili.com/video/BV15t411R79b)

[https://github.com/Unity-Technologies/Addressables-Sample](https://github.com/Unity-Technologies/Addressables-Sample)

[https://github.com/AsehesL/SceneSeparateDemo](https://github.com/AsehesL/SceneSeparateDemo)

[Unity Addressable Asset system](https://docs.unity3d.com/Packages/com.unity.addressables@1.7/manual/index.html) 

### 其他技巧

[原创 Unity小工具一 自动改变导入图片类型为sprite模式](https://blog.csdn.net/weixin_42540271/article/details/104622939)


[The new Asset Import Pipeline: Solid foundation for speeding up asset imports](https://blog.unity.com/technology/the-new-asset-import-pipeline-solid-foundation-for-speeding-up-asset-imports)



[Unity在各平台下读取StreamingAssets文件夹中的文件](https://blog.csdn.net/hundaxxx/article/details/84565779)