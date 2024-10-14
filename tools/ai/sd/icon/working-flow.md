

## Models

- [Game icon Institute_mode - V4_XL | Stable Diffusion XL Checkpoint | Civitai](https://civitai.com/models/47800/game-icon-institutemode?modelVersionId=505488)
- [全网首发 | AI游戏图标RPG较重画风|Game Icon RPG Style-LoRA-小李广宵喵-LiblibAI](https://www.liblib.art/modelinfo/6a750590404a4fc8943babac35af3953?from=search&versionUuid=0c8b34f25e7c4c1b8574686fc33122ca)
- [ReVAnimated_v122-Checkpoint-醉天涯-LiblibAI](https://www.liblib.art/modelinfo/1004b01e19714137a593e30007f3c737?from=search&versionUuid=5c60ea3485364ef596e73f7969a745c8)
- 

## Reference

- [AI- 游戏图标研究所-各种方法绘制图标](https://www.bilibili.com/video/BV1LX4y1v77z?spm_id_from=333.880.my_history.page.click)
- [AI-关于游戏界面文生图-探索_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1rh4y1E7aS/?spm_id_from=333.788.recommend_more_video.0&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)


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
