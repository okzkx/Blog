# 4 Material System

材质系统

## 4.1 Standard model

##### 材质的表面表现为漫反射项 + 高光反射项

1. diffuse componnet, fd
2. specular component, fr

![image-20230224092240378](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302240922464.png)
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161033294.png)

##### 微表面模型

- 宏观表面
- 微观表面

<img src="https://google.github.io/filament/images/diagram_macrosurface.png" alt="*Microfacets*" style="zoom: 33%;" />

- Shadowing
- Masking

<img src="https://google.github.io/filament/images/diagram_shadowing_masking.png" alt="Masking and shadowing of microfacets" style="zoom:33%;" />

表面越粗糙，反射 Lobe 越大

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161034735.png)


微表面模型表达式

<img src="https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302240926486.png" alt="image-20230224092623444" style="zoom:80%;" />

- D：微表面分布，NDF or Normal Distribution Function，主要的着色贡献项
- G：微表面的可见性项，减少反射出的光线
- F：决定高光反射和漫反射的贡献权重
- |n * v|, |n * l| 积分的归一化项

## 4.2 Dielectrics and conductors

- 金属，Metalic， Conductor
- 非金属，non-metallic，Dielectric

BSDF 

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161037667.png)

- 用 fd 模拟 BSDF 中的 Scatter
- 纯金属材质是没有漫反射的

<img src="https://google.github.io/filament/images/diagram_brdf_dielectric_conductor.png" alt="BRDF modelization for dielectric and conductor surfaces" style="zoom:33%;" />

## 4.3 Energy conservation

G 项会造成出射能量损失，需要在遮挡严重（粗糙度高），反射性强的区域进行能量补偿实现能量守恒。

## 4.4 Specular BRDF

镜面 brdf

![image-20230224093841161](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302240938225.png)

### 4.4.1 Normal distribution function (specular D)

长尾法线分布函数可以去拟合真实世界表面
D_GGX：
- 光亮衰减处长尾
- 高光处短峰

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161059781.png)


```
float D_GGX(float NoH, float roughness) {
    float a = NoH * roughness;
    float k = roughness / (1.0 - NoH * NoH + a * a);
    return k * k * (1.0 / PI);
}
```

- NoH ：视线与出射光靠近程度，结果成正比
- roughness, a : 粗糙程度，减少 NoH 影响


### 4.4.2 Geometric shadowing (specular G)

几何阴影函数，模型表面的自遮挡损失能量，减少光强，得到剩余出射的光强

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161408361.png)

```
float V_SmithGGXCorrelated(float NoV, float NoL, float roughness) {
    float a2 = roughness * roughness;
    float GGXV = NoL * sqrt(NoV * NoV * (1.0 - a2) + a2);
    float GGXL = NoV * sqrt(NoL * NoL * (1.0 - a2) + a2);
    return 0.5 / (GGXV + GGXL);
}
```

- NoV ：视线到模型表面垂直程度，正相关
- NoL ：出射光到模型表面的垂直程度，正相关
- roughness, a：负相关


#### 4.4.3 Fresnel (specular F)

菲涅尔项

描述能量在两个介质之间的传递效率

菲涅尔效应
- 入射光线和平面越接近平行，反射光越高，折射光越低
- 反射效率还取决于材质 IOR（index of refraction），即光线从空气到材质的角度比。


fx ：法线和光线夹角为 x 时的菲涅尔项的值

- f0 (incident specular reflectance) : 入射反射光线和平面近乎垂直时的折射率，正相关，由 IOR 决定。
- f90 (reflectance at grazing angles ): 入射反射光线和平面近乎平行时的折射率，在光滑表面上几乎为 1 正相关
- u , v * h: 视线和入射光线的靠近程度，在 f90 和 f0 之间插值

#### Shlick 方法来近似 fx

F_Schlick(float u, vec3 h, vec3 f0, float f90)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161422661.png)

- 在自然界中的金属和非金属 f90 一般都为 1

所以优化上面代码
```
vec3 F_Schlick(float u, vec3 f0) {
    float f = pow(1.0 - u, 5.0);
    return f + f0 * (1.0 - f);
}
```


### 4.5 Diffuse BRDF

#### 兰伯特漫反射 Fd_Lambert

任意角度的出射光相同颜色

理想状态应该积分各个方向的输入
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161439208.png)

对公式进行简化
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202306161440820.png)


```
float Fd_Lambert() {
    return 1.0 / PI;
}

vec3 Fd = diffuseColor * Fd_Lambert();
```

#### Burley 漫反射

也需要考虑到菲涅尔项，所以在模型边缘又一圈高亮反射的高光（retro-reflections）

```
float Fd_Burley(float NoV, float NoL, float LoH, float roughness) {
    float f90 = 0.5 + 2.0 * roughness * LoH * LoH;
    float lightScatter = F_Schlick(NoL, 1.0, f90);
    float viewScatter = F_Schlick(NoV, 1.0, f90);
    return lightScatter * viewScatter * (1.0 / PI);
}
```


### Improving the BRDFs

#### Energy gain in diffuse reflectance

当前的 Lambert 漫反射会产生不符合物理的能量，所以需要进行能量衰减。但是过于复杂所以不实现。

#### Energy loss in specular reflectance

镜面反射在 G 项会产生能量损失，金属度高的时候镜面反射大。粗糙度高的时候能量损失大。所以在粗糙金属时能量损失最大。使用能量补偿 energy compensation 来弥补损失。

### 4.8 Parameterization

参数化

| Parameter             | Definition                                                                                                                                                                                                                                                               |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **BaseColor**         | Diffuse albedo for non-metallic surfaces, and specular color for metallic surfaces                                                                                                                                                                                       |
| **Metallic**          | Whether a surface appears to be dielectric (0.0) or conductor (1.0). Often used as a binary value (0 or 1)                                                                                                                                                               |
| **Roughness**         | Perceived smoothness (0.0) or roughness (1.0) of a surface. Smooth surfaces exhibit sharp reflections                                                                                                                                                                    |
| **Reflectance**       | Fresnel reflectance at normal incidence for dielectric surfaces. This replaces an explicit index of refraction                                                                                                                                                           |
| **Emissive**          | Additional diffuse albedo to simulate emissive surfaces (such as neons, etc.) This parameter is mostly useful in an HDR pipeline with a bloom pass                                                                                                                       |
| **Ambient occlusion** | Defines how much of the ambient light is accessible to a surface point. It is a per-pixel shadowing factor between 0.0 and 1.0.<br />This parameter will be discussed in more details in the[lighting](https://google.github.io/filament/Filament.html#lighting) section |

#### 4.8.2 类型和取值范围

| Parameter             | Type and range                            |
| --------------------- | ----------------------------------------- |
| **BaseColor**         | Linear RGB [0..1]                         |
| **Metallic**          | Scalar [0..1]                             |
| **Roughness**         | Scalar [0..1]                             |
| **Reflectance**       | Scalar [0..1]                             |
| **Emissive**          | Linear RGB [0..1] + exposure compensation |
| **Ambient occlusion** | Scalar [0..1]                             |

metallic, roughness and reflectance parameters affect the appearance of a surface.

![](https://google.github.io/filament/images/material_parameters.png)

#### Color space

加载图片，把颜色传入 shader 时要把 sRGB 图片的颜色从 0.45 Gama 空间转到 1.0 linear 空间。压暗颜色。

### 4.8.3 Remapping 参数映射

remap the parameters  *baseColor* , *roughness* and  *reflectance* .

#### 4.8.3.1 基础颜色映射

`vec3 diffuseColor = (1.0 - metallic) * baseColor.rgb`

- 非金属(Dielectrics，绝缘体)大范围的反射，将 BaseColor 作为漫反射颜色，
- 金属(Conductor，Metal)没有漫反射项，基本没有漫反射颜色

#### 4.8.3.2 Reflectance remapping 反射率映射

##### Dielectrics 非金属反射率

- 输入的非金属反射率（感知到的反射率 perception reflectance）参数为 [0 ~ 1]，
- reflectance 一般由艺术家控制，不过怎么控制其最终 f0 都不高，一般工作流也没有 reflectance 项，所以默认为 0。
- 自然界中的绝缘体反射率为 [2% ~ 16%]，需要进行映射
- 绝缘体的镜面反射是消色差的，所以在各个颜色上的反射系数相同
- f0 指视线与法线成 0 度时的反射率，f90 指视线和法线成 90 度此时任何材质反射率都为 1

![image-20230224100211538](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302241002579.png)

<img src="https://google.github.io/filament/images/diagram_reflectance.png" width="1000" style="zoom:50%;" />


| Material                   | Reflectance | IOR          | Linear value |
| -------------------------- | ----------- | ------------ | ------------ |
| Water                      | 2%          | 1.33         | 0.35         |
| Fabric                     | 4% to 5.6%  | 1.5 to 1.62  | 0.5 to 0.59  |
| Common liquids             | 2% to 4%    | 1.33 to 1.5  | 0.35 to 0.5  |
| Common gemstones           | 5% to 16%   | 1.58 to 2.33 | 0.56 to 1.0  |
| Plastics, glass            | 4% to 5%    | 1.5 to 1.58  | 0.5 to 0.56  |
| Other dielectric materials | 2% to 5%    | 1.33 to 1.58 | 0.35 to 0.56 |
| Eyes                       | 2.5%        | 1.38         | 0.39         |
| Skin                       | 2.8%        | 1.4          | 0.42         |
| Hair                       | 4.6%        | 1.55         | 0.54         |
| Teeth                      | 5.8%        | 1.63         | 0.6          |
| Default value              | 4%          | 1.5          | 0.5          |



###### Conductors 金属反射率

|    Metal |   f0f0 in sRGB   | Hexadecimal | Color |
| -------: | :--------------: | :---------: | :---- |
|   Silver | 0.97, 0.96, 0.91 |   f7f4e8    |       |
| Aluminum | 0.91, 0.92, 0.92 |   e8eaea    |       |
| Titanium | 0.76, 0.73, 0.69 |   c1baaf    |       |
|     Iron | 0.77, 0.78, 0.78 |   c4c6c6    |       |
| Platinum | 0.83, 0.81, 0.78 |   d3cec6    |       |
|     Gold | 1.00, 0.85, 0.57 |   ffd891    |       |
|    Brass | 0.98, 0.90, 0.59 |   f9e596    |       |
|   Copper | 0.97, 0.74, 0.62 |   f7bc9e    |       |

$$
f_0 = baseColor ⋅ metallic
$$

- 金属的反射率就是其颜色
- 导体的反射是基于颜色与金属度的并且接近于 1，所以导体的 baseColor 和 metallic 在设置的时候都需要接近 1
- 反射力度无关 Reflectance

##### 总体反射率 

材质整体的反射由非金属反射和金属反射加权平均得到，非金属的反射率是 ior，金属反射率是其基本颜色

`vec3 f0 = 0.16 * reflectance * reflectance * (1.0 - metallic) + baseColor * metallic;`

#### 粗糙度映射

$$
α=perceptualRoughness^2
$$

人所感知到的略大于实际的粗糙度
<img src="https://google.github.io/filament/images/material_roughness_remap.png" width="1000"/>
### 各材质参数的大概影响

![](https://google.github.io/filament/images/material_chart.jpg)
