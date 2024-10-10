## UWA DAY 2018 移动游戏的GPU性能优化


手机性能优化

## Draw Call 

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312251442510.png)

Static Batching 生成一次 VertexBuffer
Dynamic Batching 要每帧动态改变 VertexBuffer

## RenderState 切换
避免过多的 Instance Material 生成
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312251446422.png)

GPU Instancing
- Unity 5.5+
- OpenGL ES 3.0+ , 国内设备要 3.1

## Texture2DArray
减少纹理绑定次数

## FillRate 填充率

- 总填充数
- 平均填充倍数 : 总填充数/ 分辨率 需要 <= 3
- 单像素最大填充数

Mask 组件替换为 Rect Mask, 遮罩替换为裁剪

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312251450785.png)

Unity 自带的 Overdraw 是 shader Replacement, 不透明部分也会叠加

网格资源渲染密度分析
- 网格模型在每单位像素中, 网格网格顶点的渲染数量

网格资源屏占比分析
- 平均每帧的渲染像素值

## 带宽 Bandwidth

显存和 GPU 的数据传输

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312251454448.png)
蓝色 : Mesh
绿色 : FrameBuffer
红色 : 纹理

Texture
- Mipmap
	- 减少数据传输量
	- 减少 Cache Miss 概率
- 纹理压缩
	- 减少数据量
	- 合理的纹理格式
	- Dither 算法
	- 精准的分辨率![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312251457450.png)


减少 Mesh 数据传输
- Fast Shadow Receiver 贴花
- Dynamic Batching
- Skinned Mesh
- Particle System



## 场景数据
Vertex 10~20w

Fragment Shader 200w 像素


Shader 计算 (ALU)
- Standard (Simple)
	- Vertex : 39(ins), 21.6 (cyc)
	- Fragment : 37(ins), 13.3(cyc) 建议小于 10
- Standard (Complex)
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312251430011.png)

Standard 全屏, 会造成 50ms 的 gpu 压力

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312251436934.png)

