# 玩法

Street Fighter Attack Feedback

- Animation
- Effect
- UI
- Audio
- Interface Devices

#### Publish-subscribe Pattern

发行者，订阅者，事件模式

事件定义可以策划用工具，通过自动生成。

#### 回调函数，激活函数

对象生命周期和回调函数的安全性

消息分发

Immediate 立即执行

- 会出现一帧做的事情太多，短时逻辑和长时逻辑混在一起
- 无法并行

delay 延时执行

- event queue
- 需要有小型的序列化和反序列化
- 循环队列 ring buffer
- Batching 需要有多个 event queue，管理不同类型的 event
- 执行顺序很难保证
- 可能会到下一帧才执行

## Logic Programming 游戏逻辑

热更新

脚本语言

- 热更新方便
- 简单易上手
- 解释性
- 不会造成程序崩溃
- 脚本语言对应的虚拟机执行脚本语言编译出的 byte code

游戏中的对象 GameObject 归脚本语言管理还是引擎管理

一般是脚本语言管理 GameOjbect，C# 是通过 GC 来清除 GameObject

Jit

just in time ，第一次运行的时候边解释执行边编译

aot，第一次执行前完全编译

两者速度差不多

## Visual Scripting

可视化编程系统，

没有程序背景的创作者，策划和美术

-
- Variable
- Expression
- Statement
- Control Flow
- Function
- Class

### Issues

在工业化应用的时候

- 没法 merge，很难合作
- 可读性不好，连线很乱

### Script and Graph are Twins

可以直接把图编译成为脚本

## 3C

Charactor，Control，Camera

角色，控制，相机

体验核心

- Charactor
  - 移动
  - 数值
  - 表现
- Control
  - 各种输入设备
  - 输入设备数据转化为游戏内的信号
  - 与玩家的交互
    - 摄像机 zoom in
    - 准心吸附 ：Aim assist
  - 反馈 ：Feed back，主动媒体
  - Chords：组合键
  - Key Sequences：按键顺序
- Camera
  - PoV，Fov
  - Camera Blending
    - 弹簧臂，保证摄像机别穿墙
    - 相机聚焦
  - Camera Track
  - Scene camera
  - Camera shake
  - Camera switch
  - Subjective Feeling ：主观感受
