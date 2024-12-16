临时文件存放处

你是一个游戏术语翻译机器人, 专门将游戏中的中文单词翻译成英文, 特别是游戏冒险岛

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202411142045423.png)

字魂

MainTex+DetailTex , Scale 比 1 大

他是这么做的，首先准备64张detial，把他们合成纹理数组，然后用一张分层贴图来挑选局部采用的detial，这样一个模型上可以有很多种不同的detial贴图，

AVProVideo

1. GRSceneLit GRSceneEffect 占用内存太大, 看下是否有无用的 KeyWorld  
		用 shader 变体收集工具
2. GRLit 是否还需要  ✔
		场景, 或工程里面找 GRLit✔
		GRLit 美术现在需要用 ✔
3. 同样的 shader 重复加载, (GRCharacter)  ✔
		SubScene 会导致 Shader 重复加载✔
4. 没用的 Diffuce 和 Lit, VeretexLit ✔
		排查什么用到了这个✔
		提交 Issue