
## Boids

### Components
- Boid 鱼
- Obstacle 障碍物
- School 鱼群生成者(Spawner)
- Target 目标
- SampledAnimationClip 动画

### Systems
工作流程

#### School
1. GameObject 转 Entity
    BoidSchoolAuthoring -> BoidSchoolSpawn
2. 创建足够数量的鱼 Entity 
    - Instantiate(boidSchool.Prefab, boidEntities); 
3. 多线程任务 SetBoidLocalToWorld 为这些鱼附上位置组件 
    - LocalToWorldFromEntity[entity] = localToWorld;
4. 销毁 School 自身

#### BoidConversion 
1. 将鱼的 BoidAuthoring -> Boid
    - BoidAuthoring 没有实现 IConvertGameObjectToEntity, 需要 System 手动转为组件
2. 删除鱼的 Translation, Rotation 组件? 为什么?

#### SampledAnimationClipPlaybackSystem
- 播放鱼的游动动画
- BlobArray 中存储鱼动画关键帧的 TRS 信息, Samples
- 对所有鱼的 SampledAnimationClip 组件进行循环
- 循环1: 根据 frameIndex 和 Samples 设置鱼的 transform
- 循环2: 根据 deltaTime 设置 SampledAnimationClip 中的属性

#### 关于鱼的动画
- 但是鱼 Prefab 并没有挂载 Animation 或 Animator
- 发现鱼的游动动画是通过 Shader 实现
- 顶点随时间和z轴进行三角函数形状的偏移
```
v.vertex.x += sin((cycleOffset + _Time.y) * _TimeScale + v.vertex.z * _Amount) * _Distance;
```

#### BoidSystem
- 鱼的个体控制
- 在 Update 中
- 采取多次循环遍历 Component
1. 存储所有鱼的前方向, cellAlignment
2. 存储所有鱼的位置, cellSeparation
3. 存储目标位置信息, copyTargetPositions
4. 存储障碍位置信息, copyObstaclePositions
5. 按区域将鱼放入不同的桶中, hashMap
    - NativeMultiHashMap, 多键哈希表(键可重复,桶结构),
    - 键相同的鱼表明其在同一区域
    - key 算法:
```
 var hash = (int)math.hash(new int3(math.floor(localToWorld.Position / settings.CellRadius)));
```
6. 将以上任务排序
- 因为多线程访问同一变量,会导致线程争用,所以要先排好序方便调度?
- 还是因为下一个任务需要用到下一个任务的数据,所以强行线程同步?
- JobHandle.CombineDependencies
7. MergeCells 任务
- 该任务实现接口 IJobNativeMultiHashMapMergedSharedKeyIndices
- 专门开辟一个线程遍历一个散列桶,每个桶看作一个 Cell
- HashMap 中 key 相同的鱼,包含在一个 Cell 中
- Cell 作为一个区域内鱼的集合
- 遍历所有 Cell
    1. ExecuteNext 对于一个桶中的每个对象
        1. 计算(鱼数量,总朝向 alignment,总位置 separation)
    2. ExecuteFirst 方法: 对于每个Cell
        1. Cell.position = avg(separation)
        2. 计算最近的目标和障碍物

8. **参数都准备好了,开始进行鱼的行为控制(Steer)**
- 获取外部变量,相当于往Job中赋值, WithReadOnly
- 对于每只鱼, WithSharedComponentFilter(settings) 
- 群聚算法三个参数
    1. 获取其 Cell 方向与自身方向的偏移, alignmentResult
    2. 获取其 Cell 位置与自身位置的偏移, separationResult
    3. 获取朝向目标位置的向量, targetHeading
    4. 将以上三者按照权重结合, normalHeading
    5. 获取避障向量. 朝向最近障碍物反方向, 离障碍物越近其越大, avoidObstacleHeading
    6. 根据与障碍物的距离选择采用哪种向量作为最终向量, targetForward
    7. 由 targetForward 计算下一帧的朝向, nextHeading
    8. 根据 nextHeading 计算 translation 和 quaternion ,填入 TRS
    - 前方向由 (0,0,1) 转到 nextHeading 的函数
    - quaternion.LookRotationSafe(nextHeading, math.up()) ??