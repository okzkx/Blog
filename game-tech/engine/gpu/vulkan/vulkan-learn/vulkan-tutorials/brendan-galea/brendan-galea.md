# Vulkan Game Enggine

[工程地址](https://github.com/blurrypiano/littleVulkanEngine)

## 3. Device Setup & Pipeline cont

1. Initializing vulkan and picking a physical device
2. Setup validation layers to help debug

## 4. FIxed Function Pipeline Stages

#### viewport transform

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954649.jpg)

#### frontFace

`configInfo.rasterizationInfo.frontFace`

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954002.jpg)

#### Depth Buffer explained

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954854.jpg)

#### MSAA

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291957563.jpg)

## 5. Swap Chain

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954139.jpg)

1. We will have multiple framebuffers
2. We can use wapChain.acquireNextImage() to get the next index of our framebuffers

### flow

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954912.jpg)

### Double buffer vs Triple buffer

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954414.jpg)

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954554.jpg)

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954708.jpg)

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291957199.jpg)

### Swap chain present modes explained

#### fifo

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954223.jpg)

#### Mailbox

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291954586.jpg)

#### Present Modes Comparison

##### fifo

- Vsync Bound
- Good for Mobile
- Always Supported

* Latency

##### Mailbox

- Low Latency

* Not always Supproted
* High Power Consumption

##### Immediate

- Low Latency
- Usually Supported

* Tearing
* High Power Consumption

### Render pass

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291955704.jpg)

## 5.2 Command Buffer Overview
![command-buffer](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291956061.png)

1. Record commands buffers once at initialization and then reuse for each frame
2. Record command buffer every frame, just before submitting

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291955074.png)

### Secondary command buffer

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291956701.png)

## Vertex buffers

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291955972.jpg)

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291956760.jpg)
vertex binding descriptions

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291955684.jpg)

vertex attribute descriptions

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291955748.jpg)

- float:VK_FORMAT_R32_SFLOAT
- vec2:VK_FORMAT_R32G32_SFLOAT
- vec3:VK_FORMAT_R32G32B32_SFLOAT
- vec4:VK_FORMAT_R32G32B32A32_SFLOAT
- ivec2:VK_FORMAT_R32G32_SINT (signed integer)
- uvec2:VK_FORMAT_R32G32_UINT (unsigned integer)
- double:VK_FORMAT_R32_SFLOAT (double precision float)

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291955695.jpg)

暂时没有那种更好的结论，一般使用交叉的大 vertex buffer 而不是 binding

### Host & device memory

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202508291955606.jpg)