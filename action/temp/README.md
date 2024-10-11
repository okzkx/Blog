临时文件存放处

基础起手式

```
masterpiece, best quality, 

lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry
```

游戏图标

```
Game icon Institute,Game icon,still life,official art,well-structured,(white background),

(EazyNagative:0.8),black,dark,low_resolution
```



1. 起始提示词
2. 文生图生成想要图片的大形
	1. 使用提示词少量到多
	2. 使用 LoRA 生成特定的物体
	3. Control net 控制形状
	4. 重绘幅度高, 分辨率较低, 产出图片数目多
	5. 使用 LoRA 控制成为想要的画风
3. 图生图生成想要的小图
	1. 修改提示词
	2. 重绘幅度高, 分辨率较低, 产出图片数目多
	3. 找到想要的小图
4. 图生图,从小图生成大图
	1. 细化提示词
	2. 重绘幅度低, 分辨率高, 产出图片数目少


tower,(arrow tower),whole object,medieval style,(ancient architecture),archer tower,((simple wooden building)),(two floors),dwarf,battle,

tower,(arrow tower),whole object,medieval style,(ancient architecture),archer tower,((simple wooden building)),(two floors),dwarf,battle,<lora:game icon institute_oumeiQ_v2:0.6>,

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410120153379.png)

archer,full body,(bow in hand),bow to the right,chest toward viewer,<lora:game icon institute_oumeiQ_v2:1>,face to left,

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410120204296.png)
