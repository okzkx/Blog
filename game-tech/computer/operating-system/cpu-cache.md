
# 提高缓存命中率

内存地址映射到 CPU Cache 地址里的策略有很多种，其中比较简单是直接映射 Cache

把内存地址转换为「 组标记 + 索引 + 偏移量」标记

CPU L1 Cache 分为数据缓存和指令缓存

- 对于数据缓存，我们在遍历数据的时候，应该按照内存布局的顺序操作
- 对于指令缓存，有规律的条件分支语句能够让 CPU 的分支预测器发挥作用

# Cpu 缓存一致性

[2.4 CPU 缓存一致性](https://www.xiaolincoding.com/os/1_hardware/cpu_mesi.html)

![](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost3@main/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/CPU%E7%BC%93%E5%AD%98%E4%B8%80%E8%87%B4%E6%80%A7/%E7%BC%93%E5%AD%98%E4%B8%80%E8%87%B4%E6%80%A7%E6%8F%90%E7%BA%B2.png)

## CPU Cache 的数据写入

### 写直达 （Write Through）

把数据同时写入内存和 Cache 中

### 写回 （Write Back）

当发生写操作时，新的数据仅仅被写入 Cache Block 里，只有当修改过的 Cache Block「被替换」时才需要写到内存中

## 缓存一致性问题

需要一种机制，来同步两个不同核心里面的缓存数据

- 第一点，某个 CPU 核心里的 Cache 数据更新时，必须要传播到其他核心的 Cache，这个称为**写传播（_Write Propagation_）**；
- 第二点，某个 CPU 核心里对数据的操作顺序，必须在其他核心看起来顺序是一样的，这个称为**事务的串行化（_Transaction Serialization_）**。
	- CPU 核心对于 Cache 中数据的操作，需要同步给其他 CPU 核心；
	- 要引入「锁」的概念，如果两个 CPU 核心里有相同数据的 Cache，那么对于这个 Cache 数据的更新，只有拿到了「锁」，才能进行对应的数据更新。

具体实现

#### 总线嗅探

##### 方式

当 A 号 CPU 核心修改了 L1 Cache 中 i 变量的值，通过总线把这个事件广播通知给其他所有的核心，
然后每个 CPU 核心都会监听总线上的广播事件，并检查是否有相同的数据在自己的 L1 Cache 里面, 并更新该数据

##### 缺陷

1. 一直都需要发出广播
2. 不能保证事务串行化

#### MESI 协议


- _Modified_，已修改
- _Exclusive_，独占
- _Shared_，共享
- _Invalidated_，已失效

这四个状态来标记 Cache Line 四个不同的状态。

Cache line 状态机, 对 Cache line 在不同的状态下进行操作时,
会触发不同的总线机制和状态切换机制

![](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost3@main/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/CPU%E7%BC%93%E5%AD%98%E4%B8%80%E8%87%B4%E6%80%A7/MESI%E5%8D%8F%E8%AE%AE.png)

