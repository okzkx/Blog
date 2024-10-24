
## AutoAuthoring

- BufferSpawnerAuthoring
- ManagedSpawnerAuthoring
- AutoAuthoring
- BakingTypeAutoAuthoring
	- [BakingTypeAutoAuthoring.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Baking/AutoAuthoring/BakingTypeAutoAuthoring/BakingTypeAutoAuthoring.cs#L23)

## Dependencies

```
// cause this baker to rerun:
DependsOn(authoring.Image);
```

## BakingTypes

```
var pos = GetComponent<Transform>().position;
var parentBox = GetComponentInParent<CompoundBBAuthoring>();
```

## Blob

- 内存中存储数组, 是个 ECS 共享只读资源库
- 可以看作是可以写在 ComponentData 里的只读数组

- [BlobAssetBaker](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Baking/BlobAssetBaker "BlobAssetBaker")
- [BlobAssetBakingSystem](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Baking/BlobAssetBakingSystem "BlobAssetBakingSystem")

通常使用为 Component -> Baking -> BlobReference -> ComponentData, 在常用的 Baking 流程中添加 Bulid BlobReference
- [Splines](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Graphical/Splines "Splines")
- [WireframeBlob](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/Graphical/WireframeBlob "WireframeBlob")
### 其他 API

```
var entities = CollectionHelper.CreateNativeArray<Entity>(config.Size * config.Size,  state.WorldUnmanaged.UpdateAllocator.ToAllocator);
```

```
TransformHelpers.ComputeWorldTransformMatrix(entity, out var matrix, ref localTransformLookup,  
    ref parentLookup, ref postTransformMatrixLookup);

```
