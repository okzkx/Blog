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

