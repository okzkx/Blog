# Rust 内存管理

[Visualizing memory layout of Rust&#39;s data types](https://www.bilibili.com/video/BV1KT4y167f1)

## Stack & Heap

![image-20220530134620352](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901237.png)

## Stack

Stack 存储数据和待执行的方法地址

## Data Types

![image-20220530135755655](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901962.png)

## Char

字符，4 Bytes

## Turple

* sizeof 得到大小
* align_of 得到对其单位

![image-20220530140111330](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901097.png)

## Ref

* 借用占据一个字节

![image-20220530140332029](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901724.png)

## Array and Vector

* Array 编译时大小已知，在栈中分配
* Vec 运行时动态改变大小

![image-20220530141011568](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901870.png)

## Slice 切片

* 切片的值要在运行时从内存中获取，编译时虽然数组大小已知，但是值要在运行时存入内存
* 所以切片只能引用
* 切片是胖指针，带有其他数据的指针

![image-20220530140944920](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901445.png)

## String, str, &str

* String 运行时存储在 heap
* str
  * 编译时存储在 read-only memory
  * 字符串拥有 'static 生命周期
  * &str 是胖指针
* 遵循 UTF-8 编码

![image-20220530103931430](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901200.png)

## Struct

一个 Struct 类型的变量占据的空间是其所有成员占据的空间总和

![image-20220530104207536](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901327.png)

## Enum

一个 Enum 类型所占据的空间是它占据空间最大的成员的空间

![image-20220530104431399](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901607.png)

使用 Box 指针优化最大成员空间

![image-20220530104503382](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230901599.png)

## Option Box

* Option 是一个描述是否有值的枚举
* 使用 Option <Box <>> 描述一个可空的指针
* Option<Box<\?>> 不需要序号占据枚举位

![image-20220530110559528](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902631.png)

## Copy & Move

rust 中变量类型可分为两种：可复制和不可复制

### 可复制类型拷贝

* 可复制类型赋值会进行拷贝
* 实现了 Copy Trait 的类型称为可复制类型，Copy Trait 表示这个类型可拷贝可克隆
* Copy Trail 需要实现 clone 方法，这个方法调用消耗大比 move 大，非必要就使用转移所有权的 move 来代替 clone
* 值类型和数组默认实现了 Copy Trait，Struct 默认是没有实现 Copy Trait，

![image-20220530110817028](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902514.png)

### 不可复制类型移动

* 没有实现 Copy Trail 的 Struct 称为不可复制类型
* 不可复制类型的赋值执行移动操作，转移所有权
* 不想对不可复制类型移动来赋值，要显式调用 clone 方法
* 实现 Clone Trait 来表示这个类型是不可拷贝但可克隆
* Box 类型和 Box 类型的父类型使用了堆内存
* Box 类型不能实现 Copy Trail，防止重复引用，所以一定是不可复制类型
* 一般是在运行时无法确定类型大小的时候，比如动态集合数据结构、字符串，才会使用 Box
* Drive 生成的 Copy 和 Clone 是递归调用的，要求 Struct 内的所有字段类型都要实现对应的 Trait

![image-20220530111349236](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902418.png)

## RC

- 当需要共享一个对象时，除了使用借用，还可以使用 RC，每个变量都是所有者
- RC 是智能指针指向内存区域一个类型，并多一个字节记录引用计数
- 相比于 Box 是使用引用计数来释放内存
- 当使用 clone 后将会浅拷贝指向的地址并让引用计数增加

![image-20220530111603625](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902360.png)

## Send & Sync

Send 类型：支持可以在线程间移动的类型

Sync 类型：支持可以在线程间共用数据

而 RC 不是 Send 和 Sync，因为可能会在更改数据时出现数据竞争

## Arc

arc 在 rc 的基础上，对数据的访问进行了原子化限制，防止数据竞争，有额外开销

![image-20220530113222274](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902264.png)

## Trait object

* 特征对象，指的是对具体对象的特征的引用，类似于使用接口对象代表实体
* Rust 中使用 dyn 声明
* 特征对象结构只有两个指针，一个是指向对象，一个是特征虚函数表，这也称为胖指针

![image-20220530114121090](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902230.png)

## 函数指针

指向函数机器码

## 闭包

闭包可以有三种特征 trait

Fn,FnOnce, FnMut

#### FnOnce

* 只能调用一次，无法修改
* 创建 struct 存储外部变量，相当于移动值或引用
* 在一次调用时移动 self 将其消耗掉
* call_once(self)

![image-20220530115008696](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902048.png)

#### FnMut

* 可多次调用，可以修改
* 创建 struct 引用外部变量，
* 需要声明外部变量和闭包都为可变的，因为闭包方法会改变 struct 里借用的变量而改变外部变量
* call_mut(&mut self)

![image-20220530120359317](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902759.png)

#### Fn

* 可多次调用，无法修改
* 调用时借用 self
* call(& self)

![image-20220530123053936](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902243.png)

### Move Closure

* move 语义，转移变量所有权到闭包中
* 闭包结构体中的变量不再是借用而是直接指向数据
* 当闭包使用的变量之后不再会使用时，通常使用 move

![image-20220530123214663](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202302230902967.png)
