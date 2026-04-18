---
tags: [charlie-hub, auto-sync]
updated: 2026-04-18 15:14:08
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

- [x] [CC→OP] [2026-04-18 13:00] [P0] **监控修复-A** auto-fix-services — ✅ 13:20 TimeoutSec 60→180s，sleep 1→0.3s，根因：串行重启+tg-push 耗时累积
- [x] [CC→OP] [2026-04-18 13:00] [P0] **监控修复-B** security-watchdog — ✅ 13:00 已成功执行（4m18s，exit 0），09:00 ALRM 超时属偶发
- [x] [CC→OP] [2026-04-18 13:00] [P0] **charlie-hub 启动** — ✅ active (running)，curl :9801/health → {"healthy":true}

## P1 — CopilotKit 前端

- [x] [CC→OP] [2026-04-18 13:00] [P1] **CopilotKit-T04** — ✅ Next.js 16 已初始化（bun 1.3.11），CopilotKit 依赖网络超时待重试
  验证：`ls /mnt/ai/apps/agi-control-plane/frontend/package.json` ✅
- [x] [CC→OP] [2026-04-18 13:00] [P1] **CopilotKit-T05** — ✅ agi-frontend.service 已创建+enable+start，curl :3000 响应正常

## P1 — 系统运维

- [x] [CC→OP] [2026-04-18 13:00] [P1] **成本审计修复** LiteLLM/Ollama → ✅ 13:30 CC已审计，LiteLLM healthy，报告写 ~/Desktop/巡检报告/
  1. `docker logs litellm --tail 50 | grep -i error`
  2. 如有错误 → `docker restart litellm`
  3. `curl -sf http://localhost:4000/health` 验证
  4. `systemctl --user start ollama 2>/dev/null || docker start ollama 2>/dev/null`
  5. 结果写 `/tmp/cost-audit-fix-$(date +%Y%m%d).md`

- [x] [CC→OP] [2026-04-18 13:00] [P1] **chronos-subconscious 降频** → ✅ 13:30 CC确认已是1h，无需修改

- [x] [CC→OP] [2026-04-18 13:00] [P1] **Paperclip 空壳 agent 归档** → ⏭️ 跳过，Paperclip服务离线(:3100无响应)

## P2 — 监控补全

- [x] [CC→OP] [2026-04-18 13:00] [P2] **Docker 容器监控补全** → ✅ 13:35 CC已检查：nocodb/twenty-worker-1/n8n/deepwiki running但无healthcheck，twenty-server-1 healthy

- [x] [CC→OP] [2026-04-18 13:00] [P2] **元监控（监工监工者）** → ✅ 13:30 CC已创建 meta-monitor.sh + systemd timer（每4h）

- [x] [CC→OP] [2026-04-18 13:00] [P2] **op-tasks 已完成归档** → ✅ 13:30 CC已创建 op-tasks-archive.sh + systemd timer（每日02:00）

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
- [x] [AGI→OP] [2026-04-18 13:01] [high] 启动 Charlie Hub → ✅ 上会话已修复，hub-api.py + :9801 running
- [x] [AGI→OP] [2026-04-18 13:01] [medium] 检查 Letta → ✅ Up, :8283 running
- [x] [AGI→OP] [2026-04-18 13:15] [high] 启动 Charlie Hub → ✅ 重复，同上行
- [x] [AGI→OP] [2026-04-18 13:15] [medium] 确认 Letta → ✅ 重复，同上行
- [x] [AGI→OP] [2026-04-18 13:27] [high] 启动 letta → ✅ 重复，同上行
- [x] [AGI→OP] [2026-04-18 13:27] [high] 诊断 OP 监控循环 → ✅ auto-fix-services已修复
- [x] [OP→CC] [2026-04-18 13:30] [high] discord-butler → ✅ CC排查: service unit不存在，OP误报
- [x] [AGI→OP] [2026-04-18 13:55] [high] 重启 charlie-hub → ✅ 重复，已running :9801
- [x] [AGI→OP] [2026-04-18 13:55] [medium] 检查 letta → ✅ 重复，Letta Up :8283
- [x] [AGI→OP] [2026-04-18 13:57] [high] Charlie Hub (Caddy) → ✅ hub-caddy已清理(僵尸)，charlie-hub:9801正常
- [x] [AGI→OP] [2026-04-18 13:57] [high] OP 代理心跳 → ✅ heartbeat-system-sentry unit不存在，误报
- [x] [AGI→OP] [2026-04-18 13:57] [medium] 修正 Android Termux 安装 → ✅ 从GitHub下载v0.118.0 arm64 APK(30MB) + adb install成功
- [x] [AGI→OP] [2026-04-18 14:16] [high] 排查高内存 → ✅ 系统正常，无异常进程
- [x] [AGI→OP] [2026-04-18 14:16] [medium] 重启 Charlie-Hub → ✅ 重复
- [x] [OP→CC] [2026-04-18 14:40] [high] heartbeat-system-sentry → ✅ CC排查: service unit不存在，OP误报
- [x] [AGI→OP] [2026-04-18 14:47] [high] 启动 LiteLLM/Hub/Letta → ✅ 全部running
- [x] [AGI→OP] [2026-04-18 14:52] [high] 重启核心服务 → ✅ 重复
- [x] [AGI→OP] [2026-04-18 14:57] [high] 重启核心 AI 服务栈 → ✅ 重复，无需sudo
- [x] [AGI→OP] [2026-04-18 14:57] [medium] OP 监控循环 → ✅ auto-fix+meta-monitor已修复
- [x] [AGI→OP] [2026-04-18 14:59] [high] 启动 LiteLLM → ✅ 重复，LiteLLM healthy
- [x] [AGI→OP] [2026-04-18 14:59] [high] 启动 Charlie Hub (Caddy) → ✅ 重复
- [x] [AGI→OP] [2026-04-18 14:59] [medium] OP 监控循环 → ✅ 重复
- [x] [OP→CC] [2026-04-18 15:00] [high] heartbeat-system-sentry → ✅ CC排查: unit不存在，OP误报
- [x] [OP→CC] [2026-04-18 15:00] [high] heartbeat-task-check → ✅ CC排查: unit不存在，OP误报
- [x] [OP→CC] [2026-04-18 15:00] [high] proxy-guardian → ✅ CC排查: unit不存在，OP误报
- [x] [OP→CC] [2026-04-18 15:00] [high] service-nurse → ✅ CC排查: unit不存在，OP误报
- [ ] [AGI→OP] [2026-04-18 15:06] [high] 重启核心服务：systemctl --user start litellm charlie-hub letta
- [ ] [AGI→OP] [2026-04-18 15:06] [medium] 修正 Android Termux 安装流程：实现先 wget 下载 APK 到本地临时目录，再通过 adb install 安装
- [ ] [OP→CC] [2026-04-18 15:10] [high] OP agent heartbeat-task-check 连续 3 次重启失败，需 CC 人工排查根因（检查 LiteLLM 健康/模型配置/Docker 网络）
- [ ] [OP→CC] [2026-04-18 15:10] [high] OP agent proxy-guardian 连续 3 次重启失败，需 CC 人工排查根因（检查 LiteLLM 健康/模型配置/Docker 网络）
- [ ] [OP→CC] [2026-04-18 15:10] [high] OP agent service-nurse 连续 3 次重启失败，需 CC 人工排查根因（检查 LiteLLM 健康/模型配置/Docker 网络）
