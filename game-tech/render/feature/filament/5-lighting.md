﻿﻿# 5. Lighting

- **Direct lighting** 直接光: ，Directional light 平行光，punctual lights 点光源
- **Indirect lighting** 间接光: image based lights (IBLs), for both local[2](https://google.github.io/filament/Filament.html#endnote-localprobesmobile) and distant light probes.

## 5.1 Units

光照能量单位

![*Photometric units*](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241502252.png)

光源类型以及每时刻发出的能量

![*Intensity unity for each light type*](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241501450.png)

### 5.1.1.3 Luminous intensity

$$
I = E \cdot d^2
$$

## 5.2 Dirct lighting

### 5.2.1 Dirctional lights

<img src="https://google.github.io/filament/images/diagram_directional_light.png" alt="Dirctional lights" style="zoom:50%;" />
$$
L_{out} = f(v,l) E_{\bot} \left< N \cdot L \right>
$$
Evaluate dirctional light in code


```
vec3 l = normalize(-lightDirection);
float NoL = clamp(dot(n, l), 0.0, 1.0);

// lightIntensity is the illuminance
// at perpendicular incidence in lux
float illuminance = lightIntensity * NoL;
vec3 luminance = BSDF(v, l) * illuminance;
```

### 5.2.5 Lights parameterization

![LIght types parameters](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241532556.png)

### 5.2.6 Pre-exposed lights

简单的应用相机曝光

`fragColor = luminance * camera.exposure;`

## 5.3 Image based lights

![image-20230224161643436](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241616531.png)

### 5.3.1 IBL Types

- Distant light probes : 无限远的环境光，通常基于环境纹理，不会被遮挡
- Local light probes : 多处确定位置的环境光，使用插值获得当前位置环境光
- Planar reflections : 为每个镜面多渲染一个倒影世界，作为镜面间接光
- Screen space reflection : 在屏幕空间上做光线追踪获得对应的间接光颜色

### 5.3.2 IBL Unit

- Color calibration: using a gray card or aMacBeth ColorChecker
- Camera settings: aperture, shutter and ISO
- **Luminance samples**: using a spot/luminance meter

### 5.3.3 Processing light probes

#### 5.3.4.1 Diffuse BRDF integration 漫反射积分

![image-20230224183637503](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241836649.png)

离散时域

![image-20230224183655308](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241836346.png)

但是对于半球上每个像素点采样是不可取的，需要采样图片一半的像素数量

预烘培 irradiance map，对于每个法向量，预先计算其对应的 E 存储在对应像素

<img src="https://google.github.io/filament/images/ibl/ibl_river_roughness_m0.png" title="Image-based environment" width="500"/><img src="https://google.github.io/filament/images/ibl/ibl_irradiance.png" title="Image-based irradiance map" width="500"/>

##### 使用 SH 拟合 EnvMap 节省空间

使用 3 阶 SH 公式，对应的 9 个基函数

![normalized_basis_functions_per_band](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241840579.png)

将常数项存入 shader 的全局变量

```
vec3 irradianceSH(vec3 n) {
    // uniform vec3 sphericalHarmonics[9]
    // We can use only the first 2 bands for better performance
    return
          sphericalHarmonics[0]
        + sphericalHarmonics[1] * (n.y)
        + sphericalHarmonics[2] * (n.z)
        + sphericalHarmonics[3] * (n.x)
        + sphericalHarmonics[4] * (n.y * n.x)
        + sphericalHarmonics[5] * (n.y * n.z)
        + sphericalHarmonics[6] * (3.0 * n.z * n.z - 1.0)
        + sphericalHarmonics[7] * (n.z * n.x)
        + sphericalHarmonics[8] * (n.x * n.x - n.y * n.y);
}
```



##### SH 投影

$L^m_l$ 为各个基函数的系数，通过进行半球面积分进行求得一个大致值

$L^m_l=∫_ΩL(s)y^m_l(s)ds$

$L^m_l=∫^π_{θ=0}∫^{2π}{ϕ=0}L(θ,ϕ)y^m_l(θ,ϕ)sinθdθdϕ$

$L^m_l= 2\pi∑L(s)y^m_l(s)$

##### SH 重建

对于任意一个方向累加各层基函数以及他们的系数

$$
\hat{L}(s)=∑_l∑_{m=−l}^lL^m_ly^m_l(s)
$$

### 5.3.4.2 Specular BRDF integration 镜面反射积分

镜面反射在半球上对所有方向的光线做积分

![image-20230224184549573](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241845618.png)

![image-20230224184602214](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241846259.png)

在图上表示为，对于法线对应的半球做重要性采样，在 EnvMap 对应的像素点上的光线做累加

##### 解决办法

和漫反射一样，没法实时对图片一半像素做采样，所以也需要预烘培

把渲染方程的 BRDF 项和 irradiance 项拆开来分别预烘培

$L_{out} = ∫f(\Theta)L(l)dl = ∫f(\Theta)dl * ∫L(l)dl$

$\int f(\Theta) = [f_{0}DFG_1(n⋅v,α)+f_{90}DFG_2(n⋅v,α)]$

$∫L(l)dl = LD(n,α)

###### 预烘培

1. DFG 图是横坐标为 α 纵坐标为 $⟨n⋅v⟩$ 的纹理，两张 DFG 图分别为 f0 和 f90 的系数，他们的累加是特定粗糙度和特定视线对于法线的夹角下，对于该粗糙度系列材质（更改粗糙度不需要重新生成 DFG）BRDF 的积分和。将两张单通道 DFG 组合成一张双通道 DFG 图。![](https://google.github.io/filament/images/ibl/dfg_approx.png)
2. LD 图是 LOD Environment map，使用 n 向量采样对应像素，α 为 lod 程度，越粗糙 mipmap 越高，同时采样到的原始像素数量就越多。

### 完整公式

$$
L_{out}(n,v,α,f0,f{90})≃[f_{0}DFG_1(n⋅v,α)+f_{90}DFG_2(n⋅v,α)]×LD(n,α) \\
DFG_1(α,⟨n⋅v⟩)=\frac4N∑_i^N(1−F_c(⟨v⋅h⟩))V(l_i,v,α)\frac{⟨v⋅h⟩}{⟨n⋅h⟩}⟨n⋅l⟩ \\
DFG_2(α,⟨n⋅v⟩)=\frac4N∑_i^NFc(⟨v⋅h⟩)V(l_i,v,α)\frac{⟨v⋅h⟩}{⟨n⋅h⟩}⟨n⋅l⟩ \\
LD(n,α)=\frac{∑_i^NV(l_i,n,α)⟨n⋅l⟩L_⊥(l_i)}{∑^N_i⟨n⋅l⟩}
$$



| Pipeline stage flag                                          | Required queue capability flag                               |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `VK_PIPELINE_STAGE_NONE`                                     | None required                                                |
| `VK_PIPELINE_STAGE_DRAW_INDIRECT_BIT`                        | `VK_QUEUE_GRAPHICS_BIT` or `VK_QUEUE_COMPUTE_BIT`            |
| `VK_PIPELINE_STAGE_2_PRE_RASTERIZATION_SHADERS_BIT`    `VK_PIPELINE_STAGE_TASK_SHADER_BIT_EXT`    `VK_PIPELINE_STAGE_MESH_SHADER_BIT_EXT`    `VK_PIPELINE_STAGE_VERTEX_SHADER_BIT`    `VK_PIPELINE_STAGE_TESSELLATION_CONTROL_SHADER_BIT`    `VK_PIPELINE_STAGE_TESSELLATION_EVALUATION_SHADER_BIT`    `VK_PIPELINE_STAGE_GEOMETRY_SHADER_BIT` | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_2_VERTEX_INPUT_BIT`    `VK_PIPELINE_STAGE_2_INDEX_INPUT_BIT`    `VK_PIPELINE_STAGE_2_VERTEX_ATTRIBUTE_INPUT_BIT` | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_FRAGMENT_SHADER_BIT`                      | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_EARLY_FRAGMENT_TESTS_BIT`                 | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_LATE_FRAGMENT_TESTS_BIT`                  | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_COLOR_ATTACHMENT_OUTPUT_BIT`              | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_COMPUTE_SHADER_BIT`                       | `VK_QUEUE_COMPUTE_BIT`                                       |
| `VK_PIPELINE_STAGE_HOST_BIT`                                 | None required                                                |
| `VK_PIPELINE_STAGE_2_ALL_TRANSFER_BIT` (`VK_PIPELINE_STAGE_TRANSFER_BIT`)    `VK_PIPELINE_STAGE_2_COPY_BIT`    `VK_PIPELINE_STAGE_2_BLIT_BIT`    `VK_PIPELINE_STAGE_2_RESOLVE_BIT`    `VK_PIPELINE_STAGE_2_CLEAR_BIT`    `VK_PIPELINE_STAGE_2_ACCELERATION_STRUCTURE_COPY_BIT_KHR` | `VK_QUEUE_GRAPHICS_BIT`, `VK_QUEUE_COMPUTE_BIT` or `VK_QUEUE_TRANSFER_BIT` |
| `VK_PIPELINE_STAGE_RAY_TRACING_SHADER_BIT_KHR`               | `VK_QUEUE_COMPUTE_BIT`                                       |
| `VK_PIPELINE_STAGE_ACCELERATION_STRUCTURE_BUILD_BIT_KHR`     | `VK_QUEUE_COMPUTE_BIT`                                       |
| `VK_PIPELINE_STAGE_2_ACCELERATION_STRUCTURE_COPY_BIT_KHR`    | `VK_QUEUE_COMPUTE_BIT`                                       |
| `VK_PIPELINE_STAGE_ALL_GRAPHICS_BIT`                         | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_ALL_COMMANDS_BIT`                         | None required                                                |
| `VK_PIPELINE_STAGE_CONDITIONAL_RENDERING_BIT_EXT`            | `VK_QUEUE_GRAPHICS_BIT` or `VK_QUEUE_COMPUTE_BIT`            |
| `VK_PIPELINE_STAGE_TRANSFORM_FEEDBACK_BIT_EXT`               | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_COMMAND_PREPROCESS_BIT_NV`                | `VK_QUEUE_GRAPHICS_BIT` or `VK_QUEUE_COMPUTE_BIT`            |
| `VK_PIPELINE_STAGE_FRAGMENT_SHADING_RATE_ATTACHMENT_BIT_KHR` | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_FRAGMENT_DENSITY_PROCESS_BIT_EXT`         | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_2_INVOCATION_MASK_BIT_HUAWEI`             | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_2_VIDEO_DECODE_BIT_KHR`                   | `VK_QUEUE_VIDEO_DECODE_BIT_KHR`                              |
| `VK_PIPELINE_STAGE_2_VIDEO_ENCODE_BIT_KHR`                   | `VK_QUEUE_VIDEO_ENCODE_BIT_KHR`                              |
| `VK_PIPELINE_STAGE_2_OPTICAL_FLOW_BIT_NV`                    | `VK_QUEUE_OPTICAL_FLOW_BIT_NV`                               |
| `VK_PIPELINE_STAGE_2_SUBPASS_SHADING_BIT_HUAWEI`             | `VK_QUEUE_GRAPHICS_BIT`                                      |
| `VK_PIPELINE_STAGE_2_MICROMAP_BUILD_BIT_EXT`                 | `VK_QUEUE_COMPUTE_BIT`                                       |
| `VK_PIPELINE_STAGE_TOP_OF_PIPE_BIT`                          | None required                                                |
| `VK_PIPELINE_STAGE_BOTTOM_OF_PIPE_BIT`                       | None required                                                |
