# Advanced

## BlobAsset

- 组件起名 MeshBB ,存 Mesh 有关的数据?
- 用到了 Blob, BlobArray, BlobAssetReference, 这些是做什么的?
- Blob 是只读的二进制对象块?

### Components

- MeshBBComponent -<> BlobAssetReference`<MeshBBBlobAsset>`
- 立方体 Mesh 信息, MeshBBBlobAsset
- 计算前的 MeshBBBlobAsset?,  MeshBBFactorySettings

### ConversionSystem

1. First step: for all changed GameObjects we compute the hash of their blob asset
	1. blob 散列计算资产 context = new BlobAssetComputationContext<MeshBBFactorySettings, MeshBBBlobAsset>
        关键属性 
        1. 哈希 
        2. 计算前, BlobAssetToCompute 
        3. 计算后, ComputedBlobAsset
	2. 对于每个 MeshToBoundingBoxAuthoring 取哈希值, hash
    3. 如果 context 不包含 hash, 就实例化 MeshBBFactorySettings
	4. hash 与 MeshBBFactorySettings 存入 context

2. Step two, compute BlobAssets

    1. 对于 context 中的每个 MeshBBFactorySettings
    2. job 并行计算出相应的 BlobAssetReference`<MeshBBBlobAsset>`, ComputeMeshBBAssetJob
    3. 将 BlobAssetReference`<MeshBBBlobAsset>` 与 hash 也存入 context

3. Third step, create the ECS component with the associated blob asset

    1. MeshToBoundingBoxAuthoring -> MeshBBComponent
    2. 初始化 MeshBBComponent 所需参数通过 hash 从context 中取得
    所以 context 有什么用?

### MeshBBRenderSystem

使用 Debug 将组件画在场景里

## SimpleMesh

使用 Graphics.DrawMesh 进行 Mesh 渲染

1. SimpleMeshRenderingAuthoring -> SimpleMeshRenderer
2. SimpleMeshRendererSystem 绘制 Mesh

	使用参数
    1. SimpleMeshRenderer.Mesh
    2. SimpleMeshRenderer.Material
    3. LocalToWorld.Value
