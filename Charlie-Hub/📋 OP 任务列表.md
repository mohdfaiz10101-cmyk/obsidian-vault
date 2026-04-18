---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 12:43:53
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
- [x] [AGI→OP] [medium] Android P1：ADB安装Termux APK，配置SSH server — ❌ 2026-04-18 16:50 ADB 无法从 URL 直接安装 APK，需先下载到本地
- [x] [AGI→OP] [medium] Ti验证层：扫描CLAUDE.md MUST规则对比audit.log — ✅ 上会话已完成（92条MUST规则全扫，29条死规则确认）
- [x] [CC→OP] [medium] 派 subagent 检查 CC↔OP 互学机制健康度 — ❌ 2026-04-18 16:50 bash 无法完成 subagent 任务，需 CC 执行
- [x] [AGI→OP] [high] 动态资源管理：创建 ~/launcher/resource-manager.sh（whisper用完停/chronos CPU高暂停/sunshine无串流停/ttyd按需启动） — ✅ 2026-04-18 16:50 文件已存在，内容完整
- [x] [AGI→OP] [medium] 检查微信输入法显示问题（fcitx5/bwrap环境变量） — ✅ 2026-04-18 16:50 环境变量正确（GTK/QT/XMODIFIERS=fcitx），fcitx5 运行中
- [x] [OP→CC] [2026-04-18 11:58] [high] OP agent service-nurse 连续 3 次重启失败 — ✅ CC验证：服务已自恢复，属误报
- [x] [OP→CC] [2026-04-18 11:58] [high] OP agent discord-butler 连续 2 次重启失败 — ✅ CC验证：11:40后自恢复，属误报

## 清理记录
- 2026-04-18 12:00 三次清理：OP 持续注入重复任务（charlie-hub/Letta/OP监控）
- 2026-04-18 16:00 第四次清理：charlie-hub(×2)/OP监控/代理刷新/Letta重启 → 全部空跑产物，charlie-hub 不存在
- 根因：OP 调度器未实现 IDLE_GUARD 去重规则，需在 AGENTS.md 中强化
- 2026-04-18 12:30 第五次清理：discord-butler重复(已自恢复)/charlie-hub×2/OP监控/Letta → 空跑产物
- [ ] [AGI→OP] [2026-04-18 12:37] [high] 立即重启 charlie-hub 服务以恢复 Web UI 和反向代理功能
- [ ] [AGI→OP] [2026-04-18 12:37] [low] 检查并尝试重启 letta 服务

## CopilotKit AGI Control Plane — P0 任务（2026-04-18）
详细方案见 memory/ideas-roadmap.md 或 CC 本次会话

- [ ] [CC→OP] [2026-04-18 12:40] [high] CopilotKit-T01: mkdir -p /mnt/ai/apps/agi-control-plane/{backend/routers,frontend} && echo "目录已创建"
- [ ] [CC→OP] [2026-04-18 12:40] [high] CopilotKit-T02: 创建 /mnt/ai/apps/agi-control-plane/backend/main.py — FastAPI Gateway，路由: /api/brain /api/tasks /api/cognitive /api/flows /api/systemd /api/chat /copilotkit /sse/brain。依赖: pip install fastapi uvicorn httpx 到 ~/agi/.venv。验证: curl http://localhost:9900/api/brain 返回JSON
- [ ] [CC→OP] [2026-04-18 12:40] [high] CopilotKit-T03: 创建 ~/dotfiles/systemd/agi-gateway.service，WorkingDirectory=/mnt/ai/apps/agi-control-plane/backend，ExecStart=~/agi/.venv/bin/python -m uvicorn main:app --host 0.0.0.0 --port 9900。enable + start。验证: systemctl --user status agi-gateway
- [ ] [CC→OP] [2026-04-18 12:40] [medium] CopilotKit-T04: 在 /mnt/ai/apps/agi-control-plane/frontend 初始化 Next.js: BUN_INSTALL=/mnt/ai/cache/bun /mnt/ai/cache/bun/bin/bun create next-app . --ts --tailwind --app --no-src-dir。然后 bun add @copilotkit/react-core @copilotkit/react-ui @copilotkit/runtime recharts lucide-react
- [ ] [CC→OP] [2026-04-18 12:40] [medium] CopilotKit-T05: 创建 ~/dotfiles/systemd/agi-frontend.service，WorkingDirectory=/mnt/ai/apps/agi-control-plane/frontend，ExecStart=/mnt/ai/cache/bun/bin/bun run dev --port 3000。enable + start。验证: curl http://localhost:3000
- [ ] [OP→CC] [2026-04-18 12:40] [high] OP agent discord-butler 连续 3 次重启失败，需 CC 人工排查根因（检查 LiteLLM 健康/模型配置/Docker 网络）
