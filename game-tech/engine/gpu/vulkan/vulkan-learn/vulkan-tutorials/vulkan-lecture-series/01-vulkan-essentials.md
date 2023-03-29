# Vulkan Essentials

## API

- Graphics and compute
- enable high efficiency by being low-level
- a very explicit
- cross-platform

![image-20230204192403999](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302041924137.png)

## Validation

- Validation layers
- Vulkan specification
- vulkan 的参数结构应当全部成员初始化
- 使用 vkCreateDebugUtilsMessengerEXT 来检测错误

## Objects

### Instance

![image-20230204194033736](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302041940876.png)

Physical device

![image-20230204194218090](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302041942206.png)

Device

1. extensions
2. queue configurations
3. multiple logical device per physical device

![image-20230204194232704](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302041942822.png)

### Queue

queue 用来执行 command buffer，

command buffer 同时开始，结束时间不一定？顺序执行 command buffer

#### queue family

physical device 可能有多个 queue family ，每个 queue family 可能有多个 queue

不同的 queue 并行执行 command buffer

不同的 queue family 对不同的 command 执行效率不一样

## Extensions

- Instance Extensions ：该 vulkan 版本具有的可开启的拓展功能，拥有 EXT 后缀
- Device Extensions ：这个 gpu 支持的可开启的拓展功能，拥有 GPU 型号或通用 KHR 前缀

也可以在 Instance 里查询所有可开启的 extensions

extensions 可能有 extension 依赖，需要使用字符串启用。并且在 next 字段赋值该拓展功能对应的设置

![image-20230204200328859](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042003044.png)

## Vulkan Hardware Capability Viewer

用来查看硬件参数，就上面那些 Vulkan 工具所支持的种类和数量