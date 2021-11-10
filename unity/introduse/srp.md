# SRP

## SRP

### 1.简介

渲染管线 RenderPipline

可能 包括下面步骤:

从 CPU 到 GPU

1. 模型渲染前的准备
2. 获取模型 \(VBO\)
3. 模型信息传入着色器
4. 着色器根据该模型对每个光源进行渲染

#### 官方资料

[https://docs.unity3d.com/Manual/ScriptableRenderPipeline.html](https://docs.unity3d.com/Manual/ScriptableRenderPipeline.html)

### 2. 代码示例

全部的代码示例如下

[MyScriptableRenderPipeline.cs](https://github.com/wotakuro/CustomScriptRenderPipelineTest/blob/master/Assets/Scripts/MyScriptableRenderPipeline.cs)

#### 2.1 进入渲染

1.自定义的BasicPipeInstance 类继承 :RenderPipeline

```text
class BasicPipeInstance : RenderPipeline
```

并实现其 Render 方法

```text
public override void Render (ScriptableRenderContext context, Camera[] cameras)
```

2.清除屏幕颜色缓存

context.DrawSkybox\(camera\);

可以采用 ComdBuffer 写法

```text
// clear buffers to the configured color        
var cmd = new CommandBuffer();        
cmd.ClearRenderTarget(true, true, m_ClearColor);       
context.ExecuteCommandBuffer(cmd);       
cmd.Release();
```

其他的功能实现也可以采用 ComdBuffer 写法,

和 直接 context 执行有很大的不同,context 具有上下文信息,比如 SkyBox,cmd 没有

#### 2.2 模型裁剪

对每个摄像机都进行一次模型裁剪并且获取相应的裁剪结果,

如果无法裁剪就跳过这个摄像机

```text
foreach (var camera in cameras){    
    // Create an structure to hold the culling paramaters    
    ScriptableCullingParameters cullingParams;
    //Populate the culling paramaters from the camera    
    if (!CullResults.GetCullingParameters(camera, stereoEnabled, out cullingParams))        continue;        
    // if you like you can modify the culling paramaters here
    cullingParams.isOrthographic = true;        
    // Create a structure to hold the cull results
    CullResults cullResults = new CullResults();
    // Perform the culling operation
    CullResults.Cull(ref cullingParams, context, ref cullResults);
    //接下来的代码
}
```

#### 2.3 模型选择

从裁剪出的模型 挑选出此次需要渲染的模型,根据:

\(1\) 渲染队列中的位置

\(2\) layer的序号

示例1

```text
// Get the opaque rendering filter settings 
var opaqueRange = new FilterRenderersSettings();
//Set the range to be the opaque queues
opaqueRange.renderQueueRange = new RenderQueueRange() {
    min = 0,
    max = (int)UnityEngine.Rendering.RenderQueue.GeometryLast,
};
//Include all layers opaqueRange.layerMask = ~0;
```

示例 2

```text
var filterSettings = new FilterRenderersSettings(true) {
    renderQueueRange = RenderQueueRange.transparent,
    layerMask = 1 << LayerDefine.CHARA
};
```

renderQueueRange 可通过 min和max 或者 Shader 中的渲染模式获取

layerMask 采用二进制掩码的方式获取应该渲染的层级

#### 2.4 渲染参数

模型的渲染可以考虑下面这些渲染参数的选择

* HDR vs LDR
* Linear vs Gamma
* MSAA vs Post Process AA
* PBR Materials vs Simple Materials
* Lighting vs No Lighting
* Lighting Technique
* Shadowing Technique

设置渲染参数 ，即渲染所使用的Shader配置

```text
// Create the draw render settings
// note that it takes a shader pass name
var drs = new DrawRendererSettings(myCamera, new ShaderPassName("Opaque"));
// enable instancing for the draw call
drs.flags = DrawRendererFlags.EnableInstancing;
// pass light probe and lightmap data to each renderer
drs.rendererConfiguration = RendererConfiguration.PerObjectLightProbe | RendererConfiguration.PerObjectLightmaps;
// sort the objects like normal opaque objects
drs.sorting.flags = SortFlags.CommonOpaque;
// draw all of the renderers
context.DrawRenderers(cullResults.visibleRenderers, ref drs, opaqueRange);
```

1. 代码中 new ShaderPassName\("Opaque"\) 要和 Shader 中的 Pass{Tags { "LightMode" = "Opaque"} 相对应,表示此时采用的是模型的Shasder中 名为 Opaque 的 Pass 进行计算
2. DrawRendererFlags 中 :
3. None : 不合批,
4. EnableDynamicBatching: 动态合批 
5. EnableInstancing: 手动合批
6. Note : 合批: 多个模型合成一个模型一次性传递给GPU,一次 DrawCall,要求同一个 Material\(Sahder/Pass\)
7. RendererConfiguration : 为模型加入光照贴图,泛光点?
8. SortFlags 物体排序

一般选择 CommonOpaque不透明，CommonTransparent 透明 之一。

1. DrawRenderers 通知渲染,即提交 GPU 一次DrawCall ,
2. 通过上面代码,手动合批变得十分清晰明了
3. 另外在提交 context 前可以多次通知渲染,即多次DrawCall,对不同模型选择,不同渲染参数的模型组做渲染要采用多次渲染通知。

#### 2.5 收尾工作

在每个相机循环的最后提交context 的渲染队列设置

```text
// submit the context, this will execute all of the queued up
commands. context.Submit();
}//End Foreach Camera
```

## SRP Batcher

[SRP Batcher：加速渲染](https://connect.unity.com/p/srp-batcher-jia-su-xuan-ran)

