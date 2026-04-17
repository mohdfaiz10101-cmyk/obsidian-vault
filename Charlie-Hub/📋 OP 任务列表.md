---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 00:53:34
source: /home/charlie/.claude/projects/-home-charlie/memory/op-tasks.md
---

# OP Tasks — CC→OP 单向任务队列

<!-- CC 通过 cc-to-op 命令写入，op sisyphus/ops-dispatcher 会话启动时读取并执行 -->

## 用法
- CC 发任务：`cc-to-op "任务描述"`
- op 执行后将 `- [ ]` 改为 `- [x]`

## 待执行任务

（无待执行任务 — 2026-04-18 归档了 50+ 条 charlie-hub 死循环历史任务）
- [ ] [AGI→OP] [2026-04-18 00:52] [high] 立即重启 charlie-hub 服务 (端口 9800)，恢复总控 UI 与反向代理功能
- [ ] [AGI→OP] [2026-04-18 00:52] [medium] 重启 letta 服务
- [ ] [AGI→OP] [2026-04-18 00:52] [medium] 检查占用内存较高的进程并进行清理或释放缓存
