# Dalton Skills

个人维护的 Codex Skill 源文件。统一放在 `skills/<skill-name>/`，`~/.codex/skills/` 仅为这些自建 Skill 保留软链接；系统与插件技能不纳入管理。

## 当前技能

- `requirement-dev`（需求开发）：支持飞书、本地 Markdown 需求/问题清单、验收报告和直接描述；可按问题记录继续修复；确认需求与分支后每完成一项自动本地 commit，推送另行确认。
- `requirement-acceptance`（需求验收）：支持飞书、本地 Markdown 需求/问题清单和直接描述，通过代码、测试和页面操作独立验收，产出有证据的本地报告。

开发调用示例：

> 使用 $requirement-dev，读取项目 docs/issues.md，修复其中未完成的问题；先让我确认需求和分支，每完成一项自动本地 commit，推送前再问我。

调用名为 `$requirement-dev`，需求来源不限于飞书。

验收调用示例：

> 使用 $requirement-acceptance，对照项目 docs/issues.md 验收问题 ISSUE-01 和 ISSUE-02，生成验收报告。

报告默认存入被验收项目的专用目录并由 Git 忽略，不上传仓库。

## 分支与云端推送约定

`codex/` 分支只保留本地。明确发起“推送云端”时，默认将本次已确认任务合入本地 main，验证后仅推送远端 main，不再重复确认该流程。

全局约定源文件在 `codex/AGENTS.md`，Codex 用户目录 `~/.codex/AGENTS.md` 只保留软链接。

## 日常维护

1. 在本仓库的 `skills/` 中新增或修改技能。
2. 校验技能格式，并通过真实任务验证行为。
3. 检查 Git 差异，再提交；配置远端后推送备份。

新增技能时建立链接，已存在同名目标时先检查，不覆盖：

```sh
ln -s /Users/dalton/repo/04_pers-proj/dalton-skills/skills/<skill-name> /Users/dalton/.codex/skills/<skill-name>
```

本仓库不保存凭据、公司需求快照和任务执行记录。`requirement-dev` 的任务记录存放在 Codex 用户目录外部任务区，不随技能提交。
