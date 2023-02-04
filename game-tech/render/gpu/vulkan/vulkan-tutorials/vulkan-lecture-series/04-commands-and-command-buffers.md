# Commands and Command Buffers

Different types of commands

![image-20230204232429958](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042324122.png)

Command buffer 生命周期

![image-20230204233950166](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042339330.png)

初始化时记录 command buffer

![image-20230204234915288](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042349447.png)

运行时记录 command buffer

![image-20230204234757964](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042347122.png)

## life sycle

![image-20230204235446362](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302042354451.png)

## Secondary Command buffers

command buffer 可以嵌套，不同的 command buffer 通常异步记录

## Data

1. Descriptor
2. PushConstants
3. Paramenters
4. Atrribute

Descriptor

指向 GPU 内存的指针

PushConstrants 直接向 shader 输入不变数据

Paramenters 将其他 buffer 作为参数

Attribute ：只用在 Vertex buffer 里的，顶点信息