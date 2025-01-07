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




