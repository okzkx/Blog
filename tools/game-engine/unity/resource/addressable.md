# Addressable

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

## Reference

- [com.unity.addressables](https://docs.unity3d.com/Packages/com.unity.addressables@1.22/manual/index.html) ![](https://docs.unity3d.com/uploads/Main/iconRel.png)
- [Get started with Addressables](https://learn.unity.com/course/get-started-with-addressables)
- [Addressables: Planning and best practices](https://unity.com/blog/engine-platform/addressables-planning-and-best-practices)
- [GitHub - Unity-Technologies/Addressables-Sample](https://github.com/Unity-Technologies/Addressables-Sample)
- [AsehesL/SceneSeparateDemo: 四叉树/八叉树场景动态加载](https://github.com/AsehesL/SceneSeparateDemo)
- [达哥直播：_Addressable_ Asset System全新资源管理](https://www.bilibili.com/video/BV1N4411q7NA?from=search&seid=1274696876219718682)
- [使用Unity Addressable Asset系统提升性能（英文）](https://www.bilibili.com/video/BV15t411R79b)
Blogs

- [【游戏开发探究】Unity Addressables资源管理方式用起来太爽了，资源打包、加载、热更变得如此轻松（Addressable Asset System | 简称AA）_unity aa-CSDN博客](https://blog.csdn.net/linxinfa/article/details/122390621)
