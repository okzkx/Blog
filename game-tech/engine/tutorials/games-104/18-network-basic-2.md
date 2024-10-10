# 网络架构基础 下

## Network topology

- Original Peer-to-Peer
- P2P with Host Server
- Dedicated Server

#### When RTT is too high

在各国建立各自的服务器，然后服务器之间通过专线高速连接

## Game Synchronization

游戏同步

### 同步方案

- Snapshot
- Lockstep
- State

#### Snapshot

快照同步

客户端只作为服务端的输入输出，逻辑全部在服务端做

1. 服务端希望帧率尽可能低，客户端希望尽可能高，所以客户端可以做插值补间
2. 将全体快照更换为增量快照，减少带宽
3. 逻辑清晰，代码清晰
4. 客户端算力浪费
5. 传输数据量大

#### Lockstep Origin

帧同步，类似回合制游戏，服务器同步所有客户都安，每帧收集所有客户端的输入，再统一分发给所有的客户端，让他们各自进行模拟。
![image.png](lookstep.png))

1. 每帧会被最慢的玩家卡到
2. Bucket Synchronization，设定最长的等待时间
3. Trade-off consistenscy and Interactiveity maintenance，在实时和真实之间做平衡
4. Deterministic Difficulties 确定性，需要保证在不停的平台上对于同样输入，输出结果相同

#### 确定性

1. Float point numbers
2. 只需要游戏逻辑，状态具有确定性
3. 随机数

Tracing and Debugging

需要存储快照,方便调试,从哪一帧开始出错

#### Lag and Delay

用缓存解决网络抖动问题

#### 逻辑表现分离

逻辑帧慢,渲染帧块,网络延时要比刷新率慢很多

#### Vsync

如果屏幕刷新率比 front buffer 更新的速度更快,那么容易出现屏幕撕裂

Reconnection Problem

#### 断线重连

本地快照,根据到当前帧的输入来算到当前状态,Quick Catch Up

服务端快照,提供给客户端使用,观战系统

#### Replay

回放系统

#### Lockstep Cheating Issues

反作弊

1. 状态修改挂:由于没有服务端权威状态,所以需要比较出所有客户端中唯一不同的快照,认为他是作弊
2. 状态查看挂:由插件看到玩家不应该知道的状态信息

#### Lockstep Summary

- 可以做操作,打击感好的对战游戏
- 可以做录屏
- 全图挂
- 确定性难做

## State Sync

状态同步

- 只同步关键事件,关键行为,场景对象关键状态
- 服务端具有权威状态

术语

- Authorized(1P) 本地
- Server 服务端
- Replicated(3P) 其他端

射击示例

- 同步自己开火行为
- 同步命中事件
- 同步所有对象数据

### Dumb Client Problem

本地角色的移动需要服务端同步下来造成延时
![image.png](dumb-client-problem.png))

#### client side prediction

由客户端预测来解决,守望先锋预测半个 RTT 的帧数,

在收到服务端状态的同步时,如果状态不对,需要全部回滚重算整个 RTT 的状态,缓慢插值到正确位置

#### Packet Loss 丢包问题

和解决帧同步 Leg 一样,服务端也缓存客户端的输入,防止网络波动,同时还不会影响实时性
![image.png](net-sync-vs.png))
