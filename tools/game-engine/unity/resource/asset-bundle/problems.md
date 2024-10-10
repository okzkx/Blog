
**可能存在的问题**

* 资源和AB 的管理不好做
- 代码写死不好调整
- 引擎升级，AB 改变
- 两次 AB 构建不同
- AB 黑盒
- 资源所在平台不同格式
- [场景AB重复打包问题](https://zhuanlan.zhihu.com/p/82568860)

**AssetBundle 解析工具**

* WebExtract 解压缩 AB
* Binary2Test 二进制文件转为文本文件

Asset 重建

- AB 关联
- Asset id 唯一，Instance ID 不唯一
- 美术资源预加载
- Exteranl References 所依赖的 Bundle
- AssetBundle 是压缩 Library 而不是直接进行资源的压缩
- 建议size：2m

**普通 AB，场景 AB**

- 场景 AB 体积巨大的问题
- 按照正常思路，我们认为场景中如果有 Prefab，那么场景的加载应当是依赖于这个 Prefab 的加载。
- 场景中的所有 Prefab 加载完后，场景才会加载。但实际上并不是这样，Prefab 放在场景中，最后打包时，会将其看作是场景中的一个实例。
- 和原本的 Prefab 没有任何关系。
- 这将导致重复打包。

为什么这么设计：

- 为了保证场景的加载一定是没问题的？，Prefab 丢失也能正常加载吗？

这个问题有两个解决方案，

1. 场景中不使用 Prefab，采用动态加载。
2. 场景中 Prefab 内的资源单独打成  AssetBundle 包做成依赖。
