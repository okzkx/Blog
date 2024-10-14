- [【先导片】快速了解这套Blender4.0课程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV11H4y1P7RV/?p=1&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)

游标 

移动 : shift + 鼠标右键

复制 : shift + D

选择
- 选择中间 : Ctrl + Select
- 增加选择 : Shift + Select
- 
- 循环选择  : Alt + Select
- 增加循环选择  : Alt + Shift + Select
- 环形选择 : Ctrl + Alt + Select
- 
- 反选 : Ctrl + i

挤出:
- 同方向挤出 : E
- 各自法向挤出 : Alt + E
- 流型挤出, 自动添加侧面 :![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410141616516.png)

拉平: 在对应视图下, 多选后缩放为 0

删除 : 删除边时使用 融并边, 能自动融合两边的面

切换模型预览方式 : z 
切换透视方式 : Alt + z 

用 Bool 挖洞

J / F 连接点 ?

叠加层显示信息![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410141632974.png)

分离模型 : P
合并模型 : Ctrl + J
独立显示选择对象 : /

做窗口的包边 : 反向沿法向挤出

设置原点
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410141642520.png)

命令组
- 点 : Ctrl + V
	- 倒角 Ctrl + Shift + B
	- 断开 V
	- 焊接(合并) M, 到末点
	- 连接(在网格里) J, F
- 边 : Ctrl + E
	- 倒角 Ctrl + B
	- 细分
	- 桥接
	- 环切 Ctrl + R
	- 融合(删除?) : Ctrl +X
- 面 : Ctrl + F
	- 挤出 E, Alt + E
		- 沿法向
		- 沿内部
	- 内插 I
		- + I 各个面单独插入
	- 直接填充 F
		- 填充 Alt + F
		- 栅格化填充 (完美填充)
	- 三角形化 : 自动布线

切角  : Ctrl + B
循环边桥接 : 
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410141646299.png)
![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202410141647425.png)

应用物体变换 : Ctrl + a

坐标轴
空间 : <
- 全局
- 局部
- 法向
- 游标
位置 : >
- 中心
- 游标
- 各自中心 
	- Ctrl +R, R 各自中心缩放

