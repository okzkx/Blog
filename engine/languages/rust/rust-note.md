# 笔记

记录 Rust 一些需要重点理解的地方

path 和 pathBuf 的关系类似 str 和 String，

模式匹配的时候字段名称，结构体名称和类名，都不能遗漏。

借用

* 在可变借用持续的时间内，不可再对这个变量进行借用
* 任何借用持续的时间内不能操作原变量
* 对借用的取值可以自动解引用，对借用的赋值需要手动解引用

Result handle

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
