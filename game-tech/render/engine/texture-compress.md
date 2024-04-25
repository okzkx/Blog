[移动游戏的性能优化 | 贴图压缩篇](https://mp.weixin.qq.com/s/e-wYvq2uj5ldCtWQAzTZmA)

#### 简介
1. 一张 2048 x 2048 RGBAU8 贴图，存储是16M，64张就有1G
2. JPEG、PNG，它们有很好的压缩率和压缩品质，但是必需解压整张贴图，才能读取贴图上的像素值， 不支持**随机读取**
3. GPU 渲染时只需要贴图上的局部块

#### 要求
GPU硬件层面支持贴图压缩算法必需具备如下4个基础要求
_● 支持随机读取_
_● 硬件解压效率高_
_● 较高的压缩比_
_● 压缩品质还行_

压缩码率的单位，1 bpp = 1 bit per pixel，表示一个像素的存储单位，4bpp就表示一个像素按照4bit存储。


移动平台支持的有损压缩格式有
_● PowerVR Texture Compression，PVRTC系列_
_● Ericsson Texture Compression，ETC系列_
_● Adaptive Scalable Texture Compression，ASTC_

目前最好是选择 ASTC 算法
![[ASTC_User_Guide_102162_0001_01.pdf]]


Compression

![](https://mmbiz.qpic.cn/mmbiz_png/Iy9bELzlibJlaIRicSeFecsdHGHXlTdxY8j9HS9gpVdziata8YQpicibczeTAM1FpMdrzq1pJfn4kj1ZnBYbhYjpTkA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- astc以128bit作为一个基础块进行压缩，可以自由选择合适的块大小
- 128 bit = 16 bytes = 4 x 4 bytes = 4 个 u8 颜色, 一个2 x 2 像素块
- 块越小，码率越大，贴图品质越高, 2x2 就是原版品质, 随着块变大, 
- **只有块的大小影响压缩后贴图的大小，被压缩贴图的内容/格式不影响最终的存储**
- 1024x1024的RGB和RGBA贴图，经过4x4压缩后，存储都是1M。相反，由于RGB和RGBA贴图的信息熵是不一样的，那么在压缩品质上RGB会比RGBA高。

#### 原理

astc给每个纹素的颜色分配一个梯度值，每个压缩块，只需要存储两个端点的梯度值和每个纹素的加权值即可，如下图所示，解压缩的话只要将两个端点与加权值进行插值计算即可。

![](https://mmbiz.qpic.cn/mmbiz_png/Iy9bELzlibJlaIRicSeFecsdHGHXlTdxY8VuG9V1kCulr4eAz8BFXjmmQaatafBvkc3je5iangRJ02cfs66Lu4AsQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

有时候，一个分区（Partition）无法满足要求，就需要多个分区，那么存储的数据就还包括一个索引值。

![](https://mmbiz.qpic.cn/mmbiz_png/Iy9bELzlibJlaIRicSeFecsdHGHXlTdxY8l2RcCvicicd6RPGmCT3q63CJ6PhntBkXkKwoUKCvVQbIRc53voOmy8RA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 最佳实践

**贴图像素的差异性，会影响最终的压缩品质**
如果贴图多噪点，这张RGB贴图的压缩品质会很差
张视觉渐变平缓的贴图，压缩品质会很高，例如，lightmap贴图。

双平面模式
一个像素有两个独立的加权值，通常RGB作为一组加权值，A作为另外一组加权值。
**RGBA贴图A通道的压缩品质是最高的**，在组织贴图的功能通道时，可以将重要度高的贴图数据放到A通道；法线贴图可以存储两个通道，RGB作为X，A作为Y，这种方式的压缩品质最高，也是官方推荐的。

## 编码器

_● intel的ispc，支持mac/Windows，版本是1.7.0；_

_● arm的astcenc，支持linux/mac/windows，版本是1.3。_