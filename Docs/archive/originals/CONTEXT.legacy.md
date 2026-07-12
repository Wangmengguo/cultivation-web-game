# Cultivation Web Game

A lightweight web idle game about running a cultivation sect. The first design focus is low-frequency sect management: assign disciples, let time pass, and inspect how disciple states changed.

## Language

**宗主**:
The player role. The sect leader makes assignment and resource decisions, but does not directly perform cultivation actions.
_Avoid_: 单个修士, 天道, 系统

**宗门**:
The managed organization. It is the frame that connects disciples, resources, assignments, and results.
_Avoid_: 大世界, 门派沙盒

**弟子**:
A sect member whose state changes over time based on assignments, resources, and events.
_Avoid_: 员工, 小人, 角色卡

**资质**:
A disciple's growth potential. The first version shows aptitude as a number with a plain-language label in parentheses, such as `82 (天才)`.
_Avoid_: 纯随机强度, 皮肤差异, 复杂词条

**排班**:
The player's main decision: assigning disciples to current work or cultivation priorities.
_Avoid_: 操作角色, 手动战斗

**岗位**:
An assignment option for disciples. The first version uses four jobs: cultivation, herb gathering, alchemy, and expedition.
_Avoid_: 建筑系统, 职业系统

**外出历练**:
The combat-flavored assignment that brings back materials. It uses realm or martial strength to decide whether the disciple can handle the outing, without manual battle controls.
_Avoid_: 手动战斗, 大地图探索, 外出打架

**优先突破者**:
The disciple currently receiving concentrated sect support to break through first. The first version treats this as the central resource decision.
_Avoid_: 平均培养, 自动最优培养

**材料**:
Resources gained through sect work and expeditions, then spent to support cultivation and breakthrough.
_Avoid_: 通用金币, 纯经验

**材料等阶**:
The progression layer for materials. The first version uses two tiers, `凡阶材料` and `灵阶材料`, so higher progress asks for better material access rather than just more of the same resource.
_Avoid_: 草药/灵材混作等阶, 颜色品质堆叠

**资源决策**:
Choosing how limited sect resources should support the priority breakthrough disciple and future sect production.
_Avoid_: 纯数值增长, 战力堆叠

**确定突破**:
The first-version breakthrough rule. If the disciple has enough cultivation progress and required materials, breakthrough succeeds without probability.
_Avoid_: 概率突破, 失败惩罚

**验收场景**:
The fixed place or view where the player reads the outcome of prior decisions. In the first version, the time-passage log is the center of the scene; disciple lists and assignments support it.
_Avoid_: 大地图, 建筑摆放

**弟子状态变化**:
The primary offline and idle result. The first version focuses on cultivation progress changes and possible breakthrough readiness.
_Avoid_: 纯资源结算

**收益差**:
The first-version consequence model for assignment choices. Assignments differ by output efficiency, not by punishment such as fatigue, heart demons, or accidents.
_Avoid_: 过劳惩罚, 炸炉, 心魔事故

**轻松日志**:
The tone of the time-passage log. Entries can be playful, but numeric changes should be shown in parentheses.
_Avoid_: 严肃战报, 纯数值流水

**放置中有事发生**:
The desired idle feel: the sect continues producing meaningful state changes while the player is not actively clicking.
_Avoid_: 空等, 一键收菜
