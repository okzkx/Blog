# 噪声

- [Pixel-Shader 之 「噪声」](https://zhuanlan.zhihu.com/p/41076746)
- [shader 利用FBM， noise 模拟海洋波浪](https://blog.csdn.net/lck8989/article/details/103863517)
- [（十七）unity shader之——————高级纹理之程序纹理](https://blog.csdn.net/cgy56191948/article/details/101641140)
- [Unity Shader学习：噪声noise](https://blog.csdn.net/qq_36107199/article/details/87191348)
- [《数学可视化》——基础知识（7）——随机与噪声](https://www.bilibili.com/video/BV16d4y1f7qs/?spm_id_from=333.999.0.0&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202312111824539.png)


## 介绍

程序生成的随机数列

噪声的种类和产生方式

![](https://pic3.zhimg.com/80/v2-4123cdc91e83db41bedace9d5067490e_720w.jpg)

### 梯度噪声

将坐标系划分成一块一块的晶格之后在晶格的每个顶点处生成一个随机的梯度（可以理解成方向向量），然后在计算噪声的时候会综合计算该噪声所在的晶格的顶点上的方向向量（图中绿色箭头）进行聚合计算（可以理解成加权计算合力）

![](https://pic4.zhimg.com/80/v2-3bcae2d2e63939da252e863a8e10720f_720w.webp)

#### Perlin Noise

```
// reference shadertoy
float perlinNoise(vec2 p) {
    vec2 pi = floor(p);
    vec2 pf = fract(p);

    vec2 w = pf * pf * (3.0 - 2.0 * pf);

    return mix(mix(dot(hash22(pi + vec2(0.0, 0.0)), pf - vec2(0.0, 0.0)), 
                   dot(hash22(pi + vec2(1.0, 0.0)), pf - vec2(1.0, 0.0)), w.x), 
               mix(dot(hash22(pi + vec2(0.0, 1.0)), pf - vec2(0.0, 1.0)), 
                   dot(hash22(pi + vec2(1.0, 1.0)), pf - vec2(1.0, 1.0)), w.x),
               w.y);
}
```

![](https://pic2.zhimg.com/80/v2-3c5f330dc2cc4a7ede01dc78b829e8dd_720w.webp)

Perlin Noise 加 fbm（分型布朗运动）

```
const mat2 mtx = mat2( 0.80,  0.60, -0.60,  0.80 );
float fbm6( vec2 p ) {
  float f = 0.0;

  f += 0.500000*perlinNoise( p ); p = mtx*p*2.02;
  f += 0.250000*perlinNoise( p ); p = mtx*p*2.03;
  f += 0.125000*perlinNoise( p ); p = mtx*p*2.01;
  f += 0.062500*perlinNoise( p ); p = mtx*p*2.04;
  f += 0.031250*perlinNoise( p ); p = mtx*p*2.01;
  f += 0.015625*perlinNoise( p );

  return f/0.96875;
}
```

#### Value Noise

```
float valueNoise(vec2 p) {
  vec2 w = floor(p);
  vec2 k = fract(p);
  k = k*k*(3.-2.*k); // smooth it

  float n = w.x*10. + w.y*48.;

  float a = hash(n);
  float b = hash(n+10.);
  float c = hash(n+48.);
  float d = hash(n+58.);

  return mix(
    mix(a, b, k.x),
    mix(c, d, k.x),
    k.y);
}
```

和 Perlin noise 差不多，效果差速度快

通常用来做火焰效果

## Worley Noise



voronoi

[【浅入浅出】教你实现最简单的Voronoi图](https://zhuanlan.zhihu.com/p/144477570)