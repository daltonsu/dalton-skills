# Dalton Skills

个人维护的 Codex Skill 源文件。统一放在 `skills/<skill-name>/`，`~/.codex/skills/` 仅为这些自建 Skill 保留软链接；系统与插件技能不纳入管理。

## 当前技能

- `requirement-dev`（需求开发）：支持飞书、本地 Markdown 需求/问题清单、验收报告和直接描述；可按问题记录继续修复；确认需求与分支后每完成一项自动本地 commit，推送另行确认。
- `requirement-acceptance`（需求验收）：支持飞书、本地 Markdown 需求/问题清单和直接描述，通过代码、测试和页面操作独立验收，产出有证据的本地报告。
- `fork-project-dev`（fork项目开发）：parent/main 保存上游原版，main 维护个人稳定版；默认在 codex/dev 开发后直接合回 main，上游更新直接合入本地 main，dev 可选；向原仓库提交 PR 使用独立的 contrib/ 分支。
- `next-day-trade-plan`（次日波段交易计划）：面向约两周日 K 波段、晚上选股且不盯盘的风格，支持预定时段执行或买卖条件单，规划利润垫首减、余仓等待后续冲高，在整个波段中分三批退出。

开发调用示例：

> 使用 $requirement-dev，读取项目 docs/issues.md，修复其中未完成的问题；先让我确认需求和分支，每完成一项自动本地 commit，推送前再问我。

调用名为 `$requirement-dev`，需求来源不限于飞书。

验收调用示例：

> 使用 $requirement-acceptance，对照项目 docs/issues.md 验收问题 ISSUE-01 和 ISSUE-02，生成验收报告。

报告默认存入被验收项目的专用目录并由 Git 忽略，不上传仓库。

Fork 项目调用示例：

> 使用 $fork-project-dev，在 codex/dev 开发我的特色功能并验证后直接合回 main；parent/main 保留上游原版，同步时把上游更新直接合入本地 main。

默认功能路线为 `main → codex/dev → main`，同步路线为 `upstream/main → parent/main → main`，均在验证通过后才推送或部署。dev 仅在需要时加入；上游同步使用普通 merge 保留祖先关系，不套用功能交付的 squash 约定。

上游贡献路线为 `parent/main → contrib/<改动描述> → 自己的 fork → 原仓库 PR`，每个 PR 一条分支，例如 `contrib/fix-rule-validation`。先同步原版基线，只包含本次贡献的改动；PR 目标遵循原项目贡献指南。

交易计划调用示例：

> 使用 $next-day-trade-plan，结合持仓和最新日 K，按约两周波段、利润垫首减后等待冲高的节奏，给出无需盯盘的次日计划，可采用预定时段执行或买卖条件单。

个性化持仓保存在技能目录的 `references/position.local.md`，带确认日期且由 Git 忽略。新环境没有此文件时使用对话提供的持仓；市场数据每次重新获取。该技能只生成分析计划，不执行交易。

## 分支与云端推送约定

本节用于本技能仓库的交付；使用 `fork-project-dev` 管理的 fork 项目遵循其个人分支流程。

`codex/` 分支只保留本地。明确发起“推送云端”时，默认将本次已确认任务合入本地 main，验证后仅推送远端 main，不再重复确认该流程。本次任务的 codex/ 分支完整合入 main 并验证后自动安全删除；未合并、工作区占用或无法安全删除时保留并说明。

全局约定源文件在 `codex/AGENTS.md`，Codex 用户目录 `~/.codex/AGENTS.md` 只保留软链接。

## 日常维护

1. 在本仓库的 `skills/` 中新增或修改技能。
2. 校验技能格式，并通过真实任务验证行为。
3. 检查 Git 差异，再提交；配置远端后推送备份。

新增技能时建立链接，已存在同名目标时先检查，不覆盖：

```sh
ln -s /Users/dalton/repo/03_pers-proj/dalton-skills/skills/<skill-name> /Users/dalton/.codex/skills/<skill-name>
```

本仓库不保存凭据、公司需求快照和任务执行记录。`requirement-dev` 的任务记录存放在 Codex 用户目录外部任务区，不随技能提交。
