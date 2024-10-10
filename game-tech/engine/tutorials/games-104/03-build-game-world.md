# 第三节：如何构建游戏世界

## 如何让游戏世界活起来

### Dynamic Game Object

- Drone
- Air-defense Missile
- Solider
- Artillery
- Tank

### Static Game Object

- Boxes
- Watchtower
- Shack
- Shed
- Stone

### Environment

- Sky (tone of day 日夜变换系统)
- Vegetation (植被)
- Terrain

### Other Game Objects

- Air wall
- Trigger Area
- Ruler
- Navigation mesh

### Everything is a Game Object

## How to Describe a GameObject

- Name
- Property
  - Shape
  - Position
  - Capacity of battery
- Behavior
  - Move
  - Scout

### OOP

- 通过面向对象的方式描述游戏物体，
- 但是实际上多派生会出现问题
- 实际上的现实中的物体不是清晰的树状派生关系，而是功能组合居多

### Component Base

组件化，通过组件组合成游戏对象

![image-20220401185045234](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307241815417.png)

![image-20220401185248796](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307241815686.png)

### Takeaways

- Everything is a game object in the game world
- Game object could be described in the component-based way

### Make world alive

- Object-based Tick 是直观的，但是是效率低的
- 高效的 Tick 的执行是遍历同类型的每个个体，而不是同个体的每个类型
- 按照流水线的做法比较高效，把同样的 component 放在一起进行批处理

### Interactive

- Hard code : 实时
- Events ：发送事件邮件，在下个 tick 触发
- Interface
- 可拓展的消息系统

#### Manage Game Object

如何管理游戏物体

- Game object uid
- position
- Scene management 空间管理的核心
  - No division
  - Divided by grid
    - Quadtree ：角色在场景中分布是很不均匀的，所以不用均匀的格子
  - Hierarchical segmentation 树状结构划分

![image-20220401191716648](image-20220401191716648.png)

#### 复杂情况

并行执行情况下的时序一致

- GO Binding，父子节点情况，需要先更新父节点
- 程序执行需要做到确定性，即相同的输入可以得到相同的结果
- 多线程情况下执行还是确定性的
- 需要有同步点，GameObject 间不能直接发送消息
- 可能逻辑间有循环依赖，
	- 可能在当前帧递归处理，也可能再下一帧再处理
	- 根据处理方式可能会导致延时情况（lack）

## Q&A

- 一个 Tick 运算时间过长，可以拆分单位分帧运算。即流式。
- 空气墙和其他 go 的区别
- 渲染线程和逻辑线程的同步
- 空间划分处理动态游戏对象 ：
- event 调试
- 物理和动画互相影响：受击时由动画过渡到物理





