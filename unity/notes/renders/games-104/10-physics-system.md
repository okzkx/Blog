# Physics System

- Physical Intuition
- Dynamic Environment
- Realistic Interaction
- Artistic

Outline of Physics System

![image.png](assets/outline-of-phyusics-system.png)

## Basic concepts

Actor

- Static 静态
- Dynamic 动态
- Trigger 不影响物理系统，和逻辑交互
- Kinematic 位置或者速度的强行设置，不符合物理约束

Actor Shapes

![image.png](assets/actor-shapes.png)

- 球、胶囊体、box 是最省的
- 精细的结构可以用多个 box 近似
- Convex 可以用来模拟碎片运动
- Triangle Mesh 和 Height Field 不能用来作为动态物体、

### Wrap Ojbets with Physics Shapes

用简单的 Shape 包裹复杂物体

### Shape Properties 属性

- Mass and Density

  - 质量和密度
  - Gomboc Shape 干波体，只有一个平衡点
- Center of Mass 质心
- Friction and Restitution 摩擦和弹性，物理材质定义的

### Force 力

- 长期的力
  - Gravity
  - Drag
  - Friction
- Impulse 冲量

### Movement 移动

- 牛顿第一定律

![image.png](assets/newton-1st.png)

- 牛顿第二定律

![image.png](assets/newton-2st.png)

Movement under Varying Force

![image.png](assets/movement-vary-force.png)+

Solar System movement

![image.png](assets/movem,ent.png)

- 显式欧拉积分 Explicit Euler Method，计算没法收敛
- 隐式欧拉积分是稳定解
- 半隐式欧拉积分,结果最稳定

![image.png](assets/Semi-implicit-eular.png)

## Rigid Body dynamic

假设所有的对象都是质点,质点动力学

![image.png](assets/practical-dynamic.png)

刚体动力学，

![image.png](assets/rigibody-dynamic.png)

Rotational Inertia 转动惯量

![image.png](assets/rotational.png)

Angular momenturn 角动量守恒

![image.png](assets/angular-momenturn.png)

力臂越长，转动惯量越大，角速度越小

Torque 力矩

![image.png](assets/torque.png)

![image.png](assets/angular-linear.png)


Colliction Detection

碰撞检测

Two Phases

- Broad phase
  - Find intersectied rigid body AABBs
  - Potentianl overlapped rigid body pairs
- Narrow phase
  - Detect overlapping precisely
  - Generate contact information

![image.png](assets/two-phases.png)


#### Broad Phase

- BVH Tree 树状结构存储 AABB ，更新成本低，一个物体移动只要更新一到两个节点
- Sort and Sweep ：对各个轴向排序，判断是否在某个轴上是否重叠。更新成本更低。

#### 基础的碰撞检测算法使用

##### Minkowski Difference-based Methods

- Minkowski Sum 闵可夫和
- Minkowski Difference 闵可夫减法



- 闵可夫和和减生成的图形都是闭包
- 闵可夫减所表示的图形如果有过原点，说明这两个图形存在交点
  - GJK 算法，迭代判断三角形是否包括原点
- SAT 算法，两个凸多面体不相交，一定沿着一个凸多面体一条边，另外一个多面体所有顶点在另外一侧。
  - 3D 需要使用面来判断，还要面中间的边判断
