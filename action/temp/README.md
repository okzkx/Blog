临时文件存放处

```
// ref state
query = new EntityQueryBuilder(Allocator.Temp).WithAll<T>().Build(ref state),

// entity manager
query = EntityManager.CreateEntityQuery(ComponentType.ReadOnly<SyncTransfer>());

query = EntityManager.CreateEntityQuery(new EntityQueryBuilder...);

using var query =  
    state.EntityManager.CreateEntityQuery(new EntityQueryBuilder(Allocator.Temp)  
        .WithAll<PrefabId>());

// in update
var gameStateQuery = SystemAPI.QueryBuilder().WithAllRW<GameState>().Build();
```

System singleton 可以被 SystemAPI query 到但不但能被 entityQuery query 到


SetParent

  
  

```


linkedEntityGroupLookup = state.GetBufferLookup<LinkedEntityGroup>();

var bow = state.Instantiate(PrefabType.Bow);  
state.AddComponent(bow, LocalTransform.FromPosition(math.up()));  
state.SetParentWithLookup(bow, hero, ref linkedEntityGroupLookup);
```

EntityCommandBuffer.SetBuffer 会重置 Buffer , 是 Bug 吗?
ecb 没法取得 buffer, 需要使用 SystemAPI 或者 EntityManager

[Questions around EntityCommandBuffer.SetBuffer and dynamic buffer sync points - Unity Engine - Unity Discussions](https://discussions.unity.com/t/questions-around-entitycommandbuffer-setbuffer-and-dynamic-buffer-sync-points/891447)

