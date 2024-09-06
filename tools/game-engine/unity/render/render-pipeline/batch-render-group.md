## Reference

- [BatchRendererGroup sample: Achieve high frame rate even on budget devices](https://unity.com/cn/blog/engine-platform/batchrenderergroup-sample-high-frame-rate-on-budget-devices)
- [Unity-Technologies/brg-shooter: Unity blog-post sample showing BatchRendererGroup and Burst/JobSystem. Focus is high performance even on budget mobile devices. Unity 2022.3.5 or above required](https://github.com/Unity-Technologies/brg-shooter)

## Concept

BatchRendererGroup (or BRG) is an API that efficiently generates draw commands from C# and produces GPU-instancing draw calls.

### 相对于 DrawMeshInstanced 好处

- DrawMeshInstanced 需要托管内存, 传入 TRS 数组
- DrawMeshInstanced 需要自定义着色器
- DrawMeshInstanced 每次绘制需要上传 GPU 

#### BRG shader data model

- the standard constant buffer (UnityPerMaterial) 
- custom, large GPU buffer (BRG raw buffer)
	- Shader Storage Buffer Object (SSBO, or byte address buffer)
	- The default BRG data layout is the structure of arrays (SoA) type

### Properties per instance – or not

#### BRG metadata

- Bits 0–30 define the offset of the property within the BRG raw buffer, 
- and bit 31 tells whether the property value is the same for all instances or the offset is the beginning of an array, with one value per instance.


#### 两种 Buffer

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101451721.png)

#### BRG culling and visibility indices

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101500933.png)

#### BRG matrix format and buffer

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101533341.png)

#### SoA layout

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101605647.png)

### Animating floor cells

- allocate a similar buffer in system memory (CPU can process data at full speed in system memory).
- Let’s call this second buffer a “shadow copy” of the GPU memory
- When animation is done, we upload the shadow copy buffer to the GPU using the **GraphicsBuffer.SetData** API.

#### defined as follows

1. “unity_ObjectToWorld” property is an array starting at offset 0 of the BRG raw buffer
2. “unity_WorldToObject” property is an array starting at offset 153,600
3. “_BaseColor” property is an array, starting at offset 307,200

### The devil’s in the details: GLES exception

most GLES 3.0 devices can’t access SSBO during the vertex stage (i.e., the GL_MAX_VERTEX_SHADER_STORAGE_BLOCKS value is 0).

在 OpenGL 上用 UBO 替代 SSBO

A constant buffer can be any size, but only a small part of it (a window) is visible at any given time when the shader is running. The window size depends on the hardware and driver, but a widely accepted value is 16 KiB.

BatchRendererGroup.GetConstantBufferMaxWindowSize() API to get the correct BRG window size.

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407101712688.png)

### Uploading data

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202407102046052.png)

### Main BRG user callback

1. To generate all draw commands into the output BatchCullingOut struct
2. To use (or not) information provided in the BatchCullingContext read-only struct within your own culling code

### Draw commands generation

A BRG draw command is almost a GPU DrawInstanced call.