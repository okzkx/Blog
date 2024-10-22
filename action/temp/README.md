临时文件存放处

[Boosting your game performance with Unity 6 Profiling tools | Unite 2024](https://www.youtube.com/watch?v=_cV1B2hqXGI "Boosting your game performance with Unity 6 Profiling tools | Unite 2024")

[Tackling UI challenges in Football Manager 25 | Unite 2024](https://www.youtube.com/watch?v=im49swPfWIo "Tackling UI challenges in Football Manager 25 | Unite 2024")

[Prototype mobile games faster with the Input System in Unity 6 | Unite 2024](https://www.youtube.com/watch?v=ptvjumIHxYg&t=1s "Prototype mobile games faster with the Input System in Unity 6 | Unite 2024")

[Unity 6 - Create faster and reach more platforms](https://www.youtube.com/watch?v=1SyqN3D6khI "Unity 6 - Create faster and reach more platforms")

[New lighting features and workflows in Unity 6 - YouTube](https://www.youtube.com/watch?v=IpVuIZYFRg4)

[Sculpt, Model, and Rig a Low Poly Reaper in Blender - Real-time (no timelapse) - YouTube](https://www.youtube.com/watch?v=ul5HIDAXoak)


[Boosting your game performance with Unity 6 Profiling tools | Unite 2024 - YouTube](https://www.youtube.com/watch?v=_cV1B2hqXGI)

Cpu bound

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410181440978.png)

GPU bound
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410181441406.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410181502218.png)


Gpu resistant draw 可以解决下面这个多数据传输问题

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410181501006.png)


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


Playable

[【Unity学习之路】一小时速通Playable_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Er421t7vT/?spm_id_from=333.337.search-card.all.click&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

- PlayerAnimation
- Mixer
- Rumtime,  控制动画播放状态
- 

UNITY_REVERSED_Z
HClip

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410221404799.png)

深度图越近越黑

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410221740962.png)

设置 ScriptableRenderPass 渲染目标

```
public override void Configure(CommandBuffer cmd, RenderTextureDescriptor cameraTextureDescriptor) {  
    RenderTextureDescriptor descriptor = cameraTextureDescriptor;  
    cmd.GetTemporaryRT(reflectColorID, descriptor, m_DownsamplingMethod == Downsampling.None? FilterMode.Point : FilterMode.Bilinear);  
    ConfigureTarget(reflectColorID);  
    ConfigureClear(ClearFlag.All, Color.clear);  
}
```