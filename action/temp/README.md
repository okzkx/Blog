临时文件存放处

```
// ref state
query = new EntityQueryBuilder(Allocator.Temp).WithAll<T>().Build(ref state),

// entity manager
query = EntityManager.CreateEntityQuery(ComponentType.ReadOnly<SyncTransfer>());

// in update
var gameStateQuery = SystemAPI.QueryBuilder().WithAllRW<GameState>().Build();
```

