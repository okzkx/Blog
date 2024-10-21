[Unity实现Hi-z遮挡剔除(CPU篇) - 知乎](https://zhuanlan.zhihu.com/p/697659813)

如果物体被渲染到屏幕时完全被别的物体挡住就该被剔除,
如果物体被剔除了, 就不会被渲染在屏幕上,
如果物体不被渲染在屏幕上就不知道会不会被挡住

那咋知道物体该不该被剔除嘞

[Unity实现Hi-z遮挡剔除(GPU篇) - 知乎](https://zhuanlan.zhihu.com/p/700453220)


1. 生成深度 Mipmap
	- 把 DepthTexture 使用 ComputeShader 生成
	- 把 DepthTexture 回读到 Cpu 生成
2. 将包围盒的最近深度和 DepthTextureMip 比较
	 - Gpu 比较
		1. 包围盒的最近深度数据编码成纹理传到 Gpu 进行比较
		2. 比较结果异步回读到 Cpu
	 - 异步回读 DepthTextureMip 在 Cpu 进行比较
3. 根据比较结果设置物体可见性
	1. Brg 的 Cull 回调中设置
	2. 直接在 Renderer 设置 Active