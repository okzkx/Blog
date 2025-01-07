# 配合 Unity 建模流程


## 教程

- [Blender - Export FBX to Unity 的一些配置 - 导出模型后，x 轴旋转了 90 度，和缩放不对的问题_blender导出fbx后模型变了-CSDN博客](https://blog.csdn.net/linjf520/article/details/124708277)
- [十分钟演示 Blender 游戏角色建模并导入 Unity 的全流程](https://www.bilibili.com/video/BV1XA4117737?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click)
- [Blender和Unity之间导入导出那些事儿 - 知乎](https://zhuanlan.zhihu.com/p/348423432?ivk_sa=1024320u)

## 根据对应步骤

- Sketch
  - 速写阶段需要主视图和侧视图
  - 主视图要对称
  - 导入速写图片需要调整大小到人物 1.75
- 3D model
  - 使用镜面和曲面细分修改器快速创建
  - 角色包围盒要根据肌肉块分布
- Texturing
  - 给予不同部位不同的材质
  - 展 UV
  - 多个材质烘培到同个纹理
- Rigging
  - 创建骨骼开启 x 轴对称
  - 骨骼去掉多余部位
  - 骨骼设置为 Mesh 父物体
  - 先选骨骼再选 Mesh，给 Mesh 刷骨骼顶点权重
- Animations
  - 在时间轴上记录骨骼关键帧位置，让动画时间包括所有关键帧
- Export
  - 直接导出 Blender 文件到 Unity 工程里
  - 在 Unity 里分割 Blender 文件动画
