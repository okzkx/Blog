## 第五节：光和材质

### Participants of Rendering Computation

#### The Rendering Equation

![image-20220413220314499](image-20220413220314499.png)

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
- ![image-20220414183745137](image-20220414183745137.png)

#### Normal Distribution Function : D 法线分布函数，表示高光强度曲线

- 相比 Phone 高光，高频波峰足够抖，高光逐渐消失的时候是柔和过度

![image-20220414184304102](image-20220414184304102.png)

#### Geometric attenuation term (self-shadowing) ：G，几何遮挡，表示能量损失

![image-20220414184837548](image-20220414184837548.png)

#### Fresnel Equation :

F 菲涅尔现象，视线越垂直，折射越明显，反之反射越明显

![image-20220414185056386](image-20220414185056386.png)

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

![image-20220414190145246](image-20220414190145246.png)

#### PBR Metallic Roughness

![image-20220414190714663](image-20220414190714663.png)

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

![image-20220415103912206](image-20220415103912206.png)

#### Quick shading with precomputation

![image-20220415104120942](image-20220415104120942.png)

### Classis Shadow Solution

#### Big World and Cascade Shadow

级联阴影，视线所达到的地方，用多张 ShadowMap 表示，分辨率一样，表示的范围越来越大，精度越来越低

![image-20220415104525286](image-20220415104525286.png)

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
