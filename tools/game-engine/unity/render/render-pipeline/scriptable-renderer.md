
设置 ScriptableRendererPass 渲染目标

```
public override void Configure(CommandBuffer cmd, RenderTextureDescriptor cameraTextureDescriptor) {  
    RenderTextureDescriptor descriptor = cameraTextureDescriptor;  
    cmd.GetTemporaryRT(reflectColorID, descriptor, m_DownsamplingMethod == Downsampling.None? FilterMode.Point : FilterMode.Bilinear);  
    ConfigureTarget(reflectColorID);  
    ConfigureClear(ClearFlag.All, Color.clear);  
}
```