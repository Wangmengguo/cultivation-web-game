# 已验证玩法到“宗主令”的转译依据

研究日期：2026-07-09

## 0. 结论

当前项目不需要发明一个新玩法。更稳的方向是把几类已经被验证过的玩法，翻译成修仙宗门语义：

1. **殖民地/管理游戏的工作分配** → 宗主给弟子下达长期岗位令。
2. **RPG/战术游戏的稀缺资源定向投入** → 宗主赐丹、赐法器、指定培养对象。
3. **idle/incremental 的资源链与离线推进** → 宗门按既定命令自动运转，回来读复命。
4. **prestige/转生的“旧阶段变快”** → 弟子突破后回头做旧阶段任务明显更快。

因此核心玩法应表述为：

```text
宗主查看瓶颈
→ 下达岗位令/赐丹令/突破令
→ 弟子按令执行
→ 日志复命并显示数值变化
→ 宗主调整资源偏向
→ 指定弟子突破并加速旧阶段
```

这不是“自动最优培养”，也不是“平均挂机”。玩家做的是被验证过的管理动作：选择谁做什么、资源给谁、何时兑现突破。

## 1. 已验证模式 A：工作/岗位分配

| 来源 | 已验证机制 | 可转译到本项目 |
| --- | --- | --- |
| [RimWorld Wiki: Work](https://rimworldwiki.com/wiki/Work) | 玩家可以给每个 colonist 设置工作类型与手动优先级；同优先级下按工作顺序执行。 | “排班令”不是装饰，而是核心操作：每名弟子长期执行一个岗位，玩家通过优先级/岗位选择分配唯一稀缺资源“弟子·天”。 |
| [Dwarf Fortress Wiki: Labor](https://dwarffortresswiki.org/index.php/Labor) | labor 决定矮人能或不能做哪些工作；个体技能和属性影响效率。 | 弟子岗位应是“允许/指定他做什么”，不是全员自动均摊。资质/武力/专长只需要影响岗位效率即可产生选择。 |
| [Oxygen Not Included Wiki: Errand](https://oxygennotincluded.wiki.gg/wiki/Errand) | duplicants 执行 colony errands；任务可通过 priority panel 和 sub-priority overlay 调优。 | “宗主令”可以分层：长期岗位令负责日常运转，特殊命令负责低频裁决。不要把每件事都做成逐次点击。 |
| [Majesty Gold HD Manual PDF](https://cdn.akamai.steamstatic.com/steam/apps/25990/manuals/Majesty_Manual_HD.pdf?t=1447352469) | 英雄有自己的目标，玩家通过 reward flags 间接激励他们攻击或探索。 | 本项目首版不需要“弟子不听话”，但可以学习“下令后等待执行和回报”的节奏：命令不是即时配置，而是宗门事务。 |

设计含义：

- “排班”是已验证玩法，但要写成宗主语义：岗位令、闭关令、炼丹令。
- 第一版不需要复杂 AI。RimWorld/Dwarf/ONI 已证明，简单的“谁能做什么/优先做什么”足以形成管理感。
- 不要做全自动最优分配。自动优化会把玩家从宗主降级成旁观者。

## 2. 已验证模式 B：稀缺资源定向投入

| 来源 | 已验证机制 | 可转译到本项目 |
| --- | --- | --- |
| [Football Manager 官方：Youth Development / Individual Focus](https://www.footballmanager.com/the-dugout/top-tips-youth-development-fm26) | 玩家可以给具体球员设置 individual focus，训练不是全队平均成长。 | “亲传培养对象/闭关目标”应是具体个体，而非全局自动倍率。 |
| [Darkest Dungeon Wiki: Sanitarium](https://darkestungeon.fandom.com/wiki/Sanitarium) | 玩家在城镇设施里花钱治疗具体英雄的 quirks/diseases；资源与恢复名额绑定到个体。 | 丹药、闭关名额、突破资源都应该定向给某个弟子，形成“我为什么给他”的偏心决策。 |
| [XCOM Wiki: Soldier (XCOM 2)](https://xcom.fandom.com/wiki/Soldier_%28XCOM_2%29) | 士兵属性随晋升提升，并受装备/物品/状态影响。玩家在任务前给具体士兵配置装备和物品。 | 法器、丹药、护身符等都可以是后续定向供给；首版只需要“赐丹令”。 |
| [Fire Emblem: Three Houses 官方站](https://www.nintendo.com/us/store/products/fire-emblem-three-houses-switch/) + 系列常见教学/培养结构 | 玩家以教师/指挥者身份培养具体学生/单位，资源和训练面向个体。 | 修仙外壳下，“宗主—弟子”关系比“系统—角色卡”更强。赐丹、收为亲传、准许突破是同一类个体培养动作。 |

设计含义：

- “丹药自动给优先者吃”不够。稀缺资源的乐趣来自定向投入。
- 第一版只做一种定向资源：丹药。
- 丹药的交互应是“宗主赐予”，不是“弟子自动消耗”。

## 3. 已验证模式 C：idle 资源链与离线运转

| 来源 | 已验证机制 | 可转译到本项目 |
| --- | --- | --- |
| [Melvor Idle Steam](https://store.steampowered.com/app/1267910/Melvor_Idle/) | 官方页强调 20+ 技能、离线进度、云存档、Bank/Inventory、无微交易；核心是长期技能与物品规划。 | 本项目的长期方向应是“宗门账本 + 修真百艺互相喂养”，不是纯战力按钮。 |
| [Melvor Idle Wiki: Offline Progression](https://wiki.melvoridle.com/w/Offline_Progression) | 游戏关闭后仍按在线逻辑推进，最长有上限。 | 宗门离线应按已下命令继续结算，回来给复命摘要；不要做“领取奖励”式红点。 |
| [Kittens Game Wiki: Kittens](https://wiki.kittensgame.com/en/general-information/resources/kittens) | kittens 是可分配人口；不同工作产出不同资源，例如 woodcutting、farmers、scholars、hunters、miners 等。 | “弟子·天”作为唯一原生稀缺资源是正确锚点：弟子分配到不同岗位，宗门资源链随之改变。 |
| [Kittens Game Wiki: Game Tabs](https://wiki.kittensgame.com/en/game-tabs) | Village tab 可分配 jobs，后续可派 hunters 和查看 census。 | 岗位分配先于复杂系统；先让人口/弟子分配成立，再加更高层功能。 |
| [Progress Knight GitHub](https://github.com/ihtasham42/progress-knight) / [Progress Knight playable page](https://ihtasham42.github.io/progress-knight/) | prestige 后旧技能/工作获得 XP multipliers，下一轮重新学习更快。 | 弟子突破应提供“旧阶段变快”的明确兑现：筑基弟子回头采药/修炼/炼丹要明显更快。 |

设计含义：

- 放置不是“什么都自动平均涨”。放置应是“玩家预先安排，系统按安排推进”。
- 宗门离线的最小正确形态：岗位令持续执行；赐丹/突破这类高价值裁决不自动乱用。
- 突破必须让旧资源链变快，否则只是等级文字变化。

## 4. 已验证模式 D：修仙题材已有外壳

| 来源 | 已验证内容 | 对本项目的意义 |
| --- | --- | --- |
| [最强祖师 TapTap](https://www.taptap.cn/app/238626) | 官方页将玩家设为祖师，招募弟子，并安排仙田、炼丹、炼器、挖矿等生产职位；同时强调一键生产/领取等减负。 | “祖师/宗主 + 弟子岗位”已被市场验证。但本项目应避免过度一键化，否则会削弱宗主裁决感。 |
| [了不起的修仙模拟器 Steam](https://store.steampowered.com/app/955900/Amazing_Cultivation_Simulator/) | 官方页强调从零重建宗门、训练弟子、收集功法、贸易、政治、随机地图，并提到受 RimWorld / Dwarf Fortress 启发。 | “修仙宗门管理”已成立，但深模拟成本高。我们只取“管理宗门/训练弟子/资源准备”的骨架。 |
| [修仙家族模拟器 Steam](https://store.steampowered.com/app/2013240/_/?curator_clanid=44066492&l=tchinese) / [App Store](https://apps.apple.com/cn/app/%E4%BF%AE%E4%BB%99%E5%AE%B6%E6%97%8F%E6%A8%A1%E6%8B%9F%E5%99%A8/id1612913355) | 家族、血脉、势力、岛屿、人物关系、修真百艺。 | 长线可走“组织演化”，但首版不做族谱/关系网。 |
| [Cultivation Idle Steam](https://store.steampowered.com/app/4727230/Cultivation_Idle/) | 官方页强调长期成长、资源规划、离线收益、生活技能、灵田、炼丹、锻造、洞府等。 | “修仙 + idle + 资源规划”已存在。差异化应落在宗门多弟子与资源裁决，而不是单角色 build。 |
| [Idle Cultivation Steam](https://store.steampowered.com/app/3697240/Idle_Cultivation/) | 自动成长、修炼/辅助/炼丹/锻造/符箓/采矿/园艺等技能、轮回、宗门建筑。 | 技能堆叠已被验证，但首版不需要堆技能。先验证宗主是否能通过命令偏心培养。 |

设计含义：

- 修仙外壳不是问题，市场已验证。
- 问题是要选择哪类已验证骨架：本项目应选“岗位经营 + 稀缺资源定向 + idle 资源链”，不要选“抽卡战力 + 一键收菜 + 深模拟”。

## 5. 对核心玩法的直接约束

### 必须保留

1. **宗主身份**：玩家只下令、分配、批准，不直接操作弟子。
2. **弟子·天**：弟子时间是唯一原生稀缺资源。
3. **岗位令**：玩家决定谁修炼、谁采集、谁炼丹。
4. **赐丹令**：玩家决定丹药给谁，不允许系统自动喂丹替代裁决。
5. **突破令**：玩家批准哪个弟子突破，突破是宗门事件。
6. **复命日志**：每条状态变化都说明“谁奉什么令，带来什么数值变化”。
7. **确定性与可归因**：首版不靠概率失败制造戏剧性。

### 第一版不要做

1. 多种丹药。
2. 个人背包。
3. 命令队列。
4. 弟子不满/心魔/事故。
5. 复杂战斗或历练队伍。
6. 功法/设施完整系统。
7. 抽卡招募。
8. 多人宗门战。

## 6. 建议的第一版验证问题

第一版不验证“修仙世界是否足够大”，只验证：

> 玩家是否会因为资源有限，主动偏心培养一个弟子，并从复命日志中确认这个偏心改变了宗门进度？

可观测标准：

1. 玩家能说清楚“我为什么赐丹给 A，不给 B”。
2. 玩家能说清楚“我为什么让 C 去产资源，而不是闭关”。
3. 玩家能从日志看出瓶颈是材料、丹药还是修为。
4. 第一次突破后，旧阶段任务明显变快。
5. 离线回来看到的是命令执行摘要，而不是纯资源领取。

如果这些不成立，先调数值、日志和命令粒度，不加系统。

## 7. 对 `core-architecture.md` 的修改方向

1. 把核心循环从“排班调阵”改成“议事 → 下令 → 执行 → 复命 → 裁决 → 破境”。
2. 把“优先突破者”改名为“闭关目标 / 亲传培养对象”，避免自动 buff 语义。
3. 新增“宗主令”概念，分为：
   - 岗位令：长期、可离线执行。
   - 赐丹令：低频、定向资源投入。
   - 突破令：低频、阶段兑现。
4. 阶段 0 改成“宗主令雏形”，不再说旧 MVP 已完成。
5. 明确第一版只套用已验证玩法，不发明新系统。
