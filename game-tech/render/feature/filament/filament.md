# Filament

## Principles

- Real-time mobile performance
- Quality
- Ease of use
- Familiarity
- Flexibility
- Deployment size

## Notation


| Symbol                       | Definition                                                |
| ---------------------------- | --------------------------------------------------------- |
| **v**v                       | View unit vector                                          |
| **l**l                       | Incident light unit vector                                |
| **n**n                       | Surface normal unit vector                                |
| **h**h                       | Half unit vector between**l**l and **v**v                 |
| **f**f                       | BRDF                                                      |
| **f**dfd                     | Diffuse component of a BRDF                               |
| **f**rfr                     | Specular component of a BRDF                              |
| **α**α                     | Roughness, remapped from using input`perceptualRoughness` |
| **σ**σ                     | Diffuse reflectance                                       |
| **Ω**Ω                     | Spherical domain                                          |
| **f**0f0                     | Reflectance at normal incidence                           |
| **f**90f90                   | Reflectance at grazing angle                              |
| **χ**+**(**a**)**χ+(a)     | Heaviside function (1 if**a**>**0**a>0 and 0 otherwise)   |
| **n**i**o**rnior             | Index of refraction (IOR) of an interface                 |
| **⟨**n**⋅**l**⟩**⟨n⋅l⟩ | Dot product clamped to [0..1]                             |
| **⟨**a**⟩**⟨a⟩           | Saturated value (clamped to [0..1])                       |

## Standard model

### Specular BRDF

#### Normal distribution function (specular D)

法线分布函数，根据粗糙度反射光到指定方向，剩余的光强

D_GGX(NoH, a)

- NoH ：视线与出射光靠近程度，结果成正比
- roughness, a : 粗糙程度，减少 NoH 影响

#### Geometric shadowing (specular G)

几何阴影函数，模型表面的自遮挡损失能量，减少光强，得到剩余的光强

V_SmithGGXCorrelated(float NoV, float NoL, float roughness)

- NoV ：视线到模型表面垂直程度，正相关
- NoL ：出射光到模型表面的垂直程度，正相关
- roughness, a：负相关

#### Fresnel (specular F)

菲涅尔项

菲涅尔效应，入射光线和平面越接近平行，反射光越高，折射光越低

F_Schlick(float u, vec3 f0, float f90)

- f0 :金属度， 0 表示法线和光线夹角，入射反射光线和平面近乎垂直时的折射率，正相关
- f90 : 金属度，入射反射光线和平面近乎平行时的折射率，正相关
- u , v * h: 视线和入射光线的靠近程度，在 f90 和 f0 之间插值

vec3 F_Schlick(float u, vec3 f0)

- f90 ：在自然界中的金属电介质一般为 1

### Diffuse BRDF

#### 兰伯特漫反射 Fd_Lambert

任意角度的出射光相同颜色

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

### Summary

```
float D_GGX(float NoH, float a) {
    float a2 = a * a;
    float f = (NoH * a2 - NoH) * NoH + 1.0;
    return a2 / (PI * f * f);
}

vec3 F_Schlick(float u, vec3 f0) {
    return f0 + (vec3(1.0) - f0) * pow(1.0 - u, 5.0);
}

float V_SmithGGXCorrelated(float NoV, float NoL, float a) {
    float a2 = a * a;
    float GGXL = NoV * sqrt((-NoL * a2 + NoL) * NoL + a2);
    float GGXV = NoL * sqrt((-NoV * a2 + NoV) * NoV + a2);
    return 0.5 / (GGXV + GGXL);
}

float Fd_Lambert() {
    return 1.0 / PI;
}

void BRDF(...) {
    vec3 h = normalize(v + l);

    float NoV = abs(dot(n, v)) + 1e-5;
    float NoL = clamp(dot(n, l), 0.0, 1.0);
    float NoH = clamp(dot(n, h), 0.0, 1.0);
    float LoH = clamp(dot(l, h), 0.0, 1.0);

    // perceptually linear roughness to roughness (see parameterization)
    float roughness = perceptualRoughness * perceptualRoughness;

    float D = D_GGX(NoH, a);
    vec3  F = F_Schlick(LoH, f0);
    float V = V_SmithGGXCorrelated(NoV, NoL, roughness);

    // specular BRDF
    vec3 Fr = (D * V) * F;

    // diffuse BRDF
    vec3 Fd = diffuseColor * Fd_Lambert();

    // apply lighting...
}
```

### Improving the BRDFs

#### Energy gain in diffuse reflectance

漫反射会产生不符合物理的能量，所以不能参与光线传播

#### Energy loss in specular reflectance

镜面反射在 G 项会产生能量损失，使用能量补偿 energy compensation 来弥补损失。

通过 dfg lookup table 查表来获取结果

### Parameterization


| Parameter             | Definition                                                                                                                                                                                                                                                               |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **BaseColor**         | Diffuse albedo for non-metallic surfaces, and specular color for metallic surfaces                                                                                                                                                                                       |
| **Metallic**          | Whether a surface appears to be dielectric (0.0) or conductor (1.0). Often used as a binary value (0 or 1)                                                                                                                                                               |
| **Roughness**         | Perceived smoothness (0.0) or roughness (1.0) of a surface. Smooth surfaces exhibit sharp reflections                                                                                                                                                                    |
| **Reflectance**       | Fresnel reflectance at normal incidence for dielectric surfaces. This replaces an explicit index of refraction                                                                                                                                                           |
| **Emissive**          | Additional diffuse albedo to simulate emissive surfaces (such as neons, etc.) This parameter is mostly useful in an HDR pipeline with a bloom pass                                                                                                                       |
| **Ambient occlusion** | Defines how much of the ambient light is accessible to a surface point. It is a per-pixel shadowing factor between 0.0 and 1.0.<br />This parameter will be discussed in more details in the[lighting](https://google.github.io/filament/Filament.html#lighting) section |


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

加载图片，把颜色传入 shader 时要把 sRGB 图片的颜色从 Gama 空间转到 linear 空间。压暗颜色。

#### Remapping

remap the parameters  *baseColor* , *roughness* and  *reflectance* .

##### Base color remapping

```
vec3 diffuseColor = (1.0 - metallic) * baseColor.rgb;
```

##### Reflectance remapping

将 Reflectance 映射为 f0

可通过折射率（index of refraction，IOR）计算反射率 ：

###### **Dielectrics**

绝缘体的反射率都很低，不同的绝缘体反射率不一样，需要查表对应上

vec3 f0 = 0.16 * reflectance * reflectance


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

###### **Conductors**

导体的反射率比较一致，相当于其表面的颜色

f0 = baseColor * metallic;

###### 总结

```
vec3 f0 = 0.16 * reflectance * reflectance * (1.0 - metallic) + baseColor * metallic;
```

##### Roughness remapping and clamping

a = pow(perceptualRoughness,2)

扩大反射光斑的范围

### Crafting physically based materials

![](https://google.github.io/filament/images/material_chart.jpg)
