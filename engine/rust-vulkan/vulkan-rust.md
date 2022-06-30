# 使用 Rust 写 Vulkan

## Rust 项目的总览

- 游戏引擎： bevy， fyrox， ggez， oxygengine， macroquad， godot-rust， piston， amethyst
- GPU和图形渲染： wgpu， rust-gpu，kajiya， lyon， ash， vulkano， rend3， rafx， gfx， luminance， miniquad， glow

### 精选渲染器

- [bevy](https://github.com/bevyengine/bevy) 开源游戏引擎
- [kajiya](https://github.com/EmbarkStudios/kajiya) 全局光照

### 精选图形 API

- [wgpu](https://github.com/gfx-rs/wgpu) : 可以运行在 Vulkan, Metal, WebGPU 上
- [ash](https://github.com/MaikKlein/ash) : 一个轻量级的 `Vulkan` 绑定

  - [vulkan-tutorial-rust](https://github.com/unknownue/vulkan-tutorial-rust) : A Rust implementation of the [Vulkan Tutorial](https://vulkan-tutorial.com/) based on [ash crate](https://crates.io/crates/ash).
  - [ash-nv-rt](https://github.com/gwihlidal/ash-nv-rt) : NV ray tracing with rust and ash!
  - [ash-sample-progression](https://github.com/bzm3r/ash-sample-progression) : A set of Rust+Ash tutorials provided by [LunarG vulkan tutorial](https://vulkan.lunarg.com/doc/sdk/1.0.26.0/linux/tutorial.html)
- [vulkano-rs/vulkano](https://github.com/vulkano-rs/vulkano) : Safe and rich Rust wrapper around the Vulkan API

  - [vulkan-tutorial-rs](https://github.com/bwasty/vulkan-tutorial-rs) : Rust version of [https://github.com/Overv/VulkanTutorial](https://github.com/Overv/VulkanTutorial) using [Vulkano](http://vulkano.rs/).
  - [vulkano-rs/vulkano-examples](https://github.com/vulkano-rs/vulkano-examples) : Examples of using *vulkano*
  - [taidaesal/vulkano_tutorial](https://github.com/taidaesal/vulkano_tutorial) : Tutorial series for learning Vulkan with Vulkano and Rust, [website ](https://taidaesal.github.io/vulkano_tutorial/)
  - **[vulkan-tutorial-rust](https://github.com/unknownue/vulkan-tutorial-rust)** : A Rust implementation of the [Vulkan Tutorial](https://vulkan-tutorial.com/) based on [ash crate](https://crates.io/crates/ash).
- [vulkanalia](https://github.com/KyleMayes/vulkanalia) : 也是一个轻量级的 `Vulkan` 绑定，但是用的人比 ash 少得多
