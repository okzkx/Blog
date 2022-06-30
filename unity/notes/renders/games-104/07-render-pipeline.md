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

### Fog

- Depth Fog
  - Linear fog
  - Exp fog
  - Exp Squared fog
- Height Fog
  - Ray marching
  - 或者用解析式积分雾的浓度
- Voxel-based Volumetric Fog
  - 基于视锥体的空间划分
  - 然后进行 Ray marching
- Precompute
  - 使用 3D Texture 提前存储计算结果

### Anti-aliasing 反走样

#### 出现原因

- 光栅化是对连续世界的采样，低频采样高频信息
- 常见锯齿场景
  - Edge
  - Texture
  - Specular

#### Anti-aliasing

- 对于一个像素进行多次采样，让后颜色平均
- 使颜色过度更加平滑

#### Methods

- 基于超采样的抗锯齿
  - SSAA : Super-sample AA : 四倍像素数量着色
  - MSAA : Multi-sample AA：每个像素对其中有贡献的三角形采样一次，
    - 缺陷是场景中的三角形会特别多
- 基于图像的抗锯齿
  - FXAA : Fast Approximate Anti-aliasing 最常用的抗锯齿算法
    - 通过亮度 Luma 和色差定义 Contrast 边缘检测，
    - 对于每个像素找到周围 9 格中梯度最大的像素方向
    - 再沿梯度垂直方向找到边缘的端点
    - 以整条边进行模糊，一半部分加亮，一半部分变暗
- 基于时序的坑锯齿
  - TAA
    - 基于物体的 Motion Vector，物体中的像素点在屏幕上的运动距离
    - 过去的渲染结果会在当前帧结果提供大量贡献

#### Post-process 大滤镜

- explosure 场景正确的被曝光
- Bloom
- Tone Mapping
- Color Grading
