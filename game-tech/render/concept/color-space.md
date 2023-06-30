# 颜色空间

## Reference

- [Unity - Manual: Working with linear Textures](https://docs.unity3d.com/Manual/LinearRendering-LinearTextures.html)
- [Unity - Manual: Linear or gamma workflow](https://docs.unity3d.com/Manual/LinearRendering-LinearOrGammaWorkflow.html)
- [Gamma、Linear、sRGB 和Unity Color Space，你真懂了吗？ - 知乎](https://zhuanlan.zhihu.com/p/66558476)

## sRGB  和 Gamma 矫正

1. 用 sRGB 类型的纹理存储颜色在 Gamma 0.45 空间，整体提亮图片颜色，存储更多暗部细节
2. 图片加载时数据 pow(2.2) 变暗后存储到线性空间 Gamma 1.0
3. Shader 使用线性数据计算，输出使用 Gamma 矫正 pow(0.45) 到 Gamma 0.45
4. 显示器输出的时候压暗颜色 pow(2.2) Gamma 1.0

### Unity 中的 Gamma 矫正

![gamma](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302221847777.png)

1. 勾选了 sRGB 的纹理在线性工作流中，加载时会自动 pow(2.2)，统一到 Gamma 1.0
2. Shader lab 的输出会自动 pow(0.45) 给显示器压暗

### 效果

- sRGB 纹理不启用 sRGB 将会整体偏亮

  
  
- Unity 一般纹理读取保证内存中的是 linear，sRGB 读取内存中是 Gamma 0.45
- // Unity 统一后处理伽马矫正 linear -> Gamma 0.45，再屏幕输出到人眼后变暗 (linear)
- // return half4(0.5, 0.5, 1, 1); // 直接返回的值 () -> linear，矫正 -> Gamma 0.45
- // return half4(normalSample, 1); // 直接返回 sRGB 纹理颜色 : 从 Gamma 0.45 -> linear , 矫正 -> Gamma 0.45 // 制作 sRGB png 的时候使用 Gamma 0.45 存储  
- // return half4(normalSample, 1);  // 直接返回 rgb 纹理颜色 : 预处理 从 linear -> Gamma 0.45, 从 Gamma 0.45 -> linear, 矫正 -> Gamma 0.45
- // return half4(normalSample, 1); // 直接返回 normalmap 纹理颜色 : 预处理 从 linear -> Gamma 0.45, 从 Gamma 0.45 -> linear, 矫正 -> Gamma 0.45