- [战斗数值新手入门指北：MOBA基础及装备篇 - 游资网](https://www.gameres.com/906063.html)
# 战斗数值

## 基础

## 战斗体验核心影响要素的设定

### 伤害术语

- TTK : 击杀所需时间
- RTK : 击杀所需回合数 (攻击/交互次数)
- DPS : 单位时间的伤害
- Hp (Health point): 生命值
- TTK = Hp / DPS

### 伤害公式


#### 线性公式 

- Damage = Attack - Defense * k
- 防御属性的提升价值会边际递增，在和攻击力持平时达到峰值
- ![](https://mmbiz.qpic.cn/mmbiz_png/Iy9bELzlibJnn4aCG4wMU805Yp58TPDTDHV4X7gJMUg5ibnjQp6RUu6U1NJFxt8YdTicfYKJqlSrJXsJAbUDDW7BA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
- 优势：减法公式下防御属性的成长正反馈强烈
- 劣势：在单局型公平PVP的体系下控制防御属性的投放范围阈值则会降低养成的自由度，从而牺牲玩法多样性；需要阙值

#### 除法减伤公式

- 通过减伤比例来降低攻击方造成的
- ReduceDamageRatio = Defense /（Defense+k）


#### 战斗数值多样性的扩充

以纯平 a 近战英雄为例

核心要素设定

![](https://mmbiz.qpic.cn/mmbiz_png/Iy9bELzlibJnn4aCG4wMU805Yp58TPDTDUPe9KEXfNxBLnTQRdmP14Wn0Of2YjicXtSjH6HIAveGN6yd5tpRq1ug/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

推出六种特性英雄

![](https://mmbiz.qpic.cn/mmbiz_png/Iy9bELzlibJnn4aCG4wMU805Yp58TPDTD86AA4HuS0iaDgo5xzSWA3atLic2icf9vZrp8W2QOfC5Kv07DQnFIMD3bA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

六种特性英雄相互交互得到 TTK 矩阵

![](https://mmbiz.qpic.cn/mmbiz_png/Iy9bELzlibJnn4aCG4wMU805Yp58TPDTDaWnd6WibzK5B4hp9tBjGjxJpPgYayRdP8ZicVoIoLqGnVDStY8hZeOJw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

a.  伤害能力数值维度拆分
- 时间维度
	- 从频率拆分出攻击速度，同样的DPS下通过攻击和频率的差异化分配，就能拓展出标速标伤，快速低伤和慢速高伤等等多种新的分支
	- 在稳定的TTK下，攻击和攻击速度可以在保证DPS不变的情况下互相转化（DPS=攻击*攻击频率）
- 稳定性维度
	- 对稳定的攻击拆分出暴击率和暴击伤害率，同样的频率和DPS下就拓展出了标伤标准暴击率标准，高伤低暴击率低暴击伤害率，低伤高暴击率高暴击伤害率等等多种新的分支
	- 攻击和暴击率/暴击伤害率可以在保证伤害期望不变的情况下互相转化
	- 伤害期望=不暴击概率*普通伤害+暴击概率*暴击伤害率*普通伤害
- 针对维度
	- 防御穿透
	- 收益期望上可以和攻击拉平，但针对高防和低防目标产生不同收益波动的固定防御穿透和百分比防御穿透
	- 固定防御穿透在减伤公式下, 低护甲远高于高护甲, 百分比防御穿透收益一致
	- 在稳定TTK下防御和生命值可以互相转化的，生命值和攻击也是可以互相转化的
b.  生存能力数值维度拆分
- 时间维度
	- 生命回复 / 吸血
	- 在稳定TTK下生命值和生命回复可以互相转化，1生命恢复/s=(标准TTK-1)点生命值
	- 同样的血量可以拓展出高生命值低回复，标准生命标准回复和低生命高回复等等多种新的分支
- 稳定性维度
	- 闪避 / 格挡
	- 标准血量标准闪避率，高血量低闪避率，低血量高闪避率
	- 血量和闪避率在稳定的TTK下是可以互相转化，因为有效生命值=生命值*（1/（1-减伤率）
	- 20%闪避率可以看作和20%减伤率价值期望相同

相同战斗力, 以不同维度为特点的英雄数值设定

![](https://di.gameres.com/attachment/forum/202405/28/083541no68qyofjqc1zy4w.png)

得到了基于静态标准模型下不同静态价值属性的价值比

![](https://di.gameres.com/attachment/forum/202405/28/083542ny7rwc5ykz5hgskp.png)



## 其他更多的常用维度

- 攻击距离
- 群体攻击
	- 覆盖范围
	- 单位数量
- 技能
- 魔法攻击
	- 元素反应
- 位移
- 护盾
