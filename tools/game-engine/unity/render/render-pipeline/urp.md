# Universal Render Pipeline

## 卡通渲染

[URP_Toon](https://github.com/ChiliMilk/URP_Toon)

## Urp 渲染结构

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

## 相机渲染结构

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
