


### 简介

- 无论哪种内存区域，简要来说，功能只有三个：分配，读取，回收

### Stack

- Stack 分配给默认实例化的值类型，包括结构体和地址。
- 在代码块（大括号内）执行完时自动回收。

### Heap

- Heap 分配给 new 出来的类型（注意：C# new Struct 是在 Stack，而 C++ new Struct 是在 Heap）
- 在不确定的时间由 CLR 回收所有未受到引用的实例化后的对象，这操作称为 GC。
