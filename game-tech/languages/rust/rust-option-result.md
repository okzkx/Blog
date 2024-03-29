# Rust Option 和 Result 的处理

## Handle

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
