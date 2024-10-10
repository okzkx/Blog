# 网络架构基础 上

- Network Synchronization
- Reliability
  - Network latency
  - Drop and Reconnect
- Security
  - Cheat
  - Account hacked
- Diversities
  - 跨平台
  - 热更新
- Complexities 复杂性

![image.png](online-outline.png)

#### OSI Model

![image.png](osi.png)

Network Socket-based Communication

### Transmissin Control Protocol (TCP)

安全可靠的长连接

Tcp congestion control

使用 congestion window (CWND) 进行流量控制

### User Datagram Protocol (UDP)

端到端协议，不安全的短连接

### Compare

![image.png](network-protocol-compare.png)

UDP 快速实时响应，其他用 TCP

### Reliable UDP

TCP is not time critical

基于 udp 构建可靠链接，自己写或者使用第三方

![image.png](need.png)

如何建立可靠通讯

![image.png](udp-custom.png)

1. 确认收到
2. 什么没有收到
3. 序列号
4. 过时

Sliding window protocol

滑动窗口协议

一次发送窗口内的所有包体，有包没有回应就重传该窗口

### Forward Error Correction (FEC)

xor-fec

![image.png](xor-fec.png)

异或出唯一缺失的包，并重传

#### Reed-Solomon Codes 多包恢复

![image.png](reed-solomon.png)

构建 B 矩阵

B 矩阵任意抽调 3 行，都是可逆矩阵

通过冗余数据 C，和抽调后的逆矩阵 B'，可以恢复原始信息

![image.png](custom-udp.png)

## Clock Synchronization

时钟同步

RTT

![image.png](RTT.png)

### Network Time Protocol

NTP Algorithm

![image.png](ntp-algorithm.png)

多次 RTT，丢弃不可靠 RTT，计算 Offset，对其时钟

### Remote Procedure Call RPC

彼此通信，OSM 模型，Socket 不适合写游戏业务逻辑

远程调用方法适合游戏

应用层写起来简单

#### Interface Definition Language（IDL）

Protobuf ：Google 的数据定义格式

#### RPC Stubs

票据存根，客户端和服务端建立连接后，注册好所有的 RPC

#### Stub compiler

把 IDL 编译成为对应的语言传输结构
