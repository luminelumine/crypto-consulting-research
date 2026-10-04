# Crypto Consulting Research

一个把 **Surf 实时数据** 和 **咨询式 Crypto 研究** 结合起来的 Agent Skill。

从价格、钱包和融资查询，到行业图谱、协议尽调、竞品对比与市场叙事研究，都由同一个 skill 完成。简单问题给简短答案；复杂研究输出有来源、可复核、有反方论证的报告。

## 安装

支持兼容 Agent Skills 的工具，包括 Codex 和 Claude Code：

```bash
npx skills add luminelumine/crypto-consulting-research --skill crypto-consulting-research
```

Skill 使用本机的 `surf` CLI。请按照 [Surf 官方 CLI 文档](https://agents.asksurf.ai/docs/cli/cli) 安装并配置所需访问。**不需要另外安装 Surf skill 或 CoinGecko skill。** 若已安装同名 consulting skill，请先备份，再选择更新或替换，避免重复维护两个版本。

Surf 的免费额度、付费访问和覆盖范围以服务当前规则为准。访问不可用时，skill 会尝试适合的公开来源，并披露数据缺口。密钥只在你自己的终端配置，不要粘贴到聊天或仓库。

## 使用示例

- `用 $crypto-consulting-research 查 ETH 当前价格，注明数据时间和来源。`
- `研究稳定币支付赛道，给出竞争格局、商业模式、主要风险和未来一年的验证指标。`
- `对比 Aave 与 Morpho 的用户、TVL、费用、协议收入、激励依赖和护城河。`
- `尽调一个代币：产品需求、价值捕获、解锁、持仓集中度、流动性，以及最强反方论证。`
- `查某项目最近的融资事件，并核验原始公告。`
- `分析这个公开钱包在指定期间的交易；如果历史不完整，不要把结果说成完整收益。`
- `核验“某叙事升温”：分别整理社交注意力与真实使用证据。`

## 研究流程

明确决策问题 → 建立证据计划 → Surf 数据获取 → 官方资料核验 → 叙事与社区观察 → 代币经济与链上行为 → 竞争比较 → 反方与场景分析 → 报告与质量检查。

每个重要数字记录观察时间、单位、口径与覆盖范围；结论区分事实、计算、第三方主张和分析推断。默认使用中文或用户指定语言回答。

## 文件结构

```text
SKILL.md                         统一入口与研究流程
agents/openai.yaml               Codex 界面元数据
references/surf-data.md          Surf 发现、路由、分页与错误处理
references/onchain-sql.md        链上查询与数据覆盖约束
references/research-modes.md     研究模式
references/source-policy.md      来源、证据台账与引用规则
references/report-template.md    咨询报告模板
```

## 范围与安全

本 skill 用于研究及只读分析，不执行交易、签名或资金转移。不会自动发送反馈或聊天内容、修改项目路由文件或更改遥测设置。报告文件默认 Arial，正文 11 pt、表格 10 pt；任何可见文本至少 10 pt。

Surf CLI、服务和上游 skill 由 [AskSurf](https://github.com/asksurf-ai/surf-skills) 维护。本项目是独立集成，未打包其 CLI、上游 skill 原文或 API 服务，不代表官方背书。研究结果不构成个性化投资建议。

本仓库的原创内容采用 [MIT License](LICENSE)。第三方服务及数据适用各自条款。
