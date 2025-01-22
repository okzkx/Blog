


### 简介

- 无论哪种内存区域，简要来说，功能只有三个：分配，读取，回收

### Stack

- Stack 分配给默认实例化的值类型，包括结构体和地址。
- 在代码块（大括号内）执行完时自动回收。

### Heap

- Heap 分配给 new 出来的类型（注意：C# new Struct 是在 Stack，而 C++ new Struct 是在 Heap）
- 在不确定的时间由 CLR 回收所有未受到引用的实例化后的对象，这操作称为 GC。

## 内存管理

- [浅谈Unity内存管理](https://www.bilibili.com/video/BV1aJ411t7N6/?is_story_h5=false&p=1&share_from=ugc&share_medium=android&share_plat=android&share_session_id=ff64da59-506c-4c1f-a202-32e9f2843321&share_source=QQ&share_tag=s_i&timestamp=1665712988&unique_k=W7hG6lN&vd_source=ebf06d572d5366b5ef7bc5032fefb08d)