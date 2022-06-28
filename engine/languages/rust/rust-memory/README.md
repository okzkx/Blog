# Rust 内存管理

[Visualizing memory layout of Rust&#39;s data types](https://www.bilibili.com/video/BV1KT4y167f1)

## Stack & Heap

![image-20220530134620352](rust-memory.assets/image-20220530134620352.png)

## Stack

Stack 存储数据和待执行的方法地址

## Data Types

![image-20220530135755655](rust-memory.assets/image-20220530135755655.png)

## Char

字符，4 Bytes

## Turple

sizeof 得到大小

align_of 得到对其单位

![image-20220530140111330](rust-memory.assets/image-20220530140111330.png)

## Ref

![image-20220530140332029](rust-memory.assets/image-20220530140332029.png)

## Array and Vector

![image-20220530141011568](rust-memory.assets/image-20220530141011568.png)

## Slice 切片

切片只能引用

![image-20220530140944920](rust-memory.assets/image-20220530140944920.png)

## String, str, &str

![image-20220530103931430](rust-memory.assets/image-20220530103931430.png)

## Struct

一个 Struct 类型的变量占据的空间是其所有成员占据的空间总和

![image-20220530104207536](rust-memory.assets/image-20220530104207536.png)

## Enum

一个 Enum 类型所占据的空间是它占据空间最大的成员的空间

![image-20220530104431399](rust-memory.assets/image-20220530104431399.png)

使用 Box 指针优化最大成员空间

![image-20220530104503382](rust-memory.assets/image-20220530104503382.png)

## Option Box

* Option 是一个描述是否有值的枚举
* 使用 Option <Box `<>>` 描述一个可空的指针
* Option<Box<?>> 不需要序号占据枚举位

![image-20220530110559528](rust-memory.assets/image-20220530110559528.png)

## Copy & Move

### 值类型拷贝

![image-20220530110817028](rust-memory.assets/image-20220530110817028.png)

### 引用类型移动

![image-20220530111349236](rust-memory.assets/image-20220530111349236.png)

## RC

- RC 是智能指针指向内存区域一个类型，并多一个字节记录引用计数
- 相比于 Box 是使用引用计数来释放内存
- 当使用 clone 后将会浅拷贝指向的地址并让引用计数增加

![image-20220530111603625](rust-memory.assets/image-20220530111603625.png)

## Send & Sync

Send 类型：支持可以在线程间移动的类型

Sync 类型：支持可以在线程间共用数据

而 RC 不是 Send 和 Sync，因为可能会在更改数据时出现数据竞争

## Arc

arc 在 rc 的基础上，对数据的访问进行了原子化限制，防止数据竞争，有额外开销

![image-20220530113222274](rust-memory.assets/image-20220530113222274.png)

## Trait object

特征对象，指的是对具体对象的特征的引用，类似于使用接口对象代表实体

Rust 中使用 dyn 声明

特征对象有两个指针，一个是指向具体数据，一个是特征虚函数表，这也称为胖指针

![image-20220530114121090](rust-memory.assets/image-20220530114121090.png)

## 函数指针

指向函数机器码

## 闭包

闭包可以有三种特征 trait

Fn,FnOnce, FnMut

#### FnOnce

创建 struct 存储外部变量，相当于移动值或引用

![image-20220530115008696](rust-memory.assets/image-20220530115008696.png)

#### FnMut

创建 struct 引用外部变量，

需要声明为可变的，因为闭包方法会改变 struct 里变量而影响到外部变量

![image-20220530120359317](rust-memory.assets/image-20220530120359317.png)

#### Fn

不改变外部变量的闭包，

![image-20220530123053936](rust-memory.assets/image-20220530123053936.png)

如果返回闭包，闭包中的引用变量超过变量作用域，需要使用move 语义，转移变量所有权到闭包中

![image-20220530123214663](rust-memory.assets/image-20220530123214663.png)
