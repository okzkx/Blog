# 基础 AI 上

AI Basic

- Navigation
- Steering
- Crowd Simulation
- Sensing
- Classic Decision Making Algorithms

Advanced AI

- Planning and Goals
- Machine Learning

## Navigation

Navigation Steps

1. Map representation
2. Path finding
3. Path smoothing

Walkable Area 确定可通行的区域

针对不同属性的 npc 是不一样的

Format

- Waypoint Network
- Grid
- Navigation Mesh
- Sparse Voxel Octree

#### Waypoint Network

- 关键点
- 转角点
- 捷径点

先走到路网，再沿着路网到目标点最近的位置

缺点：

- 每次更新地图都得手动更新路网
- 容易走到中间的点

#### Grid

四边形或者六边形，一般是四边形

将可通行区域栅格化，

缺点：

寻路效率低，没法表示层叠结构，内存浪费

#### Navigation Mesh

根据 mesh 自动生成可通行区域

需要是凸多边形或者三角形

Navigation Mesh 生成很复杂

没法做天空寻路

#### Sparse Voxel Octree

八叉树结构来表达空间信息，

Grid 的升维

### Pathfinding

由可通行区域生成 Graph

- 是否可以到达终点
- 快速找到较优的最短路径

#### 搜索算法

- 深度优先
- 广度优先

都比较费

Dijkstra Algorithm

贪心搜索，记录走到该点的路径成本

A*

启发式贪心搜索，记录走到该点的路径成本同时评估经过这个点时候的总路径

启发式算法在标准或者不标准网格都是用连线作为最短距离

#### Path Smoothing

多边形行走的平滑算法

Funnel Algorithm 烟囱算法

得朝某个多边形顶点行走

![image.png](funnel.png)

![image.png](funnel2.png)

### NavMesh Generation - Voxelization

NavMesh 生成

1. 先整个世界体素化
2. 标记出体素上的可通行区域，一般是体素上表面
3. 找到 Edge voxel，生成 Distance field
4. 洪水算法，从 distance 最大的地方开始生成 Navmesh
5. 需要手动修复
6. 地图重新生成的话，上次修复要失效

Polygon Flag ：可以区分不同材质的表面生成不同材质的 Navmesh，

Tile ：大地图块内寻路，邻接 Tile 的顶点要对齐

Off-mesh Link：需要支持非 mesh 的移动，手动连接，比如滑索
