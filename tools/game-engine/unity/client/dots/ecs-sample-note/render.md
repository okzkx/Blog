
Material

RegisterMaterial
[MaterialChangerAuthoring.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/master/GraphicsSamples/URPSamples/Assets/SampleScenes/4.%20API%20Examples/MaterialMeshChange/SceneAssets/MaterialChangerAuthoring.cs)

```
World.GetOrCreateSystemManaged<EntitiesGraphicsSystem>().RegisterMaterial(material);

mmi.MaterialID = m_MaterialMapping[material];

MaterialMeshInfo.MaterialID = BatchMaterialID;
```


RegisterBinding
```
[RegisterBinding(typeof(MaterialChanger), "frequency")]
```

为 Entity 添加渲染组件

[AddComponentsExample.cs](https://github.com/Unity-Technologies/EntityComponentSystemSamples/blob/e90642374cb7cd1dbb4c33e2d2736e2645521590/GraphicsSamples/URPSamples/Assets/SampleScenes/4.%20API%20Examples/RenderMeshUtilityExample/SceneAssets/AddComponentsExample.cs#L11)

```
RenderMeshUtility.AddComponents(  
    entity,  
    entityManager,  
    desc,  
    renderMeshArray,  
    MaterialMeshInfo.FromRenderMeshArrayIndices(0, 0));
```

```
[WorldSystemFilter(WorldSystemFilterFlags.BakingSystem)]
```

