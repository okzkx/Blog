临时文件存放处

```
query = new EntityQueryBuilder(Allocator.Temp).WithAll<T>().Build(ref state),

query = EntityManager.CreateEntityQuery(ComponentType.ReadOnly<SyncTransfer>());
```

