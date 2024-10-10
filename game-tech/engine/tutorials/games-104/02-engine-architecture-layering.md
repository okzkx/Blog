# 第二节：引擎架构分层

Layered Architecture of Game Engine

- Sea of Code

## 游戏引擎分层简介

### Layers

- Tool Layer
- Function Layer
	- Physics
	- Animation
	- rendering
	- Camera HUD Input
	- Script FSM AI
- Resource Layer
	- Data and Files
- Core Layer
	- Swiss Knife of Game Engine
- Platform Layer
	- Launch on Different Platforms
	- Operation Systems
	- Platform File System
	- Platform SDK
- 3rd Party Libraries

5 + 1 层次涵盖了几乎所有引擎功能

### Resource Layer

- How to Access My Data
  - Resource -> asset
  - Offline Resource Importing
  - 引擎的核心功能之一是资源关联![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201728116.png)
- Rentime Asset Manager![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201733932.png)
- Manage Asset Life Cycle ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201737502.png)
	- 生命周期管理 
	- 延时加载

### Function Layer

- How to Make the world alive![image-20220330202503555](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201740550.png)
- 每个 Tick 进行一遍所有的系统执行
- 普朗克时间就是现实世界的 Tick

#### Dive into Tick

- tick main
	- tick logic 先世界模拟
	- tick render 再进行渲染
![image-20220330202900803](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201745848.png)

#### Heavy - duty Hotchpotch

- Function Layer provides major function modules for the game engine
- Game Loop updates the systems periodically
- Blur the boundary between engine and game

#### Multi-Threading

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201751797.png)

难点在 Dependency 管理

### Core Layer

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201815930.png)

#### Math Library

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201756685.png)

- math efficiency
- 倒数平方根很慢，quake 3 有一个近似算的很快
- SIMD ：专门做向量运算的组件，可以同时进行 4 次相加

#### Data Structure and Container

![image-20220330204838371](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201808966.png)

- 需要重新写游戏引擎手动控制的容器数据结构，因为语言自带的托管内存的数据结构所占据的内存会产生很多空洞，内存的消耗是不可控的。
- 出现很多内存碎片，访问效率很低。

#### Memory Management 内存管理

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201812620.png)

- 现代计算机的内存数据读写还是图灵机方式
- 需要有高效的内存分配模式
	- Put data together 数据在一块
	- Access data in order 数据按顺序读写
	- Allocate and de-allocate as a block 按照块状分配内存

### Platform Layer

#### Target on Different Platform

需要做到平台无关性

- File system
- Graphic api , Render Hardware Interface (RHI) 重新定义一套渲染接口
	- Transparent different GPU architectures and SDK
	- Automatic optimization of target platforms
- Hardware Architecture 特定机器优化

### Tool Layer

#### Allow Anyone to Create Game

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201822798.png)


#### Manual Editors

- Level Editor
- Logical Editor
- Shader Editor
- Animation Editor
- UI Editor
- Flexible of coding languagesz

#### Digital Content Creation

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201825712.png)

- 数字资产生产工具
- Asset conditioning pipeline，把资产导入引擎
- tool chains，工具链

## Why Layered Architecture

- Decoupling and Reducing Complexity
	- Lower layers are independent from upper layers
	- Upper layers don't know how lower layers are implemented
- Response for Evolving Demands
	- Upper layers evolve fast, but lower layers are stable

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201828040.png)


## Mini Engine : Pilot

![image-20220330211847914](image-20220330211847914.png)

### Simple ECS framework

![image-20220330212201585](image-20220330212201585.png)

### Takeaways

- Engine is designed with a layered architecture
- Lower levels are more stable and upper levels are more customizable
- Virtual world is composed by a set of clocks - ticks
