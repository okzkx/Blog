# Resources & Descriptors

- 课件 ： [ECG2021_VK03_ResourcesAndDescriptors](https://image-1253155090.cos.ap-nanjing.myqcloud.com/ECG2021_VK03_ResourcesAndDescriptors.pdf)
- 课程 ：[Resources & Descriptors | "Use Buffers and Images in Vulkan Shaders" | Vulkan Lecture Series, Ep. 3](https://www.youtube.com/watch?v=5VBVWCg7riQ&list=PLmIqTlJ6KsE1Jx5HV4sd2jOe3V1KMHHgn&index=3)

## Different usage types of buffers 

- As uniform buffer : read-only buffers
- As storage buffer : load and store buffers (also support atomic operations)
- As texel buffer :  provide access to float[N] as vec3[N/3] through view
  - Uniform texel buffer : formatted load operations on uniform buffers
  - Storage texel buffer : ...
- As dynamic buffer : additional offset into VkMemory, changeable at run-time with little overhead
  - Dynamic uniform buffer 
  - Dynamic storage buffer 
- (Inline uniform block) : 直接包含数据而不指向 vkMemory

## Different usage types of images

### storage image 

- dicriptor type : VK_DESCRIPTOR_TYPE_STORAGE_IMAGE
- load and store image (also support atomic operations)
- Marked pixel at coordinates: (2, 2)

### sampled image 

- VK_DESCRIPTOR_TYPE_SAMPLED_IMAGE
- sampled load operations from image
- Normalized range: [0, 1] for x and y
- 插值得到取值结果

### input attachment

- VK_DESCRIPTOR_TYPE_INPUT_ATTACHMENT
- Load-only
- Within renderpass
- Framebuffer-local, meaning: Access to one single coordinate only, No access to other coordinates in that image

### Descriptor Types

![image-20230127235616789](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272356852.png)

### Descriptor Types and Usage in GLSL

#### SAMPLER + SAMPLED_IMAGE

```
layout (set = 0, binding = 0) uniform sampler s;
layout (set = 0, binding = 1) uniform texture2D sampledImage;
// ...
vec4 rgba = texture(sampler2D(sampledImage, s), vec2(0.5, 0.5));
```

#### COMBINED_IMAGE_SAMPLER

```
layout (set = 1, binding = 0) uniform sampler2D combinedImageSampler;
// ...
vec4 rgba = texture(combinedImageSampler, vec2(0.5, 0.5));
```

#### STORAGE_IMAGE

```
layout (set = 2, binding = 0, rgba8) uniform image2D storageImage;
// ...
vec4 rgba = imageLoad(storageImage, ivec2(2, 2));
imageStore(storageImage, ivec2(2, 2), vec4(0.299, 0.587, 0.114, 1.0));
```

#### UNIFORM_TEXEL_BUFFER

```
layout (set = 3, binding = 0) uniform samplerBuffer uniformTexelBuffer;
// ...
int index = 0;
vec4 formattedValue = texelFetch(uniformTexelBuffer, index);
```

#### STORAGE_TEXEL_BUFFER

```
layout (set = 4, binding = 0, rgba32f) uniform imageBuffer storageTexelBuffer;
// ...
int index = 0;
vec4 formattedValue = imageLoad(storageTexelBuffer, index);
imageStore(storageTexelBuffer, index, vec4(1.0, 2.0, 3.0, 4.0));
```

#### UNIFORM_BUFFER | UNIFORM_BUFFER_DYNAMIC | INLINE_UNIFORM_BLOCK_EXT

```
layout (set = 5, binding = 0) uniform UniformBuffer
{
	mat4 projection;
	mat4 view;
	mat4 model;
} uniformBuffer;
// ...
mat4 M = uniformBuffer.projection * uniformBuffer.view * uniformBuffer.model;
```

#### STORAGE_BUFFER | STORAGE_BUFFER_DYNAMIC

```
struct Particle {
	vec4 position;
	vec4 velocity;
};
layout (set = 5, binding = 1) buffer StorageBuffer {
	Particle particles[];
} storageBuffer;
// ...
int i = 0;
vec3 p = storageBuffer.particles[i].position.xyz;
```

#### INPUT_ATTACHMENT

```
layout (input_attachment_index = 0, set = 6, binding = 0) uniform subpassInput depthImage;
// ...
float depthAtCurrentFramebufferLocation = subpassLoad(depthImage).r;
```

#### ACCELERATION_STRUCTURE_KHR

```
#extension GL_EXT_ray_tracing : require
#extension GL_EXT_ray_query : require
layout(set = 7, binding = 0) uniform accelerationStructureEXT topLevelAS;
// ...
vec3 orig = vec3(0.0, 0.0, 0.0);
vec3 dir = vec3(0.0, 0.0, 1.0);
float tMin = 0.01;
float tMax = 1000.0;
traceRayEXT(topLevelAS, gl_RayFlagsNoneEXT, 0xFF, 0, 0, 0, origin, tMin, dir, tMax, 0);
// ...
rayQueryEXT rayQuery;
rayQueryInitializeEXT(rayQuery, topLevelAS, gl_RayFlagsNoneEXT, 0xFF, origin, tMin, dir, tMax);
```

