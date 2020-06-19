---
title: SubScene
date: 2020-06-19 10:00:05
categories: Unity
description:

---



# SubScene



## 1. 资料

#### Paper

[Streaming Open World In ECS Framework - Unity Mega City - 叶磊](https://paper.dropbox.com/doc/Streaming-Open-World-In-ECS-Framework-Unity-Mega-City-yjzRKhpu0iaLRmYKaTbfq)



#### Projects

[unite2019-scenedatatodots](https://github.com/Unity-Technologies/unite2019-scenedatatodots)



#### Videos

[Converting scene data to DOTS - Unite Copenhagen](https://www.youtube.com/watch?v=TdlhTrq1oYk&list=PLX2vGYjWbI0S1wHRTyDiPtKLEPTWFi4cd&index=9)



#### Archive

[Game Object Conversion and SubScene](https://gametorrahod.com/game-object-conversion-and-subscene/)

[ECS系列目录](https://blog.csdn.net/qq_30137245/article/details/99071697#ECS_136)



## 2. SubScene 源码分析



### SceneSystem

子场景加载卸载 API 的统一入口

使用方式

```c#
var sceneSystem = world.GetExistingSystem<SceneSystem>();
sceneSystem.LoadSceneAsync() // 加载场景
sceneSystem.UnloadScene() // 卸载场景
SceneSystem.LoadSceneAsync(){
    sceneEntity = CreateSceneEntity()
    AddComponent(SceneReference,sceneEntity)
    AddComponent(RequestSceneLoaded,sceneEntity)
	return sceneEntity
}
```



### SubScene.cs 

子场景脚本, 子场景的操作入口

```
// 在脚本参数受到修改后
OnValidate{
      // 获取 SceneSystem 并使用其创建 sceneEntity 实体
      AddSceneEntities{
        var sceneEntity = sceneSystem.LoadSceneAsync(_SceneGUID, loadParams);
        AddComponent(this,sceneEntity)
     }
}
```



### ResolveSceneReferenceSystem

对 SceneReference 组件的处理

1.  加载子场景二进制数据
2. 根据数据创建多个 SceneSection 

```c#
OnUpdate{

sceneEntity.ForEach{

// 取得 ArtifactHash

artifactHash = GetSubSceneArtifactHash{

 //ArtifactHash (本地资源hash?) = SceneWithBuildSettingsGUIDs + 一些资源导入的设置

 SceneWithBuildSettingsGUIDs = CreateBuildSettingSceneFile{

  在  "Assets/SceneDependencyCache" 文件夹中序列化存储 SceneWithBuildSettingsGUIDs

  SceneWithBuildSettingsGUIDs = { sceneGUID , buildSettingGUID }

 } 

 return GetArtifactHash(SceneWithBuildSettingsGUIDs)

}

// 通过 ArtifactHash 来加载子场景

ResolveScene(artifactHash){

 // 分解 ArtifactHash

 GetArtifactPaths(artifactHash, out var paths)

 sceneHeaderPath = GetLoadPathFromArtifactPaths(paths,PathType.EntitiesHeader)

 scenePath= GetLoadPathFromArtifactPaths(paths,PathType.EntitiesBinary)

 hybridPath= GetLoadPathFromArtifactPaths(paths,PathType.EntitiesUnityObjectReferences)

 // 加载场景数据

 "sceneHeaderPath : VirtualArtifacts/Extra/69/69de41af925e0796303cbad143597411.entityheader"

 // 然而现在有个问题,上面的地址不存在, TryRead 方法应该返回文件不存在才对

 sceneMetaData = BlobAssetReference<SceneMetaData>.TryRead(sceneHeaderPath)

 // 场景数据 : sceneMetaData = {SceneName, SceneSectionData[]}

 // 根据 sceneMetaData 实例化并初始化 Sections 实体

 foreach(sceneMetaData.Sections){

  sectionEntity = EntityManager.CreateEntity();

  AddComponentData(RequestSceneLoaded,sectionEntity)

  AddComponentData(SceneSectionData,sectionEntity)

  AddComponentData(SceneBoundingVolume,sectionEntity)

  AddComponentData(ResolvedSectionPath,sectionEntity)

  AddComponentData(SubScene,sectionEntity)

  SetBuffer(ResolvedSectionEntity,sectionEntity)

 }

}

}
```



**场景文件 Scene**

类 : SceneAsset

序列化文件 : xxx.unity

内容 : YAML 格式的场景数据 , 

meta :  sceneGUID

**BuildSettingSceneFile 文件**

类 : SceneWithBuildSettingsGUIDs

序列化文件 : Asset/SceneDependencyCache/{hash}.sceneWithBuildSettings

内容 : 二进制信息

meta : hash

SceneWithBuildSettingsGUIDs = { sceneGUID , buildSettingGUID }

hash = math.**hash**(SceneWithBuildSettingsGUIDs)



**对 Unity 新的导入管线的代码猜测** 

**命名空间 : UnityEditor.Experimental**

**AssetDatabaseExperimental.GetArtifactHash()**

1. ArtifactHash = math.**hash**(SceneWithBuildSettingsGUIDs+ SubSceneImporterType + syncMode) 并返回
2. 存储 ArtifactHash 与上面的运算内容成为键值

**AssetDatabaseExperimental.GetArtifactPaths**(artifactHash, out var paths);

1. 在缓存中寻找 artifactHash 为键
2. 将相对应的值组成数组, 返回 paths

**加载场景数据时对应的文件不存在,代码却能正常运行的原因,**

估计是 Unity 最新的资源加载管线的操作 Asset pipline

下图是场景资源加载的打印日志,

该打印日志在将子场景内容修改后, 需要加载到子场景时出现 



根据日志内容猜测一个内容修改后的子场景加载步骤如下

如果内容没有修改过, 那么从 9 开始

1. 向 Importer 进行加载申请, Import Request, 
2. 向 Importer 提供 sceneWithBuildSettings 路径 path(sceneWithBuildSettings),
3. 向 Importer 提供 ArtifactHash 来获得加载参数 artifactKey(Guid, Impoter)
4. Start importing , 由 UnityEditor.Experimental.AssetImpoters.ScriptedImporter 执行
5. Importer 从 sceneWithBuildSettings 得到 SceneGUID
6. Importer 从 SceneGUID 得到场景资源文件地址 
7. 加载场景文件成为二进制资源
8. 加载后的二进制资源存储位置 VirtualArtifacts/Extra/xx/{ArtifactHash}.entityheader”
9. ResolveSceneReferenceSystem 从该二进制资源位置加载 , 这个资源是不可见的 ?
10. 成功加载出所有子场景对应的 SceneSections

还没查清是谁向 Importer 进行加载申请的

**SceneSectionStreamingSystem**

**OnCreate**

1. LoadScenesPerFrame = 4
2. 创建 4个 streamWorld(LoadingWorld) 存于 streams 中 
3. 创建 1 个 SynchronousSceneLoadWorld (LoadingWorld (synchronous))

**OnUpdate**

1. 为每个场景节点设置优先级,

foreach(i,sceneSections){

 if(**RequireSyncLoad**(sceneSections[i])) priorities[i] = 0;

 else if(sceneSections[i].SubSectionIndex == 0) priorities[i] = 1;

 else priorities[i] = 2;

}

1. priorityList = 优先级最高的 4 个场景节点
2. foreach(priorityList) 针对没有任务的 stream 创建 **AsyncLoadSceneOperation** operation
3. operation.**Update**()
4. streamingManager.**EndExclusiveEntityTransaction**()
5. 如果异步加载世界的操作完成了, 将世界中的实体移动到默认世界中

if(operation.IsComplite) **MoveEntities**(streamingManager, sceneEntity)  

6.1 获得源世界实体到目标世界实体的映射

ExtractEntityRemapRefs(srcManager, out entityRemapping)

6.2 移动实体从流式加载世界到主世界

EntityManager.MoveEntitiesFrom(srcManager, entityRemapping);

**AsyncLoadSceneOperation**

**Update()**

1. 创建文件读取任务 _ReadHandle , 读入 _FileContent

 _ReadHandle = AsyncReadManager.Read(_ScenePath, &cmd, 1);

1. 如果是编辑器内直接读取文件为 objectReferences, 此时场景内所有用到的资源存在内存中

var resourceRequests = UnityEditorInternal.InternalEditorUtility.LoadSerializedFileAndForget(_ResourcesPathObjRefs);

_ResourceObjRefs = (ReferencedUnityObjects)resourceRequests[0];

1. 创建异步加载任务 AsyncLoadSceneJob

reader = Read(_FileContent)

SerializeUtility.**DeserializeWorld**(Transaction, reader, objectReferences);

1. 先执行  _ReadHandle 再执行 AsyncLoadSceneJob

**SerializeUtility.DeserializeWorld**

直接通过内存中的二进制文件在世界中生成实体

**场景序列化**

**日志**

执行过程中的日志输出在 Library\ssetImportWorker0.log

**步骤**

1. 当场景编辑完保存成 YAML 格式的文件
2. 当子场景需要加载时, 某个类发出了 Import Request

![s_1715D091BF62C8442D9F5B2E8B3BAF259CD63CD3BE88A4C71861E0CFCBB9F775_1585273525263_image](https://raw.githubusercontent.com/okzkx/Images/master/s_1715D091BF62C8442D9F5B2E8B3BAF259CD63CD3BE88A4C71861E0CFCBB9F775_1585273525263_image.png)

调用的接口 :

UnityEditor.Experimental.AssetImporters.ScriptedImporter:GenerateAssetData(AssetImportContext)

1. ScriptedImporter 找到了 SubSceneImporter 并调用场景序列化相关 API

执行过程



- 1. OnImportAsset 预处理, 获取场景文件, 依赖关系, 序列化设置等
  2. 打开场景, 加载YAML场景文件到内存中 

Scene scene = EditorSceneManager.OpenScene(scenePath, OpenSceneMode.Additive);

- 1. WriteEntityScene 生成 Entity 场景

EditorEntityScenes.WriteEntityScene(scene, settings);

- - 1. 创建一个临时的场景转换的世界 “ConversionWorld
    2. 将 scene 在 ConversionWorld 中生成所有实体

ConvertScene(scene, settings);{

conversion.MappingSystem.CreatePrimaryEntities()

Update GameObjectConversionGroup 中的系统

}

- - 1. 所有的实体都具有共享组件 SceneSection, 获取所有的 SceneSection
    2. 对于每种共享组件 SceneSection, 都创建一个部分场景世界  “SectionWorld”
    3. 从 “ConversionWorld” 转移相应实体到 “SectionWorld”
    4. 保存 SectionWorld 成二进制文件

var fileSize = WriteEntityScene(sectionManager, sceneGUID, subSection.Section.ToString(), settings, out var objectRefCount, entityRemapping);

- - - 1. 序列化 场景物体

SerializeUtilityHybrid.Serialize(scene, writer, out objRefs, entityRemapInfos);

- - - 1. 序列化  objRefs

UnityEditorInternal.InternalEditorUtility.SaveToSerializedFileAndForget(serializedObjectArray.ToArray(), objRefsPath, false);

- - - 1. 序列化所有 SceneSection 成二进制文件

WriteHeader(sceneGUID, sceneSectionsArray, scene.name, settings.AssetImportContext); 



**总结**

子场景修改后到子场景序列化和加载的全流程

1. 场景修改后保存

.unity 文件以 YAML 格式序列化保存场景设置,

对应的 .meta 文件保存保存场景的 guid,

引擎可以通过 meta 的 guid 找到这个 .unity 文件路径

SubScene

1. SubScene 脚本加载子场景

- SceneSystem

- 1. 生成 sceneEntity(场景实体) , 与子场景一一对应

ResolveSceneReferenceSystem

1. 序列化存储 SceneWithBuildSettingsGUIDs{sceneGUID, buildSettingGUID}

"Assets/SceneDependencyCache”

ResolveSceneReferenceSystem

1. 场景序列化操作, 并返回 **ArtifactHash** 

**AssetDatabase.Experimental.GetArtifactHash(**SceneWithBuildSettingsGUIDs**)**

**猜测** 引擎调用了 ScriptedImporter:GenerateAssetData

UnityEditor.Experimental.AssetImporters.ScriptedImporter:GenerateAssetData(AssetImportContext)

ScriptedImporter 找到了 SubSceneImporter 并调用场景序列化相关 API

- EditorEntityScenes

- 1. 创建临时的世界 ConvertionWorld

- GameObjectConversionMappingSystem

- 1. 将场景中的所有物体在 ConvertionWorld 中生成所有实体,
  2. 将物体中的相应脚本转换成为实体的组件

- EditorEntityScenes

- d. 如果实体的 SceneSection 只不同,

- 从 ConversionWorld 转移相应实体到多个 SectionWorld

- 1. 所有 SectionWorld 中的实体保存成为二进制文件 .0.entities

"sceneHeaderPath : VirtualArtifacts/Extra/69/69de41af925e0796303cbad143597411.entityheader"

ResolveSceneReferenceSystem

1. 通过 **ArtifactHash** 获得 entityHeader 文件来加载场景

- ResolveSceneReferenceSystem

- 1. 加载出所有 SceneSection 实体, 添加相应组件

- SceneSectionStreamingSystem 

- 1. 创建 4 个异步加载世界(LoadingWorld), 1 个同步加载世界

- AsyncLoadSceneOperation 

- 1. 按照优先级,依次加载每个 SceneSection 关联的实体到 LoadingWorld

- SceneSectionStreamingSystem

- 1. 异步加载结束后, 移动所有实体到主世界 DefaultWorld