
## Uniform Buffer

- Vulkan : BufferUsageFlags : UNIFORM BUFFER 
- Dx : Constant Buffer
- OpenGL : Uniform Buffer Object
- HLSL : cbuffer _ : register(b##bind, space##set){type param;};
- GLSL : layout (set = 3, binding = 2) uniform in_lights { Light light;};
- 一般 Alignment 16 bytes, 大小限制为 16 KiB
- Unity : Shader Properties,  CBUFFER

## Shader Storage Buffer Objects (SSBOs)

- 现代图形 api 上才有, GLES 3.1 以上
- GLSL : layout(std430, binding = 2) buffer MyBuffer
- HLSL : RW/StructuredBuffer\<MyStructType\> myBuffer : register(t2);
- Vulkan : STORAGE_BUFFER

### Push Constants 

- 只有 Vulkan 才有, 每次提交时要设置
