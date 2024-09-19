## Prefab

```
Prefab = GetEntity(authoring.Prefab,TransformUsageFlags.Dynamic)
```

## PrefabReference

运行时加载 Prefab

```
var prefabEntity = new EntityPrefabReference(authoring.Prefab);
```

- EntityPrefabReference : 在 entity 上添加 Prefab 弱引用
- RequestEntityPrefabLoaded : 请求 Prefab 加载
- PrefabLoadResult : 判断 Prefab 加载完成并实例化

## Reference

用 Reference 引用序列化好的, 但是未被加载进内存的 Unity 资产

```
[Serializable]  
public struct References : IComponentData  
{  
public EntitySceneReference EntitySceneReference;  
public EntityPrefabReference EntityPrefabReference;  
public WeakObjectSceneReference GameObjectSceneReference;  
public WeakObjectReference<GameObject> GameObjectPrefabReference;
}
```

### 加载 

判断引用合法

```
refs.EntitySceneReference.IsReferenceValid
```

#### EntityReference

资产加载后自动转化为 Entity, 仅支持 Scene 和 Prefab

##### 场景

```
SceneSystem.LoadSceneAsync(state.WorldUnmanaged, refs.EntitySceneReference)
```

##### Prefab

```
SceneSystem.LoadPrefabAsync(state.WorldUnmanaged, refs.EntityPrefabReference)
```

判断加载成功

```
SceneSystem.GetSceneStreamingState(state.WorldUnmanaged, loading.EntityPrefab) ==  
SceneSystem.SceneStreamingState.LoadedSuccessfully)
```

#### WeakObjectReference

全生命周期控制任意 Unity Object 生命周期
###### Scene

```
// Load GameObject Scene  
if (!loading.GameObjectScene.IsValid() && refs.GameObjectSceneReference.IsReferenceValid)  
{  
    loading.GameObjectScene = refs.GameObjectSceneReference.LoadAsync(new ContentSceneParameters  
    {  
        autoIntegrate = true, loadSceneMode = LoadSceneMode.Additive,  
        localPhysicsMode = LocalPhysicsMode.None  
    });  
}
```

###### Prefab

```
// Load GameObject Prefab  
if (loading.GameObjectPrefabInstance == null &&  
    refs.GameObjectPrefabReference.IsReferenceValid)  
{  
    if (refs.GameObjectPrefabReference.LoadingStatus == ObjectLoadingStatus.None)  
    {        refs.GameObjectPrefabReference.LoadAsync();  
    }  
    if (refs.GameObjectPrefabReference.LoadingStatus == ObjectLoadingStatus.Completed)  
    {        loading.GameObjectPrefabInstance =  
            Object.Instantiate(refs.GameObjectPrefabReference.Result);  
    }}
```

## Scene Load

#### Scene Loading

使用 LoadSceneAsync 加载  EntitySceneReference 

```
SceneSystem.LoadSceneAsync(state.WorldUnmanaged, requests[i].Value);
```

使用 SceneSystem.SceneStreamingState 监控加载状况

```
SceneSystem.GetSceneStreamingState(state.WorldUnmanaged, scene.EntityScene);
```

分段加载 SubScene 中的 Section

```
state.RequireForUpdate<SceneReference>(); : 在有 SceneReference 时才执行
ResolvedSectionEntity : 所有 Section
RequestSceneLoaded : 加载 Section
DisableSceneResolveAndLoad : 禁止自动加载 Section
```


基于 Volume 的 Section 流式加载

[6. StreamingVolume](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Streaming/SceneManagement/6.%20StreamingVolume "6. StreamingVolume")

SubScene 流式加载

[7. SubsceneInstancing](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Streaming/SceneManagement/7.%20SubsceneInstancing "7. SubsceneInstancing")

使用 POstLoadCommandBuffer, 在 SubScene 加载完执行 Cmb

```
// A PostLoadCommandBuffer wraps an EntityCommandBuffer that will execute commands  
// after the subscene instance is loaded.  
var buf = new PostLoadCommandBuffer();  
buf.CommandBuffer = new EntityCommandBuffer(Allocator.Persistent, PlaybackPolicy.MultiPlayback);

state.EntityManager.AddComponentData(sceneEntity, buf);
```

包括 LOD

[8. Complete](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Streaming/SceneManagement/8.%20Complete "8. Complete")

