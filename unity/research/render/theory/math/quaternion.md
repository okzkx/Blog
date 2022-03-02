# 四元数



[*四元数*——基本概念 - 知乎](https://www.baidu.com/link?url=AFYpUFAYTytyhfd7YdlN7Qi8IHH2JPiZd_IJf__LI-X4GAFUVgP4Hnxy4gOOgkaz&wd=&eqid=bbd15e960004666b0000000661b01b47)

[四元数和旋转(Quaternion & rotation)](https://www.cnblogs.com/leixinyue/p/13469155.html)



```
        public static float3 rotate(quaternion q, float3 v)
        {
            float3 t = 2 * cross(q.value.xyz, v);
            return v + q.value.w * t + cross(q.value.xyz, t);
        }
```



根据公式来看，当 xyz 为 0 向量时，四元数不旋转输入向量

## 旋转

1. 转轴加角度表示，单位向量 + 标量
2. 旋转矩阵表示，Rotate Matrix，几维的矩阵表示几维的旋转
3. 欧拉角表示，三个标量，分别表示在三个基向量上的旋转值
4. 四元数表示，四个标量表示，意义不明，一般认为和 1 差不多

#### 四者可以互相转化

- 欧拉角表示和别的表示是多对一的关系
- 欧拉角是表示的范围最少，没法表示大于 2PI 的旋转
- 欧拉角会出现万向节死锁的问题
- 最好不要用欧拉角

- 旋转矩阵对向量旋转运算最好算
- 旋转矩阵需要存储的空间最多，三维有 9 个，别的才 4 个

- 所以一般用四元数存储，用转轴加角度理解，旋转向量时转旋转矩阵与向量矩阵乘法计算