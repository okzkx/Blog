# 工程技术

### 基础系统

- component
- system
- action
- event
- element

### 枪械手感

Sway、Bob、Jolt

## 手雷

1. 手雷作为一种标准投掷物，功能上被拆解为 3 个部分：1. 投掷器（ThrowEmitter）；2. 手雷（Grenade）；3. 爆炸物（explosion）
2. 手雷综合使用 element + dots system 两个技术方案，element 向 system 传递数据的方式是 set attr （修改 component data 的数据），system 向 element 传递数据是通过 event；
3. 手雷是一个网络对象，使用 ghost 的方式进行网络数据同步；

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202408121141760.png)

### SubScene 自定义烘焙方案

在 BakingSystem 被创建出来时，所有 Baker 都会被添加到一个容器中，在 bakingSystem.Bake 中 Invoke

因此如果想要不烘焙某些组件，就需要在 bakingSystem.Bake 前从容器中移除对应的 Baker

因此，在 BakeDataUtility 中添加 Baker 移除方法，并在 PreBake 阶段将需要过滤的 Baker 从容器中移除，这样 BakingSystem 执行时就不会烘焙对应的组件

### 枪械

1. 枪械功能上被拆解为 5 个部分：1. 枪身（Gun）；2. 火控（FireControl）；3. 发射器（Emitter)；4. 弹匣(Magazine) ; 5.瞄准镜(Sight)
2. 枪械综合使用 element + dots system 的技术方案，element 向 system 传递数据的方式是 set attr （修改 component data 的数据），system 向 element 传递数据是通过 event；
3. 枪械是一个网络对象，使用 rpc 的方式进行网络数据同步；

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202408121158939.png)

![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202408121158346.png)
