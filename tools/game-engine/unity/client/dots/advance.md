## Aspect

[RotationSystem.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/master/EntitiesSamples/Assets/HelloCube/3.%20Aspects/RotationSystem.cs)

- Aspect 是一些 Component 的组合
- 算是一个语法糖
- 当 Query Asect 的时候相当于同时 Query 这些 Component
- 起到一个封装的作用, 基本没什么用

## structure-change

[ReparentingSystem.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/master/EntitiesSamples/Assets/HelloCube/6.%20Reparenting/ReparentingSystem.cs)

ecs 结构改变需要非常的谨慎

- 在遍历过程中禁止结构改变
- 在多线程并发时禁止结构改变

所以, 在很多并发或遍历 API 里不支持结构改变, 

- 结构改变的实现最基础的方式是用 EntityManager 写在主线程里
- 更好点的方式是用 ecb 在多线程里记录操作, 在 playback 统一处理结构体改变
- 再好一点是所有 system 的操作都用同步点 system 的 ecb 处理

## Hybird

[ GameObjectSync](https://github.com/Unity-Technologies/EntityComponentSystemSamples/tree/master/EntitiesSamples/Assets/HelloCube/8.%20GameObjectSync)

- 混合使用 ECS 和 GameOjbect
- 通常是将 ECS 的数据同步给 GameObject 并渲染

##### ecs 控制 GameObject 方式

Init System 创建 ManagedComponentData 持有 GameObject

``` c#
public class RotatorGO : IComponentData  
{  
    public GameObject Value;  
  
    public RotatorGO(GameObject value)  
    {        Value = value;  
    }  
    // Every IComponentData class must have a no-arg constructor.  
    public RotatorGO()  
    {    }}
```

Simulate System Query ManaedComponentData 并更新

``` c#
foreach (var (transform, speed, go) in  
         SystemAPI.Query<RefRW<LocalTransform>, RefRO<RotationSpeed>, RotatorGO>())  
{  
    transform.ValueRW = transform.ValueRO.RotateY(  
        speed.ValueRO.RadiansPerSecond * deltaTime);  
  
    // Update the associated GameObject's transform to match.  
    go.Value.transform.rotation = transform.ValueRO.Rotation;  
}
```

### JobQuery

[CollisionSystem.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/HelloCube/9.%20CrossQuery/CollisionSystem.cs#L11)

JobQuery 中有一些特别的用法 

``` c#
// On System
EntityTypeHandle = SystemAPI.GetEntityTypeHandle()
OtherChunks = boxQuery.ToArchetypeChunkArray(state.WorldUpdateAllocator)
LocalTransformTypeHandle = SystemAPI.GetComponentTypeHandle<LocalTransform>(true),

// On JobChunk
var entities = chunk.GetNativeArray(EntityTypeHandle);
var transforms = chunk.GetNativeArray(ref LocalTransformTypeHandle);
```

JobEntity

MoveProjectilesSystem.cs

```c#
[BurstCompile]  
public partial struct MoveJob : IJobEntity  
{  
    public float TimeSinceLoad;  
    public float ProjectileSpeed;  
    public EntityCommandBuffer.ParallelWriter ECBWriter;  
  
    void Execute(Entity projectileEntity, [ChunkIndexInQuery] int chunkIndex, ref LocalTransform transform,  
        in Projectile projectile)  
    {        float aliveTime = TimeSinceLoad - projectile.SpawnTime;  
        if (aliveTime > 5.0f)  
        {            ECBWriter.DestroyEntity(chunkIndex, projectileEntity);  
        }        transform.Position.x = projectile.SpawnPos.x + aliveTime * ProjectileSpeed;  
    }}
```


[MovementSystem.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/HelloCube/10.%20RandomSpawn/MovementSystem.cs#L8)

```c#
[WithAll(typeof(Cube))]  
[BurstCompile]  
public partial struct FallingCubeJob : IJobEntity  
{  
    public float3 Movement;  
    public EntityCommandBuffer.ParallelWriter ECB;  
  
    void Execute([ChunkIndexInQuery] int chunkIndex, Entity entity, ref LocalTransform cubeTransform)  
    {        cubeTransform.Position += Movement;  
        if (cubeTransform.Position.y < 0)  
        {            ECB.DestroyEntity(chunkIndex, entity);  
        }    }}
```

[LocalToWorld2DSystem.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/HelloCube/13.%20CustomTransforms/LocalToWorld2DSystem.cs#L24)

在 Job 里通过指针读写数据, 而不是 NativeArray

```c#
LocalToWorld* chunkLocalToWorlds =  
    (LocalToWorld*)chunk.GetRequiredComponentDataPtrRW(ref LocalToWorldTypeHandleRW);  
PostTransformMatrix* chunkPostTransformMatrices =  
    (PostTransformMatrix*)chunk.GetComponentDataPtrRO(ref PostTransformMatrixTypeHandleRO);
```

ComputeChildLocalToWorldJob 基本用到了所有常用的 Job 使用方式

ComponentLookup 通过多线程读取和写入

```c#
[NativeDisableContainerSafetyRestriction]  
public ComponentLookup<LocalToWorld> LocalToWorldLookup;
```

特指 chunk 中符合筛选标准的 entity

```
[ReadOnly] public EntityQueryMask LocalToWorldWriteGroupMask;
```

- System 写入 Chunk 时会将全局最新的 Version 写入 Chunk
- 在 System 更新自己 Version 到全局最新 Version 前, 检查如果 Chunk 的 Version 比自己大, 说明 Chunk 有最新数据而自己没有处理过

```
LastSystemVersion = state.LastSystemVersion,

chunk.DidChange(ref LocalTransform2DTypeHandleRO, LastSystemVersion)
```

ComponentData 两种获取方式

TypeHandle 方式效率比 Lookup 高


```
LocalToWorldTypeHandleRW = SystemAPI.GetComponentTypeHandle<LocalToWorld>(),

public ComponentTypeHandle<LocalToWorld> LocalToWorldTypeHandleRW;

chunk.DidChange(ref LocalToWorldTypeHandleRW, LastSystemVersion);

LocalToWorld* chunkLocalToWorlds =  
(LocalToWorld*)chunk.GetRequiredComponentDataPtrRO(ref LocalToWorldTypeHandleRW);

```

```
LocalTransform2DLookup = SystemAPI.GetComponentLookup<LocalTransform2D>(true),

[ReadOnly] public ComponentLookup<LocalTransform2D> LocalTransform2DLookup;

var localTransform2D = LocalTransform2DLookup[childEntity];

```

Buffer 两种获取方式

```
ChildTypeHandle = SystemAPI.GetBufferTypeHandle<Child>(true),  


[ReadOnly] public BufferTypeHandle<Child> ChildTypeHandle;  

BufferAccessor<Child> chunkChildBuffers = chunk.GetBufferAccessor(ref ChildTypeHandle);
DynamicBuffer<Child> children = chunkChildBuffers[i];
```

```
ChildLookup = SystemAPI.GetBufferLookup<Child>(true),

[ReadOnly] public BufferLookup<Child> ChildLookup;

ChildLookup.TryGetBuffer(childEntity, out DynamicBuffer<Child> children)
```

三种 Entity 状态改变方案

- 值改变
- Set Enable
- 结构改变

搜索距离最近的 Entity

- 遍历
- 基于轴的排序和二分搜索
- KD 树

同时使用了优先队列数据结构

[NativePriorityHeap.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/15105e7917e83b56f0bc863701023d6f028641e9/EntitiesSamples/Assets/HelloCube/15.%20ClosestTarget/KDTree/NativePriorityHeap.cs#L9)

