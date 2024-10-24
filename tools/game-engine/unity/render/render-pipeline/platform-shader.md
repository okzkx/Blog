[Unity - Manual: Writing shaders for different graphics APIs](https://docs.unity3d.com/Manual/SL-PlatformDifferences.html)

- 传统平台 traditional : OpenGL
- 现代平台 morden : Direct3D, Vulkan

## Render Texture coordinates

RenderTexture 的坐标

- traditional : 原点左下角
- morden : 原点左上角 

morden 宏定义 : UNITY_UV_STARTS_AT_TOP

## Clip space coordinates

从近平面到远平面 Clip space 坐标深度

- traditional : 1 ~ 0
- morden : -1 ~ 1 

## Precision of Shader computations

shader 里使用 float, 电脑使用 float 但是在手机里是使用 Half 
[Use 16-bit precision in shaders](https://docs.unity3d.com/Manual/SL-Use16BitPrecisionInShaders.html)


## The Depth (Z) direction in Shaders

**Traditional**
- 深度图 : 0~1
- clip space 深度 : [-near , far]

深度图越近越黑, 图里的数字代表距离

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410221740962.png)

Morden

- marco : UNITY_REVERSED_Z
- 深度图 : 1~0
- clip space 深度 : [near , 0]

总体

 - Fetching the depth Buffer
 - Using clip space
 - Projection matrices

