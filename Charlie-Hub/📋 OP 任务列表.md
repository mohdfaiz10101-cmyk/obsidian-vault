---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 13:23:54
source: /home/charlie/.claude/projects/-home-charlie/memory/op-tasks.md
---

# OP 待办任务（2026-04-18 全量流转）

## 已完成存档
- [x] [AGI→OP] 排查 Letta embedding 413 — ✅ 已修复
- [x] [AGI→OP] P0-A cognitive_engine.py / P0-B macg.py / P1 brain.py / flows/*.py — ✅ 已创建
- [x] [CC→OP] LiteLLM GLM 视觉模型 — ✅ glm-4.6v-flash + glm-4v-flash 已添加
- [x] [CC→OP] op-connection-guard.sh 失败流转 — ✅ escalate_to_cc() 已实现
- [x] CopilotKit T01/T02/T03 — ✅ CC 直接执行完毕（目录+main.py+agi-gateway:9900）
- [x] charlie-hub hub-api.py — ✅ 2026-04-18 已修复

---

## P0 — 立即执行

- [ ] [CC→OP] [2026-04-18 13:00] [P0] **监控修复-A** `auto-fix-services` 服务 timeout 被杀 → 检查 `journalctl --user -u auto-fix-services -n 50`，找出超时原因，修复或增加 TimeoutSec
- [ ] [CC→OP] [2026-04-18 13:00] [P0] **监控修复-B** `security-watchdog` signal 异常（3h+ 无数据）→ 检查 OpenCode job security-watchdog.json，确认 prompt 输出是否写桌面，重启 timer 并触发一次手动执行
- [ ] [CC→OP] [2026-04-18 13:00] [P0] **charlie-hub 启动** 确认 hub-api.py 正常：`systemctl --user status charlie-hub`，如未运行则 `systemctl --user restart charlie-hub`，验证 `curl http://localhost:9801/health`

## P1 — CopilotKit 前端

- [ ] [CC→OP] [2026-04-18 13:00] [P1] **CopilotKit-T04** 初始化 Next.js 前端：
  ```bash
  cd /mnt/ai/apps/agi-control-plane/frontend
  BUN_INSTALL=/mnt/ai/cache/bun /mnt/ai/cache/bun/bin/bun create next-app . --ts --tailwind --app --no-src-dir --yes
  /mnt/ai/cache/bun/bin/bun add @copilotkit/react-core @copilotkit/react-ui @copilotkit/runtime recharts lucide-react
  ```
  验证：`ls /mnt/ai/apps/agi-control-plane/frontend/package.json`
- [ ] [CC→OP] [2026-04-18 13:00] [P1] **CopilotKit-T05** 创建并启动前端服务：
  写入 `~/.config/systemd/user/agi-frontend.service`（WorkingDirectory=/mnt/ai/apps/agi-control-plane/frontend，ExecStart=/mnt/ai/cache/bun/bin/bun run dev --port 3000），enable + start，验证 `curl http://localhost:3000`

## P1 — 系统运维

- [ ] [CC→OP] [2026-04-18 13:00] [P1] **成本审计修复** LiteLLM/Ollama：
  1. `docker logs litellm --tail 50 | grep -i error`
  2. 如有错误 → `docker restart litellm`
  3. `curl -sf http://localhost:4000/health` 验证
  4. `systemctl --user start ollama 2>/dev/null || docker start ollama 2>/dev/null`
  5. 结果写 `/tmp/cost-audit-fix-$(date +%Y%m%d).md`

- [ ] [CC→OP] [2026-04-18 13:00] [P1] **chronos-subconscious 降频** 修改 timer：`OnUnitActiveSec=20min` → `OnUnitActiveSec=1h`，同时在 ExecStart 脚本中加检查 `[ $(awk '{print int($1)}' /proc/loadavg) -gt 4 ] && exit 0`（CPU 高负载跳过）

- [ ] [CC→OP] [2026-04-18 13:00] [P1] **Paperclip 空壳 agent 归档** 检查 Paperclip API（port 3100），列出所有 agent，停止心跳为 0 且 status=idle 超 7 天的 agent（最多 6 个），写结果到 `/tmp/paperclip-cleanup-$(date +%Y%m%d).json`

## P2 — 监控补全

- [ ] [CC→OP] [2026-04-18 13:00] [P2] **Docker 容器监控补全** 为以下容器添加 healthcheck 巡检（追加到 service-nurse OpenCode job）：nocodb、twenty-server、twenty-worker、n8n、deepwiki。检查方式：`docker inspect --format='{{.State.Health.Status}}' <name>`，异常则写 `/tmp/docker-health-$(date +%Y%m%d).md`

- [ ] [CC→OP] [2026-04-18 13:00] [P2] **元监控（监工监工者）** 创建 `~/.local/bin/meta-monitor.sh`：检查以下 timers 是否 active + 最近触发时间 < 阈值×2（超时则写告警到 `/tmp/meta-monitor-alert.md`）：system-health-monitor / letta-health-guard / mihomo-guardian / cc-task-auditor / opencode-job-*-security-watchdog / opencode-job-*-proxy-guardian / opencode-job-*-heartbeat-task-check。创建对应 timer，每 4h 运行一次

- [ ] [CC→OP] [2026-04-18 13:00] [P2] **op-tasks 已完成归档** 将 op-tasks.md 中所有 [x] 超过 24h 的条目移至 `op-tasks-archive-$(date +%Y%m).md`，保持活跃文件精简。每日 02:00 自动执行（创建 systemd timer）

## APK — CopilotKit Android 手机版

> **目标**：将 AGI Control Plane（port 9900 SSE + op-tasks.md）打包成 Android APK，通过 Tailscale 连接主机
> **技术栈**：Capacitor 6 包装 Next.js 前端 → Android APK
> **前置依赖**：APK-A0 完成后才能执行 APK-A1~A3

- [ ] [CC→OP] [2026-04-18] [APK-A0] **环境预检 + 搭建** 检查并安装构建 APK 所需环境：
  ```bash
  # 1. 检查 JDK
  java -version 2>&1 | head -1 || echo "JDK_MISSING"
  # 2. 检查 Android SDK（已迁移至 /mnt/ai/data/android）
  ls /mnt/ai/data/android/cmdline-tools/latest/bin/sdkmanager 2>/dev/null || echo "SDK_MISSING"
  # 3. 如果缺失 JDK，用 nix profile 安装：
  nix profile install nixpkgs#jdk17
  # 4. 如果缺失 SDK，下载 cmdline-tools：
  mkdir -p /mnt/ai/data/android/cmdline-tools && \
    wget -q https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip \
    -O /tmp/cmdline-tools.zip && \
    unzip -q /tmp/cmdline-tools.zip -d /tmp/ct && \
    mv /tmp/ct/cmdline-tools /mnt/ai/data/android/cmdline-tools/latest
  # 5. 安装 Android SDK platform + build-tools（需要先 accept-licenses）
  yes | /mnt/ai/data/android/cmdline-tools/latest/bin/sdkmanager \
    "platforms;android-35" "build-tools;35.0.0" "platform-tools" 2>&1 | tail -5
  ```
  验证：`java -version && ls /mnt/ai/data/android/platforms/android-35`
  结果写 `/tmp/apk-env-check.md`

- [ ] [CC→OP] [2026-04-18] [APK-A1] **初始化 Capacitor 项目** 在 `/mnt/ai/apps/agi-control-plane/android-app` 创建项目：
  ```bash
  ANDROID_SDK_ROOT=/mnt/ai/data/android
  export ANDROID_SDK_ROOT
  cd /mnt/ai/apps/agi-control-plane
  mkdir -p android-app && cd android-app
  BUN=/mnt/ai/cache/bun/bin/bun
  $BUN init -y
  $BUN add @capacitor/core @capacitor/cli @capacitor/android
  # 初始化 Capacitor，webDir 指向已有前端 build 输出
  npx cap init "AGI控制台" "com.charlie.agi" --web-dir=www
  # 添加 Android 平台
  npx cap add android
  ```
  验证：`ls /mnt/ai/apps/agi-control-plane/android-app/android/app/src/main/`
  结果写 `/tmp/apk-init.md`

- [ ] [CC→OP] [2026-04-18] [APK-A2] **写入 AGI 手机 UI** 在 `/mnt/ai/apps/agi-control-plane/android-app/www/` 创建单页 HTML App（无需 Next.js 编译）：
  - `index.html`：AGI 状态卡片 + 对话框 + op-tasks 列表
  - 连接目标：`http://{TAILSCALE_IP}:9900`（从 localStorage 读取 IP 配置）
  - SSE：`/sse/brain` 实时显示 AGI 状态
  - REST：`GET /api/tasks` 显示 op-tasks；`POST /api/cognitive` 发送指令
  - 离线提示：Tailscale 未连接时显示"请开启 Tailscale"
  文件路径：`/mnt/ai/apps/agi-control-plane/android-app/www/index.html`（≤200行）
  `npx cap sync android`

- [ ] [CC→OP] [2026-04-18] [APK-A3] **构建 APK + 导出** 执行 Gradle 构建：
  ```bash
  export ANDROID_SDK_ROOT=/mnt/ai/data/android
  export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))
  cd /mnt/ai/apps/agi-control-plane/android-app/android
  chmod +x gradlew
  ./gradlew assembleDebug 2>&1 | tail -20
  # APK 输出路径
  APK_PATH=$(find . -name "*.apk" -newer gradlew 2>/dev/null | head -1)
  echo "APK: $APK_PATH"
  cp "$APK_PATH" ~/Desktop/文档/agi-console-$(date +%Y%m%d).apk
  ```
  验证：`ls ~/Desktop/文档/agi-console-*.apk`
  结果写 `~/Desktop/巡检报告/apk-build-$(date +%Y%m%d).md`（含 APK 路径 + 安装命令）

## 执行规则（MUST）
- 每个任务完成后 MUST 将 `[ ]` 改为 `[x]` 并附上时间戳和结果摘要
- 失败超过 2 次 MUST 改为 `[!]` 并写明失败原因，流转回 CC
- 执行前先 grep 是否有重复任务（IDLE_GUARD）
- 结果文件统一写到 `~/Desktop/巡检报告/` 或 `/tmp/`，不要只输出到终端
- [ ] [AGI→OP] [2026-04-18 13:01] [high] 启动 Charlie Hub (Caddy) 服务以恢复总控 UI 和反向代理功能
- [ ] [AGI→OP] [2026-04-18 13:01] [medium] 检查 Letta 服务停止原因并决定是否需要重启
- [ ] [AGI→OP] [2026-04-18 13:15] [high] 启动 Charlie Hub 服务（端口 9800）
- [ ] [AGI→OP] [2026-04-18 13:15] [medium] 确认 Letta 服务是否需要运行，如需要则启动
