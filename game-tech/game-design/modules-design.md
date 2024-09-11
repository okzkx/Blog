
# 模块化设计

- 以模块拼凑游戏

### 以顶视角塔防为例

### GamePlayObjects

#### TopObjects

1. Player
2. Tower
3. Charactor
4. Summons

#### SubObjects

1. Friend
2. Enemy
3. Arrow

### Attachments

1. Buffer （每帧）
   1. AbilityUser
   2. Buff
   3. SkillUser
2. TimeLine （特定时间点）
   1. AOE
   2. Skill

### *Controller

1. Rotate controller
2. Move controller
3. Charactor move controller

### 调用链

GamePlayOjbect -创建-> Attachment ( -使用-> Controller ) -改变-> Transform

#### 能力系统 Ability

AbilityUser

Ability(abstruct)

1. 防御塔种植能力 TowerPlantAbility
2. *角色移动能力 CharactorMoveAbility
3. 寻路能力 AutoMoveAbility
4. 防御塔发射箭矢能力 TowerShotArrowAbility
5. 箭矢移动能力 ArrowMoveAbility
6. 自动转向能力 AutoRotateAbility
7. 发出伤害能力 SendDamageAbility
8. 受到伤害能力 ReceiveDamageAbility
9. 死亡能力 DepthAbility
