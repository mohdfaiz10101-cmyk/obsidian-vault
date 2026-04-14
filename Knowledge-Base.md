---
title: NixOS 系统知识库（精华整合）
date: 2026-03-29
tags: [nixos, ai, proxy, system, knowledge-base]
source: Claude 对话精华提取 + 去重整合
---

# NixOS 系统知识库

> 从 Claude Code 13 个 session + Desktop 文件 + Claude memory 提取，去重整合
> 更新时间：2026-03-29

## 系统架构

- NixOS Flake 架构，入口 flake.nix，输出端点 charlie。UEFI + GRUB 引导，EFI 挂载于 /boot（252MB，与 Windows 11 共用）
- 根分区 /dev/nvme0n1p9，89GB ext4，UUID 2d8662db-7f69-49a6-b396-ef96dc3e0b23。必须保持 <70% 使用率
- 数据盘 /mnt/data：sda4，932GB NTFS，UUID C672D33272D32649。AI 盘 /mnt/ai：100GB ext4 loopback（/mnt/data/ai-data.img）
- EFI /boot 252MB 与 Windows 11 共用，configurationLimit=3 防溢出。Gen 64 钉死为 GC root
- 桌面环境 KDE Plasma 6.6，GPU NVIDIA RTX 3060 Ti 闭源驱动。无 WiFi 硬件（Intel H510 桌机）
- 用户 charlie，密码 123456，mutableUsers=true。双系统 NixOS + Windows 11（同一 NVMe）

## 代理系统

- 代理：xray vless+ws+tls，HTTP 7890 + SOCKS5 7891，出口节点美国/新加坡
- xray 配置以 builtins.toJSON 内联写入 modules/proxy.nix（Flake 纯求值不支持 readFile /etc 路径）
- 系统代理：/etc/profile.d/proxy.sh 设置 http_proxy/https_proxy 环境变量
- GNOME 系统代理：gsettings manual 模式 127.0.0.1:7890。Firefox：user.js HTTP 代理
- mihomo 已停用（订阅节点全是香港被 Anthropic 封锁）。TUN 模式已移除

## AI 服务栈

- GLM Proxy (8787)：Anthropic 兼容代理 + 4 层记忆（L1 Claude+Letta / L2 Session / L3 Archival / L4 ChromaDB）
- Letta MCP (8788)：4 工具 (recall/search/ask/agents)，SSE 连接，agent: nixos-sysadmin
- Ollama (11434)：CUDA 加速，数据 /mnt/ai/ollama，KEEP_ALIVE=30m（冷启动 50s）
- 模型：qwen3:8b、deepseek-r1:14b。Continue 插件直连 Ollama 绕过 LiteLLM thinking 问题
- Chroma (8000)：向量数据库，数据 /mnt/ai/chroma-data/
- Docker AI 集群：Dify(3000) n8n(5678) LiteLLM(4000) AutoGen(8080)，compose 在 /mnt/ai/ai-cluster/
- 1688 智能采购：FastAPI(5000) + Chroma 去重 + Letta 筛选 + Telegram Bot 推送

## 关键操作流程

- 系统恢复：nix-recover（git pull 最新 → nixos-rebuild switch，自动处理代理）
- GRUB 黑屏救援：选 Gen 64 开机 → nix-recover。或手动 nix-env --switch-generation 64
- 安全重建：ns（先 build 验证，成功才 switch --install-bootloader）
- 清理：nc（nix-collect-garbage -d，不影响 GC root 钉住的 Generation）
- 密码重置：passwd charlie（mutableUsers 允许直接改）。SDDM 需手动输入用户名
- Docker 重置：docker compose down → 删 image → 重建。containerd snapshot 损坏需完全重置 data-root
- Flatpak 字体修复：复制 Noto Sans + CJK 到 ~/.local/share/fonts/（sandbox 不跟随 symlink）

## 踩坑记录

- /tmp 文件重启丢失——xray 配置等必须持久化到 /etc 或 home 目录
- EFI 挂载点必须正确：/boot 不是 /boot/efi。安装时注意 nvme0n1p2 的挂载路径
- pkill 误杀：用 pkill -f 'daemon.js' 精确匹配，避免杀掉 dockerd 等无关进程
- NixOS 脚本必须用 #!/usr/bin/env bash（系统没有 /bin/bash）
- Flake 纯求值：builtins.readFile 不能读 /etc 路径，用 builtins.toJSON 或 pkgs.writeText 内联
- GLM API timeout=120s 导致 systemd daemon 180s 超时被杀 → 改为 90s
- LiteLLM routing_strategy 不尊重 priority（usage-based 和 simple-shuffle 都不行）
- qwen3/deepseek-r1 thinking mode 导致 streaming 只有 reasoning_content，content 为空 → 直连 Ollama 绕过
- GNOME 覆盖 IM 环境变量为 ibus → extraGSettingsOverrides + excludePackages 修复
- 系统重装后 NTFS dirty volume：storage.nix 加 force 选项
- LiteLLM 容器 host.docker.internal 解析错误 → 改用 Docker 网关 IP 172.19.0.1
- Letta embedding endpoint 必须用 openai type + /v1 后缀（ollama type 用原生 API 导致 404）
- 根分区 88-90% 使用率危机——操作前必须 df -h / 检查

## API 配置

- Claude CLI：~/.zshrc 设置 ANTHROPIC_BASE_URL + ANTHROPIC_API_KEY
- JetBrains CC GUI 插件读 ~/.codemoss/config.json（不是 cc-switch 数据库）
- cc-switch AppImage v3.12.3 在 ~/.local/bin/cc-switch
- 2233.ai：3 个 key（包月 sk-FNY7 + 流量 sk-ycea + 流量 sk-rT6d），auto-switch-api.sh 自动故障转移
- 智谱 GLM API：open.bigmodel.cn，key c958396d，余额不足返回 429 1113
- Gemini API 免费额度 2026-02-27 用完，需等重置或升级付费

## 网络与设备

- PCDN 5G 网络：主机 192.168.2.136，平板 192.168.2.33:8022（Termux SSH）
- Windows 11 双系统：SSH 192.168.123.136
- Sunshine + Moonlight：远程桌面，端口 47990。触控：单指左键/双指右键/双指滑动滚轮/三指中键
- 平板 Termux SSH 自启：.bashrc 加 sshd 2>/dev/null
- ADB 无线调试需先 USB 连接一次（Android 11+）

## 开发环境

- JetBrains IDEA + Continue 插件：config ~/.continue/config.yaml（schema v1，需 name/version/schema 顶栏）
- Continue 聊天：Qwen3+DeepSeek 直连 Ollama，Claude 走 LiteLLM。Tab 补全：Qwen3 Ollama
- GLM CLI：glm-task '任务'（流式） / glm（交互） / glm-watch（悬浮监控）
- Obsidian：~/Documents/Obsidian/（Letta-Memory/ Memory-Fragments/ Claude-Sessions/）
- Dashboard：http://127.0.0.1:9099（Catppuccin Mocha 暗色主题，Flask 后端）
- MCP 配置（~/.claude.json）：notion(SSE) + letta-memory(SSE) + playwright(stdio)
