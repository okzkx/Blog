 
整理
		- 临时
		- 书签
	- 重读引擎书
	- 计算机基础
## 临时
Unity.Profiling
Editor.Profiling
Zyq@0592
Noesis 爬资源

- 事件驱动
- 数据驱动

- 树,图结构
- 数组结构


UniversalRenderPipeline
- RenderSingleCamera
	- renderer.AddRenderPasses (ScriptableRenderer)
		- rendererFeatures[i].AddRenderPasses
			- renderer.EnqueuePass
			- remove invalid pass
	- 
	- renderer.Setup (SRP) => null
	- renderer.Setup (UniversalRenderer)
		- pass change fields
		- renderer.EnqueuePass
	- 
	- renderer.Execute(UniversalRenderer) => null
	- renderer.Execute (ScriptableRenderer)
		- SetupRenderPasses
			- rendererFeatures[i].SetupRenderPasses
		- ExecuteBlock
			- ExecuteRenderPass(m_ActiveRenderPassQueue)
	- 

DoRenderLoop
- UniversalRenderTotal
	- RenderCameraStack
		- Begin Camera Rendering
		- Update VolumeFramework
		- InitializeCameraData
		- InitializeAdditionalCameraData
		- RenderSingleCamera : Main Camera
			- SetupCullingParamters
			- SetupPerCameraShaderConstants
			- CullScriptable
			- InitalizeRenderingData
			- Setup
			- URP Asset Renderer Execute
			- Submit
		- Overlay camera
		- End Camera Rendering
	- RenderCameraStack
