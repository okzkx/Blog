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

https://vulkan-tutorial.com/Drawing_a_triangle/Drawing/Rendering_and_presentation#page_Semaphores

- Semaphores :  is used to add order between queue operations. 
- Fences : A fence has a similar purpose, in that it is used to synchronize execution, but it is for ordering the execution on the CPU, otherwise known as the host. Simply put, if the host needs to know when the GPU has finished something, we use a fence.


VkSubmitInfo

The vertex buffer we have right now works correctly, but the memory type that allows us to access it from the CPU may not be the most optimal memory type for the graphics card itself to read from. The most optimal memory has the VK_MEMORY_PROPERTY_DEVICE_LOCAL_BIT flag and is usually not accessible by the CPU on dedicated graphics cards. In this chapter we're going to create two vertex buffers. One staging buffer in CPU accessible memory to upload the data from the vertex array to, and the final vertex buffer in device local memory. We'll then use a buffer copy command to move the data from the staging buffer to the actual vertex buffer.https://vulkan-tutorial.com/Vertex_buffers/Staging_buffer

The right way to tackle this in Vulkan is to use resource descriptors. A descriptor is a way for shaders to freely access resources like buffers and images.https://vulkan-tutorial.com/Uniform_buffers/Descriptor_layout_and_buffer

descriptor，attachments 是接口，描述了可以访问的方法

- Create an image object backed by device memory
- Fill it with pixels from an image file
- Create an image sampler
- Add a combined image sampler descriptor to sample colors from the texture
- https://vulkan-tutorial.com/Texture_mapping/Images#page_Introduction

 We'll start by creating a staging resource and filling it with pixel data and then we copy this to the final image object that we'll use for rendering.

Creating an image is not very different from creating buffers. It involves querying the memory requirements, allocating device memory and binding it, just like we've seen before.

One of the most common ways to perform layout transitions is using an image memory barrier. 

Barriers are primarily used for synchronization purposes, so you must specify which types of operations that involve the resource must happen before the barrier, and which operations that involve the resource must wait on the barrier. 

#### Image View
with the swap chain images and the framebuffer, that images are accessed through image views rather than directly. https://vulkan-tutorial.com/Texture_mapping/Image_view_and_sampler

#### Samplers
 Textures are usually accessed through samplers, which will apply filtering and transformations to compute the final color that is retrieved.
- filter
- addressing mode
  - Repeat
  - Mirrored repeat
  - Clamp to edge
  - Clamp to border 

new type of descriptor: combined image sampler. This descriptor makes it possible for shaders to access an image resource through a sampler object like the one we created in the previous chapter.https://vulkan-tutorial.com/Texture_mapping/Combined_image_sampler