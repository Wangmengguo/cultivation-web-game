# 材料台账

状态：Current

## 记录说明

原项目文档没有可靠记录具体由哪个大语言模型生成，因此这些文档的作者统一标为“未记录”，不能根据文风猜测作者。GLM 5.2 和 Sonnet 的独立评审有对应任务文件，可以按其实际模型名称登记。所有旧文件均原样保存在 `archive/originals/`，本台账只判断其内容类型和当前效力。

证据等级用于描述来源强度，不代表本项目玩法已经成立：

| 等级 | 来源类型 |
| --- | --- |
| A | 源码、许可证、官方手册、实际可玩验证 |
| B | 官方商店页、官方网站和官方介绍 |
| C | 社区 Wiki、二手媒体和社区经验 |
| D | 多个来源之上的设计推断或模型综合 |

## 现有材料

| 原始材料 | 主要内容 | 当前状态 | 证据等级 | 适用阶段 | 使用限制 |
| --- | --- | --- | --- | --- | --- |
| [CONTEXT 旧版](./archive/originals/CONTEXT.legacy.md) | 术语、早期边界和具体玩法决定 | 归档，部分可复用 | D | Game Spec | “宗主、弟子、资源裁决”可作输入；岗位数量和优先突破者不是定案 |
| [HANDOFF 旧版](./archive/originals/HANDOFF.legacy.md) | 原型状态、Blazor 实施交接和旧验收目标 | 已被替代 | D | 工程阶段追溯 | `index.html`、Blazor 和时间指标均不能指导当前工作 |
| [旧 MVP 计划](./archive/originals/mvp-prototype-plan.md) | 单文件 HTML 原型规格 | 已否决实验 | D | 原型阶段反例 | 自动吃丹和优先倍率被用户否定；验收写法可参考 |
| [旧核心架构](./archive/originals/core-architecture.legacy.md) | 数值原语、倍率、资源图、循环和长期阶段 | 待验证假设与未来参考 | C-D | 阶段 2、5、6 | 不是顶层设计；具体参数、双层转生和技术建议均需重审 |
| [修仙竞品研究](./archive/originals/research/cultivation-idle-competitors.md) | 修仙放置、经营与模拟竞品地图 | 证据与推断混合 | B-D | Game Spec、产品定位 | 商店页能证明产品表达，不能证明留存、爽点或市场空位 |
| [GitHub 放置源码对标](./archive/originals/research/github-idle-source-benchmark.md) | Progress Knight 机制、候选仓库和工程参考 | 证据与设计建议混合 | A-D | Game Spec、纸面推演、工程阶段 | 源码和许可证较强；主对标、Blazor 和节奏结论未定 |
| [Idle 资源循环笔记](./archive/originals/research/idle-resource-loop-source-notes.md) | Kittens、Evolve、IdleOn、Melvor 等机制 | 研究证据 | A-D | Game Spec、纸面推演 | 证明机制分别存在，不证明组合后适合本项目 |
| [宗主玩法转译摘要](./archive/originals/research/validated-sect-leader-play-patterns.md) | 岗位令、赐丹令、突破令的浓缩论证 | 待验证假设 | B-D | Game Spec | 文件名中的“validated”只指参考机制成熟，不代表本项目已验证 |
| [宗门管理玩法详稿](./archive/originals/research/verified-play-patterns-for-sect-management.md) | 管理、养成、增量和开罗玩法映射 | 研究证据与设计推断 | A-D | Game Spec、纸面推演 | 与前两份研究重复较多，引用时优先使用本资料库的研究综合 |
| [Web 游戏开发流程对话摘要](./archive/originals/referenced-conversation-web-game-workflow.md) | Game Spec、技术分层、竖切、测试接口和发布流程 | 外部背景，未独立核验 | D | 阶段 1、6、7 | 来自用户引用的 ChatGPT 对话，只能作为流程输入，不能冒充官方规范 |
| [GLM 5.2 评审结果](./archive/originals/model-reviews/glm-5.2-review.result.md) | 对旧核心架构、原型状态和第一版范围的评审 | 模型意见，已过时部分较多 | D | 历史追溯 | 评审沿用了当时的 Blazor、优先突破者和阶段判断，不能作为当前决定 |
| [Sonnet 评审结果](./archive/originals/model-reviews/sonnet-review.result.md) | 对旧核心架构、原型范围和技术冲突的评审 | 模型意见，已过时部分较多 | D | 历史追溯 | 评审仍把自动优先培养当成核心，相关结论已被用户否定 |
| [Fable 讨论摘要](./archive/originals/model-reviews/fable-discussion-summary.md) | 常备政策与逐次命令的区分、早期范围建议 | 会话综合，原始输出未归档 | D | Game Spec | 只保留可追溯摘要，不得当作 Fable 原文引用 |
| `AGENTS.md` | 代理协作和模型路由规则 | 操作规则 | 不适用 | 全阶段 | 留在根目录，不作为游戏设计证据 |

## 已发现的主要冲突

| 冲突 | 当前处理 |
| --- | --- |
| 旧架构称自己是顶层设计 | 已降级为历史设计假设 |
| 旧 HANDOFF 固定 Blazor、.NET 10 和 GitHub Pages | 已被替代，技术选型延后到阶段 6 |
| 旧原型让优先弟子自动吃丹并获得倍率 | 已否决，重要资源必须由玩家定向分配 |
| 旧文档称 MVP 已完成 | 已纠正，旧原型没有证明核心玩法成立，文件也已删除 |
| 3 名或 5 名弟子、3 个或 4 个岗位、2 种或 3 种资源 | 全部作为测试参数候选，不是产品决定 |
| 5、15、30 至 60 分钟的兑现节奏 | 未经玩家测试，只能在后续模型中重新提出 |
| Progress Knight 是主对标 | 调整为重要参考样本，是否成为主对标留待 Game Spec 决定 |
| 倍率总线和双层转生是底层逻辑 | 调整为后期数值与架构候选，不约束当前玩法命题 |
| 竞品空位已经被证明 | 调整为市场机会假设，现有资料不足以证明商业空位 |

## 去重原则

三份宗主或增量玩法研究保留原文，日常讨论使用 [研究综合](./04-research-synthesis.md)。需要核对来源或追踪某个具体样本时，再回到原始文件。新的模型结论不能继续另建一份“总设计”，应写入研究综合、Game Spec 输入或决策台账中对应的位置。
