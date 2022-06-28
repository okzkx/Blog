# [Comparison of graphics apis](https://alain.xyz/blog/comparison-of-modern-graphics-apis)

## Import

- Importing Dependencies : When starting a new application you need to include any dependencies you have to external APIs, and graphics APIs are no different. Depending on the API, you may also need other libraries in your project such as a shader compiler
- Shader Compiler

## Initialize API

- **Entry Point** : The entry point to a graphics API generally allows you to access the API's inner classes.
- **Physical Devices** : allow you to query for important device specific details such as memory size and feature support.
- **Logical Device** : A Device gives you access to the core inner functions of the API, such as creating graphics data structures like textures, buffers, queues, pipelines, etc. This type of data structure is the same for the most part across all modern graphics APIs with very few changes between them.
- **Queue** : allows you to enqueue tasks for the GPU to execute. A GPU is an asynchronous compute device, so the idea here is to always keep it busy while having control over when items are added to the queue.
- **Command Pool** : is a data structure that allows you to create command buffers.

## Frame Backings

- **window Surface** : allows you to bind all draw calls to an OS specific window.
- **Swapchain flips** : between different back buffers for a given window, and controls aspects of rendering such as refresh rate and back buffer swapping behavior.
- **Frame Buffers** : Are groups of output textures used during a raster based graphics pipeline execution as outputs.
- **Textures** : are arrays of data that store color information, and serve as inputs/outputs for rendering. Vulkan, DirectX 12, and WebGPU introduce the idea of having multiple views of a given texture that can view that texture in different encoded formats or color spaces. Vulkan introduces the idea of managed memory for Images and buffers, thus a texture is a triplet of an Image, Image View when used (there can be multiple of these), and Memory in either device only or in CPU-GPU accessible space.
- **Buffer** : is an array of data, such as a mesh's positional data, color data, index data, etc. Similar rules for images apply to buffers in Vulkan and WebGPU.
- **Shader** : tends to be a handle to a compiled blob of shader (HLSL, GLSL, MSL, etc.) code to be fed to a given Pipeline.
- **Shader Binding** Most modern graphics APIs feature a binding data structure to help connect uniform buffers and textures to graphics pipelines that need that data. Metal is unique in that you can bind uniforms with setVertexBuffer in a command encoder, making it much easier to architect compared to Vulkan, DirectX 12, and WebGPU.
- **Pipelines** are an overarching description of what will be executed when performing a raster draw call, compute dispatch, or ray tracing dispatch.
- **Command Buffer** : is an asynchronous computing unit, where you describe procedures for the GPU to execute, such as draw calls, copying data from CPU-GPU accessible memory to GPU exclusive memory, and set various aspects of the graphics pipeline dynamically such as the current scissor.
- **Command Lists** : are groups of command buffers pushed in batches to the GPU. The reason for doing this is to keep the GPU constantly busy, leading to less de-synchronization between the CPU and GPU
- **Fences** : are objects used to synchronize the CPU and GPU. Both the CPU and GPU can be instructed to wait at a fence so that the other can catch up. This can be used to manage resource allocation and deallocation, making it easier to manage overall graphics memory usage. [Satran et al. 2018]
- **Barriers** : A more granular form of synchronization, inside command buffers.
- **Semaphores** : are objects used introduce dependencies between operations, such as waiting before acquiring the next image in the swapchain before submitting command buffers to your device queue.

## Spaces, Alignments

Each graphics API can have different defaults for axis direction, NDC coordinate direction, matrix alignment, texture alignment, and more, for the most part this isn't much of an issue, and you'll just have to flip a y value in your UVs in your fragment shader.
