# Culling


- [Introduction to Occlusion Culling | by Umbra 3D | Medium](https://medium.com/@Umbra3D/introduction-to-occlusion-culling-3d6cfb195c79)
- [Unity中使用ComputeShader做视锥剔除（View Frustum Culling）](https://zhuanlan.zhihu.com/p/376801370)
### Hierarchical-Z map

- [【Unity】使用Compute Shader实现Hi-z遮挡_剔除_（Occlusion Culling）](https://zhuanlan.zhihu.com/p/396979267)
- [原神草地学习HIZ Culling - 知乎](https://zhuanlan.zhihu.com/p/439540044)
- [Compute Shader 进阶应用:结合Hi-Z 剔除海量草渲染](https://zhuanlan.zhihu.com/p/278793984)
- [MaxwellGeng：Hi-Z GPU Occlusion Culling](https://zhuanlan.zhihu.com/p/47615677)

1. 获取不透明物体深度图, 进行mipmap
2. cpu 在下一帧
	1. 获取这个 mipmap,
	2. 计算物体最近深度
	3. 在 mipmap 对应层级查找是否被遮挡
	4. 得到物体可见性

#### 基于 GPU 的 Culling（Vulkan / GL ES ）

- 使用 gles 3.0 提供的 occlusion query 功能来判断 draw call 的可见性，但不支持 instancing，且需要 cpu 的读回操作
- [Compute based Culling - Vulkan Guide (vkguide.dev)](https://vkguide.dev/docs/gpudriven/compute_culling/)

#### 用 cs 实现遮挡剔除（gles 3.1)

比较小的室内场景，潜在会发生很多遮挡关系的环境
[OpenGL ES SDK for Android: Occlusion Culling with Hierarchical-Z (arm-software.github.io)](https://arm-software.github.io/opengl-es-sdk-for-android/occlusion_culling.html)|

[Deferred shading on mobile - Graphics, Gaming, and VR blog - Arm Community blogs - Arm Community](https://community.arm.com/arm-community-blogs/b/graphics-gaming-and-vr-blog/posts/deferred-shading-on-mobile)

