# 使用 Rust 写 Vulkan

## Rust 项目的总览

- 游戏引擎： bevy， fyrox， ggez， oxygengine， macroquad， godot-rust， piston， amethyst
- GPU和图形渲染： wgpu， rust-gpu，kajiya， lyon， ash， vulkano， rend3， rafx， gfx， luminance， miniquad， glow

### 精选渲染器

- [bevy](https://github.com/bevyengine/bevy) 开源游戏引擎
- [kajiya](https://github.com/EmbarkStudios/kajiya) 全局光照

### 精选图形 API

- [wgpu](https://github.com/gfx-rs/wgpu) : 可以运行在 Vulkan, Metal, WebGPU 上
- [ash](https://github.com/MaikKlein/ash) : 一个轻量级的 `Vulkan` 绑定。
  - [vulkan-tutorial-rust](https://github.com/unknownue/vulkan-tutorial-rust) : A Rust implementation of the [Vulkan Tutorial](https://vulkan-tutorial.com/) based on [ash crate](https://crates.io/crates/ash).
- [vulkano-rs/vulkano](https://github.com/vulkano-rs/vulkano) : Safe and rich Rust wrapper around the Vulkan API
  - [vulkan-tutorial-rs](https://github.com/bwasty/vulkan-tutorial-rs) : Rust version of [https://github.com/Overv/VulkanTutorial](https://github.com/Overv/VulkanTutorial) using [Vulkano](http://vulkano.rs/).
  - [vulkano-rs/vulkano-examples](https://github.com/vulkano-rs/vulkano-examples) : Examples of using *vulkano*
  - [vulkano_tutorial](https://github.com/taidaesal/vulkano_tutorial) : Tutorial series for learning Vulkan with Vulkano and Rust, [website ](https://taidaesal.github.io/vulkano_tutorial/)
- [vulkanalia](https://github.com/KyleMayes/vulkanalia) : 也是一个轻量级的 `Vulkan` 绑定，但是用的人比 ash 少得多

### API sum up

- Vulkano 和 Vulkanalia 是两个比较火的 rust for vulkan api，两者的区别大意是
- vulkanalia 更偏底层，Vulkan 调用的 Api 操作基本和 C++ 的操作一致。文档也几倍呢一致
  - `vulkanalia provides raw bindings to the Vulkan API as well as a thin wrapper over said bindings to make them easier and more idiomatic to use from Rust. `
- Vulkano 封装很多的原子操作，使其更安全更容易使用，但失去了灵活性。
  - `Uses a crate which provides a safe and relatively concise wrapper around the Vulkan API`
