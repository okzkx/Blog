# 07. Synchronization

[Vulkan Synchronization | "Understand Fences, Semaphores, Barriers,..." | Vulkan Lecture Series Ep. 7Computer Graphics at TU Wien](https://image-1253155090.cos.ap-nanjing.myqcloud.com/ARTR2022_VK07_Synchronization.pdf)

## Command buffer 命令

![](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272218713.png)

## Pipeline Stages

![image-20230127225028349](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272250455.png)

## Sync Objects

#### Binary Semaphores

![1656921670422](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272254422.png)

### Timeline Semaphores

![image-20230127225712516](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272257644.png)

### Excution Barriers

![image-20230127225948175](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272259245.png)

Graphics pipline 阶段对应的 Stage mask

![image-20230127230113347](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272301405.png)

### Memory Barriers

Memory Availability and Visibility ,当内存非 Availability 时，禁止后续 cmd 访问。

Excution Barriers 是从渲染流程着手，Memory Barrier 是从内存是否是最新着手，防止后续 cmd 访问，来使 cmd 同步。

### Sub-pass Dependencies

通过 subpass 依赖来同步输入输出纹理

![image-20230127230845028](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272308092.png)

### Events

Set 和 Wait Event 共同组成一个 Barrier 实现

![image-20230127231139179](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272311232.png)

### Overview of Synchronization Methods

![image-20230127231159278](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202301272311332.png)
