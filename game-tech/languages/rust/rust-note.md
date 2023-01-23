# 笔记

记录 Rust 一些需要重点理解的地方

* path 和 pathBuf 的关系类似 str 和 String，
* 模式匹配的时候字段名称，结构体名称和类名，都不能遗漏。

## 借用

* 在可变借用持续的时间内，不可再对这个变量进行借用
* 任何借用持续的时间内不能操作原变量
* 对借用的取值可以自动解引用，对借用的赋值需要手动解引用

## Result handle

* is_ 判断后续表达式成立
* _and 表达式同时成立
* _or 将 err 替换为默认结果，并拆箱 result
* _or_default 将 err 替换为 default 结果
* _or_else 将 err 替换为对 err 的操作
* _err 对 err 的操作方法
* matches! 表达式相等
* ok(), err() 结果转取值到 option
* 操作箱内值
  * as_ref 箱内值引用
  * as_mut 箱内值可变
* 使用箱内值
  * map 操作后替换原本 result 内的值
  * map_or 操作后返回变化后的结果
  * map_or_else 操作后返回变化后的结果
  * inspect 如果 ok 就执行结果
  * inspect_err 如果 err 就执行错误
* 取得箱内值
  * panic 强退程序
    * unwrap 默认提示
    * export 手动提示
  * 拆箱
    * unwrap_or_default 取值或默认结果
    * unwrap_or 取值或默认结果
* and_ or_ 继续操作并返回这个 result
  * and_then 如果为 ok 执行后续操作这个 result
    * and_then 操作 ok 内的值
  * or 如果为 err 替换 error
  * or_else 如果为 err 操作 error
* contains 比较 result 内的值

map 取得 result 内其中一个位置值，返回 result 对应位置值，

and then，or_else 取得 result 内其中一个位置值，返回一个新的 result

## resut 处理

当一个方法返回一个 Result 时，

* 如果需要将 Result 向上传递，
  * 使用 map_err 将 Result 的 Error 使用 anyhow 的 Error 包裹，(没信息可省略这步)
  * 如果本身就是 anyhow 的 Result，上面这步可简写把 map_err 替换为 context
  * 然后使用 ？解 Result 取到 ok 值，或者将封装后的 Error 向上传递。
* 如果不需要向上传递，
  * 就使用 or_else 处理 Err(Error) 后返回 Ok，
  * 使用 ？解 Result
* 常用 Result 方法集合
  * 取 Result 值
    * unwrap_or_default() 取值要是错误返回默认值
    * unwarp_or(T t) 取值要是错误返回 t
    * ? 取值要是错误返回错误
  * Rusult 内部值替换
    * map
    * map error
  * Result 整体修改
    * and then
    * or_else
  * 

## Mut

**let** **mut** x 指在栈区的 x 指向的指是可变的

## Rust 中的引用

1. 解引用使用 * 符号
2. 当 T 实现了 Deref 这个 Trait ，* 符号调用 deref 函数返回的值就是解引用结果
3. &T 是引用了 T 的一个结构体，Box `<T>`  也是一样，* 可用来消除 & 或 Box<>
4. 一般解引用后的最终结果要是单层引用，而不能把所有权给出去，所以自己写 deref 在返回的所有权的变量前要加上 &，类似 &*x，这么写是因为不能直接把自己的变量给出去改变所有权，所以会绕一圈
5. Option< T > 提供了方法直接对 T 进行引用相关操作，as_ref, as_deref
6. &T 任意传入方法都不会影响到内存块所有权变更，保护内存不被随意释放
7. 形参传入方法会自动解引用到对应需要的类型

## Copy Clone

[What’s the difference between Copy and Clone?](https://doc.rust-lang.org/core/marker/trait.Copy.html#whats-the-difference-between-copy-and-clone)
