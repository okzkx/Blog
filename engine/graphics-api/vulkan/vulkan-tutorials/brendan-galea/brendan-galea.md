# Vulkan Game Enggine

[工程地址](https://github.com/blurrypiano/littleVulkanEngine)

## 3. Device Setup & Pipeline cont

1. Initializing vulkan and picking a physical device
2. Setup validation layers to help debug

## 4. FIxed Function Pipeline Stages

#### viewport transform
![](brendan-galea.assets/viewport-transform.jpg)


#### frontFace

`configInfo.rasterizationInfo.frontFace`

![](brendan-galea.assets/clockwise.jpg)

#### Depth Buffer explained

![](brendan-galea.assets/color-depth-duffer.jpg)

#### MSAA

![](brendan-galea.assets/msaa.jpg)

## 5. Swap Chain

![](brendan-galea.assets/swap-chain.jpg)

1. We will have multiple framebuffers
2. We can use wapChain.acquireNextImage() to get the next index of our framebuffers

### flow

![](brendan-galea.assets/flow.jpg)

### Double buffer vs Triple buffer

![](brendan-galea.assets/double-buffering.jpg)

![](brendan-galea.assets/triple-buffering.jpg)

![](brendan-galea.assets/triple-buffering-2.jpg)

![](brendan-galea.assets/vsync.jpg)


### Swap chain present modes explained

#### fifo

![](brendan-galea.assets/fifo.jpg)

#### Mailbox

![](brendan-galea.assets/mailbox.jpg)

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

![](brendan-galea.assets/render-pass.jpg)

## 5.2 Command Buffer Overview

![](brendan-galea.assets\command-buffer.png)

1. Record commands buffers once at initialization and then reuse for each frame
2. Record command buffer every frame, just before submitting

![](brendan-galea.assets/cmb-lifecycle.png)

### Secondary command buffer

![](brendan-galea.assets/secondary-cmd.png)

## Vertex buffers

![](brendan-galea.assets/vertex-buffer.jpg)

![](iamges/../brendan-galea.assets/saperate-vertex-buffer.jpg)

vertex binding descriptions

![](brendan-galea.assets/vertex-binding-descriptions.jpg)

vertex attribute descriptions

![](brendan-galea.assets/vertex-attribute-description.jpg)

- float:VK_FORMAT_R32_SFLOAT
- vec2:VK_FORMAT_R32G32_SFLOAT
- vec3:VK_FORMAT_R32G32B32_SFLOAT
- vec4:VK_FORMAT_R32G32B32A32_SFLOAT
- ivec2:VK_FORMAT_R32G32_SINT (signed integer)
- uvec2:VK_FORMAT_R32G32_UINT (unsigned integer)
- double:VK_FORMAT_R32_SFLOAT (double precision float)

![](brendan-galea.assets/binding-buffer.jpg)

暂时没有那种更好的结论，一般使用交叉的大 vertex buffer 而不是 binding

### Host & device memory

![](brendan-galea.assets/data-transfer.jpg)