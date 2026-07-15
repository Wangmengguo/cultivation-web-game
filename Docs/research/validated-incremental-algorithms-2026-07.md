# 已验证的增量游戏算法研究

状态：Current Research

日期：2026-07-15

## 研究结论

本项目采用以下机制作为低风险起点：

1. Kittens Game 与 Evolve Idle 的有限人口和岗位调度。
2. Kittens Game 的净产出、容量和几何设施成本。
3. Melvor Idle 的跨技能资源消耗链与完整动作结算。
4. Antimatter Dimensions 的批量成本、规则里程碑和旧层自动化。

这些机制有公开源码、官方 Wiki 或长期市场反馈支持。它们证明结构可以成立，不证明本项目的参数、内容和任务驱动组合已经成立。

## 人口岗位

Evolve Idle 的公开实现维护各岗位工人数，调入一个岗位时从可用人口中扣除，调出时返还。Kittens Game 的官方 Wiki 说明人口被分配到岗位后持续产生对应资源。

推荐抽象：

```text
岗位产速(j) = Σ单位贡献 × 岗位设施 × 全局修正
资源净速(r) = Σ产出(r) - Σ消耗(r)
```

来源：

- [Kittens Game Jobs](https://wiki.kittensgame.com/en/game-tabs/outpost)
- [Evolve jobs.js](https://github.com/pmotschmann/Evolve/blob/3436358dcd03d9f9e071d51ea071e0a78c0322e4/src/jobs.js#L579-L609)

## 加工动作

消耗型加工不能简单用产速乘时间，因为输入可能在中途耗尽。

```text
计划动作数 = floor((累计进度 + 时间增量) / 动作时长)
可执行动作数 = min(计划动作数, floor(各输入库存 / 单次输入))
```

来源：

- [Melvor Idle Beginner Guide](https://wiki.melvoridle.com/index.php?title=Beginners_Guide)
- [Melvor Idle Steam](https://store.steampowered.com/app/1267910/Melvor_Idle/)

## 几何成本

Kittens Game 的设施广泛采用基础成本乘以 `priceRatio` 的幂。Antimatter Dimensions 的 MIT 源码包含对应的批量购买计算。

```text
下一件成本 = C0 × m^n
购买 p 件总成本 = C0 × (m^p - 1) / (m - 1)
```

来源：

- [Kittens Game Building Cost](https://github.com/nuclear-unicorn/kittensgame/blob/117c60d48b53b5b4637fa767c6cfae0289ff90b3/js/buildings.js#L2353-L2401)
- [Antimatter Dimensions LinearCostScaling](https://github.com/IvarK/AntimatterDimensionsSourceCode/blob/8ae221fcb07db667b3d04114c2b977175966611d/src/core/math.js#L256-L295)

## 里程碑与自动化

成熟增量游戏不会只提供连续的小百分比。关键等级会解锁自动购买、保留旧层或新的生产规则。自动化通常发生在玩家已经手动掌握旧层以后。

来源：

- [Antimatter Dimensions Eternity Milestones](https://github.com/IvarK/AntimatterDimensionsSourceCode/blob/8ae221fcb07db667b3d04114c2b977175966611d/src/core/secret-formula/eternity/eternity-milestones.js#L1-L151)
- [Melvor Idle Mastery Rework](https://wiki.melvoridle.com/w/V0.17)

## Kittens Game 初期结构

Kittens Game 的开局顺序是手动采集、第一种自动设施、人口出现、岗位分配、科学解锁、加工和容量扩张。它用渐进披露减少开局信息量，同时让每次新控制权都解决一个已经出现的问题。

来源：

- [Catnip](https://wiki.kittensgame.com/en/general-information/resources/catnip)
- [Game Tabs](https://wiki.kittensgame.com/en/game-tabs)
- [Resources](https://wiki.kittensgame.com/en/general-information/resources)

## 许可证

Kittens Game 仓库的 WET PAWS LICENSE 禁止以其软件获利并限制衍生使用。本项目只研究通用机制，不复制其源码、资源、文本和内容结构。

来源：[Kittens Game License](https://github.com/nuclear-unicorn/kittensgame/blob/117c60d48b53b5b4637fa767c6cfae0289ff90b3/license.txt)

Evolve Idle 为 MPL-2.0，Antimatter Dimensions 为 MIT。当前研究不等于决定引入任何源码。
