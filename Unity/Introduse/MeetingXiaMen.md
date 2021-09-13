---
categories: Unity
---



# Unity 开放日 - 厦门分享总结

## Road Map:Unity 接下来的开发方向展望

- 动态场景实时 GI（Global illumination 全局光照）：包括光线二次弹射方案，实时光线追踪方案。
- 开放世界解决方案：GI，体素阴影，indirect draw 大量加速
- 虚拟纹理（Virtual Texture）：将原本一张高精度文理分割成为多个小块，每次内存只保留其中一个小块，streaming 加载和卸载。
- 程序化开放世界地形生成工具
- UIToolKit： 类似网页的 ui 系统
- URP： SSAO（屏幕空间环境光遮蔽），light cookie，decal（贴花），point light shadow（点光源阴影，非常耗费性能）
- 2d Demo：官方资源商店有个免费的 2D Demo，用到了各种最新的 2D 工具
- 可视化编程工具
- Savemode
- 多语言 Package：多语言工具包

## Unity 新的渲染技术

### Forward+

- Forward+ 是对传统 Forward Renderer Pipline 在多光源上的一个优化，提前将场景中的物体分组，判断每组受到哪些光照影响，之后每个物体就不必对所有的光源运算。
- 屏幕分为多个小方格，小方格内的所有物体作为一组，判断这一组受到每个光的影响。这个步骤使用 Compute Shader 计算，计算后的结果存于纹理中，需要写回内存。
- 苹果手机的 Metal 图形 API 可以将 Compute Shader 的结果不需要写回内存，保留于 GPU 中，一个 pass 就可以预处理完成。
- Unity 将支持 shader 中的特别写法（hacking hlsl ）使用苹果 metal api 功能，

## 使用 SRP 定制渲染管线

总的目的就是用各种手段优化 URP 流程，使其更加适用于移动端

- 简化渲染管线流程，使其更加轻量。减少 CPU 与 GPU 数据传输次数和带宽消耗。
- 修改一些 ShaderLibrary 中的渲染公式

- 优化后处理效果，减少采样次数。

## 卡通渲染常用技术

- 传统光照结果进行离散化，但不能用直接 pbr 结果离散化
- Rampmap：经验模型计算出光照结果后，通过查表得到最终结果
- MatCap ：提前烘焙光照结果结果到纹理上，通过改变纹理参数来改变光照着色效果，来近似 PBR。
- SSSTexture（次表面散射模拟贴图）： 使用一张单独的贴图来叠加人体此表面散射效果
- pbr + Toon：使用 mask 区分两个部分。比如衣服部分使用 PBR 着色，人体部分使用卡通着色
- 卡通描边：基于 Mesh 几何的，使用工具提前对 Mesh 进行法线平滑，法线外扩后的描边比较细腻。
- 场景描边：场景深度描边，法线描边，或图像只取高频率信息，和 SSAO 算法、效果类似
- 头发：着色使用低频高频高光叠加，产生漫画质感，动态头发使用 dynamic bone
- 后处理：卡通渲染后期特殊效果比真实的面数重要，要注重后处理

