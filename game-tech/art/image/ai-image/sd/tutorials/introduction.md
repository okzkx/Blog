- [B站第一套系统的AI绘画课！零基础学会Stable Diffusion，这绝对是你看过的最容易上手的AI绘画教程 | SD WebUI 保姆级攻略_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1As4y127HW/?spm_id_from=333.880.my_history.page.click&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)
- [Stable Diffusion 零基础入门课学习资料汇总](https://nenly.notion.site/017c3341c8b84a7ebb4c2cb16f36e28f?v=8d3885a8404b4f27a998d03b23a87f19)

## [第1课：初识应用与基本出图流程](https://nenly.notion.site/1-732d811aff564e8387924f4ff5a106bd)

下载 
- [【AI绘画·24年8月最新】Stable Diffusion整合包v4.9发布！解压即用 防爆显存 三分钟入门AI绘画 ☆更新 ☆训练 ☆汉化 秋叶整合包](https://www.bilibili.com/video/BV1iM4y1y7oA/?spm_id_from=333.999.0.0)

## [第2课：文生图入门与参数设置](https://nenly.notion.site/2-c8e765308c95425f84cef6d3e0bf1cc4)


提示词参考网站

- **AIGODLIKE**：[https://www.aigodlike.com/](https://www.aigodlike.com/)
- **AI画廊：**[](https://www.aigallery.top/aigjz?c2=15&c3&c4&t)[https://www.aigallery.top/](https://www.aigallery.top/)
- **词图PromptTool：**[https://prompttool.com/](https://prompttool.com/)

提示词分类

- 提示词类别：内容型提示词（控制画面内容） & 标准化提示词（控制画质和画风）
- 提示词权重：控制提示词呈现的优先级的增减
- 进阶提示词语法：混合、迁移与迭代
- 如何书写提示词？你可以参考这样一个“通用模版”——\
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410111101009.png)

提示词生成

- 翻译大法
- 借助工具
	- 一个工具箱：[http://www.atoolbox.net/Tool.php?Id=1101](http://www.atoolbox.net/Tool.php?Id=1101)
	- AI词语加速器：[https://ai.dawnmark.cn/](https://ai.dawnmark.cn/)
- 抄作业
	- OpenArt：[https://openart.ai/](https://openart.ai/)
	- ArtHubAi：[https://arthub.ai/](https://arthub.ai/)
	- [ Civitai](https://civitai.com/models/47800/game-icon-institutemode?modelVersionId=505488)
	- [LiblibAI](https://www.liblib.art/modelinfo/1ab88289520644869ab634ff334736af?from=search&versionUuid=36c4d0873e6b4f1cbb3a864274fe3b9f)

## [第3课：图生图入门](https://nenly.notion.site/3-13a380b6b3384fcba2d23f40a97ab0e6)


## [第4课：模型下载与应用](https://nenly.notion.site/4-de22d2ce460f4f88800cdc5ffada976f)

主流模型下载渠道
- [https://huggingface.co/models](https://huggingface.co/models)
- [https://civitai.com/](https://civitai.com/)
- [https://www.liblibai.com/](https://www.liblibai.com/)
- [https://www.esheep.com/](https://www.esheep.com/)

## [第5课：高清修复与后期处理](https://nenly.notion.site/5-b41e338382ab46c4ae5227bc53d0bc30#3274bd43c52d48508bb6e37f2c03e1bb)


- 高清修复 : 文生图中常用放大手法，本质是“再做一次图生图”，效果不错，但速度慢且吃显存
- SD放大 : 图生图中常用放大手法，将图片拆成小块重绘再拼合，效果不错，可以有效突破显存上限，但容易造成画面混乱
- 后期处理 : 传统非扩散的放大手法，会让细节变得更加锐利清晰，速度非常快但效果一般

## [第6课：进阶模型原理解析](https://nenly.notion.site/6-9754675d0bcc4df8bbb1458b92077bd2#8dfbc954f25849459a5618558d69f1a5)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410111111681.png)

- Embedings : 词嵌入模型, 用于还原角色形象, 解决画面错误
- LoRA 低秩适应模型 : 还原角色形象, 训练画风
- Hypernetwork 超网络 : 训练画风

## [第7课：局部重绘](https://nenly.notion.site/7-56b41d8a73be403bacf19fb6f7b993d7)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410111117738.png)

## [第8课：扩展安装与推荐](https://nenly.notion.site/8-8a96710184b34e7a81557e748540e95b#b35aac2e17ad4e34bd6b02d19148dcd5)

## [第9课：LoRA原理与应用](https://nenly.notion.site/9-LoRA-2280f711c4324e91b3e8d57d3a196cb6)

LoRA 应用方向

- 人物
- 画风
- 概念
- 服装
- 特定元素

## [第10课：ControlNet基础入门](https://nenly.notion.site/10-ControlNet-a802ab59c48d45f8ae4ad1cfea0a0d4d)

- Openpose:控制姿势、手部、面部细节
- Depth:控制空间组成(深度)
- Canny:控制线条轮廓
- SoftEdge:控制线条轮廓，但更加柔和、放松
- Scribble:涂鸦引导画面生成