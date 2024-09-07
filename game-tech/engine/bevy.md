# Bevy 引擎研究

- [Bevy](https://bevyengine.org/)
- [Bevy book](https://bevyengine.org/learn/book/getting-started/)
- [Bevy Engine](https://github.com/bevyengine/bevy) : A refreshingly simple data-driven game engine built in Rust

## App

- 配置全局数据 insert_resource
- 配置 Systems
	- 初始化 add_startup_system
	- 主循环 add_system
- 配置 Plugin
	- Plugin 是配置 App 其他操作的集合


### ECS
- Entity : Entity(u64)
- Component : #[derive(Component)] struct S 
- Resource ：全局单例数据，不属于 Entity
	- Event 特殊的一种全局数据，支持触发和响应
- System : 就是函数，支持特定的传入参数，没有返回值 fn(
	-  Commands : 用来增加或删除 Entity 和 Resource
	- Query< ：用来查询和修改里的 Component 数据
		- &Component, 
		- &mut Component,
		- With< Component>, 
	- Res< S>>, ：查询和修改全局数据
		- ResMut< S>>){}

## Resource

- Bevy 资源管理分析