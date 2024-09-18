## Aspect

- Aspect 是一些 Component 的组合
- 算是一个语法糖
- 当 Query Asect 的时候相当于同时 Query 这些 Component
- 起到一个封装的作用, 基本没什么用

## structure-change

ecs 结构改变需要非常的谨慎

- 在遍历过程中禁止结构改变
- 在多线程并发时禁止结构改变

所以, 在很多并发或遍历 API 里不支持结构改变, 

- 结构改变的实现最基础的方式是用 EntityManager 写在主线程里
- 更好点的方式是用 ecb 在多线程里记录操作, 在 playback 统一处理结构体改变
- 再好一点是所有 system 的操作都用同步点 system 的 ecb 处理

