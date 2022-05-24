# Vulkan

## Benefit

Validation layers are optional components that hook into Vulkan function calls to apply additional operations.https://vulkan-tutorial.com/Drawing_a_triangle/Setup/Validation_layers

Since Vulkan is a platform agnostic API, it can not interface directly with the window system on its own.https://vulkan-tutorial.com/Drawing_a_triangle/Presentation/Window_surface

Vulkan does not have the concept of a "default framebuffer", hence it requires an infrastructure that will own the buffers we will render to before we visualize them on the screen. This infrastructure is known as the swap chain and must be created explicitly in Vulkan. https://vulkan-tutorial.com/Drawing_a_triangle/Presentation/Swap_chain

 shader code in Vulkan has to be specified in a bytecode format as opposed to human-readable syntax like GLSL and HLSL.
 https://vulkan-tutorial.com/Drawing_a_triangle/Graphics_pipeline_basics/Shader_modules


The older graphics APIs provided default state for most of the stages of the graphics pipeline. In Vulkan you have to be explicit about everything, from viewport size to color blending function.
https://vulkan-tutorial.com/Drawing_a_triangle/Graphics_pipeline_basics/Fixed_functions

You have to record all of the operations you want to perform in command buffer objects. The advantage of this is that when we are ready to tell the Vulkan what we want to do, all of the commands are submitted together and Vulkan can more efficiently process the commands since all of them are available together. In addition, this allows command recording to happen in multiple threads if so desired.https://vulkan-tutorial.com/Drawing_a_triangle/Drawing/Command_buffers

Outline of a frame
At a high level, rendering a frame in Vulkan consists of a common set of steps:

Outline of a frame
At a high level, rendering a frame in Vulkan consists of a common set of steps:
- Wait for the previous frame to finish
- Acquire an image from the swap chain
- Record a command buffer which draws the scene onto that image
- Submit the recorded command buffer
- Present the swap chain image

Draw Frame

Semaphores
Fences
VkSubmitInfo

The vertex buffer we have right now works correctly, but the memory type that allows us to access it from the CPU may not be the most optimal memory type for the graphics card itself to read from. The most optimal memory has the VK_MEMORY_PROPERTY_DEVICE_LOCAL_BIT flag and is usually not accessible by the CPU on dedicated graphics cards. In this chapter we're going to create two vertex buffers. One staging buffer in CPU accessible memory to upload the data from the vertex array to, and the final vertex buffer in device local memory. We'll then use a buffer copy command to move the data from the staging buffer to the actual vertex buffer.https://vulkan-tutorial.com/Vertex_buffers/Staging_buffer