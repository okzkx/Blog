# 全局光照 GI

全局光照的意思是摄像机表面某个像素如何获取到打到这个像素的所有颜色。

理论上的全局光照是所有光源发出的光线经过无穷多次弹射进入摄像机的结果。但是这是计算机算力无法模拟的，可以通过光线追踪算法中的路径追踪，用蒙特卡洛积分去近似这个结果。

不过当前使用的着色方式更多的是光栅化，意思是摄像机在该像素点只接收到对应位置的物体表面的颜色发射出的光线颜色。对于某个物体的表面，我们使用三个光照方案叠加起来去模拟这个全局光照效果。

1. 直接光照
2. 环境光照
3. 间接光照

重要性从高到低

###  直接光照 Directional Lighting

#### 物体材质

##### PBR 物理材质

1. BRDF 双向反射分布函数 bidirectional reflectance distribution function
    1. 一般使用 BRDF 微表面模型来近似物理材质
    2. BRDF 由 F ：菲涅尔项，D：法线分布，G 几何遮蔽 三项构成
2. 但是！需要使用很复杂的公式去描述物理材质，达到艺术家希望的样子。所以大家喜欢 BRDF 与漫反射公式加权平均来达到目标的颜色。
3. Unity 使用的是 BSDF，Reflectance 换成了 Scatter，多了折射功能，这需要用到 ColorBuffer，应该是透明 PBR 材质使用的。

##### NPR 非物理材质（风格化材质）

1. 非物理材质也需要基础的明暗，通常用 Lambert 表示漫反射， Bling-Phone 表示高光。需要折射和透明的话 Schlick 公式近似菲尼尔项。
2. 得到物体的明暗信息后，使用 BumpMap 将 0 ~ 1 的值，映射到 BumpMap 采样对应的颜色。这样艺术家可以很好的调整角色半影处的色值。

#### ShadowMap

ShadowMap 描述光线可达性，PCSS、EVSM 软化阴影

### 环境光照  Evironment Lighting

#### IBL 基于图的环境光

1. CubeMap 存储环境光纹理 EnvMap
2. 为了不必对各个方向的光线在 EnvMap 进行采样，提前使用 MipMap 技巧模糊 EnvMap 预积分了环境光的入射光线。
3. 环境光无法表达光线可见性，甚至说直接光照的光线可见性描述的开销都大，这也是多光源开销大的原因。因为每个光源可见性都得用一张 ShadowMap 做，每帧都得多一次全物体投影。
4. LightProbe 光照探针，预先生成多张 EnvMap。在不同的区域使用不同的 EnvMap

#### SH 基于球面斜坡函数的环境光

1. 球面斜坡函数可以使用一些函数进行线性叠加去近似 EnvMap，所需要的存储的只需函数的系数
2. 与 IBL 相比，所需要的存储空间小，精度差
3. 可以在物体的每个顶点都存储 Environment SH 和 Visiable SH，在粗糙反射物的表达上比 IBL 更加正确。但对于光滑的反射物没法做到高精度着色。

#### Unity 中的环境光

最常用的自然是 CubeMap 存储环境光纹理 EnvMap，场景美术通常也会在环境光照差别较大的地方摆一些 LightProbe 改变对动态物体着色的 EnvMap。我在 HDRP 中有看到 SH 的使用，具体怎么用的还需要研究。

#### SSAO，Screen Space Ambient Occlusion 屏幕空间环境光遮蔽

SSAO 可以简单模拟环境光可见性项
通过屏幕空间的法线纹理 ：NormalBuffer 计算 AO 值，法线表示越凹陷处，AO 值越大，自遮挡阴影就越高。

#### SSDO，Screen Space Directional Occlusion

屏幕空间的全局光照，ColorBuffer 着色点附近随机采样点，计算对着色点的颜色贡献

### 间接光照 Indirect Lighting

最难最不重要的是间接光照，表示直接光照经过多次弹射后对物体表面贡献的光线，为了计算机性能考虑直接认为是二次弹射的贡献了，三次弹射不考虑了。

#### RSM，Reflective Shadow Map

最常用的间接光照方案是反射阴影纹理 ，将 ShadowMap 上被照亮的点作为次级光源。还不确定 HDRP 是否有应用。另外还有 LPV 和 VXGI 方案被别的游戏使用。

#### SSR，Screen Space Refrection

屏幕空间上的反射，需要使用 ColorBuffer 和 DepthBuffer，用光线步进的方式进行入射光的追踪，获取相应点的颜色。 

## 其他 GI

[GI from Local Radiance Transfer——适合移动端上的0.5ms 全动态全实时GI - 知乎](https://zhuanlan.zhihu.com/p/653044045)
