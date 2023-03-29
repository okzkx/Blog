# Swap chain

swap chian 负责将图像缓存送到窗口显示

显示策略

- Immediate ：立即显示，可能会撕裂
- FIFO ：等待 vertical blank 排队显示
- FIFO Relaxed：如果当前窗口没东西显示，就立即显示，否则排队显示
- Mailbox ：等待 vertical blank 排队显示，只允许最迟的那张图在排队中，之前排队又没显示的图不再显示

vertical blank ：窗口通过扫描，完全显示呈现完一张图的时间周期

![image-20230204214023591](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042140693.png)

#### Mail box 是最常用最好用的方法

![image-20230204220258043](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042202237.png)

## Swapchain 拓展

#### device extension

swapchain 是 GPU 中的驱动支持，需要 GPU 支持这个拓展，VK_KHR_swapchain

#### instance extension

让 instance 支持 swapchain 需要启动拓展 VK_KHR_surface ，依赖 GPU 的 swapchain 拓展

这样能直接使用 QueuePresent 来使用 GPU 的 swapchain

## Code

![image-20230204222914274](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042229441.png)

![image-20230204223008780](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042230852.png)

1. 获取窗口 surface
2. 创建 swapchain
3. 创建 Image 与 swapchain 关联
4. loop
    1. 从 swap chain 请求下一张待着色的图像，后激活 image available semaphore![image-20230204223952799](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042239930.png)
    2. 使用 graphics queue 提交绘制![image-20230204224133992](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042241100.png)
    3. 绘制结束后使用 present queue 提交显示，隐式调用 gpu swapchain ext![image-20230204224956012](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042249127.png)

