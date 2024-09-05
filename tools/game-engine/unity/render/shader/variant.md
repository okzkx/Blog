shader 变体与宏定义

## Shader 变体

- [Unity Shader 变体优化与故障排除技巧](https://mp.weixin.qq.com/s/0l6SkXNwuoRzFt9Xg0ZV4A)
- [Shader变体收集与打包 - 知乎](https://zhuanlan.zhihu.com/p/68888831)

## Shader 多重编译

有多重编译的 Shader 被称为 uber shader

- [unity shader 变种（多重编译 multi_compile）](https://www.jianshu.com/p/8750704a2f4c)

#### 全局宏定义

- #pragma multi_compile 打包所有变体
- #pragma shader_feature 打包会剔除未被材质使用的着色器变体。

#### 本地宏定义

- #pragma multi_compile_local _ LIGHTMAP_SHADOW_MIXING
- #pragma shader_feature_local _RECEIVE_SHADOWS_OFF

不能使用 Shader.EnableKeyword 全局开启定义, 而是通过材质面板或者代码 EnableKeyword

#### 指定宏定义阶段

- #pragma shader_feature_local_fragment _LASER_SCAN

指定只有 fragment 阶段才开启宏定义


Shader Warm up

https://docs.unity3d.com/ScriptReference/ShaderVariantCollection.WarmUp.html

https://docs.unity3d.com/Manual/shader-loading.html

[Shader compilation](https://docs.unity3d.com/Manual/shader-compilation.html)

[Shader variant stripping](https://docs.unity3d.com/Manual/shader-variant-stripping.html)