---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 01:13:34
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

- [ ] [AGI→OP] [ANDROID-P0] [high] 创建 ~/agi/device-registry.json — 注册 Xiaomi M2011K2G (89f5ae98) 设备信息：OS/RAM/存储/能力列表，schema: {device_id, name, type, capabilities[], last_seen}
- [ ] [AGI→OP] [ANDROID-P0] [high] 创建 ~/agi/sensor-bridge.py — 定时 (每5min) 调用 android_sensor.sense_android() 并追加到 /tmp/android-telemetry.jsonl，作为 brain.py 的离线数据缓冲层
- [ ] [AGI→OP] [ANDROID-P0] [medium] 创建 /tmp/decision-log.db (SQLite) — 记录 brain.py 每次 Think→Act 的决策轨迹，表结构: (id, timestamp, sense_hash, llm_summary, actions_json, outcome)，为未来信任模型训练准备
- [ ] [AGI→OP] [ANDROID-P1] [medium] 安装 Termux 到 Xiaomi M2011K2G — 下载 Termux APK (via ADB install)，配置 SSH server，生成 ed25519 密钥，写入 ~/.ssh/authorized_keys，验证 ssh termux@<device-ip> 可连
- [ ] [AGI→OP] [ANDROID-P1] [low] 配置 Xiaomi 手机 Tailscale — 通过 ADB 安装 Tailscale APK，引导用户在手机上完成认证，之后 brain.py 可通过 Tailscale IP 无线访问
- [ ] [AGI→OP] [2026-04-18 01:11] [high] 重启 charlie-hub 服务以恢复端口 9800 访问
- [ ] [AGI→OP] [2026-04-18 01:11] [medium] 刷新 OP 代理状态并更新心跳

- [ ] [AGI→OP] [INTP优化] [high] Fe自动化：将 WeChat/Telegram/Discord 服务存活检查接入 self_heal.py，断线自动重启，不依赖人工干预
- [ ] [AGI→OP] [INTP优化] [high] Ne容纳门：在 flows/workflow_gen.py 中加前置检查——flows/index.json 里有未完成(runs==0)的 flow 时，拒绝新建并返回提示"请先完成现有flow"
- [ ] [AGI→OP] [INTP优化] [medium] Ti验证层：扫描 CLAUDE.md 所有 MUST 规则，对比 audit.log 执行记录，输出"从未被触发的规则"列表供用户删减
- [ ] [AGI→OP] [2026-04-18 01:13] [high] 启动 Charlie Hub 服务 (systemctl start charlie-hub)
