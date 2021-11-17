# 蚂蚁模拟:

#### 蚁群算法:

- 找出最短路径
- 算法规则:
- 1. 蚂蚁会大概率沿着信息素浓度最高的地方行走,
- 2. 蚂蚁离开食物或者巢穴后,走的路径越远释放的信息素越少.
- 3. 信息素随时间增长会缓慢消失

#### GpuInstancing:

- 循环调用 Graphics.DrawMeshInstanced ,大量绘制同种材质的 Mesh,让大量蚂蚁,障碍物,合批绘制。
- (使用循环而不使用 command buffer 了?)

#### Matrix4x4

- 4x4矩阵, Matrix4x4, 为蚂蚁 mesh 的绘制提供 TRS

#### FixedUpdate:

- 蚂蚁移动相关的运算(物理计算),
- 信息素颜色相关计算(颜色计算)

#### Update

- 绘制网格
