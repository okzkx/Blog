# 光照纹理

[Lightmap（光照贴图）能实现动态光照吗？](https://www.zhihu.com/question/264405382/answer/280555940)

irradiance map 每个纹素代表采样点接收到的幅照度

irradiance map 的问题在于
1. 有高频的 normal map 时，低频的 irradiance map 不能匹配
2. 欠缺天空光和环境的遮挡关系，缺少真实感。

## Unity light map

[Static Lighting](https://catlikecoding.com/unity/tutorials/rendering/part-16/)

Directional Mode
1. Indirectional : 间接光, 即 irradiance map, 光强度纹理, 与入射光方向, 视线方向无关
2. Directional : Intensity + Directional , radiance map, 光强度 + 光方向

No more direct lighting.

![](https://catlikecoding.com/unity/tutorials/rendering/part-16/lightmapping/no-realtime-light.png)

1. Baked Lights
2. Static Geometry
3. Lightmapping Settings

#### 光照贴图烘培

5. 烘焙主光源
6. 光照模式调成烘焙间接光
7. 需要烘焙的物体调成静态
8. Lightmap 只能提供与视线方向无关入射方向无关的光照信息, 即 irradiance map, 单位面积收到的光能量
9. 自发光项也影响光照贴图烘培
10. 光照贴图也可以支持半透明表面, 但不支持单面网格

#### 光照贴图使用

1. Lightmapped Shader Variants
2. Lightmap Coordinates
3. Sampling the Lightmap

### 创建光照贴图

1. Semitransprant Shadows : 半透明物体烘培支持
2. Cutout Shadows : Cutoff 物体烘培支持
3. Adding a Meta Pass : 弹射光, 自发光颜色支持
4. Rough Metals : 对不光滑材质的支持
5. Directional Lightmaps : Normal mapping 会由于精度问题和入射光方向丢失而消失, 可使用  Directionality mode 修复
6. Light Probes : 可记录静态光照, 给动态物体使用, 密度随着光照变化速度而增大

