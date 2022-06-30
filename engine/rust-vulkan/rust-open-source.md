# Rust & Vulkan 中间件

## 插件集合

### Low-level 功能组件

#### Vulkan 绑定

* [ash](https://github.com/ash-rs/ash) : Vulkan bindings for Rust
* [Erupt](https://gitlab.com/Friz64/erupt) ：Vulkan API bindings ，Take a look at the [`erupt` user guide](https://gitlab.com/Friz64/erupt/-/blob/main/USER_GUIDE.md).
* [vulkanalia](https://github.com/KyleMayes/vulkanalia) ：Vulkan bindings for Rust. *Heavily inspired by the [`ash`](https://github.com/MaikKlein/ash) crate.*

#### 窗口创建

* [winit ](https://crates.io/crates/winit): Window handling library in pure Rust

#### 数学库

* [cgmath](https://crates.io/crates/cgmath) : A linear algebra and mathematics library for computer graphics.
* [glam ](https://crates.io/crates/glam): A simple and fast 3D math library for games and graphics.

##### GUI

* [imgui](https://crates.io/crates/imgui) : Rust bindings for Dear ImGui

#### 资源加载

* [image ](https://crates.io/crates/image): Encoding and decoding images in Rust
* [png](https://crates.io/crates/png) ：PNG decoder/encoder in pure Rust.
* [tobj](https://crates.io/crates/tobj) : A lightweight OBJ loader in the spirit of tinyobjloader
* [gltf ](https://github.com/gltf-rs/gltf)：glTF 2.0 loader
* [fbx](https://crates.io/crates/fbx) : Autodesk FBX parser for Rust

#### shader 编译

* [shaderc](https://crates.io/crates/shaderc) : Rust bindings for the [shaderc](https://github.com/google/shaderc) library. A collection of tools, libraries and tests for shader compilation.
* [rust-gpu](https://github.com/EmbarkStudios/rust-gpu) ：Making Rust a first-class language and ecosystem for GPU shaders
* [hassle-rs ](https://github.com/Traverse-Research/hassle-rs)：This crate provides an FFI layer and idiomatic rust wrappers for the new [DirectXShaderCompiler](https://github.com/Microsoft/DirectXShaderCompiler) library.

## High-level 渲染 API 封装

* [vulkano](https://github.com/vulkano-rs/vulkano) : Safe and rich Rust wrapper around the Vulkan API

## 渲染工具

* [lyon](https://github.com/nical/lyon) ：2D graphics rendering on the GPU in rust using path tessellation.
* [iced](https://github.com/iced-rs/iced) : A cross-platform GUI library for Rust, inspired by Elm
* [kajiya](https://github.com/EmbarkStudios/kajiya) : Experimental real-time global illumination renderer

## 游戏引擎

* [bevy](https://github.com/bevyengine/bevy) : A refreshingly simple data-driven game engine built in Rust
* [godot-rust](https://github.com/godot-rust/godot-rust) ：Rust bindings for GDNative
