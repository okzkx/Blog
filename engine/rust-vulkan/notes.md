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
* imageview
  * image -> swapchain khr images
* render pass
  * color attachment
    * desc
    * ref
  * subpass
    * desc
      * -> color attachment ref -> render pass create info -> color attachment
    * deps
* Frame Buffer
  * render pass -> render pass
  * attachments -> Image view
* Graphic pipeline
  * shader stages
    * shader modules
  * -> Render pass
  * descripter set layout -> pipeline layout
* Command Buffer
  * begin render pass -> render passs
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
