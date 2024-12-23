## Unity 内存分布

### 最常用

- PSS : 平分共享服务内存
- 目前 Unity 的游戏在安卓上的默认指标

### 全部

- **VSS** （用处不大）
	- Virtual Set Size
	- 虚拟耗用内存（包含共享库占用的内存） 
	- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202412231556572.png)

- **RSS**  （用处不大）
	- Resident Set Size
	- 实际使用物理内存（包含共享库占用的内存）  
	- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202412231556347.png)

- **PSS** （仅供参考）
	- Proportional Set Size
	- 实际使用的物理内存（比例分配共享库占用的内存）  
	- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202412231557073.png)

- **USS** (实际数据)
	- Unique Set Size 
	- 进程独自占用的物理内存（不包含共享库占用的内存）
	- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202412231557006.png)




Reference

- [内存耗用：VSS/RSS/PSS/USS 的介绍 - 简书](https://www.jianshu.com/p/3bab26d25d2e)
- [Unity游戏内存分布概览_unity统计的内存和windows任务管理器统计的内存为什么不一样-CSDN博客](https://blog.csdn.net/UWA4D/article/details/124430777)