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
