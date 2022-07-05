# Vulkan Lecture Series

[Vulkan Lecture Series - TU Wien](https://www.youtube.com/watch?v=tLwbj9qys18&list=PLmIqTlJ6KsE1Jx5HV4sd2jOe3V1KMHHgn&index=1)

## Vulkan Essentials

### Vulkan 简介

- Vulkan 相比 OpenGL 更加 LowLevel Api
- OpenGL 应用 ：大（高度封装）驱动层，小应用层
  - 应用层
    - 单线程
  - 驱动层
    - 资源管理
    - GLSL 编译
    - 错误检测
- Vulkan 应用 ：小驱动层，大应用层
  - 应用层
    - 内存分配，资源管理
    - 多线程控制，显性的线程同步策略
    - Command Buffer 并行策略
  - 驱动层
    - 设备控制暴露

### Vulkan SDK

- Headers
- Libraries
- Tools
  - Vulkan Configurator

### Queue

- 根据硬件提供的 Queue Family 来创建 Queue
- Queue 用来执行 Command Buffer
- Queue 中的 Command Buffer 按照顺序执行，但是是异步执行，不是串行

### Vulkan Hardware Capbility Viewer

- 查看 GPU 提供的 QueueFamily
- 查看 Instance Extensions
- 查看 Device Extensions

### 启用硬件特性

- instance-level, device-level extensions
- enable extensions
  1. list extensions you need
  2. query if an extension is support
  3. pNext chain to configuration
- Prefix of extensions
  - Khronos : VK_KHR
  - Vendor-specific : VK_AMD, VK_NV
  - Mutivendor : VK_EXT

## Swap Chain

## Resources & Descriptors

Types of resources

- Buffer
  - 无格式的字节数组
- Image
  - 有格式，有信息，有维度的字节数组
- Samplers
- Acceleration Structures

Descriptors : Where to find resource

## Command Buffer

[Command Buffer Lifecycle](https://www.khronos.org/registry/vulkan/specs/1.3-extensions/html/chap6.html#commandbuffers-lifecycle)

## Vulkan Synchronization

- Wait Idle (host sync)
  - vkQueueWaitIdle(queue)
  - vkDeviceWaitIdle(queue)
- Fences (host sync)
  - Batch -> signal Fence
- Semaphores (queue sync)
  - Binary
  - Timeline
- PipelineBarriers(command sync)

![1656924147273](image/vulkan-lecture-series/1656924147273.png)

#### Binary Semaphores

![1656921670422](image/vulkan-lecture-series/1656921670422.png)

![1656921768061](image/vulkan-lecture-series/1656921768061.png)


1. presentation queue : acquire image signals Image Available Semaphore
2. graphics queue : waits on Image Available Semaphore
3. graphics queue : draw signals Draw Finished Semaphore
4. presentation queue : present wait on Draw Finished Semaphore
