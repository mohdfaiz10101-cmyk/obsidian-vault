---
tags: [charlie-hub, index]
updated: 2026-04-18 08:03:47
---

# Charlie Hub 文档中心

> 自动同步自 `~/.claude/projects/-home-charlie/memory/`
> 最后更新：2026-04-18 08:03:47

---

## 📋 操作指南

- [[📋 操作命令速查手册]] - 所有命令的分类速查
- [[🤖 Discord Bot 测试指南]] - Discord Bot 功能测试
- [[🔧 问题排查手册]] - 常见问题解决方案
- [[📌 跨会话待办]] - 跨会话待办事项
- [[📋 OP 任务列表]] - OP 运维任务

---

## 📦 系统文档

- [[📦 Charlie Hub 部署文档]] - Hub 完整部署说明
- [[⚙️ NixOS 配置笔记]] - NixOS 系统配置
- [[🧠 系统核心档案]] - 设备清单、偏好、架构
- [[🔗 设备互联方案]] - 设备互联拓扑
- [[📓 Obsidian 同步指南]] - 同步机制说明

---

## 🔐 安全配置

- [[🔑 API Keys 管理说明]] - API Keys 集中式管理
- [[💡 经验教训记录]] - 踩坑记录和修复经验
- [[💡 经验教训归档]] - 历史踩坑归档

---

## 💭 开发资源

- [[💭 方案灵感汇总]] - 规划项目
- [[🤖 AI 工具对比]] - AI CLI 工具对比
- [[🗺️ 代码库探索缓存]] - 代码库结构缓存
- [[📱 App 开发日志]] - App/软件开发经验
- [[🔌 Cline API 分析]] - Cline API 集成分析

---

## 🌐 服务部署

- [[🌐 LiteLLM 部署]] - LiteLLM 网关配置
- [[🌐 LiteLLM 启动诊断]] - LiteLLM 故障排查
- [[📊 OP 评审报告]] - OP 运维评审
- [[📊 周报 W14]] - 2026 第14周错误回顾
- [[📊 周报 W15]] - 2026 第15周错误回顾

---

## 🔗 快速链接

- **Hub 主页**: http://localhost:9800
- **Agent Console**: http://localhost:9800/agents
- **AGI Dashboard**: http://localhost:9800/agi
- **Letta API**: http://localhost:8283

---

## 🔄 自动同步

此文档通过 `/home/charlie/.local/bin/sync-to-obsidian` 脚本自动同步。

手动同步：
\`\`\`bash
sync-to-obsidian
\`\`\`

定时同步：每 5 分钟自动执行（systemd timer）
