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
