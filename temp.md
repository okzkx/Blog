
state cinematic sequencer

Shader pragma
#pragma multi_compile _ LIGHTMAP_SHADOW_MIXING
#pragma shader_feature_local _RECEIVE_SHADOWS_OFF
#pragma shader_feature_local_fragment _LASER_SCAN


周常，性能检测：  
CPU耗时（多线程）  
GPU耗时   
顶点数/批次  
管线检查（绘制逻辑，如深度图应用，后处理添加，防止新效果不受控）  
  
1.光照效果测试（Forward+，性能，效果，合批） 
2.feature验证（遮挡，OutLine，dither）
3.渲染分级（如：shaderLod,性能，效果测试）  
4.烘焙效果验证  
5.阴影效果配置  
6.后处理配置


[UnityIngameDebugConsole : In-game Debug Console for Unity 3D](https://github.com/yasirkula/UnityIngameDebugConsole)