# Vulkan Game Enggine

## 3. Device Setup & Pipeline cont

1. Initializing vulkan and picking a physical device
2. Setup validation layers to help debug

## 4. FIxed Function Pipeline Stages

#### viewport transform
![](images/viewport-transform.jpg)


#### frontFace

`configInfo.rasterizationInfo.frontFace`

![](images/clockwise.jpg)

#### Depth Buffer explained

![](images/color-depth-duffer.jpg)

#### MSAA

![](images/msaa.jpg)

## 5. Swap Chain

![](images/swap-chain.jpg)

1. We will have multiple framebuffers
2. We can use wapChain.acquireNextImage() to get the next index of our framebuffers

### flow

![](images/flow.jpg)

### Double buffer vs Triple buffer

![](images/double-buffering.jpg)

![](images/triple-buffering.jpg)

![](images/triple-buffering-2.jpg)

![](images/vsync.jpg)


### Swap chain present modes explained

#### fifo

![](images/fifo.jpg)

#### Mailbox

![](images/mailbox.jpg)

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

![](images/render-pass.jpg)

## 5.2 Command Buffer Overview

![](images\command-buffer.png)

1. Record commands buffers once at initialization and then reuse for each frame
2. Record command buffer every frame, just before submitting

![](images/cmb-lifecycle.png)

### Secondary command buffer

![](images/secondary-cmd.png)

## Vertex buffers

![](images/vertex-buffer.jpg)

![](iamges/../images/saperate-vertex-buffer.jpg)

vertex binding descriptions

![](images/vertex-binding-descriptions.jpg)

vertex attribute descriptions

![](images/vertex-attribute-description.jpg)

- float:VK_FORMAT_R32_SFLOAT
- vec2:VK_FORMAT_R32G32_SFLOAT
- vec3:VK_FORMAT_R32G32B32_SFLOAT
- vec4:VK_FORMAT_R32G32B32A32_SFLOAT
- ivec2:VK_FORMAT_R32G32_SINT (signed integer)
- uvec2:VK_FORMAT_R32G32_UINT (unsigned integer)
- double:VK_FORMAT_R32_SFLOAT (double precision float)

![](images/binding-buffer.jpg)

暂时没有那种更好的结论，一般使用交叉的大 vertex buffer 而不是 binding

### Host & device memory

![](images/data-transfer.jpg)