# Profectus 0.7 独立技术复核

状态：Current Research

复核方：Grok 4.5，通过项目 handoff 流程执行

验证时间：2026-07-15 09:00 至 09:13 UTC

验证环境：macOS Darwin 24.6.0，隔离临时目录

## 总结

| 问题 | 结论 |
| --- | --- |
| Galaxy Git 502 是否导致干净安装失败 | Confirmed |
| 模板是否声明 Node 21 | Confirmed |
| Node 22 是否只是警告且能完整构建 | Partly confirmed，安装阶段只是警告，官方完整构建未证实 |
| 是否适合作为默认短期原型底座 | 不推荐 |
| 是否适合作为长期正式底座 | 不适合 |
| 拒绝作为长期研究底座是否有充分依据 | 有 |

## 干净安装证据

官方源：`https://github.com/profectus-engine/Profectus.git`

验证分别使用：

- Node 22.23.1，npm 10.9.8。
- 便携 Node 21.7.3，npm 10.5.0。

两次执行 `npm install --no-fund --no-audit` 均以 exit 128 失败，关键错误相同：

```text
npm error command git --no-replace-objects ls-remote
https://code.incremental.social/thepaperpilot/unofficial-galaxy-sdk.git
npm error remote: Bad Gateway
npm error fatal: The requested URL returned error: 502
```

在约十分钟的验证窗口内，站点根路径、SDK 路径、Git 路径、API 路径和连续五次根路径请求均返回 502。TLS 和连接可以建立，同机访问 GitHub、npm 和文档站正常。因此可以确认当时是稳定可复现的上游主机故障，不能据此断言永久下线。

没有找到该 SDK 的 npm 包、GitHub 镜像或 Codeberg 替代源。

## Node 版本证据

- `package.json` 声明 `engines.node: "21.x"`。
- GitHub CI 与 Forgejo CI 都使用 Node 21。
- Node 22 会产生 `EBADENGINE` 警告，但不会因 engine 声明立即停止安装。
- Node 21 与 Node 22 都在 Galaxy 依赖处失败，所以当前安装失败的直接原因不是 Node 版本。
- 用本地 stub 绕过 Galaxy 后，Node 22 下 1297 项测试通过，但完整构建仍出现类型错误。由于 stub 不是官方依赖，不能宣称 Node 22 完整兼容。

## 长期维护风险

1. GitHub 只是镜像，主协作站与关键依赖共用当前不可达的 Incremental Social 主机。
2. Galaxy 云存档从主入口硬导入，不是可选插件。
3. 存在非 registry Git 依赖和多个废弃或较旧的依赖。
4. 只有 `0.7` 标签，没有 GitHub Release 资产。
5. 0.7 后只有五个修复提交，最后代码活动为 2025-07-17。
6. 默认抽象偏向 layer、tree、prestige 和 reset，与人口岗位、有序资源链、任务驱动的领域模型错位。
7. 离线系统以较大 `diff` 广播更新，没有为多资源链提供按库存耗尽点和动作边界切片的可靠语义。
8. MIT 许可证本身不是障碍，但复制或分发实质代码时必须保留版权和许可声明。

## 最终判断

Profectus 0.7 在服务恢复并剥离 Galaxy 后，可以勉强用于其擅长的威望树式增量试验，但不值得作为本项目的默认原型底座。长期正式底座的否决不依赖 502 是否持续，因为维护、供应链、领域抽象和离线模型仍然不匹配。

值得独立借鉴的部分：

- Formula 与 Requirement 的组合思路。
- Modifier 的数据和展示分离。
- `fixOldSave` 式存档迁移钩子。
- 本地多存档槽与导入导出。
- `preUpdate`、`update`、`postUpdate` 的更新职责切分。

官方来源：

- [Profectus GitHub Mirror](https://github.com/profectus-engine/Profectus)
- [Profectus Documentation](https://moddingtree.com/)
- [Profectus MIT License](https://github.com/profectus-engine/Profectus/blob/main/LICENSE)
