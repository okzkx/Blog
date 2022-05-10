# rust for vulkan

## Vulkan 

[vulkan tutorial](https://vulkan-tutorial.com)

## API sum up

- Vulkano 和 Vulkanalia 是两个比较火的 rust for vulkan api，两者的区别大意是
- vulkanalia 更偏底层，Vulkan 调用的 Api 操作基本和 C++ 的操作一致。文档也几倍呢一致
  - `vulkanalia provides raw bindings to the Vulkan API as well as a thin wrapper over said bindings to make them easier and more idiomatic to use from Rust. `
- Vulkano 封装很多的原子操作，使其更安全更容易使用，但失去了灵活性。
  - `Uses a crate which provides a safe and relatively concise wrapper around the Vulkan API`

## API Learn

当前想法：
- 主要是学习 Vulkanalia，Vulkano 作为附加的参考
- 实际开发用哪个不一定

## resp

- [vulkano](https://github.com/vulkano-rs/vulkano) star 3.1k [tutorial](https://github.com/bwasty/vulkan-tutorial-rs)
- [ash](https://github.com/ash-rs/ash) star 1k [tutorial](https://github.com/unknownue/vulkan-tutorial-rust)
- [vulkanalia](https://github.com/KyleMayes/vulkanalia) star 27 [tutorial](https://kylemayes.github.io/vulkanalia/introduction.html)
- [rust-gpu](https://github.com/EmbarkStudios/rust-gpu)
- [kajiya](https://github.com/embarkstudios/kajiya/)

## Steps

1. A lot of steps
2. Draw first Vulkan powered triangle on screen
3. linear transformations
4. textures
5. 3D models
