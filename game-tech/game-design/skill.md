- [游戏中的技能系统设计 - 知乎](https://zhuanlan.zhihu.com/p/83817526)


一个典型的星际2技能包含以下几个部分：

- Abilities(能力)：Ability就是一个单位可以做的事情。比如：攻击, 移动, 建造之类。
- Effects(效果)：Effect是让一件事情发生。它是Aiblity幕后的实现，可以增加Buff，产生伤害，治疗单位等。
- Behaviors(行为)：效果产生的行为，常见的是“Buff”，是附加到单位并以某种方式影响它的东西。诸如：提高移动速度，禁用武器，有机会阻挡传入的伤害等等。

- [一个MMORPG的常规技能系统 - GameRes游资网](https://www.gameres.com/729192.html)

技能是一套角色行为配置

1. 技能表 : 技能的数值, 配置和资产
2. 技能管理 : 角色拥有的技能
3. 技能调用接口 : 
	1. Enter : 进入 (必须)
	2. Exit : 循环释放的技能手动退出
	3. Stop : 打断
4. 技能 (一段完整的技能动画)
	1. 前摇开始
		1. 前摇开始特效 (一般是施法的法阵)
		2. 目标锁定
	2. 前摇结束 / 后摇开始
		1. 目标特效
		2. 子弹生成
		3. 技能释放特效
		4. 代价扣除
	3. 后摇结束
		1. 后摇结束特效 (一般没有)
5. 技能树
	1. 每个技能作为一个技能节点
	2. 根据条件判断执行哪个技能节点的系统为技能树
6. 技能创生体 : 由技能或技能创生体生成
	1. 视觉表现 : 
		1. 动作
		2. 特效
		3. 镜头
	2. 听觉表现
		1. 命中音效
		2. 持续音效
	3. 技能载体
		1. Buff
			1. 互斥
			2. 增益减益
			3. 叠加
		2. 范围伤害 (瞬间, 持续性)
			1. 射线区域
			2. 形状区域
		3. 投射物
			1. 跟踪
				1. 直线投射物
				2. 抛物线投射物
			2. 非跟踪
				1. 直线投射物
				2. 抛物线投射物