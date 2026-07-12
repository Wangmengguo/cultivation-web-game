# 已验证玩法如何迁移成“宗主令”

研究日期：2026-07-09

目的：给 `core-architecture.md` 的下一轮修订提供依据。这里不主张发明新玩法，而是把已经被验证的玩法骨架套进“宗门修仙”外壳。

## 结论

下一版核心玩法应该表述为：

> 玩家作为宗主，对半自治弟子下达常备命令，并对稀缺资源与突破机会做低频裁决。弟子按令执行，系统用日志复命，突破带来增量游戏式加速。

这不是新机制。它是四类成熟玩法的组合：

1. 殖民地模拟的“给人分配工作/优先级”。
2. 角色养成与战术 RPG 的“把稀缺资源投给具体个体”。
3. idle/incremental 的“资源链 + 增速 + prestige/突破”。
4. 开罗经营的“低频决策 + 可观察反馈周期”。

## 1. 常备命令：工作优先级已被殖民地模拟验证

### RimWorld

来源：[RimWorld Wiki: Work](https://rimworldwiki.com/wiki/Work)

可核验机制：

- 玩家在 Work 菜单中指定殖民者允许做哪些工作。
- 手动优先级模式下，每个殖民者的每类工作可以设为 1 到 4，空白表示不做。
- Wiki 明确提示：高优先级工作会先完成；把不擅长的人放到错误工作会拖慢进度。

迁移到修仙：

- “排班令”是已验证机制：谁修炼、谁采药、谁炼丹，本质就是 RimWorld 的工作分配。
- 弟子属性应该直接映射岗位效率，否则玩家无法判断谁该去哪里。
- 不要把排班做成点击生产按钮；应该是常备命令，弟子每天自动执行。

不要照搬：

- RimWorld 的任务类别很多，MVP 不需要十几种工作。
- RimWorld 有情绪、事故、伤病、火灾；MVP 先不要做惩罚模型。

### Dwarf Fortress

来源：[Dwarf Fortress Wiki: Labor](https://dwarffortresswiki.org/index.php/Labor)、[Dwarf Fortress Wiki: Work orders](https://dwarffortresswiki.org/index.php/Work_orders)

可核验机制：

- 矮人是半自治个体，通常满足自身需求，也会在有能力时接工作。
- 大多数工作对应 labor，玩家可以限制某些矮人能否做某类 labor。
- work details 可以设为“只有选中的人做”“所有人做”“没人做”。
- Work orders 是更高层的管理命令，用于维护堡垒生产。

迁移到修仙：

- 宗主不是亲自执行者，而是许可、禁止、指定谁做什么。
- “常备命令”和“具体裁决”应该分层：岗位排班是常备；赐丹、突破是具体裁决。
- 资源生产可以后续加自动工单，但不是 MVP 起点。

不要照搬：

- Dwarf Fortress 的 labor/work order 系统过深，直接照搬会变成管理 UI 负担。
- MVP 只需要“弟子—岗位”的单层分配。

### Oxygen Not Included

来源：[ONI Wiki: Priority](https://oxygennotincluded.wiki.gg/wiki/Priority)、[ONI Wiki: Errand](https://oxygennotincluded.wiki.gg/wiki/Errand)

可核验机制：

- Duplicant 执行 colony 中的 errands。
- errands 可通过 priority panel 和 sub-priority overlay 调整。
- 高优先级任务先于低优先级任务。

迁移到修仙：

- “宗门任务”可以理解为 errands：采药、炼丹、闭关都是可被安排的任务。
- UI 必须解释为什么某弟子今天做了这件事，否则半自治会变黑箱。

不要照搬：

- ONI 的局部优先级、距离、子优先级很强，但会让修仙 MVP 过早复杂化。
- 先用每日离散结算，不做地图寻路和距离成本。

## 2. 低频资源裁决：把资源投给具体个体已被角色养成验证

### Fire Emblem: Three Houses

来源：[Nintendo 官方页](https://www.nintendo.com/au/games/nintendo-switch/fire-emblem-three-houses/)、[Nintendo 101 指南](https://www.nintendo.com/au/news-and-articles/fire-emblem-three-houses-101/)、[Fire Emblem Wiki: Lesson](https://fireemblemwiki.org/wiki/Lesson)

可核验机制：

- 官方定位是玩家作为教师，带领学生的学院生活和战场表现。
- 教授等级影响 Activity Points，并影响课堂中可指导学生的数量。
- Lesson 系统让玩家在每周课程中给单位增加技能经验，既有具体指导，也有目标和 group tasks。

迁移到修仙：

- “宗主指导弟子闭关/赐丹”对应 Three Houses 的教师指导学生。
- 资源限制不是丹药本身也可以是“宗主注意力/指导次数”；MVP 可先用丹药数量表达。
- 玩家应该能偏心培养某个弟子，这种偏心是养成乐趣，不是系统漏洞。

不要照搬：

- 不要做完整学院日历、好感、散步、考试路线。
- 只迁移“有限指导次数 + 指定个体成长方向”。

### Darkest Dungeon

来源：[Official Darkest Dungeon Wiki: Heroes](https://darkestdungeon.wiki.gg/wiki/Heroes_%28Darkest_Dungeon%29)、[Trinkets](https://darkestdungeon.wiki.gg/wiki/Trinkets_%28Darkest_Dungeon%29)、[Camping](https://darkestdungeon.wiki.gg/wiki/Camping)

可核验机制：

- 每个英雄有职业、属性、战斗技能、露营技能，且同时只能启用有限数量的技能。
- Trinkets 通过冒险、任务奖励或商店获得，并装备给具体英雄。
- 露营技能等准备行为服务于一次 expedition。

迁移到修仙：

- 丹药/法器/突破材料应当是“宗主指定给谁”的资源，而不是平均自动分配。
- “给谁资源、派谁出门、带什么准备”是已验证的核心裁决。

不要照搬：

- Darkest Dungeon 的压力、永久死亡、病症适合中后期，不适合第一轮宗门雏形。
- MVP 只迁移“稀缺资源绑定具体角色”的裁决感。

## 3. 增量骨架：资源链、加速、突破来自 idle/incremental

### Progress Knight

来源：[Progress Knight GitHub 源码](https://github.com/ihtasham42/progress-knight/blob/main/index.html)

可核验机制：

- 重生会失去等级和金币，但获得基于最高等级的 XP multipliers。
- 页面说明中明确写到重新学习会比前一世更快。
- evil reset 会进一步重置并开启新技能线。

迁移到修仙：

- “突破”就是小转生：修为归零，境界提高，旧阶段变快。
- 每次突破必须让玩家肉眼感到旧资源链加速，否则增量爽点不成立。

不要照搬：

- Progress Knight 是单角色自动成长；本项目要保留“宗主分配多个弟子时间”的组织感。

### Idle game 数学

来源：[GDC Vault: Quest for Progress - The Math and Design of Idle Games](https://gdcvault.com/play/1023863/Quest-for-Progress-The-Math)、[PDF](https://media.gdcvault.com/gdceurope2016/presentations/Pecorella_Anthony_Quest%20for%20Progress.pdf)

可核验机制：

- Prestige currency 常用 log 或 fractional exponent（如平方根）压缩增长。
- 设计需要保证玩家定期到达有价值的 prestige 点。
- 下一轮早期是否更快，是玩家感到力量成长的重要指标。

迁移到修仙：

- 境界突破和宗门传承应先保证“下一轮更快”，再谈复杂外壳。
- `core-architecture.md` 里的“突破后旧阶段加速”是正确方向。

不要照搬：

- 不要先上大转生和复杂 prestige 公式；MVP 只做一层炼气 → 筑基。

### Melvor Idle

来源：[Melvor Idle Steam](https://store.steampowered.com/app/1267910/Melvor_Idle/)、[Melvor Wiki: Offline Progression](https://wiki.melvoridle.com/w/Offline_Progression)、[Melvor Wiki: What to level first](https://wiki.melvoridle.com/w/What_to_level_first)

可核验机制：

- Steam 页把 Melvor 描述为以 RuneScape 为灵感的 idle/incremental，包含大量技能、物品、银行/库存和离线进度。
- Wiki 说明游戏关闭后会按在线一样计算离线进度，并有离线时长上限。
- 新手路线强调技能之间互相供给。

迁移到修仙：

- 草药 → 丹药 → 修为/突破 是 Melvor 式资源互锁的最小修仙版本。
- 库存资源必须可读，玩家要知道瓶颈在哪里。

不要照搬：

- Melvor 技能数量很大，MVP 不要做百艺集合。
- 先验证 2 个资源、3 个岗位；资源互锁成立后再扩。

## 4. 经营节奏：开罗验证了低频决策和可观察反馈

### Game Dev Story

来源：[Google Play: Game Dev Story](https://play.google.com/store/apps/details?hl=en_US&id=net.kairosoft.android.gamedev3en)

可核验机制：

- 玩家经营游戏公司，开发百万销量游戏。
- 员工有不同职业，玩家可以改变员工职业。
- 有自研主机等中长期目标。

迁移到修仙：

- 宗门可以像小公司：弟子是员工，岗位是职业，突破是晋升。
- 每个周期玩家做少量关键安排，然后等项目/弟子执行。

不要照搬：

- 不要先做大量项目类型、组合和职业树。

### Dungeon Village

来源：[Google Play: Dungeon Village](https://play.google.com/store/apps/details?hl=en_US&id=net.kairosoft.android.bouken_en)

可核验机制：

- 玩家建设 RPG 小镇，冒险者会来访并自动打怪赚钱。
- 玩家建设设施、补货商店、训练冒险者，吸引居民定居。

迁移到修仙：

- “宗门活着”来自弟子自动执行命令、日志复命和资源变化，不一定先需要复杂画面。
- 低频关键决策优先于高频点击。

不要照搬：

- 可视小人是增强项，不是第一轮核心。
- 先做文本/面板里的命令—执行—复命闭环。

## 5. 修仙外壳已经被竞品验证，但要避开同质化

已有项目研究见：[修仙放置/增量游戏竞品研究](./cultivation-idle-competitors.md)。

关键来源：

- [最强祖师 TapTap](https://www.taptap.cn/app/238626)：玩家是祖师，招募弟子，并安排仙田、炼丹、炼器、挖矿等生产职位。说明“祖师/宗主 + 弟子岗位”是可沟通的修仙经营语义。
- [Cultivation Idle Steam](https://store.steampowered.com/app/4727230/Cultivation_Idle/)：强调长期成长、资源规划、丹药、生活技能和离线收益。说明“修仙 + idle/incremental 资源规划”成立。
- [Idle Cultivation Steam](https://store.steampowered.com/app/3697240/Idle_Cultivation/)：包含修炼、战斗、掉落、生活技能、轮回和宗门。说明修仙外壳天然能容纳技能、丹药、轮回。
- [Amazing Cultivation Simulator Steam](https://store.steampowered.com/app/955900/Amazing_Cultivation_Simulator/)：强调重建宗门、训练弟子、管理、探索、政治与随机地图。说明深修仙模拟成立，但复杂度远超 MVP。

迁移判断：

- 可以借“宗主/祖师、弟子、岗位、丹药、闭关、突破”的语义。
- 不要走抽卡弟子、跨服战力、深模拟事故这三条线。
- 我们的空位是“可读资源链 + 弟子分工 + 宗主低频裁决”。

## 6. 对 `core-architecture.md` 的修改方向

### 保留

- “弟子·天是唯一原生稀缺资源”。
- “确定性、可归因、日志带数值”。
- “资源链 + 倍率 + 门槛”。
- “突破后旧阶段明显变快”。
- “先收益差，不做事故惩罚”。

### 修改

- 把核心循环从“排班调阵”改成“议事 → 下令 → 执行 → 复命 → 裁决 → 破境”。
- 把“优先突破者”改成“闭关目标/亲传培养对象”，它只负责目标追踪和 UI 高亮，不自动吃资源。
- 明确三类宗主权力：
  1. 常备命令：排班令，持续执行。
  2. 资源裁决：赐丹令，指定个体消耗丹药换成长。
  3. 突破裁决：突破令，资源满足后批准某人破境。
- 阶段 0 从旧 HTML MVP 改为“宗主令雏形”。

### 第一版雏形

- 3 名弟子。
- 3 个岗位：修炼、采药、炼丹。
- 2 个资源：材料、丹药。
- 1 条境界线：炼气 → 筑基。
- 3 个命令：排班令、赐丹令、突破令。
- 1 个复命日志。

验收问题：

> 玩家是否能在 10 分钟内说清楚：我为什么把这颗丹药给甲，而不是给乙？

如果能，宗主裁决成立；如果不能，就说明还是平均挂机，不能加功法、设施、历练。

## 7. 不要现在做

- 不做弟子个人背包。
- 不做多种丹药。
- 不做命令队列。
- 不做弟子不满、心魔、死亡、叛门。
- 不做完整功法/设施/历练队伍。
- 不做自动分配策略。

这些不是永远不做，而是等“单颗赐丹 + 单次突破”已经验证太轻、太频繁或太单调时再加。

