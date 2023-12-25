- [[UnrealCircle]《Apex Legends Mobile》渲染优化实践 | 腾讯光子工作室群 陈玉钢_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1eS4y177Ev/?vd_source=82ce4a669c99782699b427d978554b61)
- [腾讯光子陈玉钢谈《Apex Legends Mobile》渲染优化实践 - GameRes游资网](https://www.gameres.com/896788.html)

## 性能优化挑战


- 还原度
	- 场景复杂
	- Gameplay 复杂
- 高帧率
	- 基础开销
	- 功耗控制
- 适配性
	- 伸缩性
	- 基础消耗

## 场景渲染优化

间接光

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221536103.png)


阴影

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221538475.png)


物件和关卡

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221538639.png)

植被

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221539376.png)

MipBias : Mipmap 生成纹理时, 纹理减小比例的增大系数 [MipBias 知乎](https://zhuanlan.zhihu.com/p/85806587)

[Unity 纹理压缩方案](https://zhuanlan.zhihu.com/p/260761440)

### 静态合批

Hism 层级实例化静态合批
- Hierarchical stattic instance batching
- Drawcall and GPU
- Scalability
- LOD distance scale

为了防止 Batch 被不可见物体打断,
再加一个动态合批

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221628146.png)

### 动态合批

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221631149.png)


UE 的 GpuScene 需要依赖 Compute Shader, 顶点着色器采样 TextureBuffer

不使用 GpuScene, 更改使用 Uniform buffer 来存储 Primitive

Uniform 超过 2K 有兼容性的问题.

### 剔除

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221636049.png)

#### 遮挡剔除

软光栅剔除

#### 视锥剔除

基于格子的预缓存, 和分区

### 基于三角形簇的剔除管线

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221640167.png)

1. 基于空间划分三角形簇
2. 剔除三角形簇并减少三角形提交次数
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221643609.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221646887.png)


#### 场景渲染优化小结

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221647189.png)

## RenderPass 优化

UE 的渲染流水

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221654997.png)

自定义的 HDR 渲染管线

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221654205.png)

Tonemapping 比较

使用拟合 Filmic 的 aces, 好处是通过公式计算得到,不需要多一个 LUT Pass, 还能使用 Filmic 的参数

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221654121.png)

自动曝光

分帧处理

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221659016.png)

小结

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221659866.png)

Lite HDR

在画 pass 到 backbuffer 的同时, 在 Subpass 画一堆画面的平铺点的到 Hdr Storage 的 Texture, 来计算总体屏幕亮度. 进行 ToneMapping 和自动曝光

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221707798.png)

用 Subpass 另外画 depth 到低像素纹理, 去渲染低精度特效

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202311221745294.png)
