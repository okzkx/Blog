# 渲染器流程

1. SimpleLogger Init
2. Set up window
   1. Create event loop
   2. Build window
3. Create VulkanContext
   1. Create vulkan instance
      1. Ash_window::enumerate_required_extensions
      2. Create Vulkan Instance
      3. DebugUtilMessenger
   2. Create vulkan surface
   3. Create vulkan physical device and queue families indices
   4. Create vulkan device and graphics, present queues
   5. Create command buffer pool
4. Allocate command buffer
5. Create swapchain
   1. Create vulkan swapchain
      1. Swapchain format
      2. Swapchain present mode
      3. Swapchain extent (width , height)
      4. Swapchian image count
      5. Create swapchain
      6. Create swapchain image views
   2. Create vulkan render pass
      1. Attachment descriptions
      2. Color attachment reference
      3. Subpass descriptions
      4. Subpass dependency
      5. Create render pass
   3. Create vulkan frame buffers
6. Semaphore for presentation
   1. Image available semaphore
   2. render finished semaphore
7. Create fence
8. Setup Imgui
   1. Create Imgui
   2. WinitPlatform init imgui
   3. Imgui add fonts
   4. Platform attach window
9. Allocate renderer
10. Main loop
    1. platform handle event
    2. new frame event
    3. dirty swapchain
    4. UI
       1. generate ui
       2. draw ui
       3. create ui draw data
    5. device wait for preframe fence
    6. swap chain acquire next image
    7. record ui draw data to command buffer
       1. begin render pass with frame buffer, render area, clear value
       2. insert draw data
       3. end render pass
    8. vulkan_context submit command_buffers
    9. get present_result
11. LoopDestroyed
    1. destroy any thing
