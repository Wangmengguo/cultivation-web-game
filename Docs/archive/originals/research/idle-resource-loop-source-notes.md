# Idle / Incremental 已验证玩法来源笔记

研究日期：2026-07-09

目的：为 `docs/design/core-architecture.md` 的下一轮修订提供来源依据。本文只记录“已被其它游戏验证过、可套修仙宗门外壳”的玩法结构，不提出全新玩法。

## 结论

下一版核心玩法不要定义成“我们发明一个宗主令系统”，而应定义成：

> 把已验证的 idle / incremental 玩法——岗位分配、资源链加工、技能互锁、离线延续、prestige 兑现、阵容/个体配置——换成修仙宗门语义。

对应到修仙外壳：

| 已验证机制 | 已验证样本 | 修仙宗门表达 |
| --- | --- | --- |
| 单位时间是根稀缺资源 | Kittens Game、Evolve、IdleOn | 弟子·天 |
| 人/角色被分配到岗位，岗位产不同资源 | Kittens Game、Evolve、IdleOn | 排班令：修炼、采药、炼丹、历练 |
| 资源加工链比单资源堆叠更耐玩 | Melvor Idle、Kittens Game、Evolve | 草药 → 丹药 → 修为加速 / 突破 |
| 技能互锁，练 A 会服务 B | Melvor Idle、Progress Knight | 功法/境界/设施给岗位倍率 |
| 离线不是领奖，而是延续同一套生产逻辑 | Melvor Idle、IdleOn、Progress Knight 变体 | 宗门离线运转，回来读复命摘要 |
| prestige 让旧阶段明显变快 | Progress Knight、Kittens Game、Evolve | 弟子突破 / 宗门传承 |
| 多角色配置比单纯战力更有决策 | IdleOn、Idle Champions | 宗主决定谁闭关、谁生产、谁出战 |

因此，“宗主令”不是新玩法；它是这些已验证机制在修仙题材里的交互文案和权力语义：

- assign worker → 下达岗位令
- choose active skill/job → 指定弟子修炼/历练
- spend consumable on target → 赐丹
- prestige/reset → 批准突破 / 宗门传承
- formation strategy → 历练队伍编成

## 设计约束

1. 先迁移成熟结构，不先做创新系统。
2. 玩家操作必须落到“分配有限资源给具体对象”，不能只做平均自动增长。
3. 高频重复事务用常备命令，低频高价值事务用逐次裁决。
4. 每个新系统都要回答：它消耗谁的时间？产出什么资源？服务哪个突破/解锁？
5. 先做小闭环：3 名弟子、3 个岗位、2 个资源、1 次突破。不要先做全宗门模拟。

## 样本证据

### 1. Progress Knight：工作 + 技能 + 重生倍率

来源：

- [Progress Knight GitHub README](https://github.com/ihtasham42/progress-knight)
- [Progress Knight 在线版](https://ihtasham42.github.io/progress-knight/)
- [源码 `js/main.js`](https://github.com/ihtasham42/progress-knight/blob/main/js/main.js)
- [源码 `js/classes.js`](https://github.com/ihtasham42/progress-knight/blob/main/js/classes.js)

已验证结构：

- 玩家选择当前工作和当前技能。
- 工作提供收入，技能提供各种 XP、收入、寿命、开销等倍率。
- 解锁以岗位/技能/年龄/资源门槛推进。
- 生命结束后 prestige，等级与资产重置，但获得下一轮 XP 倍率。
- README 明确说明：下一世会比上一世更快重新获得等级。

源码要点：

- `jobBaseData` 定义工作收入和 XP 门槛。
- `skillBaseData` 定义技能效果，如 `Skill xp`、`Job xp`、`Expenses`、`Gamespeed`、`All xp`。
- `Task.getMaxXp()` 使用等级增长的 XP 需求。
- `Task.getMaxLevelMultiplier()` 用历史最高等级提供倍率。
- `Job.getLevelMultiplier()` 用 `1 + log10(level + 1)` 控制岗位收入倍率。
- `Requirement` 系列用任务等级、金币、年龄、evil 门槛控制解锁。

迁移到修仙：

- 工作 = 岗位令：修炼、采药、炼丹、历练。
- 技能 = 功法 / 设施 / 境界倍率。
- 重生 = 突破：修为归零，但境界和倍率保留。
- “旧阶段飞快”是突破后必须兑现的手感。

不迁移：

- 单角色人生模拟视角。
- 纯菜单文字感。
- 自动选择优先项代替玩家裁决。

### 2. Melvor Idle：技能互锁 + 银行库存 + 离线进度

来源：

- [Melvor Idle Steam 官方页](https://store.steampowered.com/app/1267910/Melvor_Idle/)

已验证结构：

- Steam 页把它定义为 feature-rich idle/incremental，并强调 20+ 技能。
- 官方描述明确写到：每个技能都有用途，并以有趣方式与其它技能互动；训练一个技能会反过来帮助其它技能。
- 官方页列出 15 个非战斗技能、8 个战斗技能、银行/库存系统、1100+ 物品和离线进度。
- 这说明“资源规划 + 技能互锁 + 长期离线进展”是成熟玩法，不是新设想。

迁移到修仙：

- 技能互锁 = 修炼、采药、炼丹、炼器、灵田等互相喂养。
- 银行库存 = 宗门库房。
- 非战斗技能 = 宗门生产岗位。
- 战斗/副本 = 历练，后置到第二阶段。

下一版雏形只需要迁移 Melvor 的最小骨架：

```text
采药产材料 → 炼丹消耗材料产丹药 → 丹药服务修炼/突破
```

不迁移：

- 20+ 技能。
- 大量物品图鉴。
- 复杂装备和宠物。

### 3. Kittens Game：人口岗位 + 工程师自动加工 + 设计原则

来源：

- [Kittens Game GitHub 仓库](https://github.com/nuclear-unicorn/kittensgame)
- [Kittens Game Wiki：Outpost/jobs](https://wiki.kittensgame.com/en/game-tabs/outpost)
- [Kittens Game Wiki：Engineer](https://wiki.kittensgame.com/en/game-tabs/outpost/engineer)
- [源码 `js/village.js`](https://github.com/nuclear-unicorn/kittensgame/blob/master/js/village.js)
- [源码 `js/workshop.js`](https://github.com/nuclear-unicorn/kittensgame/blob/master/js/workshop.js)

已验证结构：

- wiki 说明 kittens 可以被分配到不同 job 来获得资源，分配越多，该资源采集越快。
- engineer 是一种岗位：它们在工厂工作，把手动 craft 的资源加工自动化；但仍消耗同样资源，产出同样物品。
- 源码 `village.js` 中 jobs 包括 woodcutter、farmer、scholar、hunter、miner、priest、engineer。
- 每只 kitten 有 job skill 和 rank；生产受技能、leader、happiness 等影响。
- README 的设计原则很适合本项目：复用既有建筑和资源；主动操作可以鼓励但不应是必须；每个瓶颈应有多种解决方式；每个解决方案应制造新问题；新机制应留在既有规则内。

迁移到修仙：

- kittens = 弟子。
- job assignment = 排班令。
- engineer 自动加工 = 炼丹岗位，不是玩家每次手搓丹药。
- leader / rank / skill = 亲传、境界、岗位熟练度。
- “复用既有资源，不乱加新资源”应作为宗门系统扩展原则。

对当前玩法的启发：

- 常规产出可以自动化，但自动化的是岗位执行，不是稀缺资源裁决。
- 炼丹可以是常备岗位；“把成品丹给谁吃”应是宗主逐次裁决。

不迁移：

- 超深后期资源规模。
- 大量稀有资源和随机事件。

### 4. Evolve Idle：人口岗位 + 复杂资源链 + 多层 prestige

来源：

- [Evolve GitHub 仓库](https://github.com/pmotschmann/Evolve)
- [Evolve 官方 GitHub Pages](https://pmotschmann.github.io/Evolve/)
- [源码 `src/jobs.js`](https://github.com/pmotschmann/Evolve/blob/master/src/jobs.js)
- [源码 `src/resources.js`](https://github.com/pmotschmann/Evolve/blob/master/src/resources.js)
- [源码 `src/resets.js`](https://github.com/pmotschmann/Evolve/blob/master/src/resets.js)

已验证结构：

- README 定义它是 clicker + idler，并有大量 micromanagement。
- `jobs.js` 里有 farmer、lumberjack、quarry_worker、miner、craftsman、cement_worker、scientist 等大量岗位。
- `resources.js` 定义 Food、Lumber、Stone、Furs、Copper、Iron、Cement、Coal、Steel、Titanium、Alloy 等资源，并处理 craft cost / crafting ratio。
- `resets.js` 显示多种重置路线会给 Plasmid、Phage、Dark、Harmony、Artifact 等 prestige 资源。
- README 的贡献规则强调：新功能不能只是让游戏变简单；如果它让某件事更容易，必须让其它东西更难。

迁移到修仙：

- 人口岗位 = 弟子排班。
- 多资源链 = 凡材、灵材、丹药、设施材料。
- 多层 prestige = 弟子突破（小转生）+ 宗门传承（大转生）。
- “新功能不能只降难度”适合用于功法/设施设计：每个系统都要带来新取舍。

不迁移：

- 文明演化规模。
- 巨量资源和多重宇宙重置。
- 大量 micromanagement。

### 5. Legends of IdleOn：多角色同时 AFK

来源：

- [IdleOn 官方站](https://www.legendsofidleon.com/)
- [IdleOn Steam 官方页](https://store.steampowered.com/app/1476970/IdleOn/)
- [IdleOn Google Play 官方页](https://play.google.com/store/apps/details?id=com.lavaflame.MMO)

已验证结构：

- 官方站定位为“Idle MMO where you’re in Control”，玩家组建多个角色；角色在玩家离开后继续游玩。
- Steam 页强调 MMO power fantasy + incremental automation：玩家主动买升级、优化技能树和属性，第二天回来花掉 AFK 收益再买更多升级。
- Steam 页说明可创建 11 个角色，每个角色在账号内承担不同角色；还列出 Cooking、Woodcutting、Alchemy、Construction 等非战斗技能。
- Google Play 页明确说：不同于其它 idle，玩家会创建更多角色，它们同时 AFK 工作。

迁移到修仙：

- 多角色 = 多弟子。
- 每个角色在账号中承担不同角色 = 每个弟子在宗门中承担岗位/路线。
- 同时 AFK 工作 = 离线时所有弟子按宗主排班继续执行。

对当前玩法的启发：

- 多弟子不是为了“更多卡片”，而是为了让玩家分配并行生产力。
- 宗主权力感来自“我让谁去做什么”，不是单角色面板成长。

不迁移：

- MMO 社交。
- 大量职业/技能树。
- 多年内容体量。

### 6. Idle Champions：阵容位置 + 角色协同

来源：

- [Idle Champions 官方站](https://www.idlechampions.com/)

已验证结构：

- 官方站将其定义为 D&D strategy management game。
- 官方文案明确说：每解锁一个 Champion，新的 formation strategies 会出现；玩家要掌握这些策略来完成冒险。
- 这证明“自动战斗/放置增长”里仍然可以把核心决策放在角色组合、站位和协同上。

迁移到修仙：

- Champion formation = 历练队伍编成。
- 角色协同 = 弟子属性、功法、站位、法器互相影响。
- 自动冒险 = 历练自动演算 + 战报复盘。

下一版暂不做：

- 阵容站位。
- 大量角色池。
- 复杂 buff 网络。

保留到阶段 2，因为当前最需要先验证“宗主分配资源和时间”。

## 可直接用于下一版雏形的成熟玩法组合

基于以上样本，阶段 0 应该只迁移这个最小组合：

```text
多弟子并行（IdleOn / Kittens）
  → 每名弟子只能做一个岗位（Kittens / Evolve）
  → 岗位形成短资源链（Melvor / Evolve）
  → 成品资源由玩家指定给个体（资源分配决策）
  → 满足门槛后进行突破（Progress Knight / prestige）
  → 突破后旧阶段明显变快（Progress Knight / prestige）
  → 离线按同一排班快进（Melvor / IdleOn）
```

修仙命名：

```text
议事
  → 排班令：修炼 / 采药 / 炼丹
  → 宗门运转：按日结算
  → 复命：日志说明产出和瓶颈
  → 赐丹令：宗主把丹药给指定弟子
  → 突破令：资源满足后批准指定弟子破境
  → 境界倍率让旧岗位更快
```

## 对 `core-architecture.md` 的修改方向

1. 一句话定位保留 Progress Knight / FM / 开罗，但补一句：核心不是原创玩法，而是“成熟 idle 机制的宗门语义化”。
2. “优先突破者”改为“培养对象 / 闭关目标”，只做追踪和提示，不自动消耗丹药。
3. “赐丹令”不是新增复杂系统，而是成熟资源分配玩法在修仙里的表达。
4. “突破令”不是新玩法，而是 prestige/reset 的修仙化表达。
5. 阶段 0 从旧 `index.html` MVP 改为“宗主令雏形”：3 弟子、3 岗位、2 资源、1 次突破。
6. 阶段 1 之后再加功法、设施和更长资源链，遵守 Kittens/Evolve 的原则：复用已有资源，每个解决方案制造新瓶颈。

## 暂不采用的方向

- 不做抽卡：这是竞品《最强祖师》的强势方向，不是当前差异化。
- 不做深模拟事故：这会靠近《了不起的修仙模拟器》，内容和 QA 成本不匹配。
- 不做纯自动喂丹：会削弱“玩家分配资源”的核心决策。
- 不做 20+ 技能：Melvor 证明方向成立，但不是阶段 0 的体量。
- 不做复杂阵容：Idle Champions 证明阵容策略成立，但应放到历练阶段。

## 来源索引

- Progress Knight GitHub：<https://github.com/ihtasham42/progress-knight>
- Progress Knight 在线版：<https://ihtasham42.github.io/progress-knight/>
- Melvor Idle Steam：<https://store.steampowered.com/app/1267910/Melvor_Idle/>
- Kittens Game GitHub：<https://github.com/nuclear-unicorn/kittensgame>
- Kittens Game Wiki Jobs：<https://wiki.kittensgame.com/en/game-tabs/outpost>
- Kittens Game Wiki Engineer：<https://wiki.kittensgame.com/en/game-tabs/outpost/engineer>
- Evolve GitHub：<https://github.com/pmotschmann/Evolve>
- Evolve 在线版：<https://pmotschmann.github.io/Evolve/>
- IdleOn 官方站：<https://www.legendsofidleon.com/>
- IdleOn Steam：<https://store.steampowered.com/app/1476970/IdleOn/>
- IdleOn Google Play：<https://play.google.com/store/apps/details?id=com.lavaflame.MMO>
- Idle Champions 官方站：<https://www.idlechampions.com/>
- Anthony Pecorella, The Math of Idle Games Part I：<https://www.gamedeveloper.com/design/the-math-of-idle-games-part-i>
- Anthony Pecorella, The Math of Idle Games Part II：<https://www.gamedeveloper.com/game-platforms/the-math-of-idle-games-part-ii>
- Anthony Pecorella, The Math of Idle Games Part III：<https://www.kongregate.com/pages/the-math-of-idle-games-part-iii>
