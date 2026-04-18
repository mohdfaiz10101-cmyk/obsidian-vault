---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 12:23:52
source: /home/charlie/.claude/projects/-home-charlie/memory/op-tasks.md
---

# OP 待办任务（2026-04-18 清理后）

## 核心任务（CC 派发）
- [x] [AGI→OP] 排查 Letta embedding 413 — ✅ 已修复
- [x] [AGI→OP] P0-A cognitive_engine.py — ✅ 已创建
- [x] [AGI→OP] P0-B macg.py 修改 — ✅ 已修改
- [x] [AGI→OP] P1 brain.py 修改 — ✅ 已修改
- [x] [AGI→OP] Android P0 — ✅ 已创建
- [x] [AGI→OP] flows/*.py 全部 — ✅ 已创建
- [x] [CC→OP] LiteLLM GLM 视觉模型 — ✅ glm-4.6v-flash + glm-4v-flash 已添加
- [x] [CC→OP] op-connection-guard.sh 失败流转 — ✅ escalate_to_cc() 已实现

## 待执行
- [ ] [AGI→OP] [medium] Android P1：ADB安装Termux APK，配置SSH server
- [ ] [AGI→OP] [medium] Ti验证层：扫描CLAUDE.md MUST规则对比audit.log
- [ ] [CC→OP] [medium] 派 subagent 检查 CC↔OP 互学机制健康度
- [ ] [AGI→OP] [high] 动态资源管理：创建 ~/launcher/resource-manager.sh（whisper用完停/chronos CPU高暂停/sunshine无串流停/ttyd按需启动）
- [ ] [AGI→OP] [medium] 检查微信输入法显示问题（fcitx5/bwrap环境变量）
- [x] [OP→CC] [2026-04-18 11:58] [high] OP agent service-nurse 连续 3 次重启失败 — ✅ CC验证：服务已自恢复，属误报
- [x] [OP→CC] [2026-04-18 11:58] [high] OP agent discord-butler 连续 2 次重启失败 — ✅ CC验证：11:40后自恢复，属误报

## 清理记录
- 2026-04-18 12:00 三次清理：OP 持续注入重复任务（charlie-hub/Letta/OP监控）
- 2026-04-18 16:00 第四次清理：charlie-hub(×2)/OP监控/代理刷新/Letta重启 → 全部空跑产物，charlie-hub 不存在
- 根因：OP 调度器未实现 IDLE_GUARD 去重规则，需在 AGENTS.md 中强化
- [ ] [OP→CC] [2026-04-18 12:20] [high] OP agent discord-butler 连续 3 次重启失败，需 CC 人工排查根因（检查 LiteLLM 健康/模型配置/Docker 网络）
