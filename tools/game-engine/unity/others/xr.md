[AR中的三维视觉算法原理与应用精讲](https://mp.weixin.qq.com/s/AYNUTHjvLBmqSKyOGlEofA)

**想放一个虚拟物体在现实环境当中需要做什么**

1. 模型和设备之间的相对关系是需要设定出来，就是空间定位
2. 环境感知，这个虚拟物体是放在地面还是空中，以及在哪个角度看会挡住它
3. 跟宠物有小的互动，比如说用手势、眼动和手柄，这就是人机交互

![](https://mmbiz.qpic.cn/sz_mmbiz_png/NYLZoOxGjYE8Qcqb6yHW61rCGjGXB0doZB0cGYYwcMDKfv98cfyyc3dOpo5EOrLPTFDiajy5oaYa604Aw9K2E2w/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

SLAM 中文全称是即时定位与建图

输入的是无人车、VR、AR 一连串的图像序列，通过序列就可以得到相机在空中运动的轨迹

![](https://mmbiz.qpic.cn/sz_mmbiz_png/NYLZoOxGjYE8Qcqb6yHW61rCGjGXB0donV0uSKNjlz3OkTfmxwhV3EoibBkmxqt2EqZgkeO38QO0LyRyZAoGgJg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![](https://mmbiz.qpic.cn/sz_mmbiz_png/NYLZoOxGjYE8Qcqb6yHW61rCGjGXB0douia3t0ZMuboBFQHTiby5hbMZJkydSxKicXlmekOaOHVeictB7UYCyytrJQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

提取图像当中的特征，比如说一些焦点、屋顶和弧线，把它进行识别和保存下来。通过提取这些特征点联系起不同视角图，构建一个关系。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/NYLZoOxGjYE8Qcqb6yHW61rCGjGXB0doicwqHrMwxuoMWbNYkPWkcqh8VWJSPuQArTD98cyhN3LHPS3jCUozRIA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


- 不同时刻不同视角的图像, 
- 三维空间的点和图像和光心连成一个直线，另外一个视角也是构成一条直线，很明显它们是同一个点，意味着这两条直线会相交。
- 通过这种相交我们会构建几何上面的方程约束，一个点可以构件一个约束，N 个点可以构建 N 个约束*

![](https://mmbiz.qpic.cn/sz_mmbiz_png/NYLZoOxGjYE8Qcqb6yHW61rCGjGXB0do9EITqUy0xvFORULrpk9YWJk6ichkN9lXxU7lmZKrdXOmq3jmB3VZsxw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1. 根据一些图像序列**检测它的特征点**
2. 对特征点不同时刻不同角度**进行关联**
3. **构成几何约束**
4. 通过几何约束就能构建非常大的**误差函数**
5. 以此**进行优化**
6. 优化完之后就能得到想要的建筑外廓和相对的姿态
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NYLZoOxGjYE8Qcqb6yHW61rCGjGXB0doARgKGrXBuyISiaDMLH9vicbanIfAmFRQwFOVy8SlbOKanqHy0YKIiayTg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**地图已知的定位“视觉 Vision Position System”**
根据重建后的建模的特征点进行匹配