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