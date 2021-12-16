# 笔记

## 关于顶点片段着色器输入输出的顶点

- float4 positionOS : POSITION
- float4 positionCS : SV_Position

#### 在顶点着色器中

- positionOS 表示局部空间的物体顶点，w 为 1
- positionCS = MVP_Matrix * positionOS
- positionCS.xyz 是在 ClipSpace 下的顶点位置

#### 顶点和片段之间

1. 使用透视除法将顶点转换到 NDC 空间 positionCS.xyz / positionCS.w 
2. 视口变换将 NDC 空间的 positionCS.xy 变换到屏幕空间

#### 片段着色器中

- positionSS = positionCS 片段着色器中的 SV_Position 不是 ClipSpace 了，而是 ScreenSpace
- positionSS.xy 为屏幕空间的坐标，取值为 (0, 0) ~ ( _ScreenParam.x, _ScreenParam.y)
- positionSS.z 为 device depth，设备深度取值 1 ~ 0 ，即写入 DepthBuffer 的深度
- positionSS.w 为 depth in videw space，实际深度，取值为摄像机定义的 near ~ far，和上面是相反的