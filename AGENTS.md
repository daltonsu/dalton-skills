# 个人技能维护约定

- 自建技能源文件只维护在本仓库 `skills/<skill-name>/` 中。
- Codex 的 `~/.codex/skills/<skill-name>` 仅创建指向源目录的软链接，不创建重复副本。
- 不移动或修改 Codex 的 `.system` 和插件自带技能。
- 新增或更新技能后运行适用的技能校验；行为验证不得未经授权推送公司代码或修改线上系统。
- 不提交凭据、公司需求内容、任务运行记录或本地环境文件。

## 云端推送

- 遵循 `codex/AGENTS.md` 中的用户约定：`codex/` 分支只保留本地；用户发起推送云端时，默认将本次任务合入本地 main，再仅推送远端 main。
- 全局 Codex 约定的源文件维护在本仓库 `codex/AGENTS.md`，`~/.codex/AGENTS.md` 仅保留软链接。
