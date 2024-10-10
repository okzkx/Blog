- [[官方直播] 详解Unity Asset的一生](https://www.bilibili.com/video/BV1Wv41167i2)

## Asset 导入设置

1. Meta 
2. Library
3. StreamingAssets
4. 波浪线

## Meta

- 存有 Yaml 结构的 Importer 信息, 可以和 Inspecter 面板一一对应
- meta 对应的 Asset 里是资产数据, fileId 索引指向 ObjectId

### Library

- Version1 : 会把 Asset 生成到 Library 的 metadata 文件夹里
- Version2 : 资产放在 ArtifactDB, SourceDB (LMDB)里

### 波浪线

- Unity 不会导入波浪线结尾的文件夹

## AssetBundle

1. 原理
2. 参数
3. 识别
4. 策略

### 原理

#### 功能

- 虚拟文件系统
- 压缩包
- 文件依赖关系
- 跨平台
- 索引

### 知识点

- 头立刻加载
- Asset 和 Scene 要分开打

### 参数

- ChunkBasedCompression : 改良过的 LZ4 , 压缩包体大小, 可以加密
- DisableWriteTypeTree : 减小包体和内存大小
- TypeTree : 

### 识别

- 算打包之前的 AB 包哈希, 打包后的不一致

### 策略

- 不要打的过大或者过小
- 过大 : 下载慢, 内存占用多
- 过小 : 头文件占比大
- 网上下载的 1~2 m, 随包发布的 5~10m

Asset 的加载管理

1. Editor 和 Runtime 的加载策略不同, 不能使用 Editor 来检测
2. 序列化和反序列化
3. 解析 Prefab 场景会比 GameObject 快
4. 尽量做 Prefab 场景
5. TypeTree : 兼容性之树, 方便引擎跨版本, 对应 serializedVersion, 所以一般可以关掉
6. 同步和异步 ：异步至少慢一帧，总体时间长，能不卡顿
7. Preload 与 Presistent
	1. PreloadManager 有个 Option 队列
	2. PresistentManager 负责加载文件
	3. 同步和异步的 Option id 可能会相同, 会争抢 PresistentManager , 会出现 lock 现象

Asset 的卸载

1. UnloadUnusedAsset : 卸载无用的 Asset, 会造成卡顿
2. AssetBundle.Unload(bool) : true 会把 AssetBundle 里的所有 Asset 卸载, false 不会卸载 Asset