
[4.2 malloc 是如何分配内存的？](https://www.xiaolincoding.com/os/3_memory/malloc.html)


![](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86/brk%E7%94%B3%E8%AF%B7.png)


- malloc 通过 **brk()** 方式申请的内存，free 释放内存的时候，**并不会把内存归还给操作系统，而是缓存在 malloc 的内存池中，待下次使用**；
- malloc 通过 **mmap()** 方式申请的内存，free 释放内存的时候，**会把内存归还给操作系统，内存得到真正的释放**。


用 mmap 来分配内存，等于每次都要执行系统调用。

#### 内存块信息

free() 函数时，free 会对传入进来的内存地址向左偏移 16 字节，然后从这个 16 字节的分析出当前的内存块的大小，自然就知道要释放多大的内存了。