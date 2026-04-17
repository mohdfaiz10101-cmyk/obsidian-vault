---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 01:23:35
source: /home/charlie/.claude/projects/-home-charlie/memory/op-tasks.md
---

# OP 待办任务（清理后）

- [ ] [AGI→OP] [2026-04-18 01:07] [high] 排查 Letta embedding 413：curl -s http://localhost:8283/v1/models | jq；journalctl -u letta -n 30 | grep -i '413|embed'，找到原因后调整 chunk size 或切换 embedding 模型
- [⏭️] [AGI→OP] [2026-04-18] charlie-hub 启动 ×3条 → 已确认无此 systemd 服务，取消
- [ ] [AGI→OP] [2026-04-18 01:17] [high] P0-A 创建 ~/agi/cognitive_engine.py：score_cognitive_functions()→读op-tasks/audit.log/lessons-learned→输出Ti/Ne/Si/Fe评分→写/tmp/cognitive-profile.json
- [ ] [AGI→OP] [2026-04-18 01:17] [high] P0-B 修改 ~/agi/macg.py：AgentState加cognitive_profile，插入cognitive_sense节点(START→cognitive_sense→supervisor)，Supervisor提示加Ne收敛/深夜降级/Fe检查规则
- [ ] [AGI→OP] [2026-04-18 01:17] [medium] P1 修改 ~/agi/brain.py sense()末尾调用cognitive_engine注入结果；创建cognitive-engine.timer每30min刷新
- [ ] [AGI→OP] [2026-04-18 01:07] [high] Android P0：创建device-registry.json(小米89f5ae98)；创建sensor-bridge.py每5min采集写/tmp/android-telemetry.jsonl；创建decision-log.db记录brain决策
- [ ] [AGI→OP] [2026-04-18 01:07] [medium] Android P1：ADB安装Termux APK，配置SSH server，生成ed25519密钥
- [ ] [AGI→OP] [2026-04-18 00:55] [high] 实现 flows/self_heal.py（含Fe系统WeChat/Telegram/Discord存活检查+自动重启）
- [ ] [AGI→OP] [2026-04-18 00:55] [high] 实现 flows/task_decompose.py + flows/code_loop.py + flows/evolve.py + flows/workflow_gen.py（含Ne容纳门）
- [ ] [AGI→OP] [2026-04-18 01:11] [medium] Ti验证层：扫描CLAUDE.md所有MUST规则对比audit.log，输出从未触发的规则列表

---
# 已完成/已取消记录
- op 执行后将 `- [ ]` 改为 `- [x]`
- [ ] [AGI→OP] [COGNITIVE-P0] [high] 创建 ~/agi/cognitive_engine.py — 八维认知评分器。实现函数 score_cognitive_functions() 读取以下数据源并返回评分dict：(1) op-tasks.md：统计"- [ ]"vs"- [x]"计算Ne扩散率completion_rate；统计单日新建任务数峰值作为ne_burst_score (2) audit.log：统计bash命令执行成功率作为Te执行力te_score；统计systemctl/服务类命令频率 (3) memory/lessons-learned.md：统计条目数和引用率作为Si记忆密度si_score (4) 返回格式：{"Ti":85,"Ne":91,"Si":62,"Fe":41,"ne_divergence":0.83,"completion_rate":0.17,"te_execution":0.71,"last_updated":"ISO时间戳"}；写入 /tmp/cognitive-profile.json 供其他模块读取- [ ] [AGI→OP] [2026-04-18 01:20] [high] 重启charlie-hub服务 (systemctl start charlie-hub)
- [ ] [AGI→OP] [2026-04-18 01:20] [high] 分析内存占用，识别高消耗进程
