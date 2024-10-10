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
- ![image-20220426213931304](image-20220426213931304.png)
- 当 FOV 越来越窄的时候，地面在屏幕上显示的会越来越大，所以网格远处的精度需要相应的增大。
- Two Golden Rules of Optimizatioin
- View-dependent error bounds
  - Distance to camera and fov
  - Error compare to ground truth

#### Triangle-Based Subdivision 三角形抛分算法

- 正方形网格用四个等腰直角三角形组成
- 永远切分等腰直角三角形最长一边

##### T-Junction

![image-20220426214615701](image-20220426214615701.png)

- 相邻三角形需要切分的同样密，来防止裂边

##### summary

- 使用无 T 分裂的等腰直角三角形组合成地形
- 但是实际上游戏上用的不多
- 效率、绘制都没有问题
- 但是作为地形数据和保存，地形数据的序列化需要使用另外一种格式，反序列化后的渲染可以用这种格式

#### QuadTree-Based Subdivision 基于四叉树的地形表达

![image-20220426215321576](image-20220426215321576.png)

- 最大范围有个上线，不会完全归一，比如 512 * 512 米
- 用磁盘上的一块区域，比如一张纹理来表达
- 这个地形数据组织是场景地形和资源管理的总和，可以搭配上上方三角形网格生成方案来渲染

##### Solving T-Junctions among Quad Grids

###### Stitching

![image-20220426220351731](image-20220426220351731.png)

四叉树管理的地形数据反序列化成为不同密度的三角形。为了保持连续，可以使用 Stitching 方法，退化密度高的三角形，出现面积为 0 的三角形。来粘合不同密度的地块解决 T-junction 问题。

#### Triangulated Irregular Network (TIN)

- 直接从高度图生成三角形后，通过三角形简化生成三角形数量很少的地形
- 根据游戏内容的不同，需要使用不同的算法

#### GPU-Based Tessellation

- 借助 GPU 来程序化生成 LOD 地形
- 即渐变的网格密度变化

![image-20220426221350523](image-20220426221350523.png)

![image-20220426221512303](image-20220426221512303.png)

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

![image-20220426224113535](image-20220426224113535.png)

空间上划分体素，每一个体素里大概总共可能会有 15 种不同的近似平面，每个体素用一个字节的数据记录近似平面的表示的编号。

##### Transition Cell Lookup Table

Transvoxel Algorithm, voxel 也可以做 LOD

![image-20220426224501295](image-20220426224501295.png)

基于 volumetric 的表达，在未来有希望做出纯动态地型

### Terrain Materials

![image-20220426231148570](image-20220426231148570.png)

材质混合

##### Simple Texture Splatting

简单的 Alpha 混合，并不自然

![image-20220426231437730](image-20220426231437730.png)

##### Advanced Texture Splatting

叠加高度后的混合

![image-20220426231351002](image-20220426231351002.png)

叠加 Bias，一点小 Hack

#### Sampling from Material Texture Array

![image-20220426231649827](image-20220426231649827.png)

- 和 3D Texture 不一样，3D Texture 的采样需要三线性插值的采样。
- Texture Array 每层之间没关系，是 Texture Index + 双线性插值采样
- 使用 IndexMap + TextureArray 采样地面纹理

#### Parallax and Displacement Mapping

![image-20220426232055558](image-20220426232055558.png)

- Color Mapping : Albedo 着色
- Bump Mapping 法线纹理影响光照模型
- Parallax Mapping 视差贴图 ：使用 Ray Martching 的方式着色表面，出现遮挡
- Displacement Mapping 曲面细分 ：完全和细化网格是一致的

#### Expensive  Material Blending

昂贵的材质混合

- Many Texturing ，每个像素大量的 Texture Array 纹理采样和线性插值
- Huge Splat map ，加载了很大张的纹理，但是大部分的像素是用不到的

#### Virtual Texture

![image-20220426234404170](image-20220426234404170.png)

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

![image-20220427000307667](image-20220427000307667.png)

Speed Tree ，植被渲染中间件

#### Decorator Rendering

![image-20220427000426246](image-20220427000426246.png)

#### Road and Decals Rendering

![image-20220427000733087](image-20220427000733087.png)

场景中的细节可以预烘培进入 Virtual Texture

#### Procedure Terrain Creation

### Atmosphere 大气散射

#### Analytic Atmosphere Appearance Modeling

![image-20220427225019614](image-20220427225019614.png)

使用经验公式去拟合大气散射现象

#### Participating Media

![image-20220427225255363](image-20220427225255363.png)

#### How Light Interacts with Participating Media

大气散射通过一个介质分为四个阶段

![image-20220427231309822](image-20220427231309822.png)

#### Volume Rendering Equation

![image-20220427231524677](image-20220427231524677.png)

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
- ![image-20220427232300675](image-20220427232300675.png)

##### Mie scattering 米式散射

- 散射对光的波长不敏感
- 雾 Fog，光晕 Halo of Sun
- Mie Scattering Equation

![image-20220427232916397](image-20220427232916397.png)

#### Variant Air  Molecules Absorption

大气的吸收效应

- 臭氧
- 甲烷

#### Single Scattering vs Multi Scattering

![image-20220427233253363](image-20220427233253363.png)

#### Ray Marching

沿着视线的方向，对太阳光的积分

![image-20220427233606867](image-20220427233606867.png)

#### Precomputed Atmospheric Scattering

![image-20220429154220247](image-20220429154220247.png)

![image-20220506101713410](image-20220506101713410.png)

##### Challenges of Precomputed Atmospheric Scattering

- Precomputation Cost
- Authoring and Dynamic Adjustment of Environments
- Runtime Rendering Cost

#### Production Friendly Quick Sky and Atmosphere Rendering

- Simplify Multi-scattering Assumption
- 假设对于空间上某个点四面八方的入射光是均匀的
- Fixed view position and sum position to remove 2 dimensions out of LUT
- Generated a 3D LUT to evaluate aerial-perspective effects by ray marching

### 云的渲染

#### Cloud Type

![image-20220506104023361](image-20220506104023361.png)

#### Present Cloud

- Mesh-Based Cloud Modeling
- Billboard Cloud
- Volumetric Cloud Modeling
  - Weather Texture 存云的厚度
  - Noise Functions
  - Perlin Noise 棉花丝感觉
  - Worley Noise 泡泡感觉
  - ![image-20220506104451054](image-20220506104451054.png)
  - Cloud Density Model
    - ![image-20220506104738431](image-20220506104738431.png)
  - Rendering Cloud by Ray Marching
    - ![image-20220506104925464](image-20220506104925464.png)
