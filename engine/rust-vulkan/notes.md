# 临时笔记

**let** **mut** x 指在栈区的 x 指向的指是可变的

### [What’s the difference between `Copy` and `Clone`?](https://doc.rust-lang.org/core/marker/trait.Copy.html#whats-the-difference-between-copy-and-clone)

Rust 中的引用

1. 解引用使用 * 符号
2. 当 T 实现了 Deref 这个 Trait ，* 符号调用 deref 函数返回的值就是解引用结果
3. &T 是引用了 T 的一个结构体，Box `<T>`  也是一样，* 可用来消除 & 或 Box<>
4. 一般解引用后的最终结果要是单层引用，而不能把所有权给出去，所以自己写 deref 在返回的所有权的变量前要加上 &，类似 &*x，这么写是因为不能直接把自己的变量给出去改变所有权，所以会绕一圈
5. Option< T > 提供了方法直接对 T 进行引用相关操作，as_ref, as_deref
6. &T 任意传入方法都不会影响到内存块所有权变更，保护内存不被随意释放
7. 形参传入方法会自动解引用到对应需要的类型

Vulkan 初始化

* Validation Layer 检测
* Extension 开启，Extension 配置
* 选择物理设备时对检测 Extension 支持

Create Render Pass

## Vulkan words

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

使用 QueueFamily 创建对应的 Graphics Present WWqueue
