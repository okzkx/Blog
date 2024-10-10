# Advanced Animation Technology

如何把 DCC 中的动画放在游戏引擎里顺利播放

## Animation Blending

动画混合

对于每个骨骼线性插值

- Align Blend Timeline.人物走路和跑步的步频要一致，频率一致

### Blend Space

- 1D Blend Space，多个动画动作插值出当前动作
- 2D Blend Space，不能把所有动画都计算
  - Delaunay Triangulation ，2D 空间的三角划分
  - 使用重心坐标采样三个点
- ![image.png](image.png)

### Applauding on Different Poses

* 部分混合，需要融合两个动画的不同部分
* 需要骨骼 mask 分层
* ![image.png](image1.png)

### Additive Blending

* Blending 做完后再加一个旋转分量，比如人头朝向相机，
* 需要谨慎使用

## ASM （Animation State machine）

动画状态机

一个起跳可以拆成三个动画

- Jump begin
- In the air
- Jump end

状态机元素

- node 链
- blendspace
- clip
- 最终产出一个骨骼姿势

动画切换之间要一个 blend 过度，大约 0.2 秒

- no cross fade
- smooth transition
- frozen transition

Cross Fade Curve

- linear
- eazyin, eazyout

Animation State Machin in Unreal

- Action State Machine
- Transition Rule
- State Node with Blend Space
- 动画节点不止是 Clip，还可以用动画蓝图

Layered ASM 多层动画状态机

![image.png](image2.png)

手部执行攻击动画，下身执行移动动画

## Animation Blend Tree

动画节点树，将所有 Clip 按照权重混合

Unreal Animation Blueprint

![image.png](blue-print.png)

需要定义动画树的控制变量

- 各个动画 Clip 通过中间节点混合
- 通过外部变量来控制中间节点的功能

## IK

反向动力学

- IK (Inverse Kinematics)
- FK (Forward Kinematics)

### Two Bones IK

3D space

![image.png](two-bones-ik.png)

Complexity of Multi-Joint IK Solving

### Constraints of joints

![image.png](constraints-of-joints.png)

### Heuristics Algorithm

- CCD （Cyclic Coordinate Decent） 迭代
- FABRIK (Forward And Backward Reaching Inverse Kinematics)

Multiple End-Efectors

多控制点

##### Jacobian Matrix 雅可比矩阵

approaching to target step

Other IK

- Physics-based
- PBD
- Fullboody

IK is still Challenge

- Self collisioin avoidace
- IK with predication during moving
- Natural human behaviour

## Facial Animation

Facial Action Coding System

- 表情系统，46 个表情单元，Action Units Combination
- 28 Core Action Unit，28 个核心表情单元，删去了对称的表情
- Key Pose Blending，简单的对表情过度进行线性叠加，称为 Morph Target Animation，
- 大的动作，眼球，捏脸，用骨骼动画

表情动画系统

- Morph Target Animation，
- Bone Animation
- UV Texture Facial Animation
- Muscle Model Animation

## Shade Animation Among Charactor

- 动画重定向
  - source charactor -> target animation
  - 需要应用的是动画相对父节点的位移，local space trans
- Align movement by pelvis height
  - 需要骨骼动画频率一致
  - 两个角色体型差很多的情况下
- Lock Feet by IK after Retargeting
  - 使用 IK 把脚锁在地面
- Retargeting with Different Skeleton Hierarchy
  - 骨骼无法一一对应
  - Easy Solution
  - Morph Animation Retargeting problem
