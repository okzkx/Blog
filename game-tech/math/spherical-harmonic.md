# 球谐函数

- [简单理解spherical harmonic lighting（球谐光照）_leon](https://blog.csdn.net/leonwei/article/details/78269765)
- [球谐函数介绍（Spherical Harmonics） - 知乎](https://zhuanlan.zhihu.com/p/351289217)
- [由cubemap生成lightprobe - 知乎](https://zhuanlan.zhihu.com/p/471470300?)
- [球谐可视化（sh visualization） - 知乎](https://zhuanlan.zhihu.com/p/466774017)
- [由cubemap生成lightprobe - 知乎](https://zhuanlan.zhihu.com/p/471470300)
- [Unity渲染编程(灯光篇)【第一卷：Spherical Harmonics Lighting】 - 知乎](https://zhuanlan.zhihu.com/p/99316141)
- [使用Compute Shader计算球谐全局光照 | ZZNEWCLEAR13](https://zznewclear13.github.io/posts/calculate-spherical-harmonics-using-compute-shader/)
- [球谐光照笔记（旋转篇） - 知乎](https://zhuanlan.zhihu.com/p/140421707)
- [一种简易的旋转球谐函数系数的方法 - 知乎](https://zhuanlan.zhihu.com/p/51267461)

$$
Y_{lm} = \int_{sphere} f(x, y, z) Y_{lm}(x, y, z) d\Omega
$$
$$
f(x, y, z) = \sum_{l=0}^{\infty} \sum_{m=-l}^{l} Y_{lm} Y_{lm}(x, y, z)
$$
$$
Y_lm(θ, φ) = N_lm P_l^m(cos(θ)) e^(imφ)
$$