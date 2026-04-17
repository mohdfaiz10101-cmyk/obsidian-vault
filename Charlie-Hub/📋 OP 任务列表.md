---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 01:03:34
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

- [ ] [MAC→OP] [2026-04-18 00:55] [high] 实现 LangGraph 自愈巡检流水线：~/agi/flows/self_heal.py，节点：Sense(复用brain.py)→并行分叉(DiskFix/ServiceFix/ProxyFix)→Verify→重试×2→HITL→Telegram报告。工具复用macg.py全部9个。SQLite checkpointer持久化。完成后写入~/agi/flows/README.md

- [ ] [MAC→OP] [2026-04-18 00:55] [high] 实现 LangGraph 智能任务分解器：~/agi/flows/task_decompose.py，节点：UserIntent→GLM分解→Send()并行子任务→各自ReAct→汇聚→Claude验证→[通过/重试]。复用macg.py双模型架构。

- [ ] [MAC→OP] [2026-04-18 00:55] [high] 实现 LangGraph 代码变更闭环：~/agi/flows/code_loop.py，节点：Spec→ClaudeCodeGen→bash测试→[失败×3修复]→interrupt等确认→op_delegate部署。

- [ ] [MAC→OP] [2026-04-18 00:55] [high] 实现工作流自动进化L2层：~/agi/flows/evolve.py，每次工作流执行后统计节点成功率，<60%触发优化子图→Claude分析失败原因→重写节点→持久化。失败经验写lessons-learned.md。

- [ ] [MAC→OP] [2026-04-18 00:55] [high] 实现工作流自动生成L3层：~/agi/flows/workflow_gen.py，用户输入新任务类型→Claude生成LangGraph图结构→沙盒测试→通过后写入~/agi/flows/→下次直接复用。建立flows/index.json索引。

- [ ] [MAC→OP] [2026-04-18 00:55] [medium] 集成orchestrator统一入口：~/agi/orchestrator.py，intent_classifier路由（regex规则引擎）→对话→macg，代码→claude -p，运维→op-tasks，复杂多步→对应flows/子图。alias: ai="python3 ~/agi/orchestrator.py"
