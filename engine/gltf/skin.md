# 皮肤


```
jointMatrix[j] =  inverse(globalTransform) * globalJointTransform[j] * inverseBindMatrix[j]
```

* jointMatrix[j] = 编号为 j 的骨骼在当前动作下相对于初始位置的位移矩阵，每帧计算
* inverseBindMatrix ：模型原点（坐标零点）相对于骨骼在初始位置的位移矩阵，在模型导入时就已经确定
* globalJointTransform ：骨骼在当前动作下相对于模型原点（坐标零点）的位移矩阵，每帧通过动画更新
* inverse(globalTransform)：骨骼在当前动作下相对于初始位置的位移矩阵，由世界空间变换到模型空间

```
 mat4 skinMatrix = 
  a_weight.x * u_jointMatrix[int(a_joint.x)] +
  a_weight.y * u_jointMatrix[int(a_joint.y)] +
  a_weight.z * u_jointMatrix[int(a_joint.z)] +
  a_weight.w * u_jointMatrix[int(a_joint.w)];
```
