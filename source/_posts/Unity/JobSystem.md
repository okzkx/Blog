---
categories: Unity
---



## JobSystem

#### **文档**

[JobSystem](https://docs.unity3d.com/Manual/JobSystem.html)

[JobSystem in ECS](https://docs.unity3d.com/Packages/com.unity.entities@0.1/manual/entity_iteration_job.html)



#### **3 种多线程执行方式**

1. 立即执行

.Run()

基本不使用，只在 Debug 时使用，因为如果使用这个，

直接在主线程里编写代码就行了

2. 托管单线程

IJobExtensions ::

IJob.Schdule()

JobForEachExtensions::

IBaseJobForEach.ScheduleSingle()

3. 托管多线程

IJobParallelForExtensions ::

IJobParallelFor.Schdule()

JobForEachExtensions::

IBaseJobForEach.Schedule()



#### **托管线程调度模式**

托管单线程一次性调度一个作业，

托管多线程一次性调度多个作业。

![A ParallelFor job dividing batches across cores](https://docs.unity3d.com/uploads/Main/jobsystem_parallelfor_job_batches.svg)

4 个泳道，MainThread , C# JobSystem, Job Queue, Native Job System

MainThread 创建作业，安排作业依赖

C# JobSystem 将所有将调度的作业分成 Batch

Job Queue 将所有将调度的 Batch 分为 Native job， Native job 为cup 核心的作业执行单元

 Native Job System 将所有 Native job 按照顺序和依赖调度给空闲的线程执行



#### **Native Memory**

作业作为方法执行，其指令不在栈上，没有返回值。

用 Burst 编译器的作业无法使用堆中的数据。

线程之间各自拥有自己的栈，共享拥有一个属于进程的堆。

通过主线程共享自己栈方式实现主线程和作业的数据同步。

Native Container 是一种数据结构，用来将主线程的栈的数据共享出去。

其数据存放在栈上，指向这些数据的指针在堆中。

主线程在创建作业时，交给其 Native Container 堆中的指针，

作业可以用其来操作主线程中的栈实现通过共享内存的数据互通。



#### **作业限制**

有些主线程可以做的操作，在 Burst 的条件下，在作业，或者多线程作业中不可做。

1. 作业不可操作堆内存
2. 作业不可使用 EntityManager
2. 多线程作业不可同时写同一个共享内存



#### **作业限制的迂回方式**

1. 使用 NativeContainer 操作共享栈代替操作共享堆
2. 使用 EntityCommandBuffer 代替 EntityManager，
3. 使用栈上的数据代替堆上的数据，
2. 将场景数据的操作推迟到**同步点**交给主线程操作



#### **作业调度**

主线程中完成作业调度，禁止在作业中生成作业或进行作业调度。

JobHandle 可以决定依赖关系

主线程对作业 Schedule 完后，任何时候该作业都可能开始或完成。

JobHandle.Comple, 主线程等待某一作业完成。

这种情况一般是作业对共享栈进行操作，主线程需要使用到这共享栈的值。