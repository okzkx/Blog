# Native

[what-is-nativearray](https://forum.unity.com/threads/what-is-nativearray.725156/)

[Unity 内存管理和profiler详解 - 简书（重要）](https://www.jianshu.com/p/cf3ab3bac1ab)

区别于 Stack 和 Heap 的第三块内存区域 Native。

## 简介

无论哪种内存区域，简要来说，功能只有三个：分配，读取，回收

### Stack

Stack 分配给默认实例化的值类型，包括结构体和地址。

在代码块（大括号内）执行完时自动回收。

### Heap

Heap 分配给 new 出来的类型（注意：C# new Struct 是在 Stack，而 C++ new Struct 是在 Heap）

在不确定的时间由 CLR 回收所有未受到引用的实例化后的对象，这操作称为 GC。

### Native Memory

**Native** 是分配给任何游戏资源的本地内存，比如 纹理资源，模型资源。

这边的实例化后的内存占用不会被自动释放。

在语言设计方面，C# 不允许在 Native 处申請分配内存，因为这可能会导致内存泄漏。

UnityEngine 利用 C# 的 unsafe 代码，构建出了 NativeContainer 系列结构。

NativeContainer 在 Native 区域分配和释放内存。

NativeContainer 集合系列包括 Array , List, HashMap 等常用集合结构。

在设置的生命时间结束后， NativeContainer 自动回收对应内存。

## 好处

* 可直接修改 Native 区域的资源文件，以往修改资源文件需要将其实例化 Clone 到 Heap，在写回 Native，再回收 Heap
* Burst 编译器无法编译堆区内容（类？），可以使用 Native Container 来代替 STL Container。
* 对多线程的支持，这边涉及到 GC 的回收机制，GC 通常会在 main 方法中遍历出所有引用到的堆内存中的对象，对于没有遍历到的对象，将进行自动垃圾回收。在多线程中，子线程引用的对象可能不会被主线程引用到，为了不被回收，使用 NativeContainer

### Reference

[Struct NativeStream.Writer | Package Manager UI website](https://docs.unity3d.com/Packages/com.unity.collections@0.1/api/Unity.Collections.NativeStream.Writer.html)