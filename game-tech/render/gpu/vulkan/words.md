# Vulkan 术语

* color_attachment
  * desc
  * ref
* swap chain
  * swapchain helper
    * -> instance , device
  * swapchain khr
    * images
* image view
  * image -> swapchain khr images
* render pass
  * attachments
    * 1 -> color_attachment -> subpass attachment(0)
    * 2 -> depth_attachment -> subpass attachment(1)
  * subpass
    * -> color_attachments -> color_attachment_references -> attachment(0)
    * -> depth_stencil_attachment -> depth_stencil_attachment_references -> attachment(1)
  * dependencies (Subpass Dependency))
* Frame Buffer
  * render pass -> render pass
  * attachments -> [color image view , depth image view]
* Graphic pipeline
  * shader stages
    * shader modules
  * -> Render pass
  * descripter set layout -> pipeline layout
* Command Buffer
  * begin render pass
    * -> render passs
    * -> frame buffer
  * bind pipeline -> graphic pipeline
  * vertex buffer
  * index buffer
  * descriptor sets
* descriptor sets (per image)
  * -> descriptor set layout
  * write descriptor set -> uniform buffer
  * pipeline_layout
* uniform buffer
  * uniform buffer memory
    * ubo

Command buffer -> begin render passes -> frame buffer -> image view -> image (从 Swapchain 中获取或自己创建)

Command buffer -> begin render passes -> render passs -> attachments （attachments 需要和 framebuffer 格式一致）

Create Instance

Validation layer

* 从 entry 得到这个机器可以使用的 validation layers
* 检测是否具有需要的 validation layer
* 一般在 Debug build 里会开启 VK_LAYER_KHRONOS_validation 用来检测是否具有 vulkan sdk

Pick phsical device

Queue Family

* 遍历所有的 Queue Family
* 找到具有 graphics 和 present 的 Queue Family Index

Device Extension

* instance 遍历每个 physical_device 具有的所有所有拓展
* 检测每个 physical_device 的拓展是否包含有需要的，当前只有一个 Swapchian.name
* 选择具有所需要的拓展的 physical_device

Create Device

创建 device，需要启用

* Queue Family Index
* 所需要的 layer
* 所需要的 Extension
* 所需要的 feature，现在只有各向异性采样

使用 QueueFamily 创建对应的 Graphics Present WWqueu

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
