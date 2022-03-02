# 配合 Unity 建模流程

## 教程

[十分钟演示 Blender 游戏角色建模并导入 Unity 的全流程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1XA4117737?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click)

[Blender和Unity之间导入导出那些事儿 - 知乎](https://zhuanlan.zhihu.com/p/348423432?ivk_sa=1024320u)

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
  - 给 Mesh 刷骨骼顶点权重
- Animations
  - 记录骨骼关键帧位置
- Export
  - 直接导出 Blender 文件到 Unity 工程里可以直接使用
