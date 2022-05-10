# GAMES104-现代游戏引擎: 从入门到实践

[Games 104](https://www.bilibili.com/video/BV12Z4y1B7th)

## 第一节：游戏引擎导论

### 历史

Engine ，商业，inhouse，免费
Middlewares 中间件公司

### 游戏引擎

制作游戏的框架和工具合集

世界中的无限细节

### Complexity of Simulation

Combat

**Interaction**

Reaction

**Net Sync**

Prediction

Render

- Animation
- **Motor**
- *Camera
- Effect**
- Cloth
- **Sound**

### Developer Platform

为了艺术家，设计师，程序员设计工具

非常复杂的工具

#### 如何学习游戏引擎

需要学习几乎所有计算机科学知识

学习路径：沿着主干道前进

#### MVVM 模型

![image-20220316105903407](../../../.gitbook/assets/image-20220316105903407.png)

#### Cause about

- Render
- Animation
- Physics
- Gameplay （Game World Rules）
- MIsc Systems
  - Effects
  - Navigation
  - Camera

#### Tool Set

- C++ Reflection
- Data Schema

#### Online Gameing

- Lockstep Synchronization
- State Synchronization

#### Advanced Technology

- Motion Matching
- Procedural Content Generation
- Data-Oriented Programming
- Job System
- Lumen
- Nanite

#### References

- No required textbooks
- Game Engine Architecture

#### Mini Engine

- Mini runtime framework
- Mini editor
- Building basic knowledge system of game engine

![image-20220316112401142](../../../.gitbook/assets/image-20220316112401142.png)

#### 作业目标

联网对战游戏

## 第二节：引擎架构分层

### 游戏引擎分层简介

#### Layers

- Tool Layer
- Function Layer
- Resource Layer
- Core Layer
- Platform Layer
- 3rd Party Libraries

5 + 1 层次涵盖了几乎所有引擎功能

#### Chans of Editor

- Editor GUI

#### Make it visible movable and playable

- Physics
- Animation
- Rendering
- Camera HUD and Input
- Script FSM and AI

#### Data and Files

#### Swiss Knife of Game Engine

#### Launch on Different Platforms

- Operation Systems
- Platform File Systems
- Graphics API
- Platform SDK

etc

- Consoles
- Input Devices
- Publishing Platforms

#### Practice

make an animate character

### Resource Layer

- How to Access My Data
  - Resource -> asset
- Rentime Asset Manager
- Manage Asset Life Cycle

### Function Layer

- How to Make the world alive
- ![image-20220330202503555](../../../.gitbook/assets/image-20220330202503555.png)
- 每个 Tick 进行一遍所有的系统执行

#### Dive into Tick

- tick logic 先世界模拟
- tick render 再进行渲染

![image-20220330202900803](../../../.gitbook/assets/image-20220330202900803.png)

#### Heavy - duty Hotchpotch

#### Multi-Threading

难点在 Dependency 管理

### Core Layer

#### Math Library

- math efficiency
- 倒数平方根很慢，quake 3 有一个近似算的很快
- SIMD ：专门做向量运算的组件，可以同时进行 4 次相加

#### Data Structure and Container

![image-20220330204838371](../../../.gitbook/assets/image-20220330204838371.png)

需要重新写游戏引擎手动控制的容器数据结构，因为语言自带的托管内存的数据结构所占据的内存会产生很多空洞，内存的消耗是不可控的。

出现很多内存碎片，访问效率很低。

#### Memory Management 内存管理

现代计算机的内存数据读写还是图灵机方式

高效的内存分配模式

- Put data together 数据在一块
- Access data in order 数据按顺序读写
- Allocate and de-allocate as a block 按照块状分配内存

#### Foundation of Game Engine

### Platform Layer

#### Target on Different Platform

需要做到平台无关性

- File system
- Graphic api
- Hardware Architecture 特定机器优化

### Tool Layer

#### Allow artists to create game

Manual Editors

- Level Editor
- Logical Editor
- Shader Editor
- Animation Editor
- UI Editor
- Flexible of coding languages

#### Digital Content Creation

Asset conditioning pipeline

tool chains

### Why Layered Architecture

- Decoupling and Reducing Complexity
- Response for Evolving Demands

### Mini Engine : Pilot

![image-20220330211847914](../../../.gitbook/assets/image-20220330211847914.png)

### Simple ECS framework

![image-20220330212201585](../../../.gitbook/assets/image-20220330212201585.png)

### Takeaways

- Engine is designed with a layered architecture
- Lower levels are more stable and upper levels are more customizable
- Virtual world is composed by a set of clocks - ticks

## 第三节：如何构建游戏世界

### 游戏世界中的物体

#### Dynamic Game Object

#### Static Game Object

#### Environment

- Sky
- Vegetation
- Terrain

#### Other Game Objects

- Air wall
- Trigger Area
- Ruler
- Navigation mesh

#### Everything is a Game Object

### How to Describe a GameObject

- Name
- Property

  - Shape
  - Position
  - Capacity of battery
- Behavior

  - Move
  - Scout

##### OOP

通过面向对象的方式描述游戏物体，实际上的现实中的物体不是清晰的树状派生关系，而是组合居多

##### Component Base

组件化，通过组件组合成游戏对象

![image-20220401185045234](../../../.gitbook/assets/image-20220401185045234.png)

![image-20220401185248796](../../../.gitbook/assets/image-20220401185248796.png)

#### Takeaways

- Everything is a game object in the game world
- Game object could be described in the component-based way

### Make world alive

- Object-based Tick
- Tick 的执行是遍历同类型的每个个体，而不是同个体的每个类型
- 按照流水线的做法比较高效

### Interactive

- Events
- Interface
- 可拓展的消息系统

#### Manage Game Object

- Game object uid
- position
- Scene management 空间管理的核心
  - No division
  - Divided by grid
    - Quadtree
  - Hierarchical segmentation 树状结构划分

![image-20220401191716648](../../../.gitbook/assets/image-20220401191716648.png)

#### 复杂情况

- GO Binding，父子节点情况，时序很重要，因为有并行情况
  - 逻辑上的混乱性，而好的程序执行是确定性的
  - 挑战性是在多线程情况下执行还是确定性的
  - 需要有同步点，GameObject 间不能直接发送消息
  - 可能逻辑间有循环依赖，根据处理方式可能会导致延时情况

## 第四节：游戏引擎中的渲染实现

### 渲染概述

#### Challenges on Game Rendering

1. Complex render objects
2. Deal with architecture of modern computer with a complex combination of CPU and GPU
3. Commit a bullet-proof framerate, 
   1. 30 or 60 or 120 fps
   2. HD, 1080p, 4k, 8k
4. CPU memory

#### Rendering on Game Engine

A heavily optimized parctical software framework to fulfill the critical rendering requirements of games on modern hadware

#### Outline of Rendering

1. Basics of Game Rendering
   - Hardware architecture
   - Render data organization
   - Visibility
2. Materials, Shaders and Lighting
   - PBR
   - Shader permutation
   - Lighting
   - Point / Directional lighting
   - IBL / Simple GI
3. Special Rendering
   - Terrain
   - Sky / Fog
   - Postprocess
4. Pipeline
   - Forward, deferred rendering forward plus
   - Real pipeline with mixed effects
   - Ring buffer and V-Sync
   - Tiled-based rendering

### 渲染系统的对象

#### Building Block of Rendering

![image-20220413212212448](../../../.gitbook/assets/image-20220413212212448.png)

![image-20220413212321697](../../../.gitbook/assets/image-20220413212321697.png)

![image-20220413212331478](../../../.gitbook/assets/image-20220413212331478.png)

#### Computation - Texture Sampling

1. Use tow nearest mipmap levels
2. Perform bilinear inerpolation in both mip-maps
3. Linear interpolate between the results

### GPU 架构

The dedicated hardware to solve massive jobs

#### SIMD and SIMT

- SIMD 单指令多数据
- SIMT 单指令多线程

![image-20220413212402439](../../../.gitbook/assets/image-20220413212402439.png)

#### GPU Architecture

![image-20220413212419569](../../../.gitbook/assets/image-20220413212419569.png)

![image-20220413212434311](../../../.gitbook/assets/image-20220413212434311.png)

![image-20220413212513383](../../../.gitbook/assets/image-20220413212513383.png)

#### GPU Bounds and Performance

- Memory Bounds
- ALU Bounds
- TMU (Texture Mapping Unit) Bound
- BW (Bandwidth) Bound

![image-20220413212550528](../../../.gitbook/assets/image-20220413212550528.png)

![image-20220413212555358](../../../.gitbook/assets/image-20220413212555358.png)

### Renderable
可绘制的东西

- Mesh Render Component

- Building Blocks of Renderable

- Mesh Primitive
  - struct Vertex
    - position
    - color
    - normal
  - struct Triangle
    - Vertex[3]
  
- Vertex and Index Buffer
  - Vertex Datda
  - Index Data
  
  ![image-20220413212626112](../../../.gitbook/assets/image-20220413212626112.png)
  
- We Need Per-Vertex Normal

- Materials

  ![image-20220413212640347](../../../.gitbook/assets/image-20220413212640347.png)

- Material Model

  ![image-20220413212730864](../../../.gitbook/assets/image-20220413212730864.png)

- Various Textures in Materials

- Variety of Shaders



### Render Objects on Engine

- Coordinate System and Transformation

- Object with Many Materials

- Resource Pool
  - Use Handle to Reuse Resources
  
    ![image-20220413213129463](../../../.gitbook/assets/image-20220413213129463.png)
  
    ![image-20220413213209476](../../../.gitbook/assets/image-20220413213209476.png)
  
- Instancing 实例化，前边的都是物体绘制的定义，实际上绘制到屏幕上才是实例

- Sort by Material

- GPU Batch Rendering

  ![image-20220413213223322](../../../.gitbook/assets/image-20220413213223322.png)

### Visibility Culling 可见性裁剪

  - Culling One Object
    - View Frustum
    - Bounding Box
    - Hierarchial View Frustum Culling
    - Construction and Insertion of BVH in GameEngine
      - BVH 在动态物体多的情况下构建成本低
      - PVS (Potential Visibility Set) 对于空间上的一点关于其他物体的潜在可见性，即是否不会被其他物体遮挡
      - The Idea of Using PVS in Stand-alone Games
    - GPU Culling
      - Depth pre pass / early z 提前渲染深度到 z buffer，防止重复像素点着色

### Texture Compression

纹理压缩

- Traditional image compression like JPG and PNG
- In game texture compression
  - 需要支持随机访问
- Block Compressions
  - Common block-based compression format
  - On PC BC7 or DXTC
  - On mobile, ASTC or ETC

### Authoring Tools of Modeling

- Modeling
  - Polymodeling : MAX, MAYA, Blender 网格建模
  - Sculptiong : 雕刻
  - Scanning : 扫描
  - Procedural Modeling : 程序化生成

### Cluster-Based Mesh Pipeline

- Sculpting Tools Create Infinite Details 无数的细节

- GPU-Driven Rendering Pipeline

- Geometry Rendering Pipeline Architecture

- Programmable Mesh Pipeline

- Mesh shader, cluster base mesh

  ![image-20220413213322563](../../../.gitbook/assets/image-20220413213322563.png)

- GPU Driven
  - 把越来越多 CPU 上的计算放到 GPU 上做

### PILOT

小引擎项目 https://github.com/BoomingTech/Pilot

## 第五节：光和材质

### Participants of Rendering Computation

#### The Rendering Equation

![image-20220413220314499](../../../.gitbook/assets/image-20220413220314499.png)

实时解渲染方程

#### Three Main Challenges

- Light Challenges 光的表现
  - visibility to Lights
  - Light Source Complex
- How to do Integral Efficiently on Hardware 对光的积分
- Any matter will be light source 全局光照
  - Indirect illumination
  - Global illumination

### Simple Light Solution

#### Simple Light

- Using simple light source as main light
- Using ambient light to hack others
- Supported in graphics API

#### Environment Map Reflection

- Using environment map to enhance glossary surface reflection
- Using environment mipmap to represent roughness of surface

#### Blinn-Phone Materials

- 基于光叠加原理，光的线性叠加
- 漫反射 + 高光 + 环境光
- 能量不守恒
  - Unstable in ray-tracing
  - non-energy conserving
- 无法表达真实材质
  - 都是塑料质感 

#### ShadowMap

- 使用纹理记录距离光源最近像素的深度
- Resolution limit ，纹理精度导致阴影锯齿
- Depth precision ，自遮挡，加 bias 和提高纹理精度解决

#### Basic Shading Solution

- Simple light + Ambient
  - dominent light solves
  - ambient and EnvMap solve
- Blinn-Phong material
- Shadow map

#### Precompute global illumination

- 空间换时间
- Good compression rate，存储百万个 probes
- do integration with material function
- Fourier Transform 傅里叶变换
  - 任何频谱都是不同频率的波长的叠加，
  - 图片由时域转到频域，去掉高频信息或低频信息，再转回时域，能对图片保留相应信息
- Convolution Theorem 卷积
- Spherial Harmonics 球谐函数
  - 1阶 SH 只需要 9 个值，压缩后是 4 Bytes 就能存储一个 Diffuse 光场
- SH Lightmap ：Precomputed GI
  - LightMap 是预计算的，对静态物体的表面的每个点的环境光照存储
  - 基于上面 SH 的理论，规定 LIghtMap 中一个 4 Bytes 颜色块定义了空间中的一个静态物体表面中的一个点接收到的全局光照
  - 全局静态物体的表面需要二维展开到 LightMap 上，所以 LightMap 也是一个 Altas
- Light Probe: Probes In Game Space
  - Probe 在玩家感知强的地方，在环境光变换大的地方密集
  - LightProbe 可以看作定义在 3 维空间的 LightMap，但不需要逐像素体素，所以可以实时预计算
  - LIght Probe Point Generation 工业上需要自动化生成 LIghtProbe 采样点
  - Reflection Probe：反射探针可以看作高精度的，做了范围限定的 LightProbe

- 现代游戏入射光
  - 直接光照：光源类型 + shadowmap
  - 静态间接光照：LightMap
  - 实时间接光照：LightProbe，Reflection Probe
  - 后处理光照：SSAO，SSR

### Physicaly base rendering (PBR)

#### Microfacet Theory 微表面理论

#### BRDF Model Based on Microfacet

- 常用 GGX 模型
- ![image-20220414183745137](../../../.gitbook/assets/image-20220414183745137.png)

#### Normal Distribution Function : D 法线分布函数，表示高光强度曲线

- 相比 Phone 高光，高频波峰足够抖，高光逐渐消失的时候是柔和过度

![image-20220414184304102](../../../.gitbook/assets/image-20220414184304102.png)

#### Geometric attenuation term (self-shadowing) ：G，几何遮挡，表示能量损失

![image-20220414184837548](../../../.gitbook/assets/image-20220414184837548.png)

#### Fresnel Equation :

F 菲涅尔现象，视线越垂直，折射越明显，反之反射越明显

![image-20220414185056386](../../../.gitbook/assets/image-20220414185056386.png)

#### Physical Measured Material 实际上去测量真实物理材质的 BRDF

#### Disney Principled BRDF 迪士尼原则的 BRDF 模型

- 每个参数必须符合迪士尼原则
- 参数需要尽可能的少
- 数值参数需要归一化到在 0 ~ 1
- 参数的任意组合不能出现 BUG
- 引擎不是真实世界模拟器，而是游戏程序创造工具

- Disney Principle Material Parameters 迪士尼材质参数

#### PBR Specular Glossiness

SG 模型，全部参数都用纹理表达

![image-20220414190145246](../../../.gitbook/assets/image-20220414190145246.png)

#### PBR Metallic Roughness

![image-20220414190714663](../../../.gitbook/assets/image-20220414190714663.png)

convert MR to SG，MR 是对于 SG 的封装，依赖 SG，参数相对 SG 对艺术家更友好

#### PBR Pipeline MR vs SG

MR 在金属与非金属过度容易出现白边

### Image-Based Lighting(IBL) 基于图像的光照

#### Basic Idea of IBL 

cube map，提前预处理环境光

#### Diffuse Irradiance Map

提前知道卷积，模糊的结果

#### Specular 

- 使用 MipMap 存储不同粗糙度的 Specular
- LUT，提前存储这个 BRDF roughness 和 cosθ 的关系

![image-20220415103912206](../../../.gitbook/assets/image-20220415103912206.png)

#### Quick shading with precomputation

![image-20220415104120942](../../../.gitbook/assets/image-20220415104120942.png)

### Classis Shadow Solution

#### Big World and Cascade Shadow

级联阴影，视线所达到的地方，用多张 ShadowMap 表示，分辨率一样，表示的范围越来越大，精度越来越低

![image-20220415104525286](../../../.gitbook/assets/image-20220415104525286.png)

阴影是渲染管线中的最耗时的部分，场景中每个物体需要重画，同时存储量高

#### Hard Shadow vs Realistic Shadow

软阴影

- Percentage Closer Soft Shadow , PCF
- PCSS
- Variance Soft Shadow Map

#### Summary of Popular AAA Rendering

- LIghtmap + Lightprobe
- PBR + IBL
- Cascade shadow + VSSM

### Moving Wave of High Quality

- quick evolving of GPU
- Real-Time Ray-Tracing on GPU
- Real-Time Global Illumination
  - SCreen-space GI
  - SDF Based GI
  - Voxel-Based GI (SVOGI / VXGI)
  - RSM / RTX GI

#### More Complex Material Model

- BSDF (Strand-based hair)
- BSSRDF

#### Virtual Shadow Maps

ue5 的方法，平均分布 Shadow Map ，动态加载

#### Ocean of Shader

- 海量的不同的 Shader
- Artist Create Infinite More Shaders
- Uber Shader and Variants 使用宏定义产生不同的 Shader

#### Cross Platform Shader Compile

同一个 shader 可能可以编译到不同的 Graphics API

### Pilot  Engine

## 第六节：游戏中大气和云的渲染

### 地形概述

#### Environment Components in Games

- Sky and Cloud
- Vegetation
- Terrain

#### Terrain Rendering

### 地形的几何

#### Heightfield 高度图，地形渲染的主力

- Height Map
- Contour Map 
- Expressive Heightfield Terrains

#### 使用 Mesh Grid 生成网格

- Lod 近处密，远处稀疏
- Adaptive Mesh Tessellation 网格细分
- ![image-20220426213931304](../../../.gitbook/assets/image-20220426213931304.png)
- 当 FOV 越来越窄的时候，地面在屏幕上显示的会越来越大，所以网格远处的精度需要相应的增大。
- Two Golden Rules of Optimizatioin
- View-dependent error bounds
  - Distance to camera and fov
  - Error compare to ground truth

#### Triangle-Based Subdivision 三角形抛分算法

- 正方形网格用四个等腰直角三角形组成
- 永远切分等腰直角三角形最长一边

##### T-Junction

![image-20220426214615701](../../../.gitbook/assets/image-20220426214615701.png)

- 相邻三角形需要切分的同样密，来防止裂边

##### summary

- 使用无 T 分裂的等腰直角三角形组合成地形
- 但是实际上游戏上用的不多
- 效率、绘制都没有问题
- 但是作为地形数据和保存，地形数据的序列化需要使用另外一种格式，反序列化后的渲染可以用这种格式

#### QuadTree-Based Subdivision 基于四叉树的地形表达

![image-20220426215321576](../../../.gitbook/assets/image-20220426215321576.png)

- 最大范围有个上线，不会完全归一，比如 512 * 512 米
- 用磁盘上的一块区域，比如一张纹理来表达
- 这个地形数据组织是场景地形和资源管理的总和，可以搭配上上方三角形网格生成方案来渲染

##### Solving T-Junctions among Quad Grids

###### Stitching

![image-20220426220351731](../../../.gitbook/assets/image-20220426220351731.png)

四叉树管理的地形数据反序列化成为不同密度的三角形。为了保持连续，可以使用 Stitching 方法，退化密度高的三角形，出现面积为 0 的三角形。来粘合不同密度的地块解决 T-junction 问题。

#### Triangulated Irregular Network (TIN)

- 直接从高度图生成三角形后，通过三角形简化生成三角形数量很少的地形
- 根据游戏内容的不同，需要使用不同的算法

#### GPU-Based Tessellation

- 借助 GPU 来程序化生成 LOD 地形
- 即渐变的网格密度变化

![image-20220426221350523](../../../.gitbook/assets/image-20220426221350523.png)

![image-20220426221512303](../../../.gitbook/assets/image-20220426221512303.png)

- Mesh Shader Pipeline DX12 以上

#### Real-Time Deformable Terrain

- 实时的可变形的地形
- 可以做出泥土，雪地的凹陷
- 地型变化的效果，物理碰撞的更新更难做

#### Non-Heightfield Terrain

- 不基于高度图的地型
- 比如悬崖，洞穴，一般使用放置物体在上面，不属于地形一部分，属于场景一部分
- 可以去掉地型上三角形的顶点，让 GPU 不渲染周围三角形，用物体填充来弥补

#### Volumetric Representation

基于体素的表示

##### Marching Cube

![image-20220426224113535](../../../.gitbook/assets/image-20220426224113535.png)

空间上划分体素，每一个体素里大概总共可能会有 15 种不同的近似平面，每个体素用一个字节的数据记录近似平面的表示的编号。

##### Transition Cell Lookup Table

Transvoxel Algorithm, voxel 也可以做 LOD

![image-20220426224501295](../../../.gitbook/assets/image-20220426224501295.png)

基于 volumetric 的表达，在未来有希望做出纯动态地型

### Terrain Materials

![image-20220426231148570](../../../.gitbook/assets/image-20220426231148570.png)

材质混合

##### Simple Texture Splatting

简单的 Alpha 混合，并不自然

![image-20220426231437730](../../../.gitbook/assets/image-20220426231437730.png)



##### Advanced Texture Splatting

叠加高度后的混合

![image-20220426231351002](../../../.gitbook/assets/image-20220426231351002.png)

叠加 Bias，一点小 Hack

#### Sampling from Material Texture Array

![image-20220426231649827](../../../.gitbook/assets/image-20220426231649827.png)

- 和 3D Texture 不一样，3D Texture 的采样需要三线性插值的采样。
- Texture Array 每层之间没关系，是 Texture Index + 双线性插值采样
- 使用 IndexMap + TextureArray 采样地面纹理

####  Parallax and Displacement Mapping

![image-20220426232055558](../../../.gitbook/assets/image-20220426232055558.png)

- Color Mapping : Albedo 着色
- Bump Mapping 法线纹理影响光照模型
- Parallax Mapping 视差贴图 ：使用 Ray Martching 的方式着色表面，出现遮挡
- Displacement Mapping 曲面细分 ：完全和细化网格是一致的

#### Expensive  Material Blending

昂贵的材质混合

- Many Texturing ，每个像素大量的 Texture Array 纹理采样和线性插值
- Huge Splat map ，加载了很大张的纹理，但是大部分的像素是用不到的

#### Virtual Texture

![image-20220426234404170](../../../.gitbook/assets/image-20220426234404170.png)

最主流的地型绘制方案

#### VT Implementation, DirectStorage & DMA

- CPU，内存，显卡，硬盘之间的数据交互
- 现在一般是 CPU 调度内存和显存
- 次世代的 DMA 可以支持硬盘和显存

#### Floating-point Precision Error

浮点数在大数字时，小数部分变短，精度变低

- Camera-Relative Rendering ,基于摄像机空间的渲染运算，标准做法
- SubLevel，基于子场景空间坐标系的渲染运算

### 植被道路贴花

#### Tree Rendering

![image-20220427000307667](../../../.gitbook/assets/image-20220427000307667.png)

Speed Tree ，植被渲染中间件

#### Decorator Rendering

![image-20220427000426246](../../../.gitbook/assets/image-20220427000426246.png)

#### Road and Decals Rendering

![image-20220427000733087](../../../.gitbook/assets/image-20220427000733087.png)

场景中的细节可以预烘培进入 Virtual Texture

#### Procedure Terrain Creation

### Atmosphere 大气散射

#### Analytic Atmosphere Appearance Modeling

![image-20220427225019614](../../../.gitbook/assets/image-20220427225019614.png)

使用经验公式去拟合大气散射现象

#### Participating Media

![image-20220427225255363](../../../.gitbook/assets/image-20220427225255363.png)

####  How Light Interacts with Participating Media 

大气散射通过一个介质分为四个阶段

![image-20220427231309822](../../../.gitbook/assets/image-20220427231309822.png)

#### Volume Rendering Equation

![image-20220427231524677](../../../.gitbook/assets/image-20220427231524677.png)

- Transmittance ：光线被介质的衰减
- Increase factor ：光线进入眼睛的能量

#### Real Physics in Atmosphere

1. 太阳光是由不同波长即不同颜色的光线组合而成的白色

#### Scattering Types

##### Rayleigh Scattering 瑞丽散射 

- 介质直径小，光线波长越短散射越厉害
- 蓝紫光波长短
- 拟合方程
- Rayleigh Scattering Equation
- ![image-20220427232300675](../../../.gitbook/assets/image-20220427232300675.png)

##### Mie scattering 米式散射 

- 散射对光的波长不敏感
- 雾 Fog，光晕 Halo of Sun
- Mie Scattering Equation

![image-20220427232916397](../../../.gitbook/assets/image-20220427232916397.png)

#### Variant Air  Molecules Absorption

大气的吸收效应

- 臭氧
- 甲烷

#### Single Scattering vs Multi Scattering

![image-20220427233253363](../../../.gitbook/assets/image-20220427233253363.png)

#### Ray Marching

沿着视线的方向，对太阳光的积分

![image-20220427233606867](../../../.gitbook/assets/image-20220427233606867.png)

#### Precomputed Atmospheric Scattering

![image-20220429154220247](../../../.gitbook/assets/image-20220429154220247.png)

![image-20220506101713410](../../../.gitbook/assets/image-20220506101713410.png)

##### Challenges of Precomputed Atmospheric Scattering

- Precomputation Cost
- Authoring and Dynamic Adjustment of Environments
- Runtime Rendering Cost

#### Production Friendly Quick Sky and Atmosphere Rendering

- Simplify Multi-scattering Assumption
-  假设对于空间上某个点四面八方的入射光是均匀的
- Fixed view position and sum position to remove 2 dimensions out of LUT
- Generated a 3D LUT to evaluate aerial-perspective effects by ray marching

### 云的渲染

#### Cloud Type

![image-20220506104023361](../../../.gitbook/assets/image-20220506104023361.png)

#### Present Cloud

- Mesh-Based Cloud Modeling
- Billboard Cloud
- Volumetric Cloud Modeling
  - Weather Texture 存云的厚度
  -  Noise Functions
    - Perlin Noise 棉花丝感觉
    - Worley Noise 泡泡感觉
  - ![image-20220506104451054](../../../.gitbook/assets/image-20220506104451054.png)
  - Cloud Density Model
    - ![image-20220506104738431](../../../.gitbook/assets/image-20220506104738431.png)
  - Rendering Cloud by Ray Marching
    - ![image-20220506104925464](../../../.gitbook/assets/image-20220506104925464.png)



## 第七课：游戏中渲染管线、后处理和其他

Games 104 作为一个通识课

### Ambient Occlusion 环境光遮蔽

#### Precompute  AO

模型细节烘培到纹理后，法向烘培在法线纹理上，但是却丢失了几何遮蔽细节。

这时可以烘培 AO 图，烘培 AO 细节

#### Realtime AO

- Screen Space Ambient Occlusion
  - SSAO Plus 半球面
- HBAO Horizon-based Ambient Occlusion
  - Use the depth buffer as a heightfield on 2D surface
  - Trace rays directly in 2D and approximate AO from horizon angle
- GTAO Ground Truth-based Ambient Occlusion

### Depth Fog

