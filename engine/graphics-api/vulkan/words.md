# Vulkan 术语

* Entry
* Instance
* Swapchain
* Framebuffer
* ImageView
* Validation Layer
* Queue, Queue family
* Command pool, Command buffer
* Physical Device

Instance 添加 Validation layer 验证层

Physical Device 设备选择，需要检查，

1. queue families 是否具有 graphic 和 present 两种
2. 程序自定义的拓展字符串，是不是全部都支持
3. 对于 swapchian 的支持：获取设备的 surface capabilities 表面能力，surface format, present mode 都是有值的
