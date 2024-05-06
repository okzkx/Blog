# 超简短的独立游戏开发教学

- [合集·超简短的独立游戏开发教学](https://space.bilibili.com/7942241/channel/collectiondetail?sid=28106&spm_id_from=333.788.0.0)

第一季

#### [UI还能长什么样？](https://www.bilibili.com/video/BV1KL4y187mK/?spm_id_from=333.788&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

本质
- 给用户操作某些功能的入口
- 对某些信息的展示

设计
- 2D UI
- 环中间布局, 重要内容弹中间框, 或者覆盖整个屏幕
	- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061707817.png)
	- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061707817.png)
- 合并 UI
	- 越是高频使用的 UI , 玩家所需要点开的步数要越少
	- 功能可以分配给 NPC, 店铺, 道具之类的角色
- 手游里的手柄需要有区域, 而 pc , 主机不用区域, 甚至能省略

- 3D UI
	- 放在角色旁边进行提示
	- 第一人称时的 HUD
	- 成为环境的一部分

#### [草履虫也能看得懂的乐理入门](https://www.bilibili.com/video/BV1Dg411c7WH/?spm_id_from=333.788&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)
[神他妈乐理·和弦：和弦的构造、标识、功能与应用，以及](https://www.bilibili.com/video/BV1ot4y1S7jh/?spm_id_from=333.788.recommend_more_video.1)
1. 声音
	1. 一个音的振动频率为另一个音两倍, 听感相同, 为统一个音
	2. ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061739351.png)
2. 八度
	1. 距离称为八度
3. 音阶
	1. 八度内通过算法算出其他音的频率, 他们大抵比较和谐, 可用于奏乐, 一般用十二平音律
	2. 再从十二个音里挑出音阶(调式)写旋律, 
		1. 自然大调音阶 ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061741942.png)
		2. 以及阿拉伯音阶, 大调五声音阶, 日本都节音阶
4. 调式
	1. 音阶都是相对第一个音的
	2. 使用字母来表示绝对音高
	3. 以 C 为 DO, 采用自然大调音阶, 就是初学者的 C 大调
5. 和弦
	1. 三和弦
		1. 确定调性后, 选择音阶中的 1, 3, 5 个音就是一级和弦
		2. 二级和弦 2, 4, 6
		3. 三级和弦 3, 5, 7
		4. 四级和弦 4, 6, 1
	2. 和弦的转位, 用临近的音替代
	3. 和弦形式
		1. 三个音同时演奏
		2. 三个音分解
6. 和弦进行
	1. 和弦的稳定性 ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061749140.png)
	2. 遵循起承转合 I, II, V, I
	3. 增加曲折的过程![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061750433.png)
7. 节拍
	1. 几个节拍构成一个小结, 循环往复
	2. 使用每分钟的拍子数来记录速度, BPM
	3. 单个拍子的长度根据占据小结总长的比例表示
	4. 音符也是, 全音符, 二分音符
	5. 音乐节拍 : 2 / 4 (四二拍, 两个四分音符为一节), 3/4, 4/4, 6/8
	6. 开始的拍子为强拍, 后面的为弱拍, 被二整除的节拍, 中间的为次强拍
	7. 旋律上的音可以和节拍对齐, 或者空隙对齐(反拍)
8. 旋律
	1. 先写和弦进行, 再写旋律
	2. 选择和弦上本来就有的音来构成旋律
	3. 强拍上使用和弦上的音, 别的地方使用其他音阶上的音
	4. 音长参差不齐
	5. 留白
9. 编曲
	1. 使用乐器对和弦和旋律进行演奏, 
	2. 尖锐盖住沉闷, 高音盖住低音
	3. 快盖住慢
	4. 平衡和对比
	5. 要和环境音效平衡

### 世界观

 [为了讲故事创造一个世界](https://www.bilibili.com/video/BV1iq4y1P7qr/?p=10&spm_id_from=pageDriver)
 
1. 讲述一个故事
2. 需要一个背景
3. 游戏里的世界观背景往往是架空的
4. 内容和美术风格指路
5. 世界观服务于故事
6. 故事服务于玩法

### 配乐

音乐不能是一成不变的单曲循环

进入战斗时

1. 开始的铺垫
2. 中间的高潮 ,要首尾相接, 循环播放
3. 最后的结尾

进行音乐的自然过渡

1. 让程序对配乐进行深度接管
2. 根据战斗内容播放音乐不同段的组合, 而不是音乐文件
3. 让音乐变成游戏反馈的一部分,和游戏音效互相配合
4. 完全由程序程序化生成音乐
5. 旋律的复用

### 游戏中的引导

- 说明式引导
- 难题式引导
	- 事先需要让玩家知道目标
	- 给玩家线索和暗示 ,指路

#### 故事

如何构建故事

- 冲突理论
	- 渴望
	- 行动
	- 障碍
- 故事曲线
	- 阐述
	- 上升动作
	- 危机
	- 高潮
	- 下降动作
- 起承转合

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061858085.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061859426.png)


![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061859403.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061900193.png)


![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061900069.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061900347.png)


![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061904897.png)

##### 故事的织体
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061905432.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202405061906916.png)
