# 已验证的增量游戏数值机制研究（2026-07）

## 结论先行

宗门经营 Demo 不宜自行发明一套“所有选择概率相近”的结算模型。现有作品反复验证过的主干是：

> **有限劳动力重配 → 多资源生产/加工链 → 指数递增成本 → 阶段性倍增与自动化 → 新瓶颈出现。**

建议以 **Kittens Game / Evolve Idle 的人口岗位分配**作为操作骨架，以 **Melvor Idle 的原料—加工—用途互相支撑**作为资源网络，以 **Antimatter Dimensions 的里程碑倍增与逐层自动化**作为成长节奏。离线计算采用 Melvor 的“按动作结算并给出汇总”，而不是简单把当前每秒速率乘以离线时长。

这不是证明“照抄后必然好玩”。它证明的是：上述机制分别已有市场反馈或长期公开实现支撑，适合作为首轮原型的低风险起点。宗门题材真正需要自行验证的变量，只保留在岗位冲突、资源用途和突破节奏上。

## 证据等级

本文严格区分两种“验证”：

- **市场验证**：官方商店页存在足够规模、较长时间积累的用户评价。评价数不是销量或活跃人数，本文不把它们当作销量。
- **实现验证**：开发者公开源码或官方 Wiki 明确展示机制和公式。它只能说明机制确实被完整实现、维护或迭代过，不能单独证明市场接受度。

截至 2026-07-15：

| 游戏 | 市场证据 | 实现证据 | 本项目应取用的部分 |
|---|---|---|---|
| Melvor Idle | Steam 显示 15,964 条全语言评价、90% 好评；属于强市场信号，不等于销量 | 官方 Wiki 详述生产技能、精通池、离线进度 | 原料跨技能流转、动作制离线结算、局部精通与全局池 |
| Antimatter Dimensions | Steam 显示 4,440 条全语言评价，官方描述强调多层解锁、重置和“大量自动化” | MIT 许可官方源码公开成本、批量购买和自动化里程碑 | 台阶式倍增、旧层自动化、批量购买、重置层级 |
| Realm Grinder | Steam 显示 7,409 条全语言评价；官方描述强调阵营形成不同玩法 | 未使用源码作为依据 | “同一经济底盘上构筑不同流派”的市场旁证 |
| Kittens Game | 2026 年才上 Steam，当前约 50 余条评价，尚不足以单独算强市场证据 | 官方 Wiki 与开发者仓库长期公开；源码许可禁止商业衍生使用 | 人口岗位、净产出、库存上限、指数建筑成本、传承软上限 |
| Evolve Idle | 未找到可比的官方商业数据，不列为市场验证 | MPL-2.0 官方源码公开劳动力、生产、声望软上限 | 多岗位资源网、人口调度、对数型声望软上限 |
| Progress Knight | 原作缺乏可核实的官方商业数据 | Unlicense 官方源码公开等级成本、跨周目最高等级加速 | 单线成长、死亡/重生后“旧内容加速” |
| NG Space Company | 未找到可比的官方商业数据 | MIT 官方源码公开时间差模拟、指数成本与自动购买 | Web 端连续 tick、离线 delta、常规/巨构成本倍率 |

市场数据来源：[Melvor Idle Steam](https://store.steampowered.com/app/1267910/Melvor_Idle/)、[Antimatter Dimensions Steam](https://store.steampowered.com/app/1399720/Antimatter_Dimensions/)、[Realm Grinder Steam](https://store.steampowered.com/app/610080/Realm_Grinder/)、[Kittens Game Steam](https://store.steampowered.com/app/1097410/Kittens_Game/)。这些数字会随时间变化，应在对外材料中标注查询日期。

## 一、生产链：让玩家优化“流量”，而不是抽结果

### 1. 离散劳动力 + 连续资源流

Kittens Game 的资源公式把建筑产出、岗位产出、季节、升级和幸福度共同汇入每 tick 产量；其人口可被分配到不同职业。[官方资源 Wiki](https://wiki.kittensgame.com/en/general-information/resources) 给出的结构本质上是：

```text
每 tick 净变化 = 建筑产出 + 岗位产出 × 修正项 - 持续消耗
```

Evolve Idle 则直接维护各岗位 `workers`，加一名工人时从默认岗位扣除，减员时返还默认岗位；具体岗位产出由“人数 × 单工产出 × 科技/种族/环境修正”计算。[岗位调度源码](https://github.com/pmotschmann/Evolve/blob/3436358dcd03d9f9e071d51ea071e0a78c0322e4/src/jobs.js#L579-L609) 与 [伐木/采石产出源码](https://github.com/pmotschmann/Evolve/blob/3436358dcd03d9f9e071d51ea071e0a78c0322e4/src/jobs.js#L56-L139) 都能看到这一结构。

**可迁移结论：**宗门弟子不必先模拟走路和日程。每名弟子就是一个可重配的离散产能单位；玩家的主要操作是把有限人力移向当前瓶颈。

建议的基础公式：

```text
岗位产速(j) = Σ弟子[基础效率 × 岗位适性] × 设施倍率 × 全局倍率
资源净速(r) = Σ产出(r) - Σ消耗(r)
```

UI 必须同时显示库存、净速和“预计耗尽/填满时间”。只有库存数字而没有流量，玩家无法判断调人之后发生了什么。

### 2. 加工链必须按“可执行动作数”结算

Melvor 的官方介绍明确说每个技能会以有意义的方式影响其他技能；官方新手教程用“钓鱼获得生鱼 → 烹饪消耗生鱼”展示最简单的生产线。[Steam 官方页](https://store.steampowered.com/app/1267910/Melvor_Idle/)；[官方新手教程](https://wiki.melvoridle.com/index.php?title=Beginners_Guide)。

宗门加工不应允许库存变成负数。对炼丹、炼器等消耗型岗位，建议按完整动作结算：

```text
计划动作数 = floor((累计时间 + Δt) / 单次耗时)
可执行动作数 = min(计划动作数, floor(各输入库存 / 单次输入))
输入库存 -= 可执行动作数 × 单次输入
输出库存 += 可执行动作数 × 单次输出
```

若原料不足，岗位停工并明确指出缺少哪种资源；不要暗中降低效率。瓶颈被看见，才会产生调度决策。

## 二、成本：用几何增长制造投资时机

Kittens Game 的建筑下一次价格直接采用：

```text
下一件成本 C(n) = 基础成本 C0 × priceRatio^已拥有数量 n
```

其常规建筑 `priceRatio` 大量使用 1.10、1.12、1.15，特殊住房和奇观会更高；[建筑价格源码](https://github.com/nuclear-unicorn/kittensgame/blob/117c60d48b53b5b4637fa767c6cfae0289ff90b3/js/buildings.js#L2353-L2401) 可见完整计算。NG Space Company 也在公开源码中分别使用 `1.1^n`、`1.02^n` 和 `2^n` 处理常规设施、戴森类设施和储存扩张。[成本源码](https://github.com/g8hh/NGSpaceCompany/blob/adf9d20de157d2c07f3a278d8314e651b6dfd70f/src/store.js#L403-L416)

批量购买的总价是几何级数：

```text
购买 p 件总成本 = C0 × (m^p - 1) / (m - 1)
可购买数量 = floor(log(1 + 资源 × (m - 1) / C0) / log(m))
```

Antimatter Dimensions 的 MIT 源码直接使用上述公式计算批量购买。[LinearCostScaling 源码](https://github.com/IvarK/AntimatterDimensionsSourceCode/blob/8ae221fcb07db667b3d04114c2b977175966611d/src/core/math.js#L256-L295)

**Demo 起始参数建议（需要实测，不冒充业界标准）：**

- 可重复经济设施：`m = 1.15`；
- 扩容类设施：`m = 1.25`，避免库存很快失去约束；
- 一次性科技/突破：固定成本表，不套指数公式；
- 首轮只允许“买 1 / 买最大”，统一走几何级数函数，避免逐次循环误差。

几何成本的意义不是让后期变慢，而是让玩家不断比较“再买一级旧设施”与“攒钱解锁新层”哪个回本更快。

## 三、倍增里程碑：升级必须改变曲线

Antimatter Dimensions 不依赖连续的 `+5%`：其维度按组购买，源码围绕“距离下一组 10 个还差多少”组织购买，并在高层通过里程碑逐步返还旧升级、解锁各层自动购买器。[反物质维度购买源码](https://github.com/IvarK/AntimatterDimensionsSourceCode/blob/8ae221fcb07db667b3d04114c2b977175966611d/src/core/dimensions/antimatter-dimension.js#L300-L383)；[永恒里程碑源码](https://github.com/IvarK/AntimatterDimensionsSourceCode/blob/8ae221fcb07db667b3d04114c2b977175966611d/src/core/secret-formula/eternity/eternity-milestones.js#L1-L151)。Steam 官方页也把“解锁新东西并自动化旧东西”列为核心特征。

Melvor 的精通池则提供另一种台阶：动作经验的一部分进入同技能公共池；10%、25%、50%、95% 四个检查点激活被动奖励，而花掉池经验跌破检查点时奖励会失效。[官方 0.17 版本说明](https://wiki.melvoridle.com/w/V0.17)

**可迁移结论：**同一设施的小升级负责平滑增长，关键等级负责改变规则。宗门 Demo 可以先使用：

- 设施 1—4 级：每级提高基础产出；
- 5 级：解锁自动规则或新转化；
- 10 级：给予显著倍率或释放一名岗位弟子；
- 弟子突破：不是“战力 +10%”，而是增加一项生产规则、跨岗位加成或自动管理能力。

具体“5/10”只是首轮待测参数；已经被验证的是“平滑升级中嵌入规则跃迁”这一结构。

## 四、自动化：奖励已掌握的内容，而不是替玩家玩新内容

Antimatter Dimensions 的市场页明确强调逐层解锁自动化，其里程碑源码表现为：先手动完成挑战或重置，再逐步保留旧层、解锁对应 autobuyer，最终让旧层开局即完成。Melvor 也把后期被动烹饪与离线继续当前动作建立在玩家已经选定生产目标之后。

宗门版本应遵循同一原则：

1. 玩家先手动发现灵材不足；
2. 手动调人解决过至少一次；
3. 建成执事堂后才能设置“库存阈值”和“自动调度”；
4. 自动化只覆盖已掌握的旧资源层；
5. 新资源、新危机仍要求玩家重新判断。

这能实现“宗主只发号施令”的幻想，同时保留决策密度。第一版自动化只需两种规则：`库存低于 X 时优先生产`、`库存高于 Y 时启动加工`。

## 五、软上限：只压制永久倍率，不压制短局爽感

Kittens Game 对传承生产倍率使用有限递减：前 75% 上限不衰减，剩余 25% 逐渐逼近上限；Paragon 的生产加成受递减限制，但储存加成保持线性。[Paragon 官方 Wiki](https://wiki.kittensgame.com/en/general-information/resources/paragon)；[递减函数源码](https://github.com/nuclear-unicorn/kittensgame/blob/117c60d48b53b5b4637fa767c6cfae0289ff90b3/game.js#L2347-L2372)。Evolve 的 Plasmid 在 `250 + Phage` 阈值前按对数增长，超过阈值后追加更慢的对数增长。[Plasmid 源码](https://github.com/pmotschmann/Evolve/blob/3436358dcd03d9f9e071d51ea071e0a78c0322e4/src/resources.js#L3199-L3273)

**可迁移结论：**软上限适用于跨世代永久生产倍率，防止传承滚雪球吞掉所有新内容；不应在 15—20 分钟核心 Demo 的普通设施上大量使用。短局的节奏主要靠几何成本和新层解锁控制。

如果后续加入“宗门底蕴”，可采用 Kittens 式有限递减：

```text
若 x ≤ 0.75L：f(x) = x
若 x > 0.75L：
f(x) = 0.75L + 0.25L × (1 - 0.25L / (x - 0.75L + 0.25L))
```

其中 `x` 是原始永久加成，`L` 是该层软上限。容量、解锁权和自动化规则可以不走同一软上限，让传承仍有结构性价值。

## 六、重置与传承：保留“更快重走”，不要只发永久货币

Kittens 在超过 70 只猫时重置，每多一只获得 1 Paragon；Paragon 同时增强全局生产、储存与部分自动生产。[Paragon 官方 Wiki](https://wiki.kittensgame.com/en/general-information/resources/paragon)。Antimatter Dimensions 的重置层给永久货币，同时逐步让旧层自动化或不再被重置。Progress Knight 则记录上一世各任务最高等级，下一世经验倍率为：

```text
旧任务加速倍率 = 1 + 上一世最高等级 / 10
升级经验需求 = baseXP × (level + 1) × 1.01^level
```

见 [Progress Knight 任务源码](https://github.com/ihtasham42/progress-knight/blob/f6dd0c3c444d838f80325bbdca918160b99e89e5/js/classes.js#L1-L42) 与 [重生逻辑](https://github.com/ihtasham42/progress-knight/blob/f6dd0c3c444d838f80325bbdca918160b99e89e5/js/main.js#L850-L888)。这是公开实现证据，不是强市场证据。

宗门传承应优先保留以下一项或多项：

- 已经掌握过的生产层获得经验/产速追赶倍率；
- 某些自动化规则开局保留；
- 上一代突破者留下“岗位规则”而非纯战力；
- 旧危机从手动应对转为可自动派遣的资源点。

传承货币可以存在，但不应是唯一奖励；否则第一轮材料“返工后无处使用”的问题只会换一种名字继续存在。

## 七、离线进度：使用动作模拟与可解释汇总

Melvor 官方 Wiki 说明，离线时继续玩家关闭前正在进行的动作，回归时计算 XP、物品、精通等，并显示汇总；基础上限为 24 小时。[官方新手教程的离线进度段落](https://wiki.melvoridle.com/index.php?title=Beginners_Guide#Offline_Progression)。Antimatter Dimensions 的更新记录与源码表明其离线模拟会处理自动购买器，而不是只发一笔静态货币。NG Space Company 的 Web 循环使用 `delta = 当前时间 - 上次更新时间`，随后按 delta 生产并执行自动化。[Web 循环源码](https://github.com/g8hh/NGSpaceCompany/blob/adf9d20de157d2c07f3a278d8314e651b6dfd70f/src/App.vue#L2569-L2593)

宗门版推荐分两阶段：

- **Demo 阶段**：最多结算 8 小时；固定玩家离开前的岗位和自动规则；按加工动作数结算；回归时列出每种资源净变化、停工原因和弟子修为变化。
- **后续阶段**：按“下一次库存耗尽、设施完成、突破完成、阈值规则触发”切片模拟，而不是用一个巨大 Δt 一次乘完。

必须先计算输入约束再发输出，否则离线炼丹会凭空消耗负库存。离线模拟也不应替玩家自动选择尚未配置的升级或突破。

## 八、给宗门 Demo 的组合算法初稿

### 推荐主循环

```text
观察净速与耗尽时间
→ 调整弟子岗位
→ 积累基础灵材
→ 投资设施或加工丹药
→ 弟子突破 / 设施到达规则里程碑
→ 旧瓶颈被自动化
→ 新资源层或危机用途出现
```

### 最小资源图

```text
采集弟子 ──→ 灵材 ──→ 建筑升级
                  └──→ 丹房加工 ──→ 丹药 ──→ 修为
                                                └──→ 境界突破

建设弟子 ──→ 缩短设施完成时间
守山弟子 ──→ 可见且确定的防御值
```

首轮危机不采用 A/B/C 概率表。危机只读取公开能力阈值：防御不足则损失生产时间或库存，达到阈值则胜利；胜利后把危机地点转成自动资源点。这样，危机是对资源构筑的检验和新产能来源，不是另起一套抽奖结算。

### 首轮数值原则

1. 每个核心资源至少有两个竞争用途，避免返工材料闲置。
2. 玩家调动一名弟子后，5—10 秒内必须看到净速或预计完成时间变化。
3. 每 2—4 分钟出现一次规则级跃迁；小升级填充跃迁之间的等待。
4. 任何资源停产必须能追溯到明确输入、岗位或容量瓶颈。
5. 任何永久奖励必须改变下一轮规则、自动化或追赶速度，不能只发抽象点数。
6. 先用确定性模型验证经营循环；随机暴击、品质和事件等到基础循环成立后再加。

## 九、不能直接照搬的部分

- **不要照搬 Antimatter Dimensions 的数量级。**其极大数值和多重重置与本项目 15—20 分钟 Demo 不匹配；应迁移里程碑与自动化结构，而非指数规模。
- **不要照搬 Melvor 的单角色单动作限制。**它适合个人技能养成；宗门需要多弟子并行，迁移的是跨技能供需和动作结算。
- **不要照搬 Kittens/Evolve 的完整资源树。**首轮三种库存资源已经足够验证瓶颈迁移；过早增加矿种、食物、幸福度会掩盖核心问题。
- **不要复制 Kittens Game 源码。**其仓库自带的 [WET PAWS LICENSE](https://github.com/nuclear-unicorn/kittensgame/blob/117c60d48b53b5b4637fa767c6cfae0289ff90b3/license.txt) 明确禁止商业获利和衍生作品。可研究数学思想，但不可把代码当模板直接用于商业项目。
- Evolve 为 MPL-2.0，Antimatter Dimensions 与 NG Space Company 为 MIT，Progress Knight 为 Unlicense；即便许可允许，也应单独做依赖与署名审查。本稿只建议机制，不构成法律意见。

## 十、下一步应验证的三个问题

1. **岗位重配是否有可感知反馈？**玩家调一名弟子后，能否立刻解释净速与预计时间为何变化。
2. **瓶颈是否会迁移？**理想体验是“先缺灵材 → 丹房建成后缺人 → 自动化后缺丹药/修为”，而不是永远只缺同一种货币。
3. **经济投资与危机准备是否真的冲突？**如果最优解始终是先把所有钱投经济，再最后补防御，就需要让防御投入同时产生经营价值，或让准备过程分阶段兑现。

这三个问题全部通过以后，再加入世代传承、弟子性格和复杂危机。否则外围内容会使失败原因更难定位。
