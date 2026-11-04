# 任务记录

仅在建立或恢复开发任务时读取。使用 `${CODEX_HOME:-~/.codex}/requirement-dev/tasks/<task-id>/`（先解析为绝对路径），不写进公司仓库。task-id 使用时间加随机后缀；同一会话继续任务复用原 ID。

旧任务若仍位于 `${CODEX_HOME:-~/.codex}/feishu-dev/tasks/`，按原路径恢复，不迁移或覆盖历史。新一轮修复创建新 ID，通过 parent_task_id / acceptance_report 关联上轮任务或报告；同一任务的补充继续复用原 ID。

保存以下文件：
- `requirement.md`：标注各来源与选中条目的需求快照；多文件可另存 sources/ 中独立快照并记录路径。MD 读取原工作区当前内容，不限 Git 已提交版本。直接描述保存用户原话与补充。不含访问令牌或无关敏感信息。
- `plan.md`：已确认范围、需求/问题单元、验收条件、基础分支、本地提交分支、推送目标（如有）、用户确认原话与时间。
- `task.json`：结构化执行记录。

最小记录格式如下；未知值使用 null，不编造。除任务内快照引用外所有路径存绝对路径，时间使用带时区的 ISO 8601。

```json
{
  "schema_version": 3,
  "id": "20261103-xxxxxxxx",
  "sources": [
    {"type": "local_markdown", "path": "/absolute/project/docs/issues.md", "url": null, "selection": ["ISSUE-01"], "read_at": null, "version": null, "sha256": null, "snapshot": "requirement.md"}
  ],
  "parent_task_id": null,
  "acceptance_report": null,
  "repo": "",
  "remote": null,
  "target_branch": null,
  "integration_branch": "main",
  "integration_status": "not_started",
  "integrated_sha": null,
  "pushed_sha": null,
  "base_branch": null,
  "base_sha": null,
  "worktree": null,
  "task_branch": null,
  "status": "reading",
  "scope_confirmation": null,
  "branch_confirmation": null,
  "push_confirmation": null,
  "checks": [],
  "requirements": [],
  "commits": [],
  "commit_sha": null,
  "push_status": "not_started",
  "deployment_status": "unknown",
  "blocker": null,
  "updated_at": ""
}
```

来源 type 为 feishu_doc、local_markdown 或 user_message；不适用的 path/url/version 置 null。多个来源分别保存哈希，selection 记录问题 ID 或章节。旧 schema_version=1 的 source 视为单元素 sources 读取，保留原始记录。快照路径相对于任务目录，仓库和原始文件路径使用绝对路径。

`task_branch` 是用户确认的本地提交分支，`target_branch` 是计划推送的远端分支（默认 main；尚未计划推送时为 null），`integration_branch` 默认 main。`codex/` 不得作为远端目标。`branch_confirmation` 记录分支计划、用户确认原话及时间，与 `scope_confirmation` 可以来自同一次确认。

`requirements` 每项记录需求 ID、标题、验收条件、状态、checks 和 commit_shas。`commits` 每项记录需求 ID（整合提交可为 null）、SHA、分支、消息、时间和检查摘要；`commit_sha` 为最近一次任务提交，兼容旧读取方式，不能代替完整提交列表。恢复时先核对真实 Git 历史与条目映射，已完成提交不重复提交。

状态：reading → awaiting_scope → implementing → validating → committing；每个需求验证并提交后回到下一项 implementing，全部完成后为 locally_completed。需要推送时再经过 awaiting_push → integrating → delivering → completed；另有 blocked、cancelled。不推送的任务可以停在 locally_completed 并交付。检查项记录命令、退出码、结果摘要；确认项记录原话与时间。旧 schema_version=2 无提交列表时可根据真实 commit_sha 恢复，不编造多条提交。

`integration_status` 分别记录尚未合并、已合并或失败，`integrated_sha` 记录合并后本地 main 的 SHA；`pushed_sha` 记录已核实推送的 main SHA。main 已合并但推送失败时保留两阶段的真实状态。

仅在获取真实结果后更新状态。locally_completed 代表需求均已验证且本地提交（无改动条目注明原因），不代表已推送；completed 代表已授权推送交付动作完成，不代表部署成功；单独保存部署结果。关键阶段更新记录，使用临时文件替换避免半写入。不得保存凭据、环境变量全集或无关个人数据。
