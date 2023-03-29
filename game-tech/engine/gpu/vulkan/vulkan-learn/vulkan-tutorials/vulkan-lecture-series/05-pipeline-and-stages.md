# pipeline and stages

graphcs pipeline cmd 执行会经历 graphcs pipeline 的所有阶段

![image-20230205073634418](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050736499.png)

每个 cluster 是一个核，同时处于 graphcs pipeline 的某个阶段

![image-20230205074057708](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050740818.png)

一般常用的 graphcs pipeline 可以简化成

![image-20230205074839468](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050748541.png)

### Compute pipeline

![image-20230205075353463](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050753511.png)

### Ray Tracing

![image-20230205075448942](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050754010.png)

![image-20230205075824559](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050758597.png)

所有的 stages

![image-20230205080115032](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050801187.png)

Synchronization2 使用的 Stage

![image-20230205080614290](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302050806457.png)
