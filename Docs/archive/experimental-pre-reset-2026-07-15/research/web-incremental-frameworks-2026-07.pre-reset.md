# Web 增量/放置游戏底座调研（2026-07）

> 调研日期：2026-07-15  
> 目标：为“宗门多岗位资源网络”最小 Demo 选择一个不必从零搭建、核心能力经过实际项目使用的 Web 底座。  
> 来源原则：只采用项目官方文档、官方 GitHub 仓库、许可证文件、提交与发布记录。GitHub 星标仅用于判断生态规模，不等于玩法验证。

## 1. 结论先行

**建议先用 Profectus 0.7 做一轮 2—3 天的技术刺探，再决定是否正式 fork。** 它是本轮候选中唯一同时内置以下增量游戏基础设施、并仍允许自定义玩法与 UI 的完整底座：

- Vue 3 + TypeScript + Vite；
- `break_eternity` 级别的大数和格式化；
- 本地多存档、自动保存、导入导出、版本迁移入口；
- 离线时间累积与分段追赶；
- Resource、Conversion、Formula、Requirement、Repeatable、Modifier、Action、Achievement、Layer 等增量游戏原语；
- PWA 和可扩展 Vue/TSX 组件。

它与本项目的对应关系较直接：灵材/丹药/修为是 Resource，生产配方是 Conversion，岗位与设施等级是 Repeatable 或自定义 Feature，产速构成是 Modifier，突破条件是 Requirement，宗门阶段是 Layer。官方源码还提供公式求逆、积分和“最大可购买量”计算，可避免每一种升级都重新写批量购买逻辑。[Profectus 仓库](https://github.com/profectus-engine/Profectus)；[公式系统](https://github.com/profectus-engine/Profectus/tree/main/src/game/formulas)；[资源转换](https://github.com/profectus-engine/Profectus/blob/main/src/features/conversion.ts)

但这不是“直接换皮即可上线”。必须先验证两个风险：

1. **离线模拟正确性**：Profectus 把离线时间存入 `offlineTime`，随后以分段 tick 追赶；复杂的多资源消耗链、容量上限和自动规则可能产生顺序依赖，必须用确定性测试校验，而不能默认框架替我们解决了经济模拟。[游戏循环源码](https://github.com/profectus-engine/Profectus/blob/main/src/game/gameLoop.ts)；[存档与离线时间源码](https://github.com/profectus-engine/Profectus/blob/main/src/util/save.ts)
2. **UI 解绑成本**：Profectus 起源于 The Modding Tree，默认仍带 Layer、树、标签页与功能组件的视觉语法。它允许自定义 Feature 和 Vue 组件，但如果我们的目标界面最终是高度定制的“宗门调度台”，需要确认重写外观时不会持续与框架耦合对抗。[0.7 变更记录](https://github.com/profectus-engine/Profectus/blob/main/CHANGELOG.md)；[官方指南](https://moddingtree.com/guide/)

若刺探失败，第二选择不是 Phaser，而是 **React（或 Vue）+ Vite + 独立纯 TypeScript 模拟核心 + `break_eternity.js`/`emath.js`**。这条路的 UI 自由度和长期维护性更好，但离线、存档迁移、公式、批量购买、调试面板等仍需自己组合，不算真正“套完整框架”。

## 2. 评估标准

| 维度 | 对本项目的实际含义 |
|---|---|
| 可直接套用 | 是否已有游戏循环、存档、离线、大数、资源与升级原语，而不只是页面脚手架 |
| 数值能力 | 是否支持大数、成本曲线、批量购买、倍率拆解、上限和软上限 |
| 模拟能力 | 是否能稳定处理多个生产岗位、资源消耗链、自动化与离线追赶 |
| UI 约束 | 是否把游戏强制呈现成威望树/固定卡片，能否做宗门调度台 |
| 维护状态 | 近两年是否有代码提交、标签或正式发布，依赖是否现代 |
| 法律条件 | 是否有明确、适合商业改造的许可证 |
| 验证程度 | 是否有真实示例、社区 fork 或成熟生态；不把 README 自称当成验证 |

## 3. 候选总表

| 候选 | 直接套用 | 离线 / 存档 | 大数 / 公式 | UI 自由度 | 最近维护信号（截至 2026-07-15） | 许可证 | 结论 |
|---|---:|---|---|---|---|---|---|
| **Profectus 0.7** | 高 | 内置 / 内置 | 内置 `break_eternity`；公式、求逆、批量购买 | 中高，但需摆脱默认树式语法 | 最近代码提交 2025-07；0.7 标签及 2024-12 变更记录 | MIT | **首选技术刺探** |
| The Modding Tree | 高 | 内置 / 内置 | 内置 Decimal；威望、升级、里程碑、软上限 | 低到中，强烈树式/威望式 | 最近提交 2024-10，但 changelog 最后版本为 2021-09 | MIT，另含上游许可证 | 只做机制参考，不作为新项目底座 |
| Incremental Game Template（123ishaTest） | 中 | 未发现离线 / 本地存档 | 未见成熟大数与成本公式层 | 高 | Vue 模板停在 2021；库提交 2024-02；npm 旧包停在 2022 | package 标 MIT，但当前库仓库未见 LICENSE 文件 | 不选 |
| IvarK Incremental Game Template | 低 | 未完成 | `break_infinity.js` + 记数法 | 中 | 2024-09；README 明示 WIP | 未见许可证 | 不选 |
| Svelte Incremental Template | 中 | 内置 / 内置 | 未内置大数 | 高 | 最后提交 2021-12，Svelte 3 + Rollup 2 | 未见许可证 | 不选；只可借鉴结构 |
| React + Vite | 低（需组合） | 自建 | 需加库 | **很高** | React 19 与 Vite 持续活跃 | MIT | Profectus 刺探失败后的稳健路线 |
| Phaser | 低（对数值经营） | 自建 | 自建 | Canvas 表现强，DOM 数值面板不占优 | Phaser 4.2.1 发布于 2026-07 | MIT | 当前不选；以后做可视化小人层再局部引入 |
| `emath.js` | 中（工具库） | 内置保存工具，离线仍需建模 | `break_eternity`、Currency、Upgrade、Boost | 不负责 UI | 9.6.0 发布于 2024-10；代码提交延续到 2025-10 | MIT | 自建路线的优先辅助库，先审 API 稳定性 |
| React Incremental Library | 中（Hook 库） | 声称内置 | 基础 currency/production/multiplier | 高 | 仅 0.1.1，2025-05；生态极小 | MIT | 可读源码，不作为核心依赖 |

## 4. 逐项评估

### 4.1 Profectus 0.7

**定位与技术栈**

Profectus 官方称其为“a game engine that grows with you”，由 The Modding Tree 启发，但 0.7 重写了 Feature 和 Board 系统，目标是不把开发者限制在一种玩法。当前 `package.json` 是 Vue 3.5、TypeScript 5.5、Vite 5、Vitest、PWA 插件以及少量 Pixi 模块。[官方首页](https://moddingtree.com/)；[`package.json`](https://github.com/profectus-engine/Profectus/blob/main/package.json)；[0.7 changelog](https://github.com/profectus-engine/Profectus/blob/main/CHANGELOG.md#070---2024-12-31)

**可直接利用的能力**

- `createResource` 和持久化 ref：适合定义库存、修为、设施等级与解锁状态。[Resource 源码](https://github.com/profectus-engine/Profectus/blob/main/src/features/resources/resource.ts)；[Persistence 源码](https://github.com/profectus-engine/Profectus/blob/main/src/game/persistence.ts)
- Conversion：输入资源、输出资源、公式、当前收益、下个阈值、批量转换和消费方式都有明确接口，适合“灵材 → 丹药 → 修为”的链条。[Conversion 源码](https://github.com/profectus-engine/Profectus/blob/main/src/features/conversion.ts)
- Formula / Requirement：公式支持求值、求逆、积分，Requirement 可用于购买、解锁和突破条件；0.6 的更新明确把它用于 `buy max` 与最大可承担购买量。[Formula 源码](https://github.com/profectus-engine/Profectus/blob/main/src/game/formulas/formulas.ts)；[Requirement 源码](https://github.com/profectus-engine/Profectus/blob/main/src/game/requirements.tsx)；[0.6 changelog](https://github.com/profectus-engine/Profectus/blob/main/CHANGELOG.md#060---2023-04-20)
- Modifier：可以把基础值、弟子加成、设施倍率、事件惩罚按段展示，适合玩家理解“为什么产速是这个数”。[Modifier 源码](https://github.com/profectus-engine/Profectus/blob/main/src/game/modifiers.tsx)
- Action / Repeatable / Achievement / Challenge / Board / Layer：可覆盖有时长的突破、可重复设施、阶段目标、特殊危机、宗门关系图和内容分区。[Feature 目录](https://github.com/profectus-engine/Profectus/tree/main/src/features)；[Board 目录](https://github.com/profectus-engine/Profectus/tree/main/src/game/boards)
- 存档：localStorage + LZ-String，多存档、自动保存、导入编码兼容、项目 ID 校验、`fixOldSave` 迁移入口；0.6.2 还加入了特定平台的云存档支持。[save.ts](https://github.com/profectus-engine/Profectus/blob/main/src/util/save.ts)；[player.ts](https://github.com/profectus-engine/Profectus/blob/main/src/game/player.ts)
- 大数：框架通过统一 `util/bignum.ts` 封装 `break_eternity`，允许替换底层大数实现；当前源码内置 Decimal 与格式化。[bignum.ts](https://github.com/profectus-engine/Profectus/blob/main/src/util/bignum.ts)；[break_eternity 封装](https://github.com/profectus-engine/Profectus/blob/main/src/util/break_eternity.ts)

**离线进度的真实行为**

它不是一次性用解析公式算完离线收益，而是：加载时根据上次时间累积 `offlineTime`，受项目 `offlineLimit` 限制；运行时每次取剩余离线时间的一部分加到当前 `diff`，再受 `maxTickLength` 限制，逐步重放更新事件。[save.ts](https://github.com/profectus-engine/Profectus/blob/main/src/util/save.ts#L104-L137)；[gameLoop.ts](https://github.com/profectus-engine/Profectus/blob/main/src/game/gameLoop.ts)

这对简单被动产出足够方便，但对本项目的“消耗型生产链”存在必须实测的问题：同一 tick 内，灵田、丹房、修炼场的更新顺序可能影响资源是否先产后耗；大 tick 还会跨越库存上限、阈值和自动开关。**框架验证了离线追赶设施，并没有替我们验证宗门经济的离线算法。**

**维护、生态与许可证**

- 仓库未归档；GitHub 显示 34 stars、27 forks；最近默认分支代码提交为 2025-07-17。[提交记录](https://github.com/profectus-engine/Profectus/commits/main/)
- 无 GitHub Release，只有 `0.7` tag；正式度低于主流框架。[Tags](https://github.com/profectus-engine/Profectus/tags)
- 官方列出 Advent Incremental、Planar Pioneers、Universal Reconstruction、Primordia 等示例；这能证明框架被实际游戏使用，但不能证明我们的多岗位网络数值成立。[官方示例项目](https://moddingtree.com/guide/getting-started/examples)
- MIT 许可证，允许商业修改与分发，但需保留版权和许可声明。[LICENSE](https://github.com/profectus-engine/Profectus/blob/main/LICENSE)
- `package.json` 中有通过 Git URL 安装的非官方 Galaxy SDK 和 `vue-panzoom`，应锁定 lockfile，并在刺探时验证全新环境可重复安装；若无需云存档，应评估移除该依赖。[`package.json`](https://github.com/profectus-engine/Profectus/blob/main/package.json)

**本地可重复安装实测（2026-07-15）：**在全新浅克隆中使用 Node 22.23.1 与 npm 10.9.8 执行 `npm install --include=dev`，安装因 `https://code.incremental.social/thepaperpilot/unofficial-galaxy-sdk.git` 返回 HTTP 502 而以 code 128 失败，继而无法运行 `vue-tsc` 与 `vitest`。同时模板的 `engines.node` 固定为 `21.x`，在 Node 22 下会产生 engine warning。这说明“官方仓库原样克隆即可稳定构建”目前不成立。若采用 Profectus，必须先做受控 fork：移除 Galaxy 云存档耦合、更新 Node 支持范围、锁定依赖，并在干净环境重新通过构建与测试；这一项应列为采用门槛，而不只是一般风险提示。

**对本项目适配度：8/10。** 适合快速验证“资源流—岗位分配—设施倍率—阶段突破”，但前提是把模拟核心与 Vue 表现分开，并为离线一致性写测试。

### 4.2 The Modding Tree（TMT）

TMT 是基于 The Prestige Tree 的增量游戏引擎，官方 README 直言其主要开发方式是编辑/复制 `layers.js`。它内置 Decimal、存档、离线时间、威望层、升级、可重复购买、挑战、里程碑、成就、软上限、自动威望与树节点 UI。[README](https://github.com/Acamaeda/The-Modding-Tree/blob/master/README.md)；[Layer 功能文档](https://github.com/Acamaeda/The-Modding-Tree/blob/master/docs/layer-features.md)；[主项目配置与离线限制](https://github.com/Acamaeda/The-Modding-Tree/blob/master/docs/main-mod-info.md)

它的优点是模板化程度最高，1,600+ forks 说明曾形成显著的创作生态；MIT 许可证也清晰。[仓库](https://github.com/Acamaeda/The-Modding-Tree)；[LICENSE](https://github.com/Acamaeda/The-Modding-Tree/blob/master/LICENSE)

不适合作为本项目新底座的原因：

- 框架的核心抽象是“层级、威望、重置、树节点”，而我们的第一层核心是“并行岗位、连续生产、库存消耗、自动规则”。可以硬做，但会让领域模型迁就框架。
- 技术形态是全局 JavaScript + Vue 2 风格脚本与手写 HTML/CSS，没有现代模块、类型和测试底座；源码甚至会遍历 layer 中的函数并每 tick 调用，定制复杂系统时容易形成隐式性能与耦合问题。[temp.js](https://github.com/Acamaeda/The-Modding-Tree/blob/master/js/technical/temp.js)；[index.html](https://github.com/Acamaeda/The-Modding-Tree/blob/master/index.html)
- 最近仓库提交是 2024-10，但 changelog 的最后版本仍是 2021-09 的 2.6.6.2，近年没有稳定发布轨迹。[commits](https://github.com/Acamaeda/The-Modding-Tree/commits/master/)；[changelog](https://github.com/Acamaeda/The-Modding-Tree/blob/master/changelog.md)

**对本项目适配度：5/10。** 值得借鉴解锁、软上限、自动化和 UI 提示模式，不值得作为 2026 年新项目代码底座。

### 4.3 Incremental Game Template（123ishaTest）

这一名称实际包含两个项目：Vue 2 模板 `igt-vue` 和 `igt-library`。旧模板使用 Vue 2、Vue CLI 4、Tailwind PostCSS 7 与 TypeScript 4，依赖 npm 包 `incremental-game-template`；模板最后提交在 2021-10。[模板 README](https://github.com/123ishaTest/Incremental-Game-Template/blob/master/README.md)；[模板 package.json](https://github.com/123ishaTest/Incremental-Game-Template/blob/master/package.json)；[提交记录](https://github.com/123ishaTest/Incremental-Game-Template/commits/master/)

库本身提供 tick loop、定时保存、本地存档、钱包、库存、升级、成就、战利品表、统计、热键与开发面板；但当前源码未发现大数抽象和完整离线进度，暂停/恢复还会重置 `_lastUpdate`，不能直接承担挂机离线模拟。[igt-library](https://github.com/123ishaTest/igt-library)；[IgtGame.ts](https://github.com/123ishaTest/igt-library/blob/master/src/ig-template/IgtGame.ts)；[features](https://github.com/123ishaTest/igt-library/tree/master/src/ig-template/features)

维护和发布信号也不一致：npm 旧包 `incremental-game-template` 最新 1.2.4 停在 2022-05；当前仓库 package 名已改为 `@123ishatest/igt-library` 1.0.0，最近默认分支提交为 2024-02；仓库 `package.json` 写 MIT，但当前仓库根目录未见 LICENSE 文件。[library package.json](https://github.com/123ishaTest/igt-library/blob/master/package.json)；[npm package](https://www.npmjs.com/package/incremental-game-template)

**对本项目适配度：4/10。** 有一些 RPG/库存工具，但不应为了它接受旧模板和缺失的离线/大数能力。

### 4.4 其他模板

**IvarK / Incremental Game Template**

这是 Vue 3 + Vite + TypeScript + `break_infinity.js` + Antimatter Dimensions Notations 的试验性模板，但 README 直接标注“ENTIRELY WIP”，仅有极少生态、无清晰许可证，最近核心提交为 2024-09。[仓库](https://github.com/IvarK/incremental-game-template)；[README](https://github.com/IvarK/incremental-game-template/blob/main/README.md)；[package.json](https://github.com/IvarK/incremental-game-template/blob/main/package.json)

结论：可读它如何组织数值与记数法，不可作为商业项目底座。

**Svelte Incremental Template**

它确实实现了主循环、示例升级、通知、保存/加载和离线进度，是比普通页面模板更接近完整增量游戏的骨架；但最后提交在 2021-12，技术栈是 Svelte 3 + Rollup 2，也未见明确 LICENSE 文件。[README](https://github.com/jamesmgittins/svelte-incremental-template/blob/master/README.md)；[package.json](https://github.com/jamesmgittins/svelte-incremental-template/blob/master/package.json)；[提交记录](https://github.com/jamesmgittins/svelte-incremental-template/commits/master/)

结论：只借鉴模块分离，不 fork。

### 4.5 React + Vite：通用 Web 应用路线

React 的官方定位是构建 Web 和原生用户界面的组件库，Vite 提供现代开发与构建脚手架；二者不会提供增量游戏的离线、存档、大数、资源公式和层级系统。[React 官方文档](https://react.dev/learn)；[React 仓库与 MIT LICENSE](https://github.com/facebook/react)；[Vite 官方指南](https://vite.dev/guide/)

它们的价值是：宗门界面本质上是大量表格、卡片、解释文本、数值趋势、岗位拖放与响应式布局，DOM 组件框架比 Canvas 引擎更自然；也最容易把“纯 TypeScript 模拟器”与 UI、自动化测试、数据配置分开。React 仍持续发布，GitHub 生态规模远大于任何增量专用框架。[React releases](https://github.com/facebook/react/releases)

代价是必须自行组合：

- 基于真实时间差的固定/分段模拟；
- 存档 schema、迁移、压缩、导入导出；
- 大数和统一格式化；
- 成本曲线、批量购买、软上限；
- 调试面板、倍率解释与数值回放。

**对本项目适配度：7/10（长期），5/10（最快 Demo）。** 它是最稳健的生产路线，但不是“避免从零开始”的最短路线。

### 4.6 Phaser：成熟，但解决的不是当前问题

Phaser 是面向浏览器的 2D HTML5 游戏框架，提供 WebGL/Canvas 渲染、场景、输入、动画、音频、资源加载、时间步和游戏对象；官方称其已持续开发十年以上，也能与 React/Vue 集成。[Phaser 官方定位](https://docs.phaser.io/phaser/getting-started/what-is-phaser)；[Scene 文档](https://docs.phaser.io/phaser/concepts/scenes)；[GitHub](https://github.com/phaserjs/phaser)

维护与许可证非常强：MIT；Phaser 4.2.1 于 2026-07-09 发布。[LICENSE](https://github.com/phaserjs/phaser/blob/master/LICENSE.md)；[Releases](https://github.com/phaserjs/phaser/releases)

但它不内置增量游戏的大数、离线、存档迁移、资源链或成本曲线。其 Scene/GameObject/renderer 优势主要用于“小人在场景中运动”和动画表现，正好是当前最小 Demo 已明确暂不验证的部分。用 Phaser 搭纯数字面板，反而会把大量常规 DOM UI 工作放进 Canvas 或 React-Phaser 双层架构。

**对当前 Demo 适配度：3/10。** 后续若加入桌面常驻小人、宗门场景或高频视觉反馈，可把 Phaser 当表现层嵌入，而不是让它持有经济真值。

### 4.7 可作为自建路线配件的较新库

**`break_eternity.js`**

成熟的大数库，MIT，最新 release v2.1.3 为 2025-12-07。它能解决极端数量级与数学运算，但不负责游戏循环、存档或玩法公式。[仓库](https://github.com/Patashu/break_eternity.js)；[Releases](https://github.com/Patashu/break_eternity.js/releases)；[LICENSE](https://github.com/Patashu/break_eternity.js/blob/master/LICENSE)

**`emath.js`**

这是建立在 `break_eternity.js` 上的增量工具库，提供 Currency、Upgrade、Boost、保存/加载、热键等；MIT，9.6.0 发布于 2024-10，默认分支仍有 2025-10 提交。[README](https://github.com/xShadowBlade/emath.js/blob/main/README.md)；[package.json](https://github.com/xShadowBlade/emath.js/blob/main/package.json)；[Releases](https://github.com/xShadowBlade/emath.js/releases)；[LICENSE](https://github.com/xShadowBlade/emath.js/blob/main/LICENSE)

它比自己从 `break_eternity` 开始更省事，也不绑定 UI；但生态很小，离线进度仍需我们建立生产链语义，正式采用前要做存档序列化和成本计算测试。

**React Incremental Game Library**

它提供 game loop、tick、offline progress、currency、production、multiplier、upgrade、prestige 与 save/load hooks，技术栈是 React 19 + Zustand，MIT。[README](https://github.com/TDanks2000/react-incremental-lib/blob/main/README.md)；[package.json](https://github.com/TDanks2000/react-incremental-lib/blob/main/package.json)；[LICENSE](https://github.com/TDanks2000/react-incremental-lib/blob/main/LICENSE)

但它只有一个 0.1.1 release（2025-05）、1 个 GitHub star 和极少使用证据，不能称为“经过验证的核心底座”。可以借鉴 hook API，不建议依赖。

## 5. 推荐的最小技术刺探

不要先把阶段三原型整体迁移。只用 Profectus 实现一个纵向切片：

1. 三个资源：灵材、丹药、修为；
2. 三个岗位：采集、炼丹、修炼；
3. 四名弟子，以整数岗位分配产生不同倍率；
4. 两项设施：丹炉（改变转化效率）和聚灵阵（改变修炼倍率）；
5. 一个确定性目标：在固定时间前达到公开的筑基门槛；
6. 保存后分别离线 1 分钟、1 小时，与连续在线模拟结果对比；
7. 把所有产速拆成可解释的 modifier 列表；
8. 用自定义 Vue 组件做一页宗门面板，不显示默认威望树。

通过条件：

- 资源与岗位模型不必迁就 prestige/reset 抽象；
- 在线与离线结果在定义好的容差内一致；
- 纯模拟测试不依赖挂载 Vue；
- 能在一处替换全部默认视觉，而不是逐个覆盖框架 CSS；
- 全新环境按 lockfile 可重复安装和构建；
- 存档 schema 能升级，坏档能导出诊断。

任一关键条件失败，就停止继续投入 Profectus，改用“Vite + Vue/React + 纯 TypeScript 模拟核心 + `break_eternity`/`emath.js`”。不要退回 TMT，也不要因 Phaser 更像“游戏引擎”而转向 Canvas。

## 6. 最终推荐排序

1. **Profectus 0.7：** 最快验证核心增量循环；先刺探，后 fork。
2. **Vue/React + Vite + 独立模拟核心 + `emath.js` 或 `break_eternity.js`：** 最稳健的长期退路，UI 自由度最高。
3. **Phaser 作为未来可选表现层：** 只有当小人运动和场景反馈进入验证范围时再引入。
4. **TMT、123ishaTest IGT、IvarK 模板、旧 Svelte 模板：** 只读其设计与源码，不作为项目底座。

最重要的边界是：**框架可以替我们验证工程设施，不能替我们验证玩法算法。** Profectus 的 Formula、Conversion、Repeatable 证明这些技术原语能够工作；宗门岗位的最优分配是否有趣、资源瓶颈是否轮换、投资回收期是否形成决策，仍需用单独的数值模型和可重复模拟验证。
