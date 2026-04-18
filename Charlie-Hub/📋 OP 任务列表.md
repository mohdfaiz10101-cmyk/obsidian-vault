---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 11:53:52
source: /home/charlie/.claude/projects/-home-charlie/memory/op-tasks.md
---

# OP 待办任务（2026-04-18 清理后）

## 核心任务（CC 派发）
- [x] [AGI→OP] [2026-04-18 01:07] [high] 排查 Letta embedding 413 — 上个会话已修复（docker-compose.yml 添加 EMBEDDING 配置）
- [x] [AGI→OP] [2026-04-18 01:17] [high] P0-A 创建 ~/agi/cognitive_engine.py — ✅ 已创建（5.7K，八维认知评分器）
- [x] [AGI→OP] [2026-04-18 01:17] [high] P0-B 修改 ~/agi/macg.py — ✅ 已修改（cognitive_profile + cognitive_sense 节点）
- [x] [AGI→OP] [2026-04-18 01:17] [medium] P1 修改 ~/agi/brain.py — ✅ sense() 末尾已调用 cognitive_engine
- [x] [AGI→OP] [2026-04-18 01:07] [high] Android P0 — ✅ device-registry.json + sensor-bridge.py 已创建
- [ ] [AGI→OP] [2026-04-18 01:07] [medium] Android P1：ADB安装Termux APK，配置SSH server，生成ed25519密钥
- [x] [AGI→OP] [2026-04-18 00:55] [high] 实现 flows/self_heal.py — ✅ 已创建（10K）
- [x] [AGI→OP] [2026-04-18 00:55] [high] 实现 flows/task_decompose.py + code_loop.py + evolve.py + workflow_gen.py — ✅ 全部已创建
- [ ] [AGI→OP] [2026-04-18 01:11] [medium] Ti验证层：扫描CLAUDE.md所有MUST规则对比audit.log，输出从未触发的规则列表

## 新任务
- [ ] [CC→OP] [2026-04-18 11:30] [high] 配置 LiteLLM 添加 GLM 视觉模型（glm-4v-plus），更新 OP 路由支持图片输入
- [ ] [CC→OP] [2026-04-18 11:30] [medium] 派 subagent 检查 CC↔OP 互学机制健康度

## 已清理说明
- 2026-04-18 11:27 清理：删除 ~130 行重复任务（charlie-hub×30+、内存检查×40+、Letta 重启×20+、OP 监控×15+）
- 这些全部是 OP 空跑循环产物（charlie-hub 无 systemd 服务、内存高是常态）
- 原因：OP 缺乏去重逻辑，每轮巡检都生成相同任务 → 需 op-task-runner 前置去重（IDLE_GUARD 规则）
- [ ] [AGI→OP] [2026-04-18 11:28] [high] 启动 charlie-hub 服务，恢复 Web UI 及代理功能
- [ ] [AGI→OP] [2026-04-18 11:28] [high] 诊断 OP 进程状态并重启监控循环，解决数据过期问题
- [ ] [AGI→OP] [2026-04-18 11:28] [medium] 检查高内存占用进程（检查是否有残留的本地模型或 Python 进程）

- [ ] [AGI→OP] [2026-04-18 11:29] [high] 动态资源管理：创建 ~/launcher/resource-manager.sh，规则：(1) whisper.service 用完自动停(voxtype通话结束后30s)节省1.5GB内存 (2) chronos-sensory/biofeedback 在CPU>80%时暂停 (3) sunshine在无串流连接时停止 (4) ttyd-aider仅在用户打开时按需启动。实现方式：systemctl --user stop/start + inotify监控状态文件
- [ ] [AGI→OP] [2026-04-18 11:29] [medium] 检查微信输入法显示问题：确认fcitx5是否运行(systemctl --user is-active fcitx5)；检查XMODIFIERS/GTK_IM_MODULE环境变量是否注入bwrap容器；尝试在微信输入框中用英文输入是否正常，定位是候选框问题还是字符上屏问题
- [ ] [AGI→OP] [2026-04-18 11:30] [medium] 检查并重启 OP 监控守护进程，确保状态每 5-15 分钟上报
- [ ] [AGI→OP] [2026-04-18 11:33] [high] 重启 Charlie Hub 服务以恢复总控 UI 及反向代理功能
- [ ] [AGI→OP] [2026-04-18 11:33] [medium] 检查 Letta 服务日志并尝试重启
- [ ] [AGI→OP] [2026-04-18 11:35] [high] 重启 charlie-hub 服务以恢复 Web UI (端口 9800)
- [ ] [AGI→OP] [2026-04-18 11:35] [high] 重启 letta 服务
- [ ] [AGI→OP] [2026-04-18 11:35] [medium] 检查 OP 监控脚本/服务为何停止上报状态
- [ ] [AGI→OP] [2026-04-18 11:36] [high] 启动 charlie-hub 服务以恢复仪表盘和反向代理功能
- [ ] [AGI→OP] [2026-04-18 11:36] [medium] 启动 letta 服务以恢复长期记忆和状态管理
- [ ] [AGI→OP] [2026-04-18 11:36] [low] 检查 OP 守护进程状态，确认为何超过 20 小时未上报数据
- [ ] [AGI→OP] [2026-04-18 11:39] [high] 立即重启 charlie-hub 服务以恢复总控界面访问
- [ ] [AGI→OP] [2026-04-18 11:41] [medium] 检查并修复 OP 心跳机制，确保状态实时同步
- [ ] [AGI→OP] [2026-04-18 11:42] [high] 启动 Charlie Hub 服务 (systemctl start charlie-hub)
- [ ] [AGI→OP] [2026-04-18 11:42] [medium] 检查并启动 Letta 服务
- [ ] [AGI→OP] [2026-04-18 11:42] [medium] 刷新 OP 状态监控数据
- [ ] [AGI→OP] [2026-04-18 11:42] [low] 调查 Charlie Hub 和 Letta 服务停止的原因（日志分析）
- [ ] [AGI→OP] [2026-04-18 11:45] [high] 重启 Charlie Hub (Caddy) 服务，恢复 9800 端口总控 UI 与反向代理
- [ ] [AGI→OP] [2026-04-18 11:45] [medium] 检查并恢复 Letta 服务运行状态
- [ ] [AGI→OP] [2026-04-18 11:45] [medium] 排查 OP 监控守护进程，解决状态上报滞后问题
- [ ] [AGI→OP] [2026-04-18 11:46] [high] 立即重启 charlie-hub 服务以恢复总控 UI 与反向代理
- [ ] [AGI→OP] [2026-04-18 11:48] [high] 启动 charlie-hub 服务以恢复 Hub 界面和代理功能
- [ ] [AGI→OP] [2026-04-18 11:48] [low] 执行系统状态检查以刷新 OP 监控数据
- [ ] [AGI→OP] [2026-04-18 11:52] [high] 重启 Charlie Hub 服务以恢复 9800 端口访问
