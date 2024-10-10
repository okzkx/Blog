# 网络架构进阶 下

## MMOG Network Architecture

基础 MMO 架构

Game Sub-System

MMO Architecture

![image.png](mmo-architecture.png)

- Services of Link Layer : Gate way 账号服务器
- Lobby ：大厅，做一个游戏前的等待缓冲
- Character Server : 角色面板
- Trading System : 交易系统
- Socail System : 社交，聊天，邮件系统
- Matchmaking : 匹配系统
- Data Storage : 数据存储
  - 表型的关系型数据库 mysql
  - 键值非关系型数据库 mongoDB ，大存储，低查询效率
  - 内存数据库 redis : In-Memory Data Storage，内存中保存，效率高，
- Player Number Growth
  - Distributed System 分布式系统
  - 解决竞争关系
  - 幂等性
  - 复杂系统的震荡
  - 数据一致性
  - Load Balance 负载均衡
    - Consistent Hashing 一致性哈希![image.png](consistent-hashing.png)
  - Servers Management
    - 服务发现 etcd
    - 其他服务能够监视别的服务的状态

## Bandwidth Optimization 带宽优化

#### 数据量过大会产生拥塞，产生延时

- n : 角色数量
- f : 更新频率
- s : 包体大小

#### 数据压缩 Data Compression

1. 浮点数转定点数
2. 位置信息压缩 + 地图分块
3. 固定朝向的优化

#### AOI 划分相关区域 Relevance area of interest

只同步 zone 区域的数据

#####  

##### Spatial-Grid 画格子法

以一百米的单位分格子，角色的感知单位为 100 米。

角色在跨越 zone 时通知有关的所有客户端。

空间换时间

##### 十字链表法

在 x 和 y 轴对 zone 内的角色排序

#### 同步频率

越近的角色同步频率越快

## Anti-Cheat

反作弊

- 客户端代码注入
- 客户端代码破解
- 系统 SDK 破解
- 模拟键鼠输入
- 劫持网络包

### Obfuscating Memory

修改敏感内存

客户端加壳，使用 Packer 进程修改执行方法

内存混淆

修改本地的文件资源，本地文件哈希值校验

网络包的截获和修改

对称加密，非对称加密。

非对称加密 + 一次性对称加密

软件注入，钩子

Valve Anti-Cheat， Easy Anti-Cheat

AI Cheat ：AI 图像识别后模拟键鼠

AI 识别作弊

Overwatch 监察者：靠人力判断

扫描的已知的外挂软件

## Build a Scalable World

开放世界构建

![image.png](scalable-game-server.png)

1. zoning 平面空间分割
2. 副本
3. 分线

### Zoning - Seamless Zones

不平均的四叉树划分

Ghost ，A 区域的人能在 B 区域被看到，需要在 B 中创建处于 A 位置的 Ghost

阈值：防止反复跨越边界线
