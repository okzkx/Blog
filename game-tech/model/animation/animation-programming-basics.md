- [Animation Programming Basics - YouTube](https://www.youtube.com/watch?v=Jkv0pbp0ckQ)
## Overview

- Skinning
- Skeletons
- Animations
- Root Motion
- Synchronization
- Events
- Motion warping

## Skeletons and Skinning

## Skinning : Deformation Boness

## Skeletons

有用户可修改的骨骼和只有程序可以修改的骨骼

- Core Skeleton
	- 需要制作几种
	- Average Male
	- Large Female
	- Bipedal Mech
- Gamplay bones
	- 不用渲染
	- Attachement bones (holsters, weapon bones )
	- Anchor bones (used to anchor characters in the environment for actions)
	- IK Target / Offsets
- Procedural Bones
	- 辅助渲染

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111036629.png)

Bind Pose vs Reference Pose

- bind pose
	- mesh skeleton default pose 
	- skin weight
- reference pose
	- animation skeleton default pose
	- 不常用

两种 Pose 通常是不一样的

## Multi-part Character

角色的骨骼可以控制多个 Mesh 而不只是自己的 Skin mesh，比如身上的衣服

## Skeleton Best Practices

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111114774.png)


![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111125913.png)

Mistake
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111135325.png)

最重要的是分离动画骨骼和渲染骨骼，动画骨骼控制骨骼当前位置，通过两者共同的核心骨骼生成当前的渲染骨骼位置，传给 Gpu 渲染 

衣服的动画要和躯干的分离，单独依赖骨骼

## Animation

动画数据导入和采样

用关键帧数据存在曲线

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111443645.png)

数据采样

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111440923.png)

## Animation Sampling

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111504920.png)


## Blending

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111508406.png)

###  Coordinate Spaces

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111510825.png)

数据存储在 Parent Space，空间变换到 Character 或 World Space 后交给渲染系统
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111519482.png)

###  Pose API

### Interpolative and Additive

Interpolative Blending

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111527502.png)

Additive Blending

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111528435.png)

Bone Mask

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111530733.png)

Global Blend

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111534648.png)

## Root Motion

Multiple names

- Ground
- Displacement
- Root
-  Character

Character in Games

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111612394.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111613748.png)

Sampling Root Motion

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111631932.png)

More use

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111636809.png)

Best Practice

角色的 Root 最好和动画的 Root 分开

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111652077.png)

- Projected root 通常会抖动
- 而 Authored root 是直线

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111659594.png)
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111700733.png)

起点到终点需要保持 z 轴一致，方向一致
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111704828.png)

## Code vs Anim Driven

Code Driven

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111736893.png)

Anim Driven

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111739995.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111757201.png)

## Event and Synchronization

Events

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111801796.png)

Event Sampling

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111804281.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111806085.png)

Event Consumers

每帧收集所有的事件给 Event Consumer 消费

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111812412.png)

Event User

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307111822574.png)

## Synchronization

动画同步

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121530639.png)

动画速度同步

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121532841.png)

Sync Track
对 Track 进行同步，可以在最后一个 Sync 进行同步
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121538402.png)


## Motion Warping

apply root motion，move root to target point

### Basic Procedural Adjustments
- Preplace / Add rotation
- Replace / Add transform

### Warping

- 弯曲运动轨迹

弯曲关键点

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121124376.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121510046.png)

### Sampling Warped Root Motion

可能在路径上有误差

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121520898.png)


Wrap 可能到中途需要改变目标
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121518639.png)

角色旋转时脚步通常用 角度融合

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307121528658.png)

