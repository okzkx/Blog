# AssetBundle

- [Unity Asset Bundles，不可不知的使用技巧和误区](https://mp.weixin.qq.com/s/2wMpO9h7aCcv3gpVBJxQDA)

##### 作用

1. 逻辑块
2. 按需交付和更新内容
3. 减小构建
4. 补丁和 DLC

**所有这些工作都可交由 Addressables 系统处理**

要点

- **根据资产被加载和一起使用的频率来打包它们。**
- **向前兼容**, 但是一般**需要重新构建 Bundle 以保持兼容性**
- 不支持跨平台
- 加载缓存（Loading cache）：是一个共享的页面池
- **其他内部数据**　：除了游戏资产外，Asset Bundles 还包括一系列额外的信息和头文件
- **资产目录**　：Asset Bundles 中的资产映射，让你能够通过名称查找和加载每个资产
- **预加载表**　预加载表（ Preload Table）列出了 Bundle 中每个资产的依赖项。Unity 用它来正确加载和构建资产。**设计 Bundle 时最好尽量减少依赖链**。
- **TypeTrees**　定义了 Asset Bundles 中对象的序列化布局。避免将多种不同类型的对象混在一个大的 Bundle 中，　**建议保持 TypeTrees 启用**， 在加载时，TypeTrees 存储在内存中的全局缓存中，所以如果多个 Asset Bundles 存储相同类型的对象，不会增加运行时的内存成本
- **序列化文件缓冲区**　**最好避免使用大量非常小的 Asset Bundles**，每个　Ｂｕｎｄｌｅ　都要划分一定量的缓冲区
- **CRC 完整性检查** CRC（Cyclic Redundancy Check）用于对 Asset Bundles 进行校验和验证，以确保传送到游戏中的内容与预期完全一致。CRC 是基于 Bundle 的未压缩内容计算的。
	-  **PC 或移动设备）上，对从 CDN 下载的 Bundle 进行 CRC 检查就非常重要**。这是为了确保文件没有损坏或被截断，从而避免潜在的崩溃，同时也为了防止潜在的篡改。
	- **只对非缓存的远程 Bundle 启用 CRC 检查**。
- 三种在 Bundle 中查找资源的方法
	- 项目相对路径
	- 资产文件名
	- 带扩展名的资产文件名
	- 后两种要构建查找表


