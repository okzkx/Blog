

# BRDF



![image-20211207183926205](../../../.gitbook/assets/image-20211207183926205.png)






渲染方程为入射光强度（每单位面积）经过 BRDF 贡献到出射角度之和
$$
L_o = BRDF * L
$$

入射光亮度（每单位面积单位立体角）对光照的光强的贡献
$$
L = L_icos\theta_i
$$

$$
E(p) = \int_\Omega L_i(p,w)cos\theta_idw_i
$$

BRDF 分为漫反射项和镜面反射项
$$
BRDF = D + S
$$

漫反射项为漫反射系数与表面颜色 c
$$
D = k_d \frac c\pi 
$$
镜面反射项为 镜面反射系数和 DFG 项
$$
S = k_S\frac {DFG}{4cos\theta_i cos\theta_o}
$$
D 为法线分布函数

Trowbridge-Reitz GGX:![image-20211207222925985](../../../.gitbook/assets/image-20211207222925985.png)

F 为菲涅尔项，菲涅尔代表反射和折射对颜色贡献的占比，

系数越大反射率越大，其与入射光方向无关

漫反射项中默认菲涅尔项为 1，因为当其值小的时候，会在物体边缘，此时反射光强度大，会改过漫反射

Schlick近似来表示菲涅尔系数:![image-20211207223431912](../../../.gitbook/assets/image-20211207223431912.png)

G 几何衰减项

Schlick-GGX近似:![image-20211207223736808](../../../.gitbook/assets/image-20211207223736808.png)

综合公式

- ![image-20211207224647020](../../../.gitbook/assets/image-20211207224647020.png)- 

- ![image-20211207205148722](../../../.gitbook/assets/image-20211207205148722.png)

- ![image-20211207214017494](../../../.gitbook/assets/image-20211207214017494.png)
