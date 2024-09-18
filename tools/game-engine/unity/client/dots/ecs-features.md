## Aspect

- Aspect 是一些 Component 的组合
- 算是一个语法糖
- 当 Query Asect 的时候相当于同时 Query 这些 Component
- 起到一个封装的作用, 基本没什么用

## structure-change

ecs 结构改变需要非常的谨慎

- 在遍历过程中禁止结构改变
- 在多线程并发时禁止结构改变

所以, 在很多并发或遍历 API 里不支持结构改变, 

- 结构改变的实现最基础的方式是用 EntityManager 写在主线程里
- 更好点的方式是用 ecb 在多线程里记录操作, 在 playback 统一处理结构体改变
- 再好一点是所有 system 的操作都用同步点 system 的 ecb 处理

## Hybird

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