# 增量玩法研究与材料边界

状态：Current

## 综合结论

当前最可靠的玩法骨架不是新发明的概率模型，而是：

```text
有限人口重配
→ 基础资源与加工资源互相依赖
→ 几何成本迫使玩家选择投资时机
→ 规则里程碑改变生产方式
→ 自动化接管旧问题
→ 任务和事件迫使玩家重新安排增长路线
```

前三项和自动化已有成熟作品长期实现。最后一项是本项目的差异，也是最需要独立验证的部分。

## Kittens Game 的开局结构

Kittens Game 开局逐步建立控制权：手动采集基础资源；建造第一种自动设施；让基础资源同时承担生存、建设和加工用途；引入可分配岗位的人口；再逐步解锁科学、容量、加工与更多岗位。容量、季节和人口消耗共同制造安全线，阻止玩家只看总量上涨。

值得迁移的是“手动产出 → 自动设施 → 人口 → 岗位 → 加工 → 容量与科技”的解锁顺序。具体资源、美术文本、代码和完整内容树不进入本项目。

一手入口：[Catnip](https://wiki.kittensgame.com/en/general-information/resources/catnip)、[Game Tabs](https://wiki.kittensgame.com/en/game-tabs)、[Outpost Jobs](https://wiki.kittensgame.com/en/game-tabs/outpost)、[Resources](https://wiki.kittensgame.com/en/general-information/resources)、[Catnip Field](https://wiki.kittensgame.com/en/game-tabs/bonfire/catnip-field)。

## 三组算法基础

### 人口岗位与净产出

Kittens Game 与 Evolve Idle 把人口作为有限、可重配的生产单位。给一个岗位增加人口，意味着其他岗位可用人口减少。

```text
岗位产速 = 单位产出之和 × 设施修正 × 全局修正
资源净速 = 总产出 - 总消耗
```

### 跨资源链与完整动作

Melvor Idle 展示了基础采集、加工和用途如何长期互相支撑。消耗型加工受完整动作和库存约束：

```text
可执行次数 = min(时间允许次数, 各输入允许次数)
```

这可以防止负库存，并把停工原因变成玩家能够处理的瓶颈。

### 几何成本、里程碑与自动化

Antimatter Dimensions 等作品用递增成本控制投资节奏，用关键里程碑提供规则跃迁：

```text
下一件成本 = 基础成本 × 成本倍率^已拥有数量
```

小升级提供平滑增长，关键等级提供新转化、保留规则或自动化，使玩家逐步停止手动处理旧层。

## 本项目新增的任务压力

任务与事件不能成为独立抽奖层。它们需要进入以下计算：剩余期限、当前净速能否交付、临时岗位转移、投资能否及时回本、库存保留牺牲的长期增长，以及结果如何改变后续资源网络。

## 当前材料

| 研究记录 | 用途 |
| --- | --- |
| [validated-incremental-algorithms-2026-07.md](research/validated-incremental-algorithms-2026-07.md) | 人口岗位、净产出、加工动作、几何成本、里程碑和自动化 |
| [web-incremental-frameworks-2026-07.md](research/web-incremental-frameworks-2026-07.md) | Web 底座事实、许可证和维护风险，不直接决定选型 |
| [profectus-0.7-grok-verification-2026-07.md](research/profectus-0.7-grok-verification-2026-07.md) | Profectus 的安装、Node、维护、供应链和长期适配复核 |

其他研究对象包括 Kittens Game 官方 Wiki 与仓库、Evolve Idle 官方仓库、Melvor Idle 官方页面与 Wiki，以及 Antimatter Dimensions 官方页面与 MIT 源码。

## 许可证与输入边界

- Kittens Game 只作为公开机制研究对象，不复制其源码、内容或受保护表达作为商业衍生基础。
- Evolve Idle 的 MPL-2.0 代码若未来需要使用，必须单独审查文件级义务；当前只研究实现。
- Antimatter Dimensions 的 MIT 源码可以依法研究和复用，但当前仍优先独立实现通用公式。
- 任何未来依赖进入项目前，都要记录版本、许可证、来源和替换成本。
- [archive/](archive/) 下所有文件均为失效试验或已合并资料，不得提供当前人物、事件、数值、阶段结论或技术栈。

## 研究尚未证明的事情

现有研究不能证明三组机制组合后一定好玩，不能证明任务压力一定优于纯增量节奏，也不能给出本项目的具体成本倍率、时间长度、资源规模、长期重置结构或技术底座。这些问题必须依次通过完整框架、开局纸面段、高潮纸面段和全程数值模型验证。
