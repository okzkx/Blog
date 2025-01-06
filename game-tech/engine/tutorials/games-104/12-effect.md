# 粒子系统和音效系统

- Position
- Velocity
- Size
- Color
- Lifetime

## 组成

- Emitter
- System
  - 由多个 Emitter 组成
- Spawn Position
  - Single position
  - area
  - base on mesh
- Swapn Mode
  - 随机爆发
  - 喷泉
- Simulate
  - Particle 在空间上的行为，可以使用流场
  - 在时间上的行为，设置当前的速度，力，等
  - 颜色和尺寸，黑体辐射，火焰颜色，烟从浓密到稀薄
  - 和环境有互动，可以被障碍物阻挡反弹，需要有高效的物理碰撞检测
- Particle Type
  - Billbaord，尺寸比较大的话需要内容要变化，不然会被看出来是面片
  - Mesh， 要做出随机感
  - Ribbon，光带，通过控制点来转弯，通常用 Catmull 曲线，保证圆滑并且通过所有控制点
- Rendering
  - 大量的半透明物体排序
  - 按全局排序 Global，绘制开销大
  - 按照发射器排序，Per emitter，简单但效果不好
- Full-Resolution
  - 性能杀手
  - 半透明物体所有都要渲染，开销大
  - 半透明物体在更低的分辨率上进行渲染，然后和不透明物体在正常分辨率上混合

## Processing Particle on GPU


ＧＰＵ　里生成粒子，所有步骤都可以在 ＧＰＵ　里实现

数据结构

- Partical Pool
- Dead list
- Alive list

其他操作

* View frustum culling
* Depth compare
* Sort
* Swap alive list
* 集群性的运算都可以在 compute shader 里写

归并排序

- GPU 适合用多线程做归并排序
- 单线程是 nlogn，多线程是 logn

碰撞

- 将粒子和 Depth buffer 做碰撞

## Advance particle

### 使用粒子系统表示人群

* 使用 mesh particle
* 只用一个骨骼
* 动画对于顶点移动烘培成为纹理
* particle 使用状态机

### Navigation Texture

- 对于地图烘培导向图，流场

和环境进行交互，与游戏逻辑交互

### Unreal Niagara System

- Modules
- Emitter
- System

## Audio

音效

- player
- realism
- atmosphere

声音对情绪的影响

声音的元素

- Volume 音量，纵波，单位面积对耳膜的压强，单位分贝
- Pitch 频率
- Timbre 音色

降噪耳机

- 反向波抵消

PCM

- 将波进行存储，压缩

Sampling

- 两倍频率的采样就能无损
- 尽量高效果好

Quantizing

Audio Format

- wav
- flac
- mp3 只支持立体声，有专利保护
- ogg 最多用，有损，高压缩率，多声道
