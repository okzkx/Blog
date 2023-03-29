# Vulkan Game Enggine

[工程地址](https://github.com/blurrypiano/littleVulkanEngine)

## 3. Device Setup & Pipeline cont

1. Initializing vulkan and picking a physical device
2. Setup validation layers to help debug

## 4. FIxed Function Pipeline Stages

#### viewport transform

![](viewport-transform.jpg)

#### frontFace

`configInfo.rasterizationInfo.frontFace`

![](clockwise.jpg)

#### Depth Buffer explained

![](color-depth-duffer.jpg)

#### MSAA

![](msaa.jpg)

## 5. Swap Chain

![](swap-chain.jpg)

1. We will have multiple framebuffers
2. We can use wapChain.acquireNextImage() to get the next index of our framebuffers

### flow

![](flow.jpg)

### Double buffer vs Triple buffer

![](double-buffering.jpg)

![](triple-buffering.jpg)

![](triple-buffering-2.jpg)

![](vsync.jpg)

### Swap chain present modes explained

#### fifo

![](fifo.jpg)

#### Mailbox

![](mailbox.jpg)

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

![](render-pass.jpg)

## 5.2 Command Buffer Overview

![](brendan-galea.assets\command-buffer.png)

1. Record commands buffers once at initialization and then reuse for each frame
2. Record command buffer every frame, just before submitting

![](cmb-lifecycle.png)

### Secondary command buffer

![](secondary-cmd.png)

## Vertex buffers

![](vertex-buffer.jpg)

![](iamges/../brendan-galea.assets/saperate-vertex-buffer.jpg)

vertex binding descriptions

![](vertex-binding-descriptions.jpg)

vertex attribute descriptions

![](vertex-attribute-description.jpg)

- float:VK_FORMAT_R32_SFLOAT
- vec2:VK_FORMAT_R32G32_SFLOAT
- vec3:VK_FORMAT_R32G32B32_SFLOAT
- vec4:VK_FORMAT_R32G32B32A32_SFLOAT
- ivec2:VK_FORMAT_R32G32_SINT (signed integer)
- uvec2:VK_FORMAT_R32G32_UINT (unsigned integer)
- double:VK_FORMAT_R32_SFLOAT (double precision float)

![](binding-buffer.jpg)

暂时没有那种更好的结论，一般使用交叉的大 vertex buffer 而不是 binding

### Host & device memory

![](data-transfer.jpg)