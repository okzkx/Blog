
## 1. ForEach

## Entity
- class : ConvertToEntity
- 挂在 GameObject 上面,游戏开始自动将其转化为 Entity

### Component
- 存放数据
- [GenerateAuthoringComponent] struct : IComponentData
- 直接挂在 GameObject 上面,在 GameObject 转化为 Entity 后,作为其属性

### System
- class: SystemBase
- 每帧遍历实体?,获取对应组件,执行相应逻辑回调
- OnUpdate(){Entities.WithName().ForEach({//Do Something }.ScheduleParallel();}


## 2. IJobChunk

### Component
- [Serializable] struct : IComponentData
- 需要与 Authoring 配合,
- 不挂载在 GameObject 上面

### Authoring
- class : MonoBehaviour, IConvertGameObjectToEntity
- 将与其配合的 Component 与 Entity 关联, Convert() 

### Job
- 任务(原子性?)
- [BurstCompile] struct : IJobChunk
- 用 Job 执行的代码只能操作 Job 内部的属性?
  
### System
- 在 System 创建时获得实体集合, OnCreate(){ GetEntityQuery }
- 在每帧执行 Job 任务, OnUpdate
    - 获得组件集合, GetArchetypeChunkComponentType
    - 创建和初始化 Job  
    - Job 执行, job.Schedule(m_Group, Dependency);

## 3. SubScene

## 4. SpawnFromMonoBehaviour

## Prefab
- 首先准备好带有 Authoring 的 Prefab

### Spawner
- class : MonoBehaviour
- Start
- Get settings, entityPrefab, entityManager
- 实例化实体, entityManager.Instantiate
- 为实体的组件赋值, entityManager.SetComponentData()

## 5. SpawnFromEntity

### System
- [UpdateInGroup(typeof(SimulationSystemGroup))] class : SystemBase
- OnCreate 获取系统类 BeginInitializationEntityCommandBufferSystem 作为属性
- World.GetOrCreateSystem<BeginInitializationEntityCommandBufferSystem>()
- OnUpdate
    1. commandBuffer = CreateCommandBuffer().ToConcurrent();
    2. 使用 ForEach 找到 SpawnerEntity
    3. Create Entitys
    4. Destroy Entity

### Commend Buffer
- 为了防止竞争条件, Unity 事先对多线程进行调度
- 在 Job 运行时,不能改变 ECS 中的结构. 即不能增加删除实体
- 这些操作放在 Command Buffer 中, Command Buffer 中的命令会在帧结束时运行
- commandBuffer.Instantiate
- commandBuffer.SetComponent
- commandBuffer.DestroyEntity
- 注册buffer的调用事件? m_EntityCommandBufferSystem.AddJobHandleForProducer(Dependency);

## 6. SpawnAndRemove