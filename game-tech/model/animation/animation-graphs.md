- [Animation Graphs - YouTube](https://www.youtube.com/watch?v=R-T3Mk5oDHI&t=1s)
- 
## Overview
How to implement an animation graph runtime

- HFSM based
- Highly flexible/extensible
- High Performance
- Physics Inntegration
- Pose LOD


## GamePlay animation

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121620417.png)


animation system 职责

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121625420.png)

Animation Graph

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121628587.png)

Blend Tree

角色当前的动画是所有做好的动画的加权叠加

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121629478.png)

State Machine
用状态机定义在角色各状态时各个动画的权重

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121632816.png)

Hierarchical State Machines

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121639100.png)

State machine
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121646703.png)

Animation graph 是一个黑盒

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121648790.png)

### Graph nodes

- Pose Node
- Value Node

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121716818.png)

Value Node
- lazy
- screenshot
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121720024.png)

Target

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121723824.png)

Control parameters

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121724537.png)

Virtual Parameters

类似函数，可以 cache 输出

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121726918.png)

Pose Node

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121729066.png)
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121744927.png)

Source Node

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121746430.png)

Animation data sets

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121747292.png)

Parametric Blending

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131004345.png)

KRG Example

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131008636.png)

## State machine

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131011119.png)

Transaction node

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131011586.png)

Transition Conduits

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131019287.png)

Jump example

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131022413.png)

Forced Transition

## Layers

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131103065.png)

Layer State Data

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131106311.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131140915.png)

State Machine Tooling

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131357432.png)

Entry State Override

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131359878.png)

Slide state Machine

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131400971.png)

Global State Transition
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131408525.png)

Gameplay / Animation Communication

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131413357.png)

State Event

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131419150.png)
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131424570.png)

### Best practices

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131427904.png)

### Motion Matching

当前在作为解决方案并不理想

## Post Tasks

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131432300.png)


![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131432089.png)


Post buffer pool

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131434186.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131436123.png)

## IK + Physics

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131455882.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131517790.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131603449.png)

Interleaved IK

Rowered Ragdolls

External Graphs

Preformance

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131618612.png)

Memory Layout

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307131622302.png)

Task List LOD