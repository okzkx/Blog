[Sli97](https://space.bilibili.com/303957852)

[《多人联机对战游戏》客户端和服务端的实现与构架是怎么样_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1GM4m1Z7DW?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

账号服务
1. GateWay 注册到 Register
2. 客户端 Login 到 Rigster 获取 Gateway Ip
3. 客户端与 Gateway 建立长连接
4. Gateway 与 GameService 进行数据交互
5. 存储到 DB

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071018682.png)

战斗部分
1. 客户端上传操作到服务端
2. 服务端计算角色数据
3. 同步状态到所有客户端
4. 视线区域外剔除

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071021046.png)

优化

1. 每个 Battle 服务限制人数
2. ProtoBuf 替代 Json
3. 缓冲数组延时发送操作指令 (16ms -> 50ms)
4. 合并持续请求到开始请求 + 结束请求, 需要在服务器保持状态
5. 中间件 : 消息队列, 缓存数据库

[《地图随机生成》柏林噪声算法如何实现游戏世界_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19f42197ME?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

柏林噪声

1. 基于晶格的连续梯度算法
2. 当入参连续时, 返回值也是连续的

函数叠加细化地形

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071036512.png)

### 均匀随机采样

#### 米切尔最佳和候选算法

可以用四叉树优化

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071039282.png)

[《游戏中的寻路算法》游戏中有哪些常见的寻路算法_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1k4421Q744?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

BFS 广度优先搜索

- 战旗可行进区域
- 效率较低

启发式搜索

- 只对看起来更近的方向扩散
- 常用曼哈顿距离 H![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071058761.png)
- A* ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071059180.png)
- 
各种距离
-  四方向 : 曼哈顿
- 八方向 : 切比雪夫
- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071100024.png)

- Theta* 算法![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071104085.png)

- 其他
- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071103242.png)

[《游戏中的避障算法》游戏单位如何实现动态避障_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Nb421n7qJ?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

ROV2

[warmtrue/RVO2-Unity: use rvo2 (Optimal Reciprocal Collision Avoidance) in unity.](https://github.com/warmtrue/RVO2-Unity)


[《游戏中的AI技术》状态机、行为树、决策树如何实现游戏AI_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV16H4y1F7c2?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

有限状态机 : 
- FSM ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071121929.png)

- 子状态机 ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071121516.png)
- 分层状态机![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071122401.png)

行为树 : 
- 方便策划配置行为跳转,需要可视化编辑器 ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071123400.png)

决策树 :
- 预测模型, 收集历史数据进行汇总
- 熵越大排列在树的越高位, 对决策结果影响越大
- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071129243.png)

[《地图随机生成》波函数坍缩算法是如何实现的_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Br421M7Vm?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

1. 连续坍缩
2. 约束规则
3. 从多个可能状态到一个确定状态
4. 优先坍缩高概率地块 (熵低处)
5. 回溯机制, 拥有快照
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071415923.png)

[《JPS跳点搜索算法》比A*算法快百倍的JPS寻路算法是如何实现的_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV18z421i7s8?spm_id_from=333.788.player.switch&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

跳点搜索算法 : 没有预处理的情况下, 最快的 2D 网格搜索算法

跳点搜索
1. 强迫邻居
	1. N 是 X 的强迫邻居 : 节点 X 是到节点 N 的必经节点
	2. 定义 : 当节点x的八个邻居中存在障碍，且节点x的父节点p，经过节点x到达节点n的距离代价总是小于不经过节点x到达节点n的任意路径的距离代价，则称节点n是节点x的强迫邻居
	3. ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071429812.png)
2. 邻居裁剪
	1. 劣性节点只能由父节点到达, 自然节点只能由子节点到达
	2. ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071430849.png)
	3. ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071431993.png)
3. 跳点
	1. 条件一:节点x是起点或者终点
	2. 条件二:节点x至少有一个强迫邻居
	3. 条件三:节点x的父节点p在斜线方向，并且节点x的直线方向(水平或垂直)上存在满足条件一或二的节点
4. 按照先直线后斜线的方式搜索跳点

[PathFinding.js](https://qiao.github.io/PathFinding.js/visual/)

[《游戏中的空间划分》四叉树、KD树如何优化游戏性能_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV188tYezEDu?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

范围内静态物体查询优化

空间数据结构
- QuadTree
- KDTree
- BVH
- BSP Tree

四叉树更新
1. 物体运动时更新, 移动元素到另一节点
2. 边界情况

[《游戏中的AOI算法》九宫格法、十字链表法如何优化MMO网游_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1kpSmYTEoy?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

## AOI
Area Of interest

- 九宫格法![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202501071524030.png)
	- 简单, 快速, 方便
	- 密集时性能退化, 空间占用大

- 十字链表法
	- 根据单位进出视野来发送事件
	- 用十字链表记录位置状态

对比 
- 内存 : 
	- 十字链表 : 单位数量
	- 九宫格 : 单位密度和场景大小
- 视野范围
	- 十字链表 : 自适应
	- 九宫格 : 固定
- 数据结构维护
	- 十字链表适合小幅度
	- 九宫格 : 直接设定

[《群体寻路与避障》Flow Field流场寻路算法如何实现群体寻路_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV12bzZY2EfA?spm_id_from=333.788.videopod.sections&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

1. 热力图
2. 向量场
3. 向量插值

其他问题
1. 转弯效果
2. 单位间碰撞
3. 单位障碍碰撞