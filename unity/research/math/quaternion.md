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

