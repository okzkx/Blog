## Reference

- [BatchRendererGroup sample: Achieve high frame rate even on budget devices](https://unity.com/cn/blog/engine-platform/batchrenderergroup-sample-high-frame-rate-on-budget-devices)
- [Unity-Technologies/brg-shooter: Unity blog-post sample showing BatchRendererGroup and Burst/JobSystem. Focus is high performance even on budget mobile devices. Unity 2022.3.5 or above required](https://github.com/Unity-Technologies/brg-shooter)

## Concept

BatchRendererGroup (or BRG) is an API that efficiently generates draw commands from C# and produces GPU-instancing draw calls.

#### BRG shader data model

- The shader variant can fetch data from the standard constant buffer (UnityPerMaterial) or from a custom, large GPU buffer (BRG raw buffer).
- It’s up to you to manage how you store your data in the raw buffer, which is a Shader Storage Buffer Object (SSBO, or byte address buffer). The default BRG data layout is the structure of arrays (SoA) type.

#### BRG metadata

- Bits 0–30 define the offset of the property within the BRG raw buffer, 
- and bit 31 tells whether the property value is the same for all instances or the offset is the beginning of an array, with one value per instance.

#### 两种 Buffer

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101451721.png)

#### BRG culling and visibility indices

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101500933.png)

#### BRG matrix format and buffer

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101533341.png)

SoA layout

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101605647.png)

### Animating floor cells

- allocate a similar buffer in system memory (CPU can process data at full speed in system memory).
- Let’s call this second buffer a “shadow copy” of the GPU memory
- When animation is done, we upload the shadow copy buffer to the GPU using the **GraphicsBuffer.SetData** API.

defined as follows

1. “unity_ObjectToWorld” property is an array starting at offset 0 of the BRG raw buffer
2. “unity_WorldToObject” property is an array starting at offset 153,600
3. “_BaseColor” property is an array, starting at offset 307,200

### The devil’s in the details: GLES exception

he problem is that most GLES 3.0 devices can’t access SSBO during the vertex stage (i.e., the GL_MAX_VERTEX_SHADER_STORAGE_BLOCKS value is 0).

在 OpenGL 上用 UBO 替代 SSBO

A constant buffer can be any size, but only a small part of it (a window) is visible at any given time when the shader is running. The window size depends on the hardware and driver, but a widely accepted value is 16 KiB.

BatchRendererGroup.GetConstantBufferMaxWindowSize() API to get the correct BRG window size.

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101712688.png)



#### Buffer

Uniform Buffer

- Vulkan : BufferUsageFlags : UNIFORM BUFFER 
- Dx : Constant Buffer
- OpenGL : Uniform Buffer Object
- HLSL : cbuffer _ : register(b##bind, space##set){type param;};
- GLSL : layout (set = 3, binding = 2) uniform in_lights { Light light;};
- 一般 Alignment 16 bytes, 大小限制为 16 KiB
- Unity : Shader Properties,  CBUFFER

Shader Storage Buffer Objects (SSBOs)

- 现代图形 api 上才有, GLES 3.1 以上
- GLSL : layout(std430, binding = 2) buffer MyBuffer
- HLSL : RW/StructuredBuffer\<MyStructType\> myBuffer : register(t2);
- Vulkan : STORAGE_BUFFER

Push Constants 
- 只有 Vulkan 才有, 每次提交时要设置

### Uploading data

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407102046052.png)

### Main BRG user callback

1. To generate all draw commands into the output BatchCullingOut struct
2. To use (or not) information provided in the BatchCullingContext read-only struct within your own culling code

### Draw commands generation

A BRG draw command is almost a GPU DrawInstanced call.